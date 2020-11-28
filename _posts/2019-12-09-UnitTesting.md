---
layout: post
title:  "Unit Testing"
date:   2019-12-09
categories: process
---
If you are anything like me, you heard the phrase "unit testing" thrown around a lot before it meant anything real. And terms such as "unit testing", "integration testing" and "functional testing" had little distinction. They all fell into the category of "should be doing but not on THIS project". Most of my software experience at that point comprised of tight deadlines, unfamiliar technologies, poorly defined specifications or tight budgets that didn't account for anything as fancy as "testing".

During one particularly frustrating bug-chasing afternoon, I had arrived at that familiar near-tears, nothing-works, I-am-stupid-and-I-should-just-quit-forever moment and was ready to submit my resignation and go start a new career as a grocery store clerk. Already at the bottom, I figured I'd give testing a try since it could hardly be worse.

To my delight, the tests created magnificent clarity about the actual problems. It forced me to think in very small units -- hey! an actual unit test! - and tiny, logical steps. Does this one method return the right number? Does it handle the null case? What about the next one?

Since then, I've been a convert. Testing isn't just about code reliability (although, of course, that's awesome, too) it's a tool to help build successful thinking and execution.

#### Unit testing is exactly what it sounds like
Ideally, a "unit" of code has one job and is encapsulated - meaning you can reliably give input and get expected output, without side effects. If you are connecting to a database or a network resource or some other external dependency or interaction, you aren't unit testing. 

While I always understood unit tests as a way to make sure the software works correctly, it's really a *design* tool to help you write better code. I've discovered that my tests are dead simple when my code is really good...and nearly impossible when my code is spaghetti-fied crap.

Most languages have built-in tests and test runners, so if you want to give it a try, just start super slow. Test just one method that has a crystal clear answer. 

For example, I recently wrote a method that ended up being a more complex aggregation than I expected, to calculate some monthly totals:

```javascript
export const calcSubcategoryTotals = (subcategories) => {
    let result = [];
    const aggregated = subcategories.reduce((a,sc)=> {
        sc.values.map(v => {
            if(!a[v.month]) {
                a[v.month]  = [Number(v.value)];
            } else {
                a[v.month].push(Number(v.value));
            }
        });
        return a;
    },{});
    Object.entries(aggregated).map(([k,v]) => {
        result.push({ month: k, value: v.reduce((a,n) => a + n, 0)});
    });
    return result;
}
```

In the middle of `mapping` inside my `reduce` I was like, "Well, I have no idea if this is the right approach.". So I wrote a super minimal unit test, which both helped me understand what I was doing and also let me know if I was done.

```javascript
import {calcSubcategoryTotals} from "./utils";

it('calculates the aggregation correctly', () => {
    const subcats = [
        {
            name: "Subcat Name",
            values: [
                {month: "Jan", value: "10"},
                {month: "Feb", value: "10"},]
        },
        {
            name: "Subcat Name Two",
            values: [
                {month: "Jan", value: "10"},
                {month: "Feb", value: "10"},]
        },
    ];
    const actual = calcSubcategoryTotals(subcats);
    const expected = [{month: 'Jan', value: 20}, {month: 'Feb', value: 20}];
    expect(actual).toStrictEqual(expected);
})
```

This is one nano test that took me about five minutes to write, but it helped me clarify my thinking enormously. And, even better, anyone that comes along after me will have this super simple example of what I was trying to do. And when they discover that my code bombs on the null case, it's easy to write a test to prove it and know exactly what to fix.

#### Red, Green, Refactor
The other handy benefit is that once my test passed, I can go in there and make the original method less...um, obtuse. And as long as the test(s) are clear, I know I haven't broken anything essential.

This is the conceptual basis of test-driven development -- 
1. Red: write the failing test (e.g. call the skeleton method)
1. Green: write the method so the test passes
1. Refactor: make it good while _knowing_ you aren't breaking anything

I was shocked at the amount of mental order this brought to my problem solving. My code improved markedly overnight not because I had any additional specialized knowledge, but because I was able to break the code into sufficiently small parts. So small and simple, in fact, that months later I could go back and read what I had written without effort.

#### But what about testing that the software actually works for reals?
This is the domain of integration and functional testing, and it is also incredibly valuable. However, without a solid foundation of unit tests it can be just as messy, fragile and hard to understand as the code you are struggling to prove works. I have read a hundred Medium articles where devs have all this complex mocking behavior and click event rendering whatever and that's great -- super useful. But even the simplest, least fancy test can make a big impact.

#### So, yeah, I'm a fan but the real world is pretty messy

Most of the time as a dev, you inherit code from someone else that only barely makes sense to you. Writing tests for code that's already complicated is not only difficult, it's damn near herculean. So my approach is the same one I take about flossing or getting regular exercise: some is better than none, consistency is more important than perfection and when you just can't get to it because "reasons" then just move on and fight the next fire.

