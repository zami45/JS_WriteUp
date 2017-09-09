Introduction
------------

One of the most confusing and misused keyword in javascript is THIS keyword.For a javascript beginner THIS feels like shrouded in mystery.But if properly used THIS can be proved as a valuable asset in the toolbox of a javascript developer.Here I would like to shed some light on different misused cases of THIS keyword and techniques to manipulate THIS in a javascript application.As the word this bears its own meaning ,in this discussion i will refer to the keyword THIS written in capital letter.The concept might appear too difficult to wrap you head around,but if you give effort and practice the concept by writting small code you will get the hang of 
it eventually.You could just follow along and try out the code provided in the discussion in your browser.

Some Common Misuse of THIS 
----------------------------

In order to write better and modular javascript code, understanding the concept of THIS is a prerequisite. Incomplete or improper 
understanding may lead to many unresolved and misunderstood bugs in the code. Some common errors which may appear in your code due to incomplete understanding of THIS keyword will be discussed in this topic

#### 1. Creating Unwanted Global Variables :


We all know global variables, right? These are the variables which can be accessed from anywhere in your javascript code.A variable's
visibility depends on its scope. Global variables can be accessed from global scope and function scope (Local Scope) equally. Chances are that you might not be familier with the concept of scope. So,I would like to discuss here about the concept of scope in a nutshell.
 
Simply put, Scope controls the visibility of a variable. Variables declared in global scope , has global visibility.They are accessible from anywhere in the script.On the other hand local variable resides in local scope (alos known as function scope). Visibility of local variable is limited to local scope. Generally Outside environment can not access a local variable. Of course there are some ways to interact with local variables from outside environment, But that discussion is for later.For now let's just stick to the fact that local variables are confined to local scope. They are only available in local scope. So we can say, javascript deals with two different kinds of scopes,They are :

- **Global Scope**
- **Function Scope (Local Scope)**

Here is a visual illustration for the secret world of javascript. Following illustration will provide a better understanding on relationship between variables and scope in javascript.

```javascript
//javascript world
--------------------------------------------------------------------------------
-                                                                              -
-   //Global Scope                                                             -
-   //Declare Global Variable Here in Global Scope                             - 
-   var global_var = "global variable" //here global_var is a global variable  -
-                                                                              -
-   function sample_js_function(){                                             -
-                                                                              -
-      var local_var = "local variable" // it's a local variable               -
-                                                                              -
-         /*                                                                   -
-          function scope resides inside a function                            -
-          inside a function we can declare local variable                     - 
-          Local variables can't be accessed from outside of the function      -
-          scope but we can access any global variable from here               -
-          global_var  can be accessed within this function                    -
-         */                                                                   -
-                                                                              -
-     }                                                                        -
-                                                                              -
--------------------------------------------------------------------------------
```
Above illustration explains that global variable can be accessed from both in global scope (obviously, as they are created in global scope) and Local scope(Within any function). 

 keep in mind that THIS refers to global object (window object) by default.Any global variable becomes a property of global object.We can access any global variable through THIS keyword.THIS is analogas to window object. If we check strict equality between THIS and window , it will return true 
 
 ```javascript
 THIS === window // true
```
Let's justify what i've just said with an example.For this, we need to create a global variable first. Let's create one 

```javascript
var global_var  = "a global variable" 
```
If we want to see what value our global_var variable holds, we can access it directly by the variable name 

```javascript
console.log(global_var) // prints "a global variable"
```
On the other hand, as we already know global variables are added to global/window object as property,we can also access the `global_var` variable through global object or window object

```javascript
console.log(window.global_var) // access global_var through window object, prints "a global variable"
& 
console.log(this.global_var) // access global_var thorugh THIS. So, THIS === window
```
both works equally.

Now the actual point is how we can create global variable unknowingly due to lack of proper understanding of THIS keyword.We are all familiar with constructor function in javascript. They are used to create new object.General layout of  a constructor function is 

```javascript
function Person(){

   this.username = "zami"
   
}
```

just keep in mind that if no other execution context is provided, THIS inside a function refers to global object.It is a default behaviour of THIS keyword.

Try to think what might happen if we invoke Person function. A global variable called username can be created on the fly when javascript interpreter hit `this.username = "zami"` expression inside Person function.Besically what's happening here is javascript interpreter is looking for a   property called `username` in `THIS/global` object. If such property not found in global object ,it creates one and assign value to it. On the other hand if such a property already exists, it just assign value to the existing one. 

If we invoke the Person function , a global variable called username will be created and `"zami"` string will be assigned to that username variable.In the mean time a property called username will be added to global object as i've stated earlier.

```javascript
function Person(){

    this.username = "zami";
	
}

Person()

//a property called username is added to global object
//now username variable can be accessed from global scope 

console.log(username) // prints "zami" 

// as a property called username is added to global object, we can access it thourgh THIS or window

console.log(this.username) // also prints "zami"

console.log(window.username) // also prints "zami"
```

Thus using THIS, we can create global variable on the fly if we don't have proper understanding of THIS keyword.

#### **2. Some Consider THIS Points to Function Scope :**


It is tempting to use THIS as a mean of accessing local variable inside a function. Consider the following piece of code 

```javascript
function Person(){

    var username = "zami"
	
    console.log(this.username)

}

Person() // prints undefined 
```

here username is a local variable declared inside a function called Person. Variable username is not available in global scope as it belongs to the local scope of Person function. Someone may find it provocative to use THIS keyword to access the username variable inside the function scope. Here, after invocation you will see `undefined` in the console.

```javascript
console.log(this.username) //will yield undefined
```

As you might guess it correctly , `this.username` will look for a global variable called `username`.As there is no global variable called `username` declared , it will yield undefined.By no means `this.username` will refer to the local username variable because this refers to global object.

#### **3. Sudden Change of Context :

Whoever is executing the task is called context. In other words, what THIS refers to is called context. If we invoke a method of an object litereal, that particular object becomes the context. Context plays an important role in determining what THIS will be pointing to. Let's clarify what i have just said with an example

```javascript
var person = {

    username : "zami",
	
    sayName : function(){
        console.log(this.username) // prints "zami"
    }
	
}

person.sayName() // invoke sayName method 
```
`person` is an object literal. It has a property called `username` and a method called `sayName`. After invoking `sayName` method it will execute `console.log(this.name)` statement. Here `this.username` will print the `username` property of the `person` object. It is possible because execution context of `sayName` is `person` object. So, inside `sayName` , THIS will be bound to person object.

```javascript
  this.username === person.username
  
  // we can also replace this.username with person.username inside sayName method
```

Ok, assume  there is a `setTimeout` function inside `sayName` method. And i want to print out the `username` after 5 second of invocation.
Let's rewrite the sayName method of person object.

```javascript
var person = {
    username = "zami",
	
	sayName : function(){
	
	    setTimeout(function(){
		             console.log(this.username)
				},5000)
				
	}
	
	//invoke the sayName method
	
	person.sayName() // will print undefined
	
```

Inside setTimeout THIS doesn't point to `person` object. Because setTimeout itself is a function and context gets change for each function. Javascript
engine binds `person` as the context of sayName method. But when sayName gets executed , it encounters another function which is setTimeout and when setTimeout
gets executed, context gets changed. I will left it here, perhaps I will discuss it another time as it deserves a separate article and way out of the range
of this article.

# What THIS is all about ?
 
As i've already mentioned, THIS refers to the `EXECUTION CONTEXT. Execution context is a topic for another discussion. What you need to know is,
execution context indicates where the control is at the moment during javascript execution.If it feels confusing, don't worry. Following example 
will clarify your confusion. If you pay close attention, you will apprehend the general idea about execution context and why it is responsible 
for determining what THIS will be pointing to during javascript execution.

> ## **This refers to execution context**

If you go through the illustration i've given while discussing scope, you will see that inside javascript world there are two different kinds of scope
,one is global and another is local. When javascript begin executing the code, it first enters into the global context. Global context is window. So in 
global context(or I can say global scope), THIS refers to window. Let's explain the concept of context First.

**i)   At the beginning of execution , execution context is global.**

**ii)  When we invoke any function , execution enters that function's private context.**

**iii) If that function's context is not bound with any object, the context is window/global.THIS refers to global object.**

**iv)  If the context is bound with an object, THIS refers to that object**

Let's explain the four points stated above with an example. 
	   
```javascript

console.log(this) // here THIS refers to global/window

var person = {

	 username : "zami",
	 
	 sayName  : function(){
	 
			console.log(this) // here THIS will be person
					
			console.log(this.username) //prints the value of username property
					
			function inner(){
			
			    console.log(this) // here THIS will be window or global/window object
				
			}
			
			inner() // invoke inner function
					
		}
			  
}

person.sayName() //  invoke sayName 
```
What's going on here? Let's explain step by step.Here I have a `person` object.It has a property called `username` and a method called `sayName`. Inside `sayName` there
is another function called `inner`. 

At the beginning of execution, javascript engine enters into the global context. That's why THIS refers to global object inside global scope.

```javascript
console.log(this) // so, it will pint window object in the console
```

Then, it found an object called person and maps it into memory.Then it finds an invocation of sayName method of the person object.

```javascript
person.sayName() // it invokes sayName method and set THIS inside the function to person object
```
That implies why the first `console.log(this)` inside sayName method prints `person` object in the console.Because as we invoked the sayName method under the context of 
person object. THIS now indicates `person` object. That makes sense why putting a `console.log(this.username)` inside sayName method will print the valu of username property
of person object.
	   
Now execution comes to the invocation of `inner` function. The point is, when javascript engine maps internal variables and function of an object to memeory, it doesn't bind
the context object to the functions that are declared inside a particular object method. That means person object is not boudn with the inner function. As a result inner function 
fall subject to default binding which is global object.Thats why `console.log(this)` inside `inner` function prints window in the console. So, THIS refers to the global/window 
object inside `inner` function.

	   
	   
	   
	   
	   
	   
