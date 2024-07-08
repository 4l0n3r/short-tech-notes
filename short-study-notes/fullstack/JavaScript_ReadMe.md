**How JavaScript Works and Executed ?**

	-	Everything in JavaScript will executed inside the Execution Context ( Memory = Variable Environment , Code = Thread of Execution)
	-	JavaScript is a Synchronous and Single Threaded Language
		    Synchronous 	: Next line will be executed after the current line has done. (In order)
			Single-Threaded : Only executed one command at a time.

**Closures :**

	Combination of a function bundled together with it's Lexical Environment (Reference of i, not value).
	If we return the function itself and execute that function in other scope it still remembers the Lexical Environment.
	setTime
	Uses:
		Module Pattern
		Function Currying
		Data hiding and encapsulation
	Relation with Garbage collection
		Garbage :
			c , c++ , java => we need to allocate , deallocate the Memory
			javascript     => will be handled by browser itself.
			Smart Garbage Collector.  => Removing unreachable variables from Lexical Environment.										


**Callback functions and Event Listeners :**

		Passing a function as a parameter to other function.
		JavaScript Synchronous  , But we can achive Asynchronous by using callback + setTimeout functions
		Call Stack => the current execution function and its Environment.
		Event Listeners will keep the all data (Lexical Environment) thats why the application will slow whenever we have so many Event Listeners.
		If we delete Event Listener , the Memory will be handled to garbage Collector.


**Comments :** 

	//
	/* */ 

**Declaration :**

	let 
		- like local variable
	var
		- like global variable
	const
		- we can't change it
		- we can push values to list but we cant assign new value to it.

Case sensitivity 

**Classes and Inheritance :**

	constructor()
	static function()
	super()
	unlike function declaration , class declaration is not a part of Javascript Hoisting. So we need to declare the class before invoking it.


**Template Literals:**

	document.getElementById("id").innerHTML = template data
	${var}
	${function('parameter')}
	` data `

**Strings & Numbers:**

	string.startsWith(anotherString)
	Number.IsFinite(3) , -IsInteger , -IsNaN

**Spread operator**

	- 3 dots 

**Destructuring**
	
	define what values to unpack from the source variable.
	name:sample  -> will be used to restrict the naming clashes.
**forEach :**

	- function_name
		array.forEach(function_name)
	- anonymous function
		array.forEach(function(var){ implementation })
	- for ( i of Array )
	- for ( i in Object )

Data Structures :

	Set :
		-.add
		-.delete
		-.clear
		-.size
		WeakSet()

	Map :
		new Map([[key,value],[key,value]])
		-.set
		-.delete
		-.size
		WeakMap()

Arrow Notation :

	We can't use arrow function inside the class method.
	If we are using this keyword inside the fuction then we can't use Arrow function.
	'this' will refer to the object calling that function.Since we are using Arrow function 'this' value will be '{}'
	Remove function key word and place the => symbol
	If we have only one line in array function then remove braaces and return statement.

**Generator :**

		function *g1() {}
		next()
		next().value

**Higher Order Functions :**

	map
		- transformation of current array
		- create updated values from old values
	
	filter
		- filter some values 
		- true or false
	
	reduce 
		Note: we have an array we need to make one output out of that array.
		- summation , aggregation 

	sort
		- sort based on function
		- positive or negative
**Module**

	module will create a private space for the code instead of global access.
	module.exports = myFunction;

**Others :**
	
	default parameter
	console.table(object)


**Resources :**

	https://www.youtube.com/watch?v=KWUcTt15fQs&list=PLillGF-RfqbZ7s3t6ZInY3NjEOOX7hsBv&index=3	
	https://www.youtube.com/watch?v=PkZNo7MFNFg