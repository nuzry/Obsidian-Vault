## Multiplication Table Generator

A simple web app that generates a multiplication table based on user input.

### How it works
- User enters **rows** and **columns** → clicks submit → table renders on the page

### `createTable(rows, columns)`
- Loops through rows and columns to build an HTML `<table>` string
- Each cell value = `row index × column index`
- Returns the complete table HTML string

### `generateNumbers()`
- Reads row and column values from inputs
- Converts them to numbers with `Number()`
- Calls `createTable()` and injects result into `#table-container` via `innerHTML`

```js
// Core logic
for (let i = 1; i <= rows; i++) {
    for (let j = 1; j <= columns; j++) {
        html += `<td>${i * j}</td>`; // multiply row × col
    }
}
```