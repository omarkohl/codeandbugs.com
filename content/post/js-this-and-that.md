+++
date = "2015-09-01T11:21:05+02:00"
draft = true
title = "JavaScript this and that"
categories = [ "JavaScript" ]

+++

Have you ever wondered about the JavaScript *this* and *that* variables?

The explanation is quite simple. Every JavaScript (JS) function implicitly has
a variable called *this*. The value of this variable depends on the way the
functions is called.

For instance if the function is called as a method then *this* is a reference
to the object on whom you are calling the method.

```javascript
def myfunc = function() {
    this.value = 123;
};
var someObj = {value: 0;}
someObj.mymethod = myfunc;
someObj.mymethod();
console.log(someObj.value); # value === 123
```

If you call that function simply as a function, then *this* is a reference to
the global context. For example:

```javascript
def myfunc = function() {
    this.value = 123;
};
myfunc()
console.log(value); # global variable value === 123
```
