## 16.1 SpEL Expressions with Guards

It is also possible to use SpEL expressions as a replacement for a full Guard implementation. Only requirement is that expression needs to return a Boolean value to satisfy Guard implementation. This is demonstrated with a guardExpression() function which takes an expression as an argument.