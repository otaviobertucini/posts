# Clusters

As I mentioned before, when Node.js starts, it only uses one thread to handle the event loop. But, that can limit the performance and scalability of apps that need to process a lot of requests or require heavy processing.

To fix that, Node.js comes with a module called "cluster". The "cluster" module lets developers create a cluster of worker processes that can work together and split the workload. Each worker process runs on a separate thread and has its own event loop. When a new request arrives, the master process puts it in a queue and distributes it among the worker processes. Each worker process picks up the next request in the queue and processes it using its own event loop.

The "cluster" module uses a round-robin scheduling algorithm to distribute incoming requests among the worker processes in the cluster. This means that each incoming request is assigned to the next available worker process in a cyclic order, starting from the first worker process in the list of active workers. Once all worker processes have been assigned a request, the algorithm starts over from the beginning of the list.

Here is an example code:
```js
const cluster = require('cluster'); // Import the 'cluster' module.
const express = require('express'); // Import the 'express' module.
const numCPUs = require('os').cpus().length; // Determine the number of CPU cores available.

if (cluster.isMaster) { // Check if the current process is the master process.
  console.log(`Master ${process.pid} is running`); // Log a message indicating that the master process is running.

  // Fork workers.
  for (let i = 0; i < numCPUs; i++) { // Loop over the number of CPU cores available.
    cluster.fork(); // Fork a new worker process for each CPU core.
  }

  cluster.on('exit', (worker, code, signal) => { // Set up an event listener for the 'exit' event emitted by worker processes.
    console.log(`Worker ${worker.process.pid} died`); // Log a message indicating which worker process has died.
    cluster.fork(); // Restart the worker process.
  });
} else { // If the current process is not the master process, it must be a worker process.
  console.log(`Worker ${process.pid} started`); // Log a message indicating that a worker process has started.

  const app = express(); // Create an instance of the Express application.

  app.get('/', (req, res) => { // Define a route handler for the root URL.
    res.send('Hello, World!'); // Send a response to the client.
  });

  app.get('/heavyLoad', (req, res) => { // Define a route handler for the '/heavyLoad' URL.
    for (let i = 0; i < 1e9; i++) { // Loop a billion times to create a heavy load on the CPU.
      // Do nothing.
    }
    res.send('Heavy load complete!'); // Send a response to the client.
  });

  app.listen(3000, () => { // Start the Express server listening on port 3000.
    console.log(`Worker ${process.pid} listening on port 3000`); // Log a message indicating which worker process is listening on which port.
  });
}
```

Creating too many clusters can lead to a degradation in performance and increased overhead, as each cluster requires system resources to manage and distribute incoming requests among its worker processes. It's important to balance the number of clusters with the available system resources to ensure optimal performance and scalability.

One alternative to "clusterise" your application is by using PM2, which allows you to easily create a cluster of Node.js worker processes with a single command. PM2 automatically detects the number of available CPU cores and creates a corresponding number of worker processes, each running in a separate thread and sharing the workload.

In general, clusters are a good choice for high-performance applications that need to handle a large volume of incoming requests, while workers are a good choice for background processing or computationally-intensive tasks that can be offloaded from the main event loop.