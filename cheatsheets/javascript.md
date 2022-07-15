# JavaScript Cheat Sheet

### 1: Variables

| Code                           | What It Is                                          |
| :----------------------------- | :-------------------------------------------------- |
| `var a;`                       | variable                                            |
| `var b = "this";`              | string: "this"                                      |
| `var c = "Hi " + "John";`      | string: "Hi John"                                   |
| `var d = 1 + 2 + "3";`         | string: "33"                                        |
| `var e = [2, 3, 5, 8];`        | array                                               |
| `var f = false;`               | boolean (true/false)                                |
| `var g = /()/;`                | regex                                               |
| `var h = function() {};`       | function object (doesn't really need the semicolon) |
| `const PI = 3.14`              | constant (value cannot be reassigned)               |
| `var a = 1, b = 2, c = a + b;` | one line                                            |
| `let z = "zzz"`                | block scope local variable                          |

#
### 2: Values

| Value                                 | Type    |
| :------------------------------------ | :------ |
| `false`, `true`                       | boolean |
| `18`, `3.14`, `0b1001`, `0xF6`, `NaN` | number  |
| `"flower"`, `"John"`                  | string  |
| `undefined`, `null`, `Infinity`       | special |
#
### 3: Operators

| Operator | What It Does                                           |
| :------- | :----------------------------------------------------- |
| `=`      | assignment                                             |
| `+`      | addition                                               |
| `-`      | subtraction                                            |
| `*`      | multiplication                                         |
| `/`      | division                                               |
| `%`      | modulo (gives the remainder of a division calculation) |
| `a++`    | postfix increment                                      |
| `--b`    | prefix decrement                                       |
| `!`      | bang operator (logical not)                            |
| `==`     | equality, equals                                       |
| `!=`     | not equal, inequality, unequal                         |
| `===`    | strict equlity, strictly equals, identical             |
| `!==`    | strict inequality, strictly not equals, not identical  |
| `<`      | less than                                              |
| `>`      | more than                                              |
| `&&`     | logical and                                            |
| `\|\|`   | logical or                                             |
| `x += y` | x = x + y                                              |
| `x -= y` | x = x - y                                              |
| `x *= y` | x = x \* y                                             |
| `x /= y` | x = x / y                                              |
| `x %= y` | x = x % y                                              |
| `?`      | ternary (like a shorthand `if/else`)                   |
#
### 4: Functions

#### A: SYNTAX

```
function name(parameter1, parameter2, parameter 3) {
    // code to be executed
}
```

#### B: CALLING/INVOKING THE FUNCTION

##### Ex. 1

```
function name() {
    console.log("Hello World!");
}
name(); // calls the function
console.log(name); // logs the function object in the console
```

![function console.log](https://github.com/ZanClifton/javascript-cheat-sheet/blob/main/images/function-console-log.png)

##### Ex. 2

```
function multiply(p1, p2) {
    return p1 * p2;
}
console.log(multiply(4, 3)); // the values passed via the parameters when invoking the function are called arguments
let x = multiply(2, 5);
console.log(x);
```

![multiply console.log](https://github.com/ZanClifton/javascript-cheat-sheet/blob/main/images/multiply-console-log.png)
#
### 5: Hints and Tips

JavaScript is a loosely typed language which means it doesn't require you to declare the type of variable. It can handle comparisons such as `3 == "3"` (as in, the number 3 loosely equals the string "3"), for example, without any issues. But watch out! It definitely handles `3 + 3` differently to `3 + "3"`!
