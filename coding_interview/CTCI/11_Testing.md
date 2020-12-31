# Testing

### Introduction

+ 4 types of tests: real world project; piece of software; for a function; troubleshoot an existing issue
+ Requirements: 
  + big picture understanding; understanding how pieces fit together (ecosystem); 
  + structured approach to testing; 
  + practicality (user friendly)

### Testing real world project/ a piece of software

+ Aspects:
  + Manual/ automated
  + Black box/ white box
+ Steps
  + Determine which aspect
  + Who& why
  + What use cases: eg. access
  + Bound of use
  + Stress/ failure conditions
  + Test method

### Testing a function

+ Constraints
+ Test cases: 
  + normal
  + extremes (empty array, very small, very large)
  + illegal input: nulls, negatives
  + strange input
+ Define expected result
+ Write test code

### Troubleshooting 

+ Understand scenario: how long, how often, what environment, error reports?
+ Break down problem to testable units: iterate through use case steps that encapsulates failure
+ create tests



## Interview Questions

### 11.1 Mistake

Find the mistake(s) in the following code:

```java
unsigned int i;
for(i=100; i>=0; --i) // loops infinitely because unsigned int always>0. Use i>0 instead
    printf("%d\n",i); //%u instead of %d for unsigned int
```



### 11.2 Random crashes

You are given the source to an application which crashes when it is run. After
running it ten times in a debugger, you find it never crashes in the same place. The application is single threaded, and uses only the C standard library. What programming errors could be causing this crash? How would you test each one?

+ random variable: 
  + fix variable/ fix seed instead
+ uninitialized variable: can take on an arbitrary value
  + initialize all variables
  + use runtime tools (debugger)
+ memory leak: depend s on number of processes running at that time
  + close down all other applications/ run on larger memory machine
+ external dependencies
  + create dummy inputs instead of dependencies input

### 11.3 Chess test

We have the following method used in a chess game: boolean canMoveTo( int x,
int y). This method is part of the Piece class and returns whether or not the piece can move to position (x, y) . Explain how you would test this method.

[text a function]

+ Constraints: board size/ available locations. A set of legal movements
+ Test cases:
  + normal: take each legal and illegal movements and check if they all work
  + extremes (empty array, very small, very large)
    + nearly empty board
    + unbalanced white/ black board
  + illegal input: nulls, negatives
    + going out of boarder
    + moving direction is already occupied by other pieces
  + strange input
+ Define expected result
+ Write test code



### 11.4 No Test Tools

How would you load test a webpage without using any test tools?

+ Load test definition: 
  + response times
  + throughput rates
  + resource-utilization levels
  + Maximum load
+ Approach:
  + simulate load, and meanwhile measure each of these criteria
  + Examples to simulate:
    + concurrent virtual users with multi-thread program
    + measure response time, data I/O



### 11.5 Test a pen

How would you test a pen?

[Real world project]

+ Who& why
  + children: are they safe for children?
+ What use cases: eg. access
  + drawing: are the colors correct?
+ Bound of use
  + on clothing: does it writ properly?
  + intended to wash off: does it wash off properly?
+ Stress/ failure conditions
  + How long/ many draws does it last
+ Test method

p164





p429