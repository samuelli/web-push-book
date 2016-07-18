# Prerequisites

Before we look at the specifics of how to do the 3 steps discussed, it's
worth covering some key aspects of the API that might be new to some
developers.

If you are comfortable with promises and / or service workers then you should
skip section, but if either of those topics are new to you, it's worth
covering some of the fundamentals because in the rest of the book it'll be
glossed over.

## ES2015

If you are new to ES2015, here is the mega short run through of features
/ syntax I'll be using in this book.

### Fat Arrow Functions

Fat arrow functions look weird, but aren't too bad once you get the hand of
them.

A normal function is:

    function() {
      // Do codez here
    }

The equivalent fat arrow function is:

    () => {
      // Do codez here
    }

If you have multiple arguments passed in, you go from:

    function(arg1, arg2) {
      // Do codez here
    }

To fat arrow this:

    (arg1, arg2) => {
      // Do codez here
    }

The last final thing to master, if there is only one argument, you don't need
the brackets:

    arg1 => {
      // Do codes here
    }

### var vs const and let

In JavaScript `var` is used to define new variables. In ES2015, you can
use `const` and `let` instead.

`let` is the same as var. It's a variable that can be re-assigned as many
times as you like.

`const` is a variable that can only be assigned once. The object it self can
be mutated and altered, it just can be re-assigned.

The subtleties of this were covered in an excellent article by Mathias Bynens
that I'd strongly recommend you check out for more
info^[https://mathiasbynens.be/notes/es6-const].

## Intro to Promises

Promises are a way of handling asynchronous code in a different manner to
callbacks.

You might be used to code that looks like this:

    someAPI(function((err, result) {
      if (err) {
        // Do something with the error
        return;
      }

      // Do something with the result
    });

This was easy to use and worked well. The only downside is that error cases
weren't standardardised and it can lead to heavy nesting of function calls.

Promises is essentially a standardised why of approaching asynchronous method
that can simplify APIS. Te equivalent promise of the code above is as
follows:

    someAPI()
    .then(result => {
      // Do something with the result
    })
    .catch(err => {
      // Do something with the error
    })

The difference is minor, the result and error are seperated via the `then` and
`catch` method, both passing in a callback function.

The major benefit of promises is that your can *chain* promises together by
returning a promise inside of a promises callback.

Let's say we were making waiting for a browser event and afterwards we wanted
to make a network request, if we were using callbacks, we would do the
following:

    waitForBrowserEvent(function(err) {
      if (err) {
        // Handle error
        return;
      }

      makeNetworkRequest(function(err, result) {
        if (err) {
          // Handle error
          return;
        }

        // Do something with network result
      });
    });

With promises we can chain them by returning a promise inside of a  promise
callback:

    waitForBrowserEvent()
    .then(() => {
      return makeNetworkRequest();
    })
    .then(result => {
      // Do something with network result
    })
    .catch(err => {
      // Handle error
    })

This should be enough on promises to get you through the reast of this book,
but it's not clear, I strongly recommend Jake Archibalds introduction to
Promises, he covers a tonne of examples and
usecases^[https://developers.google.com/web/fundamentals/primers/promises/].

On a personal note, I found it really helpful to flatten my promise chains.
By this, what I mean is that even though you can do this:

    waitForBrowserEvent()
    .then(() => {
        return makeNetworkRequest()
        .then(result => {
          // Do something with the network request
        });  
    })
    .catch(err => {
      // Handle error
    });

Generally I've found it cleaner and easier to follow a Promise chaing, if you
pass the result onto the next step rather than nest Promise chains.

## Intro to Service Worker

Final thing I wanted to cover was service workers and this will really be a
high level run through as I'll be mentioning service workers **a lot**
throughout this book.

Every developer is (or eventually becomes) happy with the notion that
JavaScript runs inside of a web page.

![A browser with a web page loaded and JavaScript running inside of that page](build/images/browser-with-javascript.png)