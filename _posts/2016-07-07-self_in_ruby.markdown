---
layout: post
title:  "self in Ruby"
date:   2016-07-07 10:01:54 -0400
---


Everything in Ruby is an object. Every piece of code you write belongs to some object. And every object has a class. Inside those classes there is self which refers to object itself which is an instance of that class.

```
♥ string = "hello"
=> "hello"
♥ string.object_id
=> 70313455665560
♥ string.class
=>String
```
```
♥ arr = Array.new([1, 2, 3])
=> [1, 2, 3]
♥ arr.object_id
=> 70313455618260
♥ arr.class
=> Array
```
```
♥ 2.object_id
=> 7
♥ 2.class
=> Fixnum
```

***self*** is a special variable that points to the object that owns the currently executing code within the class definition.

Below we have an instance of Transfer class. And let's assume we stopped our code at where binding.pry is right after it is initialized with instance variables.

```
class Transfer
  attr_reader :amount, :sender, :receiver
  attr_accessor :status

  def initialize(sender, receiver, amount)
    @status = "pending"
    @sender = sender
    @receiver = receiver
    @amount = amount
=>  binding.pry
  end
```
```
♥ self
=> #<Transfer:0x007ff2f4062568
 @amount=50,
 @receiver=#<BankAccount:0x007ff2f40625b8 @balance=1000, @name="Avi", @status="open">,
 @sender=#<BankAccount:0x007ff2f4062680 @balance=1000, @name="Amanda", @status="open">,
 @status="pending">

♥ self.amount
=> 50

♥ self.receiver
=> #<BankAccount:0x007ff2f40625b8 @balance=1000, @name="Avi", @status="open">

♥ self.sender
=> #<BankAccount:0x007ff2f4062680 @balance=1000, @name="Amanda", @status="open">

♥ self.status
=> #<BankAccount:0x007ff2f40625b8 @balance=1000, @name="Avi", @status="open">
```


Using ***self*** without understanding when to use it, might make your code harder to read. 

Below we have two examples of *Transfer* class. Both examples work just fine. However on the one on the left use of self is redundant.
![](http://i.imgur.com/QFWe38s.png)

**optional uses of *self***

* when getting instance variables 
* using self to indicate which method call you are referring 


<!-- ![](http://i.imgur.com/LPZ4Wce.png) -->


**non-optional uses of *self***

* setting instance attributes inside a class definition
```
self.status = "closed"
```
* passing it as an argument to a method
```
some_method(self)
```
* to indicate a method within the class definition as a class method

```
class BankAccount
  @@customers = []

  def self.all
    @@customers
  end
end
```

 * use ***self*** when you need to save an instance of the class

```
class BankAccount
  @@customers = []
  attr_reader :name
  attr_accessor :status, :balance

  def initialize(name)
    @name = name
    @balance = 1000
    @status = "open"
    save
  end

  def save
    @@customers << self
  end
end
```
In the end you can avoid using self in many cases, but it might be helpful to use it in some cases to increase readability of your code. And there are non-optional cases where you have to use self. 

~aehmt
