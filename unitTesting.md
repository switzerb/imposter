# Unit Testing

If you are anything like me, you heard the phrase "unit testing" thrown around a lot before it meant anything real. And terms such as "unit testing", "integration testing" and "functional testing" had little distinction. They all fell into the category of "should be doing but not on THIS project". Most of my software experience at that point comprised of tight deadlines, unfamiliar technologies, poorly defined specifications or tight budgets that didn't account for anything as fancy as "testing".

During one particularly frustrating bug-chasing afternoon, I reached my limit and decided to focus on testing as a skill set to reduce future headaches. I'm focusing on unit testing in this article because it's provided the most direct value in terms of the quality of my code.

#### Unit testing is exactly what it sounds like
Ideally, a "unit" of code has one job and is encapsulated - meaning you can reliably give input and get expected output, without side effects. If you are connecting to a database or a network resource or some other external dependency or interaction, you aren't unit testing. 

While I always understood unit tests as a way to make sure the software works correctly, it's really a *design* tool to help you write better code. I've discovered that my tests are dead simple when my code is really good...and nearly impossible when my code is spaghetti-fied crap.

#### Red, Green, Refactor
This is the conceptual basis of test-driven development -- 
1. Red: write the failing test (e.g. call the skeleton method)
1. Green: write the method so the test passes
1. Refactor: make it good while _knowing_ you aren't breaking anything

I was shocked at the amount of mental order this brought to my problem solving. My code improved markedly overnight not because I had any additional specialized knowledge, but because I was able to break the code into sufficiently small parts. So small and simple, in fact, that months later I could go back and read what I had written without effort.

#### But what about testing that the software actually works for reals?
This is the domain of integration and functional testing, and it is also valuable. However, without a solid foundation of unit tests it can be just as messy, fragile and hard to understand as the code you are struggling to prove works.

#### So, yeah, I'm a fan but the real world is pretty messy

Over time, I began to appreciate just how subtle and difficult test-driven development is in the real world. Unless you have the good fortune to work in product engineering in a highly structured environment (which likely means you are not reading this anyway), carving out the time/space/mental organization to actually write the code that can be tested in units is quite a challenge...and that doesn't even _include_ the actual tests.

