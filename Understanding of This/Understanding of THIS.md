Introduction
------------

One of the most confusing and misused keyword in javascript is THIS keyword.For a javascript beginner THIS 
feels like shrouded in mystery.But if properly used THIS can be proved as a valuable asset in the toolbox of 
a javascript developer.Here I would like to shed some light on different misused cases of THIS keyword and 
techniques to manipulate THIS in a javascript application.As the word this bears its own meaning ,in this
discussion i will refer to the keyword THIS written in capital letter.The concept might appear too difficult to
wrap you head around,but if you give effort and practice the concept by writting small code you will get the hang of 
it eventually.You could just follow along and try out the code provided in the discussion in your browser.

Some Common Misuse of THIS 
----------------------------

In order to write better and modular javascript code, understanding the concept of THIS is a prerequisite. Incomplete or improper 
understanding of this may lead to many unresolved and misunderstood bugs in the code. Some common errors which may 
appear in your code due to incomplete understanding of THIS keyword will be discussed in this topic

#### 1. Creating Unwanted Global Variables :


We all know global variables, right? These are the variables which can be accessed from anywhere in your javascript
code. Global variables can be accessed from global scope and function scope (Local Scope) equally. Chances are that
you might never heard of the concept of scope. So, I would like to discuss about the concept of scope in a nutshell.
 
Simply put, Scope controls the visibility of a variable. Variables declared in global scope , has global visibility.They are 
accessible from anywhere in the script.On the other hand local variable resides in local scope (alos known as function
scope). Visibility of local variable is limited to local scope. Outside environment can not access a local variable. It is only 
available to local scope. So we can say, javascript deals with two different kinds of scopes,They are :

1. Global Scope
2. Function Scope (Local Scope)

the following illustration will provide a better understanding of relationship between global variable and  scope in 
javascript.

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
Above illustration explains that global variable can be accessed from both in global scope (obviously, as they are 
created in global scope) and Local scope(Within any function). 

Now the actual point is how we can create global variable global variable unknowingly due to lack of proper understanding 
of THIS keyword.We are all familiar with constructor function in javascript. They are used to create new object.General 
layout of  a constructor function is 

```javascript
function Person(){

   this.name = "zami"
   
}
```

just keep in mind that THIS inside a function refers to global object.It is a default behaviour of THIS keyword.This 
behaviour can be changed with the help of NEW keyword.I shall explain it later.But for now keep in mind that THIS 
refers to global object (window object) by default.Any global variable becomes a property of global object.We can 
access any global variable through THIS keyword.

```javascript
var global_var  = "a global variable" 
```

if we want to print the global_var we can access it directly by the variable name 

```javascript
console.log(global_var) 
```

On the other hand, as we already know global variables are added to global/window object as property, we can also access the `global_var` variable through global object or window object

```javascript
console.log(window.global_var) // access global_var through window object
& 
console.log(this.global_var) // access global_var thorugh THIS. So, THIS === window
```

both works equally.

Now back to our Person constructor function. A global variable called name can be created on the fly using `this.name = "zami"` 
expression inside Person function.If we invoke the Person function , a global variable called name will be created and
"zami" string will be assigned to that name variable.In the mean time a property called name will be added to global
object as i've stated earlier.

```javascript
function Person(){

    this.name = "zami";
	
}

Person()

//a property called name is added to global object
//now name variable can be accessed from global scope 

console.log(name) // prints "zami" 

console.log(this.name) // also prints "zami"

console.log(window.name) // also prints "zami"
```

Thus using THIS we can create global variable on the fly if we don't have proper understanding of THIS keyword.We 
need to bind THIS to an object or context of our own choosing in order to prevent this from happening.

#### **2. Some Consider THIS Points to Function Scope :**


It is tempting to use THIS as a mean of accessing local variable inside a function. Consider the following piece of code 

```javascript
function Person(){

    var name = "zami"
	
    console.log(this.name)

} 

Person() // prints undefined 
```

here name is a local variable declared inside a function called Person. Variable name is not available in global scope 
as it belongs to the local scope of Person function. Someone may find it provocative to use THIS keyword to access the 
name variable inside the function scope. Here, after invocation you will see `undefined` in the console.

```javascript
console.log(this.name) //will yield undefined
```

As you might guess it correctly , `this.name` will look for a global variable called `name`.As there is no global variable 
called `name` declared , it will yield undefined.By no means `this.name` will refer to the local name variable because this
refers to global object.
