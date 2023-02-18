```js
    const fs = require("fs");
    const http = require("http");

    function heavyLoad() {
    console.log("entered heavy load");
    let i = 0;
    for (i; i < 1e9; i++) {}
    console.log("exited heavy load");
    return i;
    }

    async function makeRequest() {
    console.log("entered request");
    return new Promise((resolve) => {
        http
        .request("http://www.google.com", (res) => {
            res.on("data", (data) => {});
            res.on("end", () => {
            console.log("exited request");
            resolve("request");
            });
        })
        .end();
    });
    }

    async function controller() {
    console.log("entered controller");

    const timeout = new Promise((resolve) => {
        console.log("entered timeout");
        setTimeout(() => {
        console.log("triggered timeout");
        resolve("timeout done");
        }, 0);
        console.log("exited timeout");
    });

    // this call is sync, which is blocking the event loop
    const file = fs.readFileSync("./A.csv");

    // with await, the event loop will be blocked.
    // interesting: while waiting for the http response, node process some callback
    // on the queue because it has nothing else to do
    const request = await makeRequest();

    // Blocking operation on Event Loop
    // Once this code is reached, other callbacks/events will be executed
    const heavy = heavyLoad();

    console.log("passed controller");
    return Promise.all([timeout, heavy, file, request]);
    }

    controller().then((res) => {
    console.log(res);
    });

```