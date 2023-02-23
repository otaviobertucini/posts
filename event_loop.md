## NodeJS Event Loop

- Node is single-threaded, wich means it executes only one thing at a time
- Two important things in Node runtime: the stack and the queue
  - Stack: chunks of code executed in LIFO mode
  - Queue: chunks of code that will be executed when the stack is empty/idle
- How does Node handle async code?
    - It executes the code normally and when Node reaches the async part (eg. http request or read a file), it creates a parallel job in backgound and continues to execute the code
    - When the async job is done, a callback function with the result of the async execution is added the the queue
    - When the stack is empty or idle, the callbacks in the queue will be executed
    - Exemple: 
        ```js
        async function controller() {
            console.log("entered function");
            const timeout = new Promise((resolve) => {
                console.log("entered timeout");
                setTimeout(() => {
                console.log("triggered timeout");
                    resolve("timeout done");
                }, 0);
                console.log("exited timeout");
            });
            timeout.then((res) => {
                console.log(res);
            });
            console.log("exited function");
        }
        controller().then((res) => {});
        ```
    - The output of this chunk of code will be:
        ```
            entered function
            entered timeout
            exited timeout
            exited function
            triggered timeout
            timeout done
        ```
    - But how the timeout trigger is the last if we set the timeout to 0?
        - This is because the setTimeout function creates a parallel job in the backgound. It is not executed in the stack. 
        - Once the parallel job is terminated (in this case immediatelly), the callback of the async job will be added to the queue.
        - And because the stack is not empty (there are some console.logs to be executed), the callback will only be executed when all synchronous code is done. 
    - Now, something interesting can happen when we add an `await` on a async code
        - `await` will block the event loop until the promise is resolved (ie. the async job it is waiting is executed), so Node will not execute the following lines of code.
        - BUT, Node can use this "idle" time while waiting for the promise to execute some callbacks in the queue
        - Example: 
        ```js
            const http = require("http");

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
                console.log("entered function");
                const timeout = new Promise((resolve) => {
                    console.log("entered timeout");
                    setTimeout(() => {
                    console.log("triggered timeout");
                    resolve("timeout done");
                    }, 0);
                    console.log("exited timeout");
                });
                timeout.then((res) => {
                    console.log(res);
                });

                const request = await makeRequest();

                console.log("exited function");
            }
            controller().then((res) => {});

        ```
        - The output of this chunk of code will be:
        ```
            entered function
            entered timeout
            exited timeout
            entered request
            triggered timeout
            timeout done
            exited request
            exited function
        ```
        - We can see that while waiting for the request, Node executed the callbacks from the timeout. 
    - Now, let's see an example of code that will make our event loop block.
        - To to this, I simulated a heavy load of code by making a big for loop. 
        - I also changed the setTimeout to trigger after 2000ms (2 seconds)
        ```js
            function heavyLoad() {
                console.log("entered heavy load");
                let i = 0;
                for (i; i < 1e10; i++) {}
                console.log("exited heavy load");
                return i;
            }

            async function controller() {
                console.log("entered function");
                const start = Date.now()
                const timeout = new Promise((resolve) => {
                    console.log("entered timeout");
                    setTimeout(() => {
                        console.log("triggered timeout in ", Date.now() - start);
                        resolve("timeout done");
                    }, 2000);
                    console.log("exited timeout");
                });
                timeout.then((res) => {
                    console.log(res);
                });

                heavyLoad();

                console.log("exited function");
            }
            controller().then((res) => {});

        ```
        - The output of this chunk of code will be:
        ```
            entered function
            entered timeout
            exited timeout
            entered heavy load
            exited heavy load
            exited function
            triggered timeout in  15112
            timeout done
        ```
        - Wait. Why the timeout took 15 seconds to execute, once we have set it to two seconds?
            - This is because the "heavy load" function blocked the event loop and it could not execute other callbacks or events. 
            - The "heavy load" is made of synchronous code, so it will block the event loop until it finishes. 
            - We can prevent this by using Worker Threads, but this is another subject. 
