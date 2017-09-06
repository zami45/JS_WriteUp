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
