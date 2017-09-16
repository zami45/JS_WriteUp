# Undefined & Scope

you might notice something strange in the title. It's doesn't make sense. `undefined` is undefined. What does it have 
to with scope. In case you don't know scope, check out this link.We all know what `undefined` is. It is a primitive 
value which means not defined. Say, if a variable is declared and it is not initialized , the variable will print 
`undefined` in the console.Let's declare one and see for yourself in the console.

```javascript
var fruit

console.log(fruit) // prints undefined 
```

`undefined` is expected to generate a falsy value when used as a test case in if-else block. If a variable needs to be checked
whether or not it is defined in a if-else block ,`undefined` is our friend in need.Let's check out this one

```javascript
var fruit

if(fruit == undefined){
    console.log("fruit is not defined")
}else{
    console.log("fruit is defined")
}
```

`undefined` is a property of global object.As it is a property of global object, question may arise whether or not we can create
a variable called `undefined` and assign value to it.Let's try

```javascript
var undefined = true

if(undefined){
    console.log("undefined is set to true")
}else{
    console.log("undefined is still undefined")
}
```

It will print the log statement inside `else` case.So setting `undefined` to `true` will not work in global scope. The `undefined`
property of global object will prevail in global scope. But scenario might change with the change of scope and pre-assumed norms can 
change with it.

In javascript every function ships with it's own private scope. It is called local scope.When a function invocation occurs, javascript 
execution enters into a local scope. Now what `undefined` have to do with it. We've already seen that we can not change the default 
behaviour of `undefined`. As MDN page say


> ### **undefined is a property of the global object; i.e., it is a variable in global scope....**


Can you identify the subtle difference? It doesn't say anything about local scope. That means we can create a variable called `undefined`
in the local scope,doesn't it? Let's give it a try

```javascript
function myFunc(){
   
	var undefined = true
	
    if(undefined){
        console.log("undefined is set to true")
    }else{
        console.log("undefined is still undefined")
    }

}

// now invoke it 

myFunc() // prints "undefined is set to true
```

A variable called `undefined` is created inside `myFunc` function's local scope and it's value is set to `true`. Will it pass the test this time?
Surprisingly it does pass the test. The `if` statement proves to be `truthy` and code inside the `if` statement gets executed.


> ### **`undefined` is just another variable in local scope....**
 
 
That's interesting but as well as problematic. How we can check for undefined value in `if-else` statement now. A solution might be not using 
`undefined` for checking test cases at all.Then what's is the alternative? you might ask.the answer is `void` function. `void` takes an expression
and returns a `undefined` value.


> ### ** `void` always returns undefined,it doesn't care about the scope.... **


```javascript
var three = 3

function aFunction (){

   return 3
 
}
console.log(void(0))  // undefined

console.log(aFunction()) // undefined
```

`void` will always return `undefined`. So an `if-else` statement can be written using `void` statement to check for undefined value

```javascript

function myFunc(){

    var x
	
	if(x === void(0)){
	    console.log("x is undefined")
	}else{
	    console.log("no,it's not")
	}

}

myFunc() // prints "x is undefined"
```

It is safer to use `void` instead of using `undefined` directly when multiple people working on a same file.Though it is a rare mistake. 
In practical no one with proper knowledge of javascript will ever declare a variable called `undefined` in their script. But it's good to 
to know that there is a workaround for using `undefined`.