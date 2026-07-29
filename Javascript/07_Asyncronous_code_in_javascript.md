## JS Callbacks & Async

- JS is **single-threaded** → only one operation at a time
- **Callbacks** → functions passed to async operations, called when done
- The rest of the program **keeps running** while async op completes
- ⚠️ Async results are **only available inside** the callback

```js
// ✅ Correct - use result inside callback
setTimeout(() => {
    console.log("runs after 2s"); // result available here
}, 2000);

console.log("runs immediately"); // this runs first!

// ⚠️ Common mistake - trying to use result outside callback
let result;
setTimeout(() => {
    result = "done"; // assigned after 2s
}, 2000);
console.log(result); // undefined → callback hasn't run yet!

// Real-world pattern (file read, network request)
readFile("data.txt", (error, content) => {
    if (error) {
        console.log("Error:", error);
        return;
    }
    console.log(content); // ✅ use result here, inside callback
});
```

## JS Promises

- Cleaner alternative to nested callbacks ("callback hell")
- A promise has 3 states: **pending** → **fulfilled** or **rejected**
- Chain `.then()` → `.catch()` → `.finally()` for readable async flow

```js
const myPromise = new Promise((resolve, reject) => {
    setTimeout(() => {
        const success = true;
        if (success) resolve("Data loaded!");
        else reject("Something went wrong!");
    }, 1000);
});

myPromise
    .then(message => console.log(message))  // "Data loaded!" if resolved
    .catch(error => console.log(error))     // "Something went wrong!" if rejected
    .finally(() => console.log("Done!"));   // always runs
```

### Chaining Promises
```js
fetchUser()
    .then(user => fetchPosts(user.id))   // waits for fetchUser
    .then(posts => fetchComments(posts)) // waits for fetchPosts
    .catch(error => console.log(error)); // catches any error in chain
```

| Method       | Triggered when                |
| ------------ | ----------------------------- |
| `.then()`    | promise **resolved** ✅        |
| `.catch()`   | promise **rejected** ❌        |
| `.finally()` | always, regardless of outcome |
