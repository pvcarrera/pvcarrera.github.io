---
layout:     post
title:      "Rails polymorphic associations"
date:       2015-07-27 20:12:00
categories: general
comments:   true
---
Working on a Rails project, I faced a problem similar to this:
Given an ActiveRecordModel called Booking which can have multiple bills:
{% highlight Ruby %}
class Bill < ActiveRecord::Base
  belongs_to :booking
end
{% endhighlight %}
{% highlight Ruby %}
class Booking < ActiveRecord::Base
  has_many :bills
end
{% endhighlight %}
The amount of the Bill comes from the Booking's amount:
{% highlight Ruby %}
class Bill < ActiveRecord::Base
  belongs_to :booking
  delegate :amount, to: :booking
end
{% endhighlight %}
Now we need a new model, Subscription, and a Subscription can also have a Bill so:
{% highlight Ruby %}
class Bill < ActiveRecord::Base
  belongs_to :subscription
  belongs_to :booking
  delegate :amount, to: :booking
end
{% endhighlight %}
{% highlight Ruby %}
class Subscription < ActiveRecord::Base
  has_many :bills
end
{% endhighlight %}
And here is the problem, it isn't possible to delegate to both Booking and Subscription.

Ok, let's stop thinking about Rails and try to solve this with OO and plain Ruby. In plain Ruby, the Bill class would probably look like this:
{% highlight Ruby %}
class Bill
  def initializer(booking)
    @booking = booking
  end

  def amount
    @booking.amount
  end
end
{% endhighlight %}
Then, when the Subscription entity enters the game, the only thing we need to take care of is the fact that Booking and Subscription have a common interface with one method amount. This can be written as follows:
{% highlight Ruby %}
class Bill
  def initializer(billable)
    @billable = billable
  end

  def amount
    @billable.amount
  end
end
{% endhighlight %}

Now, the Bill class doesn't have to mind about Bookings or Subscriptions anymore.

Good, back to Rails. How do I model this in Rails? I would have to change the previous Rails code to something like this:
{% highlight Ruby %}
class Bill < ActiveRecord::Base
  belongs_to :billable
end
{% endhighlight %}
The answer is polymorphic associations. This type of association allows Bill to belong_to an interface. The only thing we have to do is mark this relation as polymorphic, and the Booking and Subscription models as billable
{% highlight Ruby %}
class Bill < ActiveRecord::Base
  belongs_to :billable, polymorphic: true
end
{% endhighlight %}
{% highlight Ruby %}
class Booking < ActiveRecord::Base
  has_many :bills, as: :billable
end
{% endhighlight %}
{% highlight Ruby %}
class Subscription < ActiveRecord::Base
  has_many :bills, as: :billable
end
{% endhighlight %}

Maybe in a future post I dig into how Rails maps these models to the database, but that's it for today. Here I leave other posts about Rails polymorphic associations that I found interesting:

 - [Refactoring rails STI](https://about.futurelearn.com/blog/refactoring-rails-sti)
 - [Whats the deal with Rails polymorphic associations](https://robots.thoughtbot.com/whats-the-deal-with-rails-polymorphic-associations)

