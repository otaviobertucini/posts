# Advanced NodeJS

[https://www.udemy.com/course/advanced-node-for-developers/](https://www.udemy.com/course/advanced-node-for-developers/)

![Untitled](Advanced%20NodeJS%20afaa880b2f5b469bbe92ac17753f96ff/Untitled.png)

- Node is an abstraction of V8 and libuv, so that we don’t have to write C++ code to use their features.
- Also, it encapsulates a lot of well design APIs such as HTTP or fs

- Inside NodeJS project
    - lib folder: expose the javascript functions
    - src folder: implementations in C++
- Processing
    - Process have threads
    - Each thread has steps that need to be executed in the CPU Cores
    - More cores, more threads can be executed
    - I/O operations are asynchronous and the time they wait for the response can be used to process other threads
- Event Loop
    - The NodeJS Event Loop decides what is going to be processed in any given time (what one thread is going to do)
    - There is exactly one Event Loop in each Node program
    - [https://www.youtube.com/watch?v=8aGhZQkoFbQ](https://www.youtube.com/watch?v=8aGhZQkoFbQ)
    - All the event loop executes in one tick
    - After every tick, Node checks if the event loop should still be running. The checks are:
        - Pending timers?
        - Pending OS tasks?
        - Pending long running operations?
    - In each tick, Node does the following:
        - execute ready setTimeouts and setInterval
        - execute ready OS tasks and operations
        - pause executions and wait for
            - triggered timeout
            - os task completed
            - processing completed
        - execute ready setImmediate
        - execute “close” events
    - Once a function call is complete, it gets removed from the stack. Once the stack is empty, we're ready for the next item in the Queue.
    - Best article on the matter: [https://thomashunter.name/posts/2013-04-27-the-javascript-event-loop-presentation](https://thomashunter.name/posts/2013-04-27-the-javascript-event-loop-presentation)
    - Concurrency vs Parallelism
        - Concurrency: when you can do two things, but one at a time
        - Parallelism: when you can do two things, both at the same time
    
    ![Untitled](Advanced%20NodeJS%20afaa880b2f5b469bbe92ac17753f96ff/Untitled%201.png)
    
    ### JS Woking Threads
    
    - Good to avoid synchronous heavy jobs
    - Unlike the I/O operations, CPU-based operations are executed by the Event Loop itself and not delegated to the thread pool for asynchronous execution. Hence, for time consuming CPU operations, the Event Loop remains occupied, and the program is blocked from further execution.
    - Internal to the Javascript implementation, there are likely threads that are making these asynchronous events work, but those threads do not need to be exposed to the programmer directly for the asynchronous functionality to be there.
    - [https://deepsource.io/blog/nodejs-worker-threads/](https://deepsource.io/blog/nodejs-worker-threads/)
    
    More about threads:
    
    - [https://stackoverflow.com/questions/25082867/how-are-promises-implemented-in-javascript-without-threads](https://stackoverflow.com/questions/25082867/how-are-promises-implemented-in-javascript-without-threads)
    - [https://stackoverflow.com/questions/7575589/how-does-javascript-handle-ajax-responses-in-the-background/7575649#7575649](https://stackoverflow.com/questions/7575589/how-does-javascript-handle-ajax-responses-in-the-background/7575649#7575649)
    
    [Redis](Advanced%20NodeJS%20afaa880b2f5b469bbe92ac17753f96ff/Redis%20fafd246cc84b46be999c69e84d246d6d.md)
    
    [Testing](Advanced%20NodeJS%20afaa880b2f5b469bbe92ac17753f96ff/Testing%205d86c2a8b4b94924a508a7619215ae2d.md)