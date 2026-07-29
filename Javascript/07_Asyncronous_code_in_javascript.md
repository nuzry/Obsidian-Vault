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