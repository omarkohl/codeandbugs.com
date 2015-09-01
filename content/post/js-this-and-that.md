+++
date = "2015-09-01T11:21:05+02:00"
draft = true
title = "JavaScript: this and that"
categories = [ "JavaScript" ]

+++

Have you ever wondered about the JavaScript *this* and *that* variables?

## *this*

The explanation is quite simple. Every JavaScript (JS) function implicitly has
a variable called *this*. The value of this variable depends on the way the
functions is called.

For instance if the function is called as a method then *this* is a reference
to the object on whom you are calling the method.

{{< highlight javascript >}}
var myfunc = function(newValue) {
    this.value = newValue;
};
var someObj = {value: 0};
someObj.mymethod = myfunc;
someObj.mymethod(123);
console.log(someObj.value); // value === 123
{{< /highlight >}}

If you call that function simply as a function, then *this* is a reference to
the global context (window). For example:

{{< highlight javascript >}}
var myfunc = function(newValue) {
    this.value = newValue;
};
// value === undefined
// window.value === undefined
myfunc(123);
console.log(value); // global variable value === 123
console.log(window.value); // global variable value === 123
{{< /highlight >}}

## *that*

Now the real problem comes when you try to define a function inside a method
(e.g. to declare a callback that is triggered on some event).

{{< highlight javascript >}}
var buttonHandler = {
    element: document.getElementById("bigButton"),
    counter: 0,
    startCounting: function() {
        console.log("Start counting...");
        this.element.onclick = function() {
            console.log("Clicked on button");
            this.counter += 1;
        }
    }
};

buttonHandler.startCounting();
// Click on the button 5 times
console.log(buttonHandler.counter); // counter === 0, why is that?
{{< /highlight >}}

The function that is triggered by *onclick* events tries to increase the
counter of the *buttonHandler* but *this* is no longer a reference to the
*buttonHandler* object but to the global context (window). (The global variable
*counter* does have value 5. Is that what you intended?)

The solution is a simple **convention**. Create a variable *that* that points
to the outer *this* and can be used inside the callback functions.

{{< highlight javascript >}}
var buttonHandler = {
    element: document.getElementById("bigButton"),
    counter: 0,
    startCounting: function() {
        console.log("Start counting...");
        that = this;
        this.element.onclick = function() {
            console.log("Clicked on button");
            that.counter += 1;
        }
    }
};

buttonHandler.startCounting();
// Click on the button 5 times
console.log(buttonHandler.counter); // counter === 5
{{< /highlight >}}

## Further reads

* Douglas Crockfords excellent book *Javascript: The Good Parts*
* A very insightful [blogpost by Yehuda
  Katz](http://yehudakatz.com/2011/08/11/understanding-javascript-function-invocation-and-this/)
