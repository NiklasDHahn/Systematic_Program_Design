# Chapter 1 Beginning Student Language (BSL)

## 1a Expressions
- Provide an expression to Dr.Racket to recive a value
- Example expression:
``(+ 3 4)`` $\to$ Adds the two values after the addition sign
- Expression can be very complex
- An expression is always surrounded by round brackets (actually you can use also square brackets, but it is the convention in this course to use round ones)
- Inside the brackets you start with a primitive like addition, substraction, etc. and then any number of other expressions
- Expression can be values or more complex functions
- `#i` is used to represent irrational number, since the computer can only approximate those

## 1b Evaluation
Example expression:
``(+ 2 (* 3 4) (- (1 2) 3))``
- Call to a primitive $\to$ an expression that starts with an opening bracket, followed by a primitive
- In a primitive call, the primitive is called the operator
- All expressions that follow the operator are called operands

Evaluation of the above expression:
- Identify as primitive call
    - First, reduce operands to values
    - Then, apply primitive/operator to the values

Apply to example expression:
- The first operand is `2`, which is already a value
- The second operand is `(* 3 4)`, which is another expression that has to be reduced to a value
    - The expression is a call to a primitive
    - Both operands (`3` and `4`) are values
    - Applying multiplication to both operands equals `12`
- The third operand is `(- (+ 1 2) 3)`, which is an expression
    - The first operand is another expression (`(+ 1 2)`) and its its second operand is a value (`3`)
    - The expression that is the first operand has two value (`1` and `2`) and the primitive is addition. Once applied to the values, the expression equates to `3`.
    - Now, both operands are reduced to values, which means the outer expression can be evaluated by subtracting `3` from `3`, which equates to `0`
- All inner expressions have been reduced to values
    - The final expression looks like this: `(+ 2 12 0)`
    - After applying the primitive (addition) the result is: `14`

Take away:

`Expressions are evaluated from left to right and from inside to outside`

The above expression is evaluated to the **primitive call rule**.

## 1c Strings and Images
- Calls to string and image primitives work just like calls to number primitives
- A string in BSL is a number of characters surrounded by double quotes (e.g.: "apples")
- Strings are values
- Stings can be combined with the `string-append` primitive
- The number of characters in a string can be computed with the `string-length` primitive
- With the `substring` primitive a part of a string (a subset of the characters) can be extracted
    - The arguments are the string, the start and the end index
    - The end index is not included
    - 0-based indexing
- The standard BSL does not support image functionality out of the box
- To get access to image functions an external library has to be imported
- To import an external library the `require` keyword is used
- The image library used in this course is `2htdp/image`
- The convention is to place the import statement at the top of the program
- To load the library include `require 2htdp/image` in the program
    - This provides image functions from the 2nd edition of the How to Design Programs book

Some of the image primitives of this library and how to use them are listed below:

- ``circle``
    - Draws a circle on the screen
    - Arguments
        1. Radius in screen coordinates (pixels)
        2. Define if the circle should be `solid` or `outline`
        3. Color of the circle
- ``rectangle``
    - Draws a rectangle on the screen
    - Arguments
        1. Width in screen coordinates (pixels)
        2. Height in screen coordinates (pixels)
        3. Define if the rectangle should be `solid` or `outline`
        4. Color of the rectangle
- ``text``
    - Draws the given text as an image on the screen
    - Arguments
        1. A string
        2. Fontsize
        3. Color of the text
    - Note: this is an image of a string and not the string itself
- ``above``
    - Produces an image with all its arguments stacked up, and lined up on their horizontal centers
- ``beside``
    - Produces an image with all its arguments side by side, and lined up on their vertical centers
- ``overlay``
    - Produces an image with all its arguments one on top of each other, and lines up on their centers

## 1d Constant Definitions
- Definitions are used to give names to values in order to use them in different parts of the program
- A constant can be created with the primitive `define`
    - Arguments
        1. Name of the constant (typically in capital letters)
        2. The expression of the constant
- Names of constants cannot change, they are immutable

Rules for evaluating a constant definition:
1. Evaluate the expression
2. Record the resulting value as the value of the constant with the given name
3. The value of the constant name is the recorded value

## 1e Function Definitions
- Functions make a program dynamic
- Functions help to reduce the amount of redundancy in a program
- A **parameter** is the varying component of a function
- A function can be used repeatatly with any value each time

Function generating mechanism in BSL:

```
(define (function-name parameter-name)
    body of the function ...)
```
- Functions make code more concise
- A well named function can give the code more meaning

Functions definition rules:
- The `define` keyword is used to start the function definition
- A function name has to be choosen
- Parameter names have to be choosen
- An expression need to be provided as the function body

Calling a function:
- Warp everything in parenthesis
- Start with the function name
- Followed by operands/values according to the number of parameters

Evaluating a function call:
- Extend the rules for evaluating a primitive call
1. Reduce operands (also called arguments) to values
2. **Replace function call by body of the function in which each occurance of parameters are replaced by the corresponding arguments**
3. Apply primitives to the values

## 1f Booleans and if Expressions
- Booleans represent the answer to true/false questions
- if expressions are used to make decisions based on the result of such questions
- Values and syntax of booleans in BSL
    - `true`
    - `false`
- **Predicates** are primitive functions that produce a boolean value
- An if expression lets us change the behavior of the program based on the answer to a boolean question

```
(if <expression> ;; question
    <expression> ;; true answer
    <expression> ;; false answer)
```

Rules for evaluating an if expression:
1. If the question expression is not a value, evaluate it and replace it with the value
2. If the question is true replace the entire if expression with the true answer expression
3. If the question is false replace the entire if expression with the false answer expression
4. If the question is a value other than true or false produce an error

Usefule primitives for if expressions and boolean expressions:
- ``and`` (interpretation: logical and)
    - Concatenate multiple boolean expressions
    - The behavior of the program is determined by the state of all boolean expressions
    - Only if all boolean expressions evaluate to `true`, the answer will be `true` and `false` if any of the expressions if `false`
    - During evaluation each boolean expression is evaluated individually, but evaluation stops as soon as an expression is `false`
- ``or`` (interpretation: logical or)
    - Concatenate multiple boolean expressions
    - If any of the boolean expressions results to `true`, the whole expression is evaluated to `true`
    - During evaluation each expression is evaluated individually but evaluation stops as soon as an expression is `true`
- ``not`` (interpretation: logical not)
    - Reverse the answer of a boolean expression
    - Returns `true` if the expression is `false`
    - Returns `false` if the expression is `true`
