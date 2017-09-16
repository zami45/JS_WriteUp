# Introduction

One of the most confusing and misused keyword in javascript is `this` keyword.For a javascript beginner, `this` feels like shrouded in mystery.But if properly used, `this` can be proved as a valuable asset in the toolbox of a javascript developer.Here I would like to shed some light on what `this` actually is  and techniques to manipulate `this` in a javascript application.The concept might appear too difficult to wrap you head around,but if you give effort and practice the concept by writing small code you will get the hang of it eventually.You could just follow along and try out the code provided in the discussion in your browser.

# Let's Talk About Scope

Any javascript programmer using `this` without any prior knowledge of the outcome of the code may caught up in a situation which is hard to debug, confusing and very difficult to fix. To understand `this` one must
have clear understanding of another beautiful concept of javascript which is called `scope`. Before getting into how `this` works and how we can manipulate it, I would like to discuss a bit about the concept of
scope.

We all know global variables, right? These are the variables which can be accessed from anywhere in your javascript code.A variable's
visibility depends on its scope. Global variables can be accessed from global scope and function scope (Local Scope) equally. Chances are that you might not be familier with the concept of scope. So,I would like to discuss here about the concept of scope in a nutshell.
 
> ### **Scope determines the visibility level...........**

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
-          global_var  can be accessed within `this` function                    -
-         */                                                                   -
-                                                                              -
-     }                                                                        -
-                                                                              -
--------------------------------------------------------------------------------
```
Above illustration explains that global variable can be accessed from both in global scope (obviously, as they are created in global scope) and Local scope(Within any function). 

 keep in mind that `this` refers to global object (window object) by default.Any global variable becomes a property of global object.We can access any global variable through `this` keyword.`this` is analogas to window object. If we check strict equality between `this` and window , it will return true 
 
 ```javascript
 `this` === window // true
```
Let's justify what i've just said with an example.For `this`, we need to create a global variable first. Let's create one 

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
console.log(`this`.global_var) // access global_var thorugh `this`. So, `this` === window
```
both works equally.


# What `this` is all about ?
 
What you have just learned about scope will help you to understand `this` with more clarity. As I've already mentioned,`this` refers to the EXECUTION CONTEXT. Execution context is a topic for another discussion. What you need to know is,execution context indicates which object is bound to the current scope.

> ### **Execution context refers to the object which is bound to the current scope**

If it feels confusing, don't worry. Following example will clarify your confusion. If you pay close attention, you will apprehend the general idea about execution context and why it is responsible for determining what `this` will be pointing to during javascript execution.

> ### **`this` refers to execution context**

If you go through the illustration I've given while discussing scope, you will see that inside javascript world there are two different kinds of scope ,one is global and another is local. When javascript begin executing the code, it first enters into the global context. Global context is window. So in global context(or I can say global scope), `this` refers to window.
Concept of scope and context are related to each other.Let's explain the relation between scope and context First :

**-   At the beginning of execution,javascript engine enters into global scope.So,execution context is global.So `this` will point to global object.**

**-  When we invoke any function ,javascript engine enters into that function's local scope.But Execution context will still be global, so `this` will still point to global object inside that function's local scope.**

**-  If we bind the function with an object other than global object, context will refer to that bound object inside the function's private scope instead of global object.So `this` will now become that particular object.**

**-  After function execution is over,javascript engine jumps out of the function and land into global scope again.So `this` will refer to global/window object again.**

Let's explain the five points stated above with an example. 
	   
```javascript

console.log(`this`) // here `this` refers to global/window

var person = {

	 username : "zami",
	 
	 sayName  : function(){
	 
			console.log(`this`) // here `this` will be person
					
			console.log(`this`.username) //prints the value of username property
					
			function inner(){
			
			    console.log(`this`) // here `this` will be window or global/window object
				
			}
			
			inner() // invoke inner function
					
		}
			  
}

person.sayName() //  invoke sayName 

/*
 Now execution context will jumps back into global context right after 
 finishing function execution.
 As execution is out of a function's private scope, it will again 
 point to global object
 
*/
console.log(`this`) 



```
What's going on here? Let's explain step by step.Here I have a `person` object.It has a property called `username` and a method called `sayName`. Inside `sayName` there
is another function called `inner`. 

At the beginning of execution, javascript engine enters into the global context. That's why `this` refers to global object inside global scope.

```javascript
console.log(`this`) // so, it will pint window object in the console
```

Then, it found an object called person and maps it into memory.Then it finds an invocation of sayName method of the person object.

```javascript
person.sayName() // it invokes sayName method and set `this` inside the function to person object
```
That implies why the first `console.log(`this`)` inside sayName method prints `person` object in the console.Because as we invoked the sayName method under the context of 
person object, `this` will not set to `person` object. That makes sense why putting a `console.log(`this`.username)` inside sayName method will print the value of username property
of person object.
	   
Now execution comes to the invocation of `inner` function. The point is, when javascript engine maps internal variables and function of an object to memeory, it doesn't bind
the context object to the functions that are declared inside a particular object method. That means person object is not boudn with the inner function. As a result inner function 
fall subject to default binding which is global object.Thats why `console.log(`this`)` inside `inner` function prints window in the console. So, `this` refers to the global/window 
object inside `inner` function.


# How `this` gets bound to a particular object 
	   
I suppose you already get the idea that `this` can refer to a default value which is global object or it can be bound to a particular javascript object.There are two ways to bind `this` 
to a particular object in javascript.   
	   
### Implicit Binding :

Implicit binding of `this` happens when we invoke a method of an object or create a new instance of a class.

```javascript
var person = {

    username : "zami",
	sayName  : function(){
	          console.log(`this`)
			  }
}

person.sayName() // invoke sayName method of person object
```

Here we have an object literal called person.When we invoke the sayName method of person object, `this` automatically gets bound to that object on which the method is defined. In our
case it gets bound to person object.

Implicit binding also takes place when we create new instance of a class with new keyword.Let's see `this` in action with another example

```javascript
// first declare a constructor function which will act as a boiler plate for our newly created instance

function Person(){
    `this`.username = "zami";
    `this`.sayName  = function (){
	            console.log(`this`);
	    }
			 
// create an instance of person constructor

var p = new Person()

/*
it will create an object under the hood call p.
p object will have a property called username and a method called sayName
*/

p.sayName() 

/*
will print the p object in the console
`this` will be set to p object here
*/

// let's create another instance of person constructor

p2 = new Person() 

p2.sayName() // It will print p2 object in the console, `this` will be set to p2 object

```

### Explicit Binding

Explicit binding is required when we want to manipulate the `this` value. Sometimes we need `this` to be set to an object of our own choosing.Fortunately javascript has some built in 
ways to manipulate `this` value. Let's explore them briefly

#### Call

`call` is method which helps to set `this` value inside a function. Let's see an example

```javascript
var person = {
       username : "zami"
}

function Shout(greetings){
   console.log(greetings+" "+`this`.username)
}

Shout() // prints undefined

Shout.call(person,"welcome!") // prints "welcome! zami"

```

Now let's take a closer look. Here an object literal called `person` is declared. It has only one property called `username`.We also defined a function called `Shout` which logs 
``this`.username`. As you already know `this` inside a function by default points to window/global object. That's why invoking `Shout` in global context prints undefined in the console.
Because as you might have guessed, `this` inside `Shout` is global.Thus it's trying to find a global variable called `username` but fails. Thus prints undefined in the console. But if
we use call method during invocation of Shout function and pass person object as a parameter, it binds the `this` inside Shout to person object instead of global object. That's why it
successfully prints the value of `person.username` as ``this`.username` int the console.

#### Apply

`apply` is a built in javascript method which does the same thing as `call`. Only difference being that in case of `call` we need to send the function parameters by separating them by
comma and for `apply` we need to pass all the required parameters in an array.Let's see an example

```javascript
var person = {
         username : "zami"
}

function Shout(greetings,message){
    console.log(greetings+" "+`this`.username+","+message)
}

Shout.apply(perosn,["hello","you are now a member of our football team."]) 

/*
first parameters is the context object which is person
Here Shout requires two parameters
That's why second parameter is an array containg two stirng value required for the parameters of Shout function
*/

```

#### Bind

`bind` is also a method to bind `this` to a particular context rather than the default one. `bind` is completely different for both `call` and `apply`. Unlike `call` and `apply`, `bind` doesn't
invoke the function, rather it returns a completely new function with `this` value set to a context of our own choosing.Following example will shed some light on the concept

```javascript
var person = {
       username : "zami"
}

function Shout(){
   console.log(`this`.username)
}

var shout_two = Shout.bind(person)

shout_two() // prints "zami"

```

Here we would like to bind `this` inside `Shout` to the person object. For `this` we have used `bind` method.

```javascript
var shout_two = Shout.bind(person)
```

what the avove piece of code is doing is, it binds `this` inside Shout function to person and returns a new function which gets assigned to `shout_two` variable. Now `shout_two` is a new function which 
is exactly similar to `Shout` function , only difference being that `this` inside shout_two function is set to person object. That's the reason why after invocking 
shout_two we get the value of `username` property of `person` object.


# Some Common Misuse of `this` 


In order to write better and modular javascript code, understanding the concept of `this` is a prerequisite. Incomplete or improper 
understanding may lead to many unresolved and misunderstood bugs in the code. Some common errors which may appear in your code due to incomplete understanding of `this` keyword will be discussed in `this` topic

#### 1. Creating Unwanted Global Variables :


How we can create global variable unknowingly due to lack of proper understanding of `this` keyword? We are all familiar with constructor function in javascript. They are used to create new object.General layout of  a constructor function is 

```javascript
function Person(){

   `this`.username = "zami"
   
}
```

just keep in mind that if no other execution context is provided, `this` inside a function refers to global object.It is a default behaviour of `this` keyword.

Try to think what might happen if we invoke Person function. A global variable called username can be created on the fly when javascript interpreter hit ``this`.username = "zami"` expression inside Person function.Besically what's happening here is javascript interpreter is looking for a   property called `username` in ``this`/global` object. If such property not found in global object ,it creates one and assign value to it. On the other hand if such a property already exists, it just assign value to the existing one. 

If we invoke the Person function , a global variable called username will be created and `"zami"` string will be assigned to that username variable.In the mean time a property called username will be added to global object as i've stated earlier.

```javascript
function Person(){

    `this`.username = "zami";
	
}

Person()

//a property called username is added to global object
//now username variable can be accessed from global scope 

console.log(username) // prints "zami" 

// as a property called username is added to global object, we can access it thourgh `this` or window

console.log(`this`.username) // also prints "zami"

console.log(window.username) // also prints "zami"
```

Thus using `this`, we can create global variable on the fly if we don't have proper understanding of `this` keyword.

#### **2. Some Consider `this` Points to Function Scope :**


It is tempting to use `this` as a mean of accessing local variable inside a function. Consider the following piece of code 

```javascript
function Person(){

    var username = "zami"
	
    console.log(`this`.username)

}

Person() // prints undefined 
```

here username is a local variable declared inside a function called Person. Variable username is not available in global scope as it belongs to the local scope of Person function. Someone may find it provocative to use `this` keyword to access the username variable inside the function scope. Here, after invocation you will see `undefined` in the console.

```javascript
console.log(`this`.username) //will yield undefined
```

As you might guess it correctly , ``this`.username` will look for a global variable called `username`.As there is no global variable called `username` declared , it will yield undefined.By no means ``this`.username` will refer to the local username variable because `this` refers to global object.

#### **3. Sudden Change of Context :**

Whoever is executing the task is called context. In other words, what `this` refers to is called context. If we invoke a method of an object litereal, that particular object becomes the context. Context plays an important role in determining what `this` will be pointing to. Let's clarify what i have just said with an example

```javascript
var person = {

    username : "zami",
	
    sayName : function(){
        console.log(`this`.username) // prints "zami"
    }
	
}

person.sayName() // invoke sayName method 
```
`person` is an object literal. It has a property called `username` and a method called `sayName`. After invoking `sayName` method it will execute `console.log(`this`.name)` statement. Here ``this`.username` will print the `username` property of the `person` object. It is possible because execution context of `sayName` is `person` object. So, inside `sayName` , `this` will be bound to person object.

```javascript
  `this`.username === person.username
  
  // we can also replace `this`.username with person.username inside sayName method
```

Ok, assume  there is a `setTimeout` function inside `sayName` method. And i want to print out the `username` after 5 second of invocation.
Let's rewrite the sayName method of person object.

```javascript
var person = {

    username = "zami",
	
	sayName : function(){
	
	    setTimeout(function(){
		            
			console.log(`this`.username)
				
		},5000)
				
	}

}
	
//invoke the sayName method
	
person.sayName() // will print undefined
	
```

Inside setTimeout `this` doesn't point to `person` object. Because setTimeout itself is a function and context gets change for each function. Javascript
engine binds `person` as the context of sayName method. But when sayName gets executed , it encounters another function which is setTimeout and when setTimeout
gets executed, context gets changed. I will left it here, perhaps I will discuss it another time as it deserves a separate article and way out of the range
of `this` article.

# Conclusion

If you are reading `this` paragraph, i can hope that you have completed the whole topic. That's pretty much conclude the discussion of `this`. To be honest, `this` is a confusing topic. It still confuse me 
from time to time. The code I've provided so far is simple and precise. Of course you will find more complex scenario in real life coding. But what I've written in `this` article is the basic concept and
it will apply to any scenario in real life practice. I believe if you go through the example code and watch the results on the console and tweak them just a bit to see how the behaviour changes, you will
be able to understand any piece of code you will ever come across. 
