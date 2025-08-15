A magic number is a numeric literal that is used in the code without any explanation of its meaning, example:
```C#
CanBuyBeer(int age, int money) {
	if (age >= 18 && money >= 20) {
		return true;
	}
	return false;
}
```
In this example, the literal numbers "18" and "20" are arbitrary numbers. Because of the method you may infer via context that 18 is the legal drinking age and 20 is the price, but the fact that there are just literal numbers there make it harder to understand at first sight. The way to fix this to create named constants for the numbers:
```C#
CanBuyBeer(int age, int money) {
	const int legalDrinkingAge = 18;
	const int beerPrice = 20;
	if (age >= legalDrinkingAge && money >= beerPrice) {
		return true;
	}
	return false;
}
```
Also, if something changes in the future, like the beer price, then it will be much easier to change, end less easy to make an error attempting to change it, especially if that specific number appears more than once.