# Hello, World

The beginning of a project for me, particularly a greenfield project where no legacy code exists, is both exciting and terrifying. The sense of potential and possibility is almost intoxicating. And, for those of us with control issues and aspirations of perfection, a blank page also represents code with no errors in it yet.

Crossing the threshhold into complexity doesn't take long and, inevitably the pride in accomplishment gets married to a strong impulse to refactor everything, RIGHT NOW.

Which is why sometimes it's hard to get started. Avoidance of the actual messiness of real life and fear of failure can generate a remarkable heap of procrastination. 

So small, simple formal beginnings can be quite useful in solving that problem. In particular, when I am overwhelmed by impending complexity, this provides a clear open door to walk through. 

As a concrete example, this year I've set out to learn some Java as a way to focus on coding fundamentals, which intimidated me. 

```
public static void main(String[] args) throws IOException {
  System.out.println("Hello, World");
}
``` 

This was the first Java I ever wrote and is likely the place most beginners start.

The uses of a simple "Hello, World" start are many:

* It's a simple entry point and the complexity is managable even to a beginner
* It's concrete, visible and short time-to-execute
* It demonstrates the development environment/deploy/stack is set up correctly
* It exposes environmental and system problems, like a cumbersome deploy process

When I start personal or professional projects, I include a "Hello, World" unit test as well to confirm my test environment will run and tests things. It usually looks something like this:

```
    @Test
    public void testHello() {
        assertEquals(true, true);
    }
```

