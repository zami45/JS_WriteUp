Introduction
------------

        One of the most confusing and misused keyword in javascript is THIS keyword.For a javascript beginner THIS 
		feels like shrouded in mystry.But if properly used THIS can be proved as a valuable asset in the toolbox of 
		a javascript developer.Here i would like to shed some lignt on different misused cases of THIS keyword and 
		techniques to manipulate THIS in a javascript application.As the word this has its own meaning ,in this
		discussion i will refer to the keyword THIS in capital letter.

Some Common Misuse of THIS :
----------------------------

To use THIS in a javascript application, understanding the concept of THIS is a prerequisit.Incomplete or no 
proper understanding of this can lead to many unresolved and misunderstood bugs in the code.Some common errors 
which may appear in your code due to incomplete understanding of THISkeyword will be discussed in this topic

###### Creating Unwanted Global Variables :

We all know global variables, right? These are the variables which can be accessed from anywhere of your javascript
code.Global variables can be accessed from global scope and function scope (Local Scope) equally.In case you don't 
know javascript deals with two different kinds of scope,They are

1. Global Scope
2. Function Scope (aka Local Scope)

the following illustration will provide a better understanding of relationship between global variable and  scope in 
javascript.
```javascript
--------------------------------------------------------------------------------
-                                                                              -
-   Global Scope                                                               -
-   (Declare Global Variable Here in Global Scope)                             - 
-   var global_var = "global variable" //here global_var is a global variable  -
-                                                                              -
-   function sample_function(callback){                                        -
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
Above illustration explains that global variable can be accessed from both in global scope (obviously, as they are 
created in global scope) and Local scope.Now the actual point is how we can create global variable global variable 
unknowingly due to lack of proper understanding of THIS keyword.We are all familiar with constructor function in 
javascript.They are used to create new object.General layout of  a constructor function is 

```javascript
function Person(){

   this.name = "zami"
   
}
```

just keep in mind that this inside a function refers to global object.It is a default behaviour of THIS keyword.This 
behaviour can be changed with the help of NEW keyword.I shall explain it later.But for now keep in mind that THIS 
refers to global object (window object) by default.Any global variable becomes a property of global object.We can 
access any global variable through THIS keyword.

```javascript
  var global_var  = "a global variable" 
```

if we want to print the global_var we can access it directly by name 

```javascript
console.log(global_var) 
```

On the other hand we can access it through global object or window object

```javascript
console.log(window.global_var) 
& 
console.log(this.global_var) 
```

both works equally.

Now back to our Person constructor function. A global variable called name can be created on the fly using this.name 
expression .If we invoke the Person function , a global variable called name will be created and "zami" string will be
assigned to that name variable

```javascript
function Person(){

    this.name = "zami";
	
}

Person()

//now name variable can be accessed from global scope 

console.log(name) // prints "zami" 
```

Thus using THIS we can create global variable on the fly if we don't have proper understanding of THIS keyword

###### Some Consider THIS Points to Function Scope :

To a beginner ,it is tempting to use THIS as a mean of accessing local variable inside a function.If we refer
to the following piece of code 

```javascript
function Person(){

    var name = "zami"
	
    console.log(this.name)

} 
```

here name is a local variable declared inside a function called Person. Variable name is not available in global scope 
as it belongs to the local scope of Person function A new comer in the javascript world, may find it provocative to 
use THIS keyword to access the variable inside the function scope. Here,` console.log(this.name) ` will yield undefined.
As you might guess it correctly , this.name will look for a global variable called name.As there is no global variable 
called name declared , it will yield undefined.By no means this.name will refer to the local name variable.
