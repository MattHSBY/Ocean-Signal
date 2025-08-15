It is to be recognised that there is great tension between the need for individual developers to be able to express themselves in code in a way that seems natural and efficient, and the business need to consistency and the ability for developers to work with each other's code. It is for that reason that this style guide is advisory, and not a set of rigid rules.

To a great extent, modern IDEs eliminate the need for rigid or overly-specific style guides.

## General Principles

> A computer program is a means of telling other people what you want the computer to do
> \[Donald Knuth\]
### Strive for Readability

Code should be self-explanatory. It should be possible to examine a function or method and immediately grasp what the method does and how it does it. This is the core principle enshrined in the book [Clean Code]() by Robert C. Martin. If this principle is followed aggressively, then comments become largely redundant.

There is a debate about the value of comments and whether they enhance or hurt the readability of code. Modern thinking is that the need for a comment is a _code smell_.

- Prefer clean, readable code over comments.
- Use comments where necessary and only if the code cannot be made understandable by other means.

Example of obtuse code requiring comments:

### The KISS Principle: Keep It Simple, Stupid

Avoid using fancy tricks that may be difficult for another person to understand.

### Premature Optimisation is the Root of All Evil#

99.9% of the time, code optimisation is at best a waste of effort, and at worst can cause subtle bugs and make the code hard to understand. Therefore:

- Do not waste time optimising code unless you can show that it is necessary.
- Prefer readability over performance, unless performance is of the essence.

### Comments Considered Somewhat Harmful

A contentious statement, perhaps, but it shouldn't be. Classical education teaches programmers to comment everything. However, this thinking is outdated.

Comments are harmful because:
- They are a crutch for poorly-written code
- They often get out of date with respect to the code they are supposed to document
- Too many comments clutter up the code and make it harder to read.

Therefore, as a general principle:
- Prefer more readable code over comments.

That said, if you cannot make your code readable for some reason, then use comments to explain it.