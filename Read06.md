# Game of Greed 1

## How to use the Randomize

In case you want to implement a random function or give a random number value **The Random module** contains some very useful functions.

So first of all **Make Sure To Import** `random` **Before The Code** in order to use it

**Randint:** This will generate a value between 2 numbers given Ex:

`print(random.randint(10,25))`

This will give you a random integer between 

**Choice** Generate a random value from a sequence Ex:

`colors = ['red', 'black', 'green']`
`random.choice(colors)`

**Shuffle** shuffles the elements in list in place, so they are in a random order Ex:

`x = [[i] for i in range(10)]`
`random.shuffle(x)`

**Randrange** Generate a randomly selected element from range(start, stop, step) Ex:

`for i in range(3):`
    `print random.randrange(0, 101, 5)`

## Risk Analysis

> The probability of any unwanted incident is defined as Risk

> risk analysis is the process of identifying the risks in applications or software that you built and prioritizing them to test.

### Why use Risk Analysis

- highlights the potential problem areas
- helps the developers and managers to mitigate the risks

risks that you could encounter list:
- Use of new hardware
- Use of new technology
- Use of new automation tool
- The sequence of code
- Availability of test resources for the application

there are certain risks that are unavoidable such as:
- The time that you allocated for testing
- A defect leakage due to the complexity or size of the application
- Urgency from the clients to deliver the project
- Incomplete requirements

### Risk Identification

There are different sets of risks:

- **Business Risks:** It is the risk that may come from your company or your customer, not from your project
- **Testing Risks:** You should be well acquainted with the platform you are working on, along with the software testing tools being used.
- **Premature Release Risk:** a fair amount of knowledge to analyze the risk associated with releasing unsatisfactory or untested software is required
- **Software Risks:** You should be well versed with the risks associated with the software development process.

### Risk Assessment

![image](assets/Read06.jpg)

#### The perspective of Risk Assessment

**Effect:** To assess risk by Effect. In case you identify a condition, event or action and try to determine its impact.

**Cause:** To assess risk by Cause is opposite of by Effect. Initialize scanning the problem and reach to the point that could be the most probable reason behind that.

**Likelihood:** To assess risk by Likelihood is to say that there is a probability that a requirement won’t be satisfied.
