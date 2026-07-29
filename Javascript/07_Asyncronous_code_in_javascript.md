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
fs.readFile("data.txt",'utf-8')
    .then(content => {
	    console.log(content);
	    return content
    })
    .catch(err =>{
	    console.log(err)
    })
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

## JS Async/Await

- Syntactic sugar for promises → same behavior, cleaner syntax
- `await` → pauses execution inside the function until promise resolves
- `async` → required on any function that uses `await`
- Use `try/catch/finally` instead of `.then()/.catch()/.finally()`
- ⚠️ Code after `await` still runs asynchronously → not truly synchronous

```js
function fetchData() {
    return new Promise((resolve, reject) => {
        setTimeout(() => resolve("Data loaded!"), 1000);
    });
}

// Promises way
fetchData()
    .then(data => console.log(data))
    .catch(err => console.log(err))
    .finally(() => console.log("Done!"));

// Async/await way (same behavior, cleaner)
async function execute() {
    try {
        const data = await fetchData(); // waits for promise to resolve
        console.log(data);             // "Data loaded!"
    } catch (err) {
        console.log(err);              // runs if rejected
    } finally {
        console.log("Done!");          // always runs
    }
}

execute(); // kick off async function (no await needed at top level)
```

| Promises     | Async/Await     |
| ------------ | --------------- |
| `.then()`    | `await`         |
| `.catch()`   | `catch` block   |
| `.finally()` | `finally` block |
