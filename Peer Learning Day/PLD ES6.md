1. # ES6 Basics
    - What ES6 is?
ES6 is ecma2015 which is a standard for implementing javascript in the browser
    - What new features were introduced in ES6?
shortcuts, arrow function, shortname for classes, export/import
    - What is the difference between a constant and a variable?
a constant is immutable var isn't
    - What are block-scoped variables and how do they differ from function-scoped variables?
block scoped variable only exist in a block and not in the entire function so in an if only and a for only then it diseapear outside of the block
    - Why does using var inside a block sometimes lead to unexpected overwriting?
because var is not block scoped it's function scoped
    - What benefits do let and const bring compared to var?
they are block scoped and they allow more control over what the variable can or cannot allow
    - What are arrow functions and how do they differ from traditional function expressions?
arrow function are simple function that can be created anywhere they differ by the lack of boilerplate and more versatility
    - Why do arrow functions not have their own this context?
because arrow functions are not meant to be used multiple times they are single use
    - How do default parameters simplify function definitions?
default parameters can allow fall back mechanics in case that we want a default behavior instead of crashing
    - What are rest parameters and how do they allow handling an unknown number of arguments?
rest parameters are ... they allow the handling of an unknown amount of arguments they produce arguments as an array and can take any type of arguments
	- What is the spread operator and how does it simplify array or object manipulation?
spread operator is similar in syntax than rest it is used for combining arrays or datas it can be used in object for combining datas
	- What is string templating in ES6 and how does it improve readability compared to concatenation?
string templating allows to use variables inside a string it can make things more readable by not having to do multiple concatenation operation
    - How do template literals allow embedding expressions in strings?
by using ${} you can embed any variable that has a string representation
    - What improvements does ES6 introduce for object creation and property definition?
property shorthands, method definition shorthands, computed property names, Object.assign, prototype

    - What is the object property value shorthand syntax and why is it useful?
instead of writting {name: name, age: age} you just write {name, age}, it's useful when the property name and the variable name are the same it makes the code more readable

	- What are computed property names and when would you use them?
computed property name allows to use a variable as a property name it can be used for configuration or renaming
    - What are ES6 method properties and how do they simplify object method definitions?
ES- method properties let's you write method without the function() key word which make the code easier to read

    - What is the difference between for...of and for...in loops?
of reference the value inside an array or object in reference the property key

    - What are iterables in JavaScript and how do iterators work?
 iterables are object you can loop over with for...of loop anything with a Symbol.iterator method
 they are similar to java iterable they use iterable.next until no items are left
 
	- How does ES6 make it easier to build structured data objects, such as department reports or grouped employee lists?
easier extraction, easier iteration, easier definition with shorthand properties and methods, rest and spread for data concatenation or deconstruction
# ES6 classes
    
	- How to define a Class in ES6?
    class myclass{
    }
    - How to add methods to a class?
	 class myclass {
		 method(){
		 }
	 }
    - Why and how to add a static method to a class?
     class myclass {
		 static cringe() {
			 console.log(cringe);
		 }
     }
    - How to extend a class from another?
    class A extend B {
    }
    - What is metaprogramming and how do Symbols participate in it?
    Metaprogramming = code that can inspect, modify, or influence other code.
	
## **Symbols define _well-known behaviors_ of objects**
	JavaScript has special built-in Symbol keys like:
		- `Symbol.iterator` → controls how `for...of` works
		- `Symbol.toStringTag` → customizes `Object.prototype.toString`
		- `Symbol.hasInstance` → customizes `instanceof`
		- `Symbol.toPrimitive` → customizes conversions (`+`, numbers, strings)


    - What is the purpose of using constructors inside ES6 classes?
    constructors are used to intantiate an object of a class

	- Why is it recommended to store attributes in underscore-prefixed properties (e.g., _name)?
	it's a naming convention to represent private variables and encourage the use of getters and setters

    - What is the role of getters and setters in class design?
    they are meant to be used to access encapsulated datas/private attributes
    - Why should setters validate data types before assigning values?
    setters should validate data types because javascript is an interpreted language and can accept any types of variables
    - How do class instances differ from plain JavaScript objects?
    class instances have need to be instanciated using the new keyword they inehrit from the class while bare javascript object don't
    - What is the difference between calling a method on an instance and calling a static method?
    calling a method on an instance is refering to that specific method while static methods are more generalized they are for libraries like behavior
    - How does inheritance allow code reuse between related classes?
    by using super() in the constructor you can then use the inherited method and properties
    - What is the purpose of the super() call inside a subclass constructor?
    the purpose is to instanciate the parent because javascript doesn't have an MRO (method resolution order)
    - How can symbols be used as unique keys for object or class properties?
    symbols can be used to define some specific properties like iteration or species or string properties
    - What advantages do symbols offer compared to standard property names?
		- **Symbol unique → no name collision**  
		- **Symbol hidden → no accidental “UGA smash property”**
		- **Symbol powerful → lets you bend JS language rules**
		- **Symbol useful → perfect for internal/private-ish data**
    - How does JavaScript handle hoisting for classes compared to functions?
     functions get hoisted immediately you can use them before they are defined in the code but function are hoisted but cannot be used before initialization
	- Why are class declarations not hoisted in the same way as function declarations?
	 class declaration are meant to represent structure they need to be instantiated before use or it'll turn into chaos since classes have methods that needs to exist before creating an object
    - What happens if you attempt to access a class before it is defined?
    you can a ReferenceError
    - How can computed property names be used inside classes, especially for dynamic method names?
     they can be used for config like systems they can also be used for modifying instances behavior and naming they can also use symbols for pseudo private attributes
     
    - How does ES6 improve the structure, readability, and maintainability of object-oriented code through classes?
    
     they improved it by using shorthand writting styles using computed property names for flexibility and readability