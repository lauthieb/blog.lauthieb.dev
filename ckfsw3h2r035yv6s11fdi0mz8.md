## JavaScript, back to basics - Variable Scope

Who has never been in front of a JavaScript code w/ a WTF effect in certain situations? In this series of articles, we will return to basics by discovering the essential concepts of JavaScript. Today, we will talk about **variable scope**.

# The `var` keyword

First of all, let's talk about an old friend we all have already met: `var`.
This key word can sometimes reserve you some surprises !

To avoid this, let's look at the variable scope in JavaScript right now.

## Function-scoped

If there is one thing you need to remember, it's that `var` is function-scoped.

Imagine you have a function stored in a variable `hello`. Inside this function, you declare and initialize a variable `msg` and log it.
After in your code, you want to log `msg` outside of this function. Let's run it:

```javascript
var hello = function() {
    var msg = 'Welcome';
    console.log(msg);
};
hello();
console.log(msg);
``` 

Wow... We can't :(
It's normal, variables declared with `var` are function-scoped. It means that we can't access to these variables outside the function in which they were declared.

Let's continue, if we add a `msg` variable outside the `hello` function. What will happen in the second `console.log`?

```javascript
var msg = 'Hey';
var hello = function() {
    var msg = 'Welcome';
    console.log(msg);
};
hello();
console.log(msg);
``` 

It works ! So we see that inside the `hello` function, "Welcome" is logged while outside we have "Hey". This is because the second `console.log` is running in the global scope.

**Variables defined in a function are accessible only in the function. That's why we can often see bits of JavaScript code described in this way: `(function() { ... })()`. This is an obsolete approach to encapsulation which is no longer necessary with ES6 modules.**

Go further. We will add another variable `who` that we will use in our `hello` function. I let you launch this new example:

```javascript 
var msg = 'Hey';
var who = 'DEV Community';
var hello = function() {
    var msg = 'Welcome';
    console.log(msg + ' ' + who);
};
hello();
``` 

Hmmm... It works, we see "Welcome DEV Community". And it is very dangerous! Indeed, we saw that the variables were not accessible outside the functions but a function has access to its external context!
In this example, no risk, but let's take a more dangerous example:

```javascript 
var msg = 'Hey';
var hello = function() {
    msg = 'Welcome';
    console.log(msg);
};
hello();
console.log(msg);
``` 

OH ! You can see that "Welcome" is logged twice!
So we see that a function is able to mutate its external environment and there is assured side effects...

**As a reminder, to avoid this in your legacy code: include your code in self-executing functions**

## For loop's side effect

Be careful, this scope is only valid when it comes to functions. In a loop, the variable is defined outside. It's just a natural result of function-scoped variables being declared with `var`.


A simple example to show this side effect with for loops:

```javascript
for (var i = 0; i < 3; i++) {
    console.log(i);
}
console.log(i);
``` 

You can see that outside of this loop, we can access to `i` and it's pretty dangerous ! It's because browsers interpret this code this way:

```javascript
var i;
for (i = 0; i < 3; i++) {
    console.log(i); // 0, 1, 2
}
console.log(i); // 3
```
hm, you'll tell me that all these quirks do not make a nice ad for JavaScript... Luckily, [ES6](https://www.ecma-international.org/ecma-262/6.0/) has arrived in 2015 with some new keywords: `let` and `const`.

# ES6: `let` & `const` keywords

You will see since the arrival of these keywords, our life of JS developer has become much easier!
Indeed, these keywords are no longer function-scoped but block-scoped, and that changes everything!

## Block-scoped

Let's take our previous example with the for loop by replacing `var` with `let`:

```javascript
for (let i = 0; i < 3; i++) {
    console.log(i);
}
console.log(i);
```

You can see and error outside the block of the for loop, and it's what we were looking for to avoid side effects!

Let's continue and use `const` instead of `let`

```javascript
for (const i = 0; i < 3; i++) {
    console.log(i);
}
console.log(i);
```

You can see an error and it is normal because we want `i` to be a constant.

## Be careful with `const`

We have to be careful with `const` because it's a constant by reference and not by value!

Let's see this through an example:

```javascript
const me = {
    name: 'Laurent'
};

me = 'laurent';
console.log(me);
```

Here we have an error. It's normal because we have changed the reference attributed to this constant. BUT if we do this...

```javascript
const me = {
    name: 'Laurent'
};

me.name = 'Alain';
console.log(me);
```

Our name was changed! 

**So, please pay attention to the `const` keyword.**

# Voilà!

I hope this article has allowed you to better understand the variable scope in JavaScript. In the [next episode](https://blog.lauthieb.dev/javascript-back-to-basics-hoisting) of this series, I will talk about hoisting.

Feel free to share this article to your friends/colleagues/family/pets!
And tell me in comment if you have any recommendations or remarks.

See you soon!
