# What is promise?
JavaScript is single threaded, meaning that two bits of script cannot run at the same time; they have to run one after another. So `promise` is to solve asynchronous problem in JS multithread programming.

# Basic Specification
- A promise can only succeed or fail *once*. It cannot succeed or fail twice, neither can it switch from success to failure or vice versa.
- If a promise has succeeded or failed and you later add a success/failure callback, the correct callback will be called, even though the event took place earlier.

This is useful for async success/failure, because you're less interested in the exact time something became available, and more interested in reacting to the outcome.

## Promise terminology
A promise is an object which can be returned synchronously from an asynchronous function. It will be in one of 3 possible states:
- `Fulfilled`: onFulfilled() will be called (e.g., resolve() was called)
- `Rejected`: onRejected() will be called (e.g., reject() was called)
- `Pending`: not yet fulfilled or rejected

A promise is settled if it’s not pending. Once settled, a promise can not be resettled. Calling resolve() or reject() again will have no effect. 
The **immutability** of a settled promise is an important feature.
Native JavaScript promises don’t expose promise states. Instead, you’re expected to treat the promise as a black box. Only the function responsible for creating the promise will have knowledge of the promise status, or access to resolve or reject.

## Important Promise Rules
A standard for promises was defined by the [Promises/A+ specification](https://promisesaplus.com/implementations) community. `Promises` following the spec must follow a specific set of rules:
- A promise or “thenable” is an object that supplies a standard-compliant .then() method.
- A pending promise may transition into a fulfilled or rejected state.
- A fulfilled or rejected promise is settled, and must not transition into any other state.
- Once a promise is settled, it must have a value (which may be undefined). That value must not change.
 -Change in this context refers to identity (===) comparison. An object may be used as the fulfilled value, and object properties may mutate.

The `.then()` method chained with a promise must comply with these rules:
- Both onFulfilled() and onRejected() are optional.
- If the arguments supplied are not functions, they must be **ignored**.
- onFulfilled() will be called after the promise is fulfilled, with the promise’s value as the first argument.
- onRejected() will be called after the promise is rejected, with the reason for rejection as the first argument. Using Error objects is recommended for showing the error trace tree.
- Neither onFulfilled() nor onRejected() may be called more than once.
- .then() may be called many times on the same promise. In other words, a promise can be used to aggregate callbacks.
- .then() must return a new promise, promise2.
- If onFulfilled() or onRejected() return a value x, and x is a promise, promise2 will lock in with (assume the same state and value as) x. Otherwise, promise2 will be fulfilled with the value of x.
- If either onFulfilled or onRejected throws an exception e, promise2 must be rejected with e as the reason.
- If onFulfilled is not a function and promise1 is fulfilled, promise2 must be fulfilled with the same value as promise1.
- If onRejected is not a function and promise1 is rejected, promise2 must be rejected with the same reason as promise1.

# Promise chaining
Because .then() always returns a new promise, it’s possible to chain promises with precise control over how and where errors are handled. Promises allow you to mimic normal synchronous code’s try/catch behavior.

## Error handlering
Note that promises have both a success and an error handler, and it’s very common to see code that does this:
```
aPromise().then(
  handleSuccess,
  handleError
);
```
But what happens if handleSuccess() throws an error? The promise returned from .then() will be rejected, but there’s nothing there to catch the rejection — meaning that an error in your app gets **swallowed**.

For that reason, some people consider the code above to be an anti-pattern, and recommend the following, instead:
```
save()
  .then(handleSuccess)
  .catch(handleError)
;
```
The difference is subtle, but important. In the first example, an error originating in the save() operation will be caught, but an error originating in the handleSuccess() function will be swallowed.

<div align=center>
<img src=https://github.com/KOVERcjm/Technical_Notes/raw/master/Pictures/Promise%20using%20guide%20-%20with%20catch.png>
<br />
Without .catch(), an error in the success handler is uncaught.
<img src=https://github.com/KOVERcjm/Technical_Notes/raw/master/Pictures/Promise%20using%20guide%20-%20without%20catch.png>
<br />
With .catch(), both error sources are handled.
</div>

So it's recommend ending all promise chains with a `.catch()`.


# *Reference*
> * [JavaScript Promises: an Introduction](https://developers.google.com/web/fundamentals/primers/promises#promise-terminology)
> * [Master the JavaScript Interview: What is a Promise?](https://medium.com/javascript-scene/master-the-javascript-interview-what-is-a-promise-27fc71e77261)
> * [We have a problem with promises](https://pouchdb.com/2015/05/18/we-have-a-problem-with-promises.html)
> * [We have a problem with promises (Chinese version)](https://mp.weixin.qq.com/s?__biz=MzAxODE2MjM1MA==&mid=2651551800&idx=1&sn=d06d319c002fdca153bc2abe9352e959&chksm=8025aff9b75226efe21a5094ce14a29c467be74ef2eb631157ea2732106642357617935e9464&mpshare=1&scene=1&srcid=0224AMZJRxN6nMI53jiG8DeX&key=494a5736f574d32d344982760d1421bb12d64307aedc362f175f134d7798eae7d9573dfc22c0731501cb5f318fd59eff2edadae87b920e8c8960759de29358dc6df930836b0cfeb2bb79880f72bba96b&ascene=0&uin=MjUwNzcwOTcwMA%3D%3D&devicetype=iMac+MacBookPro9%2C2+OSX+OSX+10.11.6+build(15G31)&version=12010210&nettype=WIFI&fontScale=100&pass_ticket=JQVnZiE4ehmQ22J9EiHcr7TGgYeDIQiMZl%2FEpRgnGN%2B3VGKkGyphZLcPq8VFePjQ)
> * [Diagram Source](http://stackoverflow.com/questions/24662289/when-is-thensuccess-fail-considered-an-antipattern-for-promises)