# 🚧 Under construction 🚧

```js
const { Worker, isMainThread, parentPort } = require("worker_threads");
if (isMainThread) {
  console.log("entrei");

  const worker = new Worker(__filename);

  const wait = new Promise((resolve, reject) => {
    console.log("entrei pro");

    worker.on("message", (msg) => {
      console.log(msg);
      resolve("oie");
    });

    worker.postMessage("oieee");
    console.log("sai");
  });

  wait.then((res) => {
    console.log(res);
    worker.terminate();
  });

  console.log("here1");

  const wait1 = new Promise((resolve, reject) => {
    console.log("entrei pro1");
    setTimeout(() => {
      resolve("meu");
    }, 2000);
    console.log("sai1");
  });

  console.log("here2");
  const wait2 = new Promise((resolve, reject) => {
    console.log("entrei pro2");
    setTimeout(() => {
      resolve("amigo");
    }, 3000);
    console.log("sai2");
  });
  console.log("here3");

  wait1.then((res) => {
    console.log(res);
  });
  wait2.then((res) => {
    console.log(res);
  });
} else {
  parentPort.on("message", (msg) => {
    console.log(msg);
    for (i = 0; i < 1e9; i++) {}
    parentPort.postMessage("returned");
  });
}

```