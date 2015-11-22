---
layout:     post
title:      "Entities and Value Objects"
date:       2015-10-22 20:30:00
categories: general
comments:   true
---
Entities and value objects are key concepts when talking about [Domain Driven Design][ddd].

Entities are objects represented by its identity. For example, in our system we can have a `User` object with multiple attributes like name, password, etc. This object, regardless of its attributes, is going to be represented by an id. This means we can change any of the object's attributes (name, password) and it will remain the same object. Example:

{% highlight Ruby %}
class User
  attr_accessor :name, :password
  def initialize(name, password)
    @name = name
    @password = password
  end
end
user = User.new('pablo', 'password')
# <User:0x007fc0199c5028 @name="pablo", @password="password">
user.name = 'peter'
# <User:0x007fc0199c5028 @name="peter", @password="password">
second_user = User.new('peter', 'password')
# <User:0x007fc01a171c90 @name="peter", @password="password">
second_user == user # false
{% endhighlight %}

On the other hand we have value objects. These objects represent something that measures, quantifies or describes an entity, and they don’t have an identity. Value objects are described by its attributes. Following the previous example, a `User` can have an `Address` where the address is a characteristic of this user. `Address` is characterised by its street and postal code attributes. If we change any of these attributes, we get a different `Address`.

Since any change in its attributes turn value objects into different objects, value objects should be treated as immutable objects. When you change the `Address` of a `User`, you don’t change the attributes of the `Address` object, instead you create a new `Address` and assign it to the `User`.

As we can have multiple objects representing the same `Address`, a common practice when working with value objects is to override the equality and hash methods.

Value object example:

{% highlight Ruby %}
class Address
  attr_reader :street, :postcode
  def initialize(street, post_code)
    @street = street
    @post_code = post_code
    freeze
  end
  def ==(other)
    other.is_a?(Address) && street == other.street && postcode == other.postcode
  end
  def hash
    [street, postcode].hash
  end
  alias_method :eql?, :==
end
address = Address.new('my address', 'NW18NH')
#<Address:0x007fc35205aba0 @street="my address", @post_code="NW18NH">
second_address = Address.new('second address', 'NW28JH')
#<Address:0x007fc352051578 @street="second address", @post_code="NW28JH">
copy_address = Address.new('my address', 'NW18NH')
#<Address:0x007fc352049008 @street="my address", @post_code="NW18NH">
address == second_address # false
address == copy_address # true
{% endhighlight %}

This was my introduction to entities and value objects. I’m currently reviewing my knowledge about [Domain Driven Design][ddd] and I mean to write a series of posts about it. If you want to read more about value objects and entities, here are some resources:

 - [Matin Fowler: Value Object](http://martinfowler.com/bliki/ValueObject.html)
 - [Martin Fowler: Evans Classification](http://martinfowler.com/bliki/EvansClassification.html)
 - [Domain Driven Design][ddd]

[ddd]: http://www.amazon.com/Domain-Driven-Design-Tackling-Complexity-Software/dp/0321125215
