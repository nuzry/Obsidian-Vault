#### Multiplication Table Generator

A simple web app that generates a multiplication table based on user input.

##### How it works
- User enters **rows** and **columns** → clicks submit → table renders on the page

###### `createTable(rows, columns)`
- Loops through rows and columns to build an HTML `<table>` string
- Each cell value = `row index × column index`
- Returns the complete table HTML string

##### `generateNumbers()`
- Reads row and column values from inputs
- Converts them to numbers with `Number()`
- Calls `createTable()` and injects result into `#table-container` via `innerHTML`

```js
function createTable(rows, columns) {
    let html = '<table border=1 style="border-collapse: collapse;">';

    for (let i = 1; i < rows + 1; i++) {
        html += '<tr>';
        for (let j = 1; j < columns + 1; j++) {
            html += `<td>${i * j}</td>`; // multiply row × col
        }
        html += '</tr>';
    }

    html += '</table>';
    return html;
}

function generateNumbers() {
    const rows = Number(document.getElementById("row-input").value);
    const columns = Number(document.getElementById("columns-input").value);

    const tableHTML = createTable(rows, columns);
    document.getElementById("table-container").innerHTML = tableHTML;
}
```

##### HTML Structure
```html
<label>rows: <input type="text" id="row-input"></label>
<label>columns: <input type="text" id="columns-input"></label>
<input type="button" value="submit" onclick="generateNumbers()">
<div id="table-container"></div> <!-- table renders here -->
```