## JavaScript, back to basics - Hoisting

Who has never been in front of a JavaScript code w/ a WTF effect in certain situations? In this series of articles, we will return to basics by discovering the essential concepts of JavaScript. Today, we will talk about **hoisting**.

# "Hoisting", what?!

Before I start, I would like to define what is hoisting. 
As I don't like to reinvent the wheel, I will give you the definition given by the [MDN](https://developer.mozilla.org/en-US/docs/Glossary/Hoisting): 

> Conceptually, for example, a strict definition of hoisting suggests that variable and function declarations are physically moved to the top of your code, but this is not in fact what happens. Instead, the variable and function declarations are put into memory during the compile phase, but stay exactly where you typed them in your code.

In short, remember that **the code you write in JavaScript is not necessarily run up and down as you expected**. Enough talk, let's dev!

# Deep into hoisting in JS

## Basic example w/ `var`

Let's begin with a simple example:

```javascript
console.log(hello);
```

Not surprisingly, we receive this error message "hello is not defined". Now, let's declare a variable named `hello`:

```javascript
console.log(hello);
var hello = 'Hi !';
```

Now, we don't have any error message but a log containing "undefined"... What's happen?! The JavaScript interpreter proceeded to the hoisting, so the code executed previously is equivalent to this one:

```javascript
var hello;
console.log(hello); // undefined
hello = 'Hi !';
```

We see that the declaration has been raised to the top of `hello`'s [variable scope](https://dev.to/lauthieb/vanillajs-back-to-basics-1-variable-scope-2n5f) while the initialization of this variable is located at the same place.
That's why we have to be careful when we use variable in JavaScript. 

BTW, we have the same problem when we declare a `var` in a for loop ;)

## `let` & `const` are still there ❤️

Fortunately, when we use `let` or `const`, the declaration will not be hoisted. So, we will not have this WTF effect!

```javascript
console.log(hello);
let hello = 'Hi !';
```

In this example, we can see the error message that we expected. So, please use `let` & `const` from [ES6](https://www.ecma-international.org/ecma-262/6.0/) instead of `var`!

## Let's use functions!

We will see that this behavior is identical with functions. However, I will give you some tips to avoid side effects ;)

Let's start with a simple example:

```javascript
hello();
var hello = function() {
    console.log('Hey!');
}
hello();
```

Without surprise, this example confirm what we said earlier. The declaration is hoisted but not the implementation.

**What if we directly define the function without storing it in a variable?**

```javascript
hello();
function hello() {
    console.log('Hey!');
}
hello();
```

WOW! We can see "Hey" twice in the console...
It means that the complete block of the `hello` function is hoisted at the top of the current scope.
Since it's both a declaration and an implementation, we have our log that appears twice *(and it's ugly...)*.

The interpretation of our previous code is the following:

```javascript
function hello() {
    console.log('Hey!'); // 'Hey!' x2
}
hello();
hello();
```

Let's continue with another example:

```javascript
var hello = function() {
    function a() {
        return 3;
    }
    return a();
}
console.log(hello());
```

No surprise, this code is logging "3". Now, make this example more complex:

```javascript
var hello = function() {
    function a() {
        return 3;
    }
    return a();
    function a() {
        return 4;
    }
}
console.log(hello());
```

This example is logging "4". Indeed, as we said before, functions defined in this way are directly hoisted and so we have this kind of weird behavior.

**How to avoid this?!** 
I advise you to always declare your functions in a variable, so you will avoid the hoisting of the implementation.

```javascript
var hello = function() {
    var a = function() {
        return 3;
    }
    return a();
    var a = function() {
        return 4;
    }
}
console.log(hello());
```

Here, this code is logging well "3" and that's what we wanted!

**So, please pay attention about hoisting when you use variables and functions in JavaScript.**

# Voilà!

I hope this article has allowed you to better understand the hoisting in JavaScript. In the next episode of this series, I will talk about closures.

Feel free to share this article to your friends/colleagues/family/pets!
And tell me in comment if you have any recommendations or remarks.

See you soon!