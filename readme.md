# Intro to Object-Oriented Programming in Python

## Learning Objectives

- Review the principles of Object Oriented Programming
- Describe the relationship between a class and an instance
- Define a Python Class and instantiate it
- Distinguish local, instance, or class variables
- Examine interaction with objects through methods
- Explain inheritance in Python
- Look at Python's "magic methods"

## Framing: Review, why OOP?

#### Easy to Understand

Objects help us build programs that model how we tend to think about the world.
Instead of a bunch of variables and functions (procedural style), we can group
relevant data and functions into objects, and think about them as individual,
self-contained units. This grouping of properties (data) and methods is called
*encapsulation*.

#### Managing Complexity

This is especially important as our programs get more and more complex. We can't
keep all the code (and what it does) in our head at once. Instead, we often want
to think just a portion of the code.

Objects help us organize and think about our programs. If I'm looking at code
for a Squad object, and I see it has associated *people*, and those people can
dance when the squad dances, I don't need to think about or see all the code
related to a person dancing. I can just think at a high level "ok, when a squad
dances, all it's associated people dance". This is a form of *abstraction*... I
don't need to think about the details, just what's happening at a high-level.

#### Ensuring Consistency

One side effect of *encapsulation* (grouping data and methods into objects) is
that these objects can be in control of their data. This usually means ensuring
consistency of their data.

Consider the bank account example... I might define a bank account object
such that you can't directly change it's balance. Instead, you have to use the
`withdraw` and `deposit` methods. Those methods are the *interface* to the
account, and they can enforce rules for consistency, such as "balance can't be
less than zero".

#### Modularity

If our objects are well-designed, then they interact with each other in
well-defined ways. This allows us to refactor (rewrite) any object, and it
should not impact (cause bugs) in other areas of our programs.

## OOP Syntax: JavaScript vs. Python
```bash
$ ipython
```

```py
class User:
    def __init__(self, name):
        self.name = name
    
    def greet(self):
        print("Hi! My name is {}".format(self.name))

me = User("Ali")
me.greet() # prints: Hi! My name is Ali!
```
Let's break down this syntax. On the first line, we declare and name the class -- this one is called `User`. Then, on the next line, we see a method called `__init__`. The `__init__` method is the initializer -- for the purposes of this class we will be using it like the `constructor` in JavaScript. We use it to set the initial values of our instance's attributes. The `__init__` method takes two parameters: `self` and `name`. Self, by convention, is the first parameter to each method in a Python class. It refers to the instance of the object. `name` is another attribute we will set in our `__init__` method. Finally, the greet method displays a greeting formatted with the instance's name. At the end, we instantiate a new user with `me = User("Ali")`. 

In JavaScript, we could write this class:

```js
class User {
  constructor (name) {
    this.name = name
  }
  greet () {
    console.log(`Hi! My name is ${this.name}`)
  }
}

const me = new User()
me.setNameTo('Ali')
me.greet() // prints: Hi! My name is Ali!
```

## Exercise: Create a `BankAccount` class.
* Bank accounts should be created with the `kind` of account (like "savings" or "checking").
* Each account should keep track of it's current `balance`.
* Each account should have access to a `deposit` and a `withdraw` method.
* Each account should start with a `balance` set to zero.
* return the amount withdrawn, for convenience

Create a checking account and a savings account. Withdraw money from the savings
account and deposit that amount into the checking account.

Bonus: start each account with an additional `overdraft_fees` property that
starts at zero. If a call to `withdraw` ends with the `balance` below zero
then `overdraft_fees` should be incremented by twenty.

## Inheritance in Python
Inheritance allows us to build new classes out of old classes.
It allows us to extend functionality defined in a `parent`
class and create `children` classes that extend and
compartmentalize different pieces of functionality.

Inheritance models natural hierarchies that we're used to
thinking about in the world. We can define one general class
to model something like an **Phone** and then `inherit` the
methods and properties of the class to make new classes out of
the first class, like **IPhone** and **AndroidPhone**.

When we say sub-classes, or child classes, `inherit` methods
and properties from a parent class we mean the child class
has access to all of the functionality of it's parent, and
it can define it's own functionality on top of that.

When we define a class to represent a **Phone** we can add
the basic properties and functionality that all phones
have.

* All phones have a phone number
* All phones can place phone calls
* All phone can send text messages

After we define what a **Phone** is we can create classes
that `inherit` from the Phone class and add their own
properties and functionality.

Let's define two new classes that `inherit` from the **Phone** class.
We'll make an **IPhone** and an **AndroidPhone**.

* iPhones have a unique `unlock` method that accepts a fingerprint
* iPhones have a unique `set_fingerprint` method that accepts a fingerprint
* Android phones have a unique `set_keyboard` method that accepts a keyboard

```python
class Phone:
    def __init__(self, phone_number):
        self.number = phone_number
    
    def call(self, other_number):
        print("Calling {} from {}.".format(self.number, other_number))
    
    def text(self, other_number, msg):
        print("Sending text from {} to {}:".format(self.number, other_number))
        print(msg);
```
    
```python
class IPhone(Phone):
    def __init__(self, phone_number):
        super().__init__(phone_number):
        self.fingerprint = None
    
    def set_fingerprint(self, fingerprint):
        self.fingerprint = fingerprint
        
    def unlock(self, fingerprint=None):
        if (fingerprint == self.fingerprint):
            print("Phone unlocked. Fingerprint matches.")
        else:
            print("Phone locked. Fingerprint doesn't match.")
  

class Android(Phone):
    def __init__(self, phone_number):
        super().__init__(phone_number)
        self.keyboard = "Default"
        
    def set_keyboard(self, keyboard):
        self.keyboard = keyboard
```

## Inheritance Syntax
There's two new pieces of syntax used in the code above.

* Class definitions can accept a parameter specifying what class they inherit
  from.
* Child classes can invoke a method called `super()` to gain access to
  methods defined in the parent class and execute them.
  
Take another look at the Phone classes to see how these pieces of syntax
are used to define how the classes define their inheritance and how the
`super()` method is used.

Notice how the Android class doesn't repeat the code that attaches the
phone_number passed to the `__init__` method to the `self` reference. The
Android class calls the parent constructor through the super method and
allows the parent class to execute that default behavior.
  
```python
class Parent:
    def __init__(self, phone_number):
        self.phone_number = phone_number
    
    
class Android(Phone):
    def __init__(self, phone_number):
        super().__init__(phone_number)
```

## Exercise: Write Bank Account Classes
Let's practice writing classes and using inheritance by modelling different types
of Bank accounts.

* Create a base **BankAccount** class
  * Bank accounts keep track of their current `balance`
  * Bank accounts have a `deposit` method
  * Bank accounts have a `withdraw` method
  * the `deposit` method returns the balance of the account after adding
    the deposited amount.
  * the `withdraw` method returns the amount of money that was successfully
    withdrawn.
  * Bank accounts return `False` if someone tries to deposit or withdraw
    a negative amount.
  * Bank accounts are created with a default interest rate of 2%
  * Bank accounts have a `accumulate_interest` method that sets the balance
    equal to the balance plus the balance times the interest rate
  * `accumulate_interest` returns the balance of the account after calculating
    the accumulated interest
* Create a **ChildrensAccount** class
  * Children's bank accounts have an interest rate of Zero.
  * Every time `accumulate_interest` is executed on a Child's account the
    account  always gets $10 added to the balance.
* Create an **OverdraftAccount** class
  * An overdraft account penalizes customers for trying to draw too much
    money out of their account.
  * Overdraft accounts are created with an `overdraft_penalty` property
    that defaults to $40.
  * Customer's aren't allowed to withdraw more money than they have in their
    account. If a customer tries to withdraw more than they have then the
    withdraw method returns `False` and their balance is deducted only by
    the amount of the `overdraft_penalty`.
  * Overdraft accounts don't accumulate interest if their balance is below zero.
    
Sample Input:
```python
basic_account = BankAccount()
basic_account.deposit(600)
print("Basic account has ${}".format(basic_account.balance))
basic_account.withdraw(17)
print("Basic account has ${}".format(basic_account.balance))
basic_account.accumulate_interest()
print("Basic account has ${}".format(basic_account.balance))
print()

childs_account = ChildrensAccount()
childs_account.deposit(34)
print("Child's account has ${}".format(childs_account.balance))
childs_account.withdraw(17)
print("Child's account has ${}".format(childs_account.balance))
childs_account.accumulate_interest()
print("Child's account has ${}".format(childs_account.balance))
print()

overdraft_account = OverdraftAccount()
overdraft_account.deposit(12)
print("Overdraft account has ${}".format(overdraft_account.balance))
overdraft_account.withdraw(17)
print("Overdraft account has ${}".format(overdraft_account.balance))
overdraft_account.accumulate_interest()
print("Overdraft account has ${}".format(overdraft_account.balance))
```

Sample Output:
```
Basic account has $600
Basic account has $583
Basic account has $594.66

Child's account has $34
Child's account has $17
Child's account has $27

Overdraft account has $12
Overdraft account has $-28
Overdraft account has $-28
```

## You Do: Codebar (25 minutes, 11:45 - 12:10 / 4:15 - 4:40)

> 20 minutes exercise. 5 minutes review.

Clone down [this repo](https://git.generalassemb.ly/ga-wdi-exercises/codebar) and follow the instructions in the readme.


### Review Exercise: Create a BankAccount class.

* Bank accounts should be created with the kind of account (like "savings" or "checking").
* Each account should keep track of it's current balance.
* Each account should have access to a deposit and a withdraw method.
* Each account should start with a balance set to zero.
* return the amount withdrawn, for convenience

### What are Dunder Methods?

We've seen one dunder method before, `__init__`, which is called whenever you create an instance of a class. These methods are invoked by Python when you use a built-in method. For example the `__str__` dunder method is called whenever we use the `str()` function on an instance of the class. Let's see what that looks like:

```py
class Dog:
    def __init__(self, name):
        self.name = name
    
    def __str__(self):
        return self.name


maddie = Dog('Maddie')
print(str(maddie))
print(maddie)
```
Some others include:
* `__getattr__` for when you get an attribute (i.e. `maddie.name`)
* `__setattr__` for when you get an attribute (i.e. `maddie.name = 'Madison'`)
* `__len__` for when you call `len` on the class
* `__add__` for when you add instances of the class
* `__getitem__` for using bracket notation on an instance of the class (i.e. `maddie['food']`)

etc. They exist for almost every operator! [Reference on more](http://www.diveintopython3.net/special-method-names.html).

### Exercise: Fancy Bank Accounts

* When you print the bank account, make it so that it prints a well formatted blurb about the kind of account and its balance. (ex. `Savings Account: $50`) 
* Make it so that you can add the balances of two bank accounts by adding two instances of the class.
* Make it so that you can subtract, multiply, and divide the balances of two bank accounts by performing those mathematical operations on two instances of the class.
* Make it so that you can compare the balances of two bank accounts by using the greater than, less than, and equals to operators in Python on two instances of the class.
