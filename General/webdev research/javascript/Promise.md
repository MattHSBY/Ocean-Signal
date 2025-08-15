---
sticker: emoji//1f91e
---
A promise is a feature in javascript which helps write asynchronous code. In javascript, when designing methods with [[Callback|callbacks]], you can often find yourself writing code falling into the category of '[[Callback Hell]]'. Promises are a good way to avoid this and maintain readable code. For an example of this see [[Callback Hell]].
```JS
let promise = new Promise((resolve, reject) => {
	if (1+1==2) {
		resolve("Success");
	} else {
		reject("Fail");
	}
})
promise.then((msg) => {
	console.log(msg);
})
```
This is an example of a javascript Promise. In this case, it will always resolve because 1+1 is always equal to 2, but you could resolve/reject for whatever reason you choose.