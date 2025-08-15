---
sticker: emoji//1f919
---
A callback in javascript is just a convention, it's not a feature. A callback is a function that is stored as a reference and designed to be called by another function. 
For example:
```JS
function hello(callback) {
	console.log("hello");
	callback();
}
function world() {
	console.log("world!");
}

hello(world);
// output:
// hello
// world!
```
In this case, 'world()' is a callback and it is passed into the 'hello(callback)' function as a parameter, it is then called at the end of the 'hello()' method which causes the order of the output to be 'hello', then 'world!'.