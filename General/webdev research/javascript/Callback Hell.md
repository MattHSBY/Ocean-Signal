---
sticker: emoji//1f479
---
Callback Hell describes a written program involving [[Callback|callbacks]] where there are too many [[Callback|callbacks]] in a chain, causing code to become messy and hard to maintain. 
For example:
```JS
async function loadAsset(url, callback){ 
	console.log(`Loading asset from ${url}...`);
	setTimeout(() => {
		if (Math.random() > 0.1) {
			callback(null, `Asset loaded successfully from ${url}`);
		} else {
			callback(`Failed to load asset from ${url}`);
		}
	}, 1000);
}
```
In the loadAsset function we just use a random number to simulate the potential failure of the method, with a fairly low chance of failure.

```JS
console.log(`starting process...`);
loadAsset('url:1', (err, result) => {
	if (err) {
		console.error(`An error ocurred: ${err}`);
		return;
	} else {
		console.log(result);
	}
	loadAsset('url:2', (err, result) => {
		if (err) {
			console.error(`An error ocurred: ${err}`);
			return;
		} else {
			console.log(result);
		}
		loadAsset('url:3', (err, result) => {
			if (err) {
				console.error(`An error ocurrec: ${err}`);
				return;
			} else {
				console.log(result);
			}
			loadAsset('url:4', (err, result) => {
				if (err) {
					console.error(`An error occured: ${err}`);
					return;
				} else {
					console.log(result);
				}
				loadAsset('url:5', (err, result) => {
					if (err) {
						console.error(`An error occured: ${err}`);
						return;
					} else {
						console.log(result);
					}
					loadAsset('url:6', (err, result) => {
						if (err) {
							console.error(`An error occured: ${err}`);
							return;
						} else {
							console.log(result);
						}
					})
				})
			})
		})
	})
})
```
Here is a different implementation of the same code using [[Promise|promises]]:
```JS
async function loadAsset(url) {
	return new Promise((resolve, reject) => {
		console.log(`Loading asset from ${url}...`);
		setTimeout(() => {
			if (Math.random() > 0.1) {
				resolve(`Asset loaded successfully from ${url}`);
			} else {
				reject(`Failed to load asset from ${url}`);
			}
		}, 1000);
	})
}
console.log(`starting process...`);
loadAsset('url:1')
	.then((msg) => {
		console.log(msg);
		return loadAsset('url:2');
	})
	.then((msg) => {
		console.log(msg);
		return loadAsset('url:3');
	})
	.then((msg) => {
		console.log(msg);
		return loadAsset('url:4');
	})
	.then((msg) => {
		console.log(msg);
		return loadAsset('url:5');
	})
	.then((msg) => {
		console.log(msg);
		return loadAsset('url:6');
	})
	.then((msg) => {
		console.log(msg);
		console.log('All assets loaded successfully');
	})
	.catch((error) => {
		console.error(`Error: ${error}`);
	});
```
This code is much more readable and maintainable. A [[Promise|promise]] calls its 'resolve' or 'reject' method which runs the code passed into the 'then' if it is resolved, and runs the code passed into the 'catch' if rejected.


