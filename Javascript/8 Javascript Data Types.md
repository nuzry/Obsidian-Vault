#### Ways to define a variable in JS
- var - function-scoped variables are defined
- let - block-scoped variables are defined
- const - when assigned a value can not change anymore.
**mostly let and const are used while defining variables in Js**

#### The 8's in JS

1. Numbers
  - integer and floating point numbers are both *number* type.
  - negative numbers are also allowed - `let x = -10`
  - hex decimal numbers - `let hex = 0x2A`
  - binary numbers - `let binary = 0b101010`
  - octal numbers - `let octal = 0o52`
  - large numbers(scientific notations) - `let sixtyMillion = 6e7`
  - *NaN* is not a number where when one number and any other type is used as operands `let answer = 10 * "oops" --> NaN`
  - *Infinity* is a type when you divide small number with big number `let infinity = 9/10`
  - *NaN* and *Infinity* are also considered as numbers
  - All numbers are stored as 64 bit floating point values
  - Big numbers in JS are accurate up to 15 digits.
  - When working with decimals answers can be off sometimes `let x = 0.1 + 0.7  >> 0.799999999`
 
2. Strings
  - Strings can be defined
```
     let singleQuoteString = 'Hello!'
     let doubleQuoteString = "Hello!"
     let bactickString = `Hello!`
```
  - Backticks can use for some other tasks as well called #template-literals where it allows you to use variable value in the string.
```
	let backtickString = `Hello ${ name}`
```
  - Get information about the string in JS
```
	let myString = `Hello`
	myString.length  //5
	myString.charAt(0) //H
	myString.toUpperCase() //HELLO
	myString.toLowerCase() //hello
```
  - Usage of #escape-character.
```
	let question = ""How do I use quotes?" she asked" --> wrong
	let question = "\"How do I use quotes?\"she asked" --> correct
	let question = '"How do I use quotes?"she asked' --> correct
	
	//other escape characters use cases
	let nextLine = "first line\nsecond line"
	/*
		first line
		second line
	*/
```
  - #String-concatenation 
```
	let fname = "nusry"
	let lname = "hamza"
	let fullName = fname+ " "+lname  //nusry hamza
```
1. Booleans
2. Objects
3. Functions
4. Undefined
5. Bigints
6. Symbols
