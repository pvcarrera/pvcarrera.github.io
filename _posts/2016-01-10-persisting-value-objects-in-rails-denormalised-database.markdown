---
layout:     post
title:      "Persisting Value Objects in Rails - Denormalised database"
date:       2016-01-10 18:00:00
categories: general
comments:   true
---
In [my previous post][previous_post], I talked about some of the options we have to persist value objects in a relational database. In this one, I’m going to show an example using Rails and the denormalised database approach.

Let’s say we want to model a Client entity which has an Address. In the [Entities and Value Objects post][vo_post], I already used an Address value object as an example, let’s bring it back

{% highlight Ruby %}
  class Address
    attr_reader :street, :postcode
    def initialize(street, postcode)
      @street = street
      @postcode = postcode
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
{% endhighlight %}

Following the denormalised database approach, we’ll put together in the same database table the Client and the Address attributes. In Rails this would result in a migration similar to this

{% highlight Ruby %}
  class CreateClient < ActiveRecord::Migration
    def change
      t.string :name
      t.string :address_street
      t.string :address_postcode
    end
  end
{% endhighlight %}

In Rails we can say that ActiveRecord models are entities where the identity is provided by the ActiveRecord id. If we model the Client as an entity, the Client class could be something like

{% highlight Ruby %}
  class Client < ActiveRecord::Base
    def address
      @address ||= Address.new(address_street, address_postcode)
    end

    def address=(address)
      self[:address_street] = address.street
      self[:address_postcode] = address.postcode

      @address = address
    end

    private
    attr_writter :address_street, :address_postcode
  end
{% endhighlight %}

Since one of the value objects' main characteristics is immutability, in order to prevent unwanted modification attempts from the Client, this implementation restricts the access of Rails attribute writers to private.

It’s also worth mentioning that this denormalised approach can be handy when you are trying to extract behaviour from a convoluted Rails model. Imagine a Client model in which the Address attributes are already part of it, extracting a value object is trivial since there is no need to touch the database scheme. In fact, this refactor is a common one in articles that talk about refactoring fat ActiveRecord models.

And that’s it, we have a Client entity that has a value object Address where all its attributes (Client and Address attributes) are persisted in the same database table.

**References:**

- [The Rails Way](https://leanpub.com/tr4w)
- [7 Patterns to Refactor Fat ActiveRecord Models](http://blog.codeclimate.com/blog/2012/10/17/7-ways-to-decompose-fat-activerecord-models)

[previous_post]:{% post_url 2015-12-13-persisting-value-objects-in-a-relational-database %}
[vo_post]:{% post_url 2015-10-22-entities-and-value-objects %}
