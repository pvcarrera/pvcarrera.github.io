---
layout:     post
title:      "Persisting Value Objects in a Relational Database"
date:       2015-12-13 11:30:00
categories: general
comments:   true
---
How a VO persists has nothing to do with Entity/VO semantics, but it’s something that has to be considered since business data frequently has to be persistent. When you want to persist a value object in a relational database, there are tree main options. As always, there is not a correct one and all of them have pros and cons:

- Use a normalised database approach
    - How:
        - Use the auto generated id to identify the table but hiding it in the VO objects.
    - Pros:
        - Efficient database access. All the benefits of normalised databases: easily query, merge and update data.
        - Provides an easy way to solve one to many relations between VO objects.
    - Cons:
        - Difficult to hide/manage the id at class/object level.
        - Disconnection of the object from its database id. Easy to end with more than one database row for the same object.
        - Not easy to clean a destroyed VO from the database. Can lead to stale data in the DB.
        - Slow global update of entities with value objects in the DB.
- Use a denormalised database
    - How:
        - Since VO objects don’t make sense without their Entity, at least at DB level, we can merge all the attributes in the same table.
    - Pros
        - True value objects without database ids.
        - This approach makes it easy to move out a value object from an entity. Only the objects and the ORM mapping would need to change, there is no need to change the database scheme.
    - Cons
        - Can be very tricky to implement properly with distributed systems.
        - Difficult to implement when the relation is not one to one.
- Serialise representation next to the entities
    - How:
        - Store the object graph in a column. This can be accomplished using serialisation.
    - Pros
        - Efficient database loading.
    - Cons
        - Can be very tricky to implement properly with distributed systems.
        - Deserialisation.
        - Query limitations.

This is my overview/index on persistence and value objects. I’ll try to dig more and illustrate with some examples each of these alternatives in future posts.

**Related posts:**

- [DDD and relational databases](http://gojko.net/2009/09/30/ddd-and-relational-databases-the-value-object-dilemma)
- [Persisting value objects](http://thepaulrayner.com/persisting-value-objects)
- [How are value objects stored in the database](http://stackoverflow.com/questions/679005/how-are-value-objects-stored-in-the-database)
- [The Ideal Domain-Driven Design Aggregate Store](https://vaughnvernon.co/?p=942)
