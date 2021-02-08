---
layout: post
title:  "Dijkstra's Two-Stack Algorithm"
date:   2019-1-12
categories: algorithms
---

## Dijkstra's Two-Stack Algorithm

Dijkstra was pretty good at his job, as evidenced by the fact that his name is on a bunch of useful algorithms. This is one that was put in front of me as I was trying to solve [Advent of Code, Day 18 2020](https://adventofcode.com/2020/day/18). I'd never heard of it so thought I'd look in more detail.

The gist of the puzzle (you can read the full text above) is evaluating arithmetic expressions in a way that is non-standard, like so: 

`The homework (your puzzle input) consists of a series of expressions that consist of addition (+), multiplication (*), and parentheses ((...)). Just like normal math, parentheses indicate that the expression inside must be evaluated before it can be used by the surrounding expression. Addition still finds the sum of the numbers on both sides of the operator, and multiplication still finds the product.`

`However, the rules of operator precedence have changed. Rather than evaluating multiplication before addition, the operators have the same precedence, and are evaluated left-to-right regardless of the order in which they appear.`

So the gist is that you start with an expression like `((2 + 4 * 9) * (6 + 9 * 8 + 6) + 6) + 2 + 4 * 2` and get an answer of `23340` instead of `3208` like you'd get with a calculator.

--

Although I was able to solve the puzzle without Dijkstra's Two-Stack Algorithm, once it was pointed out to me I saw immediately how much better a solution it is. I also discovered via Google it is a [totally standard CS 201 thing to do](http://faculty.cs.niu.edu/~hutchins/csci241/eval.htm). Yet another opportunity for me to beat myself up for not studying computer science in school when I had the chance.

I found a succinct explanation of the algorithm from [David Matuszek on his magnificent web page](https://www.cis.upenn.edu/~matuszek/) which I have shamelessly used to further my own understanding of the fundamentals of computer science. 

I also learned about `infix` and `prefix` and `postfix` arithmetic expressions, which I was unaware of. It's a way of changing the syntax of a mathematical expression without changing the semantic. It was interesting to learn this since I've been watching my son work his way through math word problems in the fifth grade and it's...there is a lot of overlap. It's incredible to see how much the **way you talk** about a problem affects the **way you solve** it.

Infix is operators between operands: `3 + 4`
Prefix is operators before operands: `+ 3 4`
Postfix is operators after operands: `3 4 +`

Humans are more effective understanders of infix and computers prefer postfix...so something to remember as a handy rule of thumb for translation. The algorithm below is for infix expressions.

### Dijkstra's Two-Stack Algorithm, In Plain English Language Words:

You need two stacks, a value stack (operands), and an operator stack. Numbers will be double values, operators will be char values. The whole of the expression is made up of `items` - the "stuff", ignoring whitespace.

1. While there are still items to read
   1. Get the next item
   1. If the item is:
       1. **A number**: push it onto the value stack.
       1. **A variable**: get its value, and push onto the value stack.
       1. **A left parenthesis**: push it onto the operator stack.
       1. **A right parenthesis**:
            1. While the top of the operator stack is not a left parenthesis
                1. Pop the operator from the operator stack.
                1. Pop the value stack twice, getting two operands.
                1. Apply the operator to the operands, in the correct order.
                1. Push the result onto the value stack.
            1. Pop the left parenthesis from the operator stack, and discard it.
       1. **An operator** `currentOp`:
          1. While the operator stack is not empty, and the top of the
           operator stack has the same or greater precedence as `currentOp`,
                1. Pop the operator from the operator stack.
                1. Pop the value stack twice, getting two operands.
                1. Apply the operator to the operands, in the correct order.
                1. Push the result onto the value stack.
          1. Push `currentOp` onto the operator stack.
1. While the operator stack is not empty,
    1. Pop the operator from the operator stack.
    1. Pop the value stack twice, getting two operands.
    1. Apply the operator to the operands, in the correct order.
    1. Push the result onto the value stack.
1. At this point the operator stack should be empty, and the value
   stack should have only one value in it, which is the final result.

You can see a lot of opportunity here for helper methods and for more efficiency, but the logic is clean and intuitive. In any case, once I got over myself and learned a bit, I re-implemented the puzzle solution. Here is a part of what I ended up with, that represents the algorithm above:

```kotlin
private fun result(op: Char, a: Long, b: Long): Long {
    return when(op) {
        '+' -> a + b
        '-' -> a - b
        '*' -> a * b
        '/' -> a / b
        else -> throw IllegalArgumentException("$op is not a recognized operator.")
    }
}

fun evaluate(expr: CharIterator) : Long {
    val ops = MutableStack<Char>();
    val values = MutableStack<Long>();

    while (expr.hasNext()) {
        val c = expr.nextChar()
        when(c) {
            '(' -> ops.push(c)
            ')' -> {
                while(!ops.isEmpty() && ops.peek() != '(' ) {
                    values.push(result(ops.pop(), values.pop(), values.pop()))
                    ops.pop()
                }
            }
            '+', '-', '*', '/' -> {
                while(!ops.isEmpty() && (precedence(ops.peek()!!) >= precedence(c))) {
                    values.push(result(ops.pop(), values.pop(), values.pop()))
                }
                ops.push(c)
            }
            else -> values.push(c.toString().toLong())
        }
    }
    return values.pop()
}
```


