# JObs in background

As I said in my other post about event loop, Node can only run one thing at a time. This fact means that some heavy operations can block the event loop, making Node unavailable to proccess new requests or events that might appear.

Let's use a basic express application as an example, were Node makes a heavy workload when a GET request is made in "/heavy". As you can see in the code below, when this route is called a long for loop is executed and since this is not a async operation, Node will not be able do execute anything until the for loop ends.

```js
// server.js

function heavyLoad() {
  console.log("entered heavy load");
  let i = 0;
  for (i; i < 1e10; i++) {}
  console.log("exited heavy load");
  return i;
}

app.get("/heavy", (req, res) => {
  heavyLoad();
  res.send("Hello World");
});

app.use("/fast", (req, res) => {
  res.send("I'm fast");
});
```

In this code, if we make a request to "/heavy" and shortly thereafter make a request to "/fast", the second request will have to wait until the first request ends to be executed. In other words, a small request such as "/fast" will be blocked and not executed until "/heavy" ends.

Fortunetelly, there are three things we can do to prevent this blocking behavior to happen:

- Clusters
- Workers
- Jobs

I'll cover only the last two in this article and Clusters will be covered in other one.

## Workers

As said in the documentation of Node, "the node:worker_threads module enables the use of threads that execute JavaScript in parallel.". So, the idea here is to execute the blocking code in parallel so that Node can execute other things while waiting for the heavy load to complete. Because we are working with Workers, I splited the previous code in two files: `server.js` and `worker.js` (it's possible to do in one, but I prefer this way):

```js
// server.js

const { Worker } = require("worker_threads");
const express = require("express");

const app = express();

app.get("/heavy", async (req, res) => {
  console.log(req.baseUrl);

  const heavy_load_response = await new Promise((resolve, reject) => {
    const worker = new Worker("./worker.js");
    worker.postMessage(1e10);

    worker.on("message", resolve);
    worker.on("error", reject);
  });

  res.send(heavy_load_response);
});

app.use("/fast", (req, res) => {
  res.send("I'm fast");
});

app.listen(2001, () => console.log("server started"));
```

```js
// worker.js

const { parentPort } = require("worker_threads");

function heavyLoad(number) {
  console.log("entered heavy load");
  let i = 0;
  for (i; i < number; i++) {}
  console.log("exited heavy load");
  return i;
}

parentPort.on("message", (message) => {
  heavyLoad(Number(message));

  parentPort.postMessage("Done heavy load!");
});
```

To create a new thread in Node, we instantiate a object of the class `Worker` from the library `worker_threads` and pass to the constructor the file path where the code to be executed in the new thread is located. Once the new thread is created, the main thread (the one running express) can communicate with child threads via `.postMessage` method. This command basically sends a message to a child thread. Note that the Worker was intantiated inside a Promise, because we will wait until the execution of the child thread is done to proceed.

On the child thread (see the worker.js file), we use the `parentPort` object from `worker_threads` library to communicate with the parent thread. The `parentPort.on('message', cb)` is executed when the child receives a message. In our case, when receiving a message from the parent thread we will execute the `heavyLoad` function that was previously being executed inside the exepress route. After executing the heavy load, the child sends a message to the parent saiyng the execution was done.

Back in the parent, when receiving a message from the child thread via the `parentPort.on('message')` method, we will resolve the Promise that we created and pass the massage we received as the value of the Promise, that in this case is "Done heavy load!". Therefore, we send the result of the Promise as the response of the request. 

Doing so we execute the heavy load in an asynchronous manner and Node can execute other things while the child thread is executing. To prove it, if we send a request to "/heavy" and shortly thereafter make a request to "/fast", the second request will be executed immediatelly, not having to wait until the "/heavy" request to be done. Remember that since we are creating new threads, the requests and the heavy load are executed in the same server. But what if we would like to execute the heavy load in other server? This is possible with Javascript jobs.

## Jobs

Another option is to use jobs to make the heavy load be executed in another server. Let's say we have a server that is configured to handle heavy and long work loads and we want to pass all the heavy executions to it. Then, when receiving a request on "/heavy" endpoint, instead of executing the heavy load on the `express` server, we will execute it on the heavy load server. We can use the `bullmq` library to achieve this, by creating a queue that will be the linking between the express server and the heavy load server. The express server add items to the queue and the heavy load server listen to new items on the queue and process them.

```js
// server.js
const express = require("express");
const bullmq = require("bullmq");
const { Queue } = bullmq;

const app = express();

const queue = new Queue("Loads", {
  connection: {
    host: "127.0.0.1",
    port: "6379",
  },
});

app.get("/heavy", async (req, res) => {
  console.log(req.baseUrl);

  await queue.add("heavyLoad", 1e10);

  return res.status(200).json({ processing: true });
});

app.use("/fast", (req, res) => {
  res.send("I'm fast");
});

app.listen(2001, () => console.log("server started"));
```

```js
// worker.js
const bullmq = require("bullmq");
const { Worker } = bullmq;

async function processJob(job) {
  if (job.name === "heavyLoad") {
    console.log("entered heavy load");
    let i = 0;
    for (i; i < job.data; i++) {}
    console.log("exited heavy load");
    return i;
  }
}

const worker = new Worker("Loads", processJob, {
  connection: {
    host: "127.0.0.1",
    port: "6379",
  },
  concurrency: 1,
});

worker.on("completed", (job, result) => {
  console.log(result);
});

```

On the `server.js` code, I create a queue using the `Queue` class from `bullmq` library and I passed to the contructor a name, that in this case is "Loads", and a Redis connection object. `bullmq` uses Redis to build its queue and that the reason we need to pass a Redis server as parameter. 

On the "/heavy" route, we will add a new item on the queue created in the step above intead of executing the heavy load each time the route is called. We do that by using the method `queue.add`. Once the item is addded to the queue, we just return success to the requester. So, using jobs we will not wait until the execution of the heavy load is done, like we did in the Workers approach.

Now, we must have the second server to read the items in the queue and execute them. To do this, we create a new file and instantiate a new `Worker` class from `bullmq` library. This class receives three argumets: the name of the queue to be read, a callback that will be called once new messages arrive to the queue and a settings object, containing the connetion data to the Redis server that is storing the queue and how many concurrent jobs can be runned at a time. 

In our example we passed the `processJob` in the callback argument and it will be executed each time new items are added to the queue. The function checks if the name of the job is "heavyLoad" and if so, executes the heavy load code with the value passed as the item data and when it completes the value is returned. We also tell the worker that each time a job is complete the result must be logged, by using the `worker.on("completed", cb)` method. 

Note that each file will generate one server, which means the requests will be executed in one server and the heavy loads on other. We also made the heavy load server execute on job at a time using the `concurrency` option, to ensure that it won't break when a lot of jobs need to be handled. 

## Conclusion

In this article, it was showed how NodeJS can handle heavy synchronous code without blocking the event loop by using Workers and Jobs. In the next one I will talk about Clusters, a feature that also allows Node to deal with blocking behaviour on servers. 