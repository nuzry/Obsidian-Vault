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

3. Booleans
 - True or False
 - falsie values in JS are **empty-strings, 0, NaN, 0n, null, undefined, false**

4. Objects
  - Objects in JS are different than other languages.
  - Objects is key value pair of grouped related data
```
	let person = {
		name: "nusry",
		age: 21,
		eyeColor: "brown"
	}
	
	//accessing properties in side an object
	1. . method
	   console.log(person.name)
	2. [] method
	   console.log(person["name"])
	   
	//changing values of the object
	person.name = "Nusry Hamza"
	person["name"] = "Nusry Hamza"
```
  - variable **does not own an object**. It stores a **reference (pointer) to the object in memory**
```
Before modification:

user1  ─────┐                     user1  ─────┐ 
            │                                 │
            ▼                                 ▼
        ┌───────────┐                     ┌───────────┐
        │  Object   │      -->>>          │  Object   │
        │ name:John │   name changed      │ name:Alex │
        │ age:25    │   same refernce     │ age:25    │
        └───────────┘                     └───────────┘
            ▲                                 ▲
            │                                 │
user2  ─────┘                     user2  ─────┘  
```
  - If a function changes an object's body inside the function that change affects for the outside as well.
```
	function myFunc(obj){
		obj.name = "changed!"
	}
	
	myFunction(myObject)
	
	myObject.name --> changed!
```
  - *null* type is an *object* in javascript
```
	let nullVal = null
	console.log(nullVal)  //object
```
- *Side Topic - Arrays in Javascript*
  - Array is an object in javascript
  - Array can have multiple data types in the same array.
  - Array has multiple built-in methods
```
	let myArray = [1, 2, 3, 4, 5, 6, 7]
	myArray.length --> gives the length of the array (element count)
	myArray.push(8)  --> adds as an elemenet at the end of the array
	myArray.pop() --> removes the last element from the array
	myArray.indexOf(2) -->shows the index number where the number 2 exist in array
	myArray.map(x => x+1) --> [2, 3, 4, 5, 6, 7, 8]
	myArray.filter(x => x < 4) [5, 6, 7]
	myArray.sort() --> [1, 2, 3, 4, 5, 6, 7]
```

5. Functions
  - function in JS means it takes ar and after some *process* the function *returns a value* 
5. Undefined
6. Bigints
7. Symbols
