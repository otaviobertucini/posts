```js
    async function controller() {
    // make heavy cpu load
    // make fs request
    // make async request
    }

    controller().then((res) => {
    console.log(res);
    });
```