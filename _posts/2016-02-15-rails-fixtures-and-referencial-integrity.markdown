---
layout:     post
title:      "Rails fixtures and PostgreSQL referencial integrity"
date:       2016-02-15 18:00:00
categories: general
comments:   true
---
Recently, on a Rails project, I had the need to create a disposable Heroku app to show features in progress and get quick feedback from the stakeholders. Not a long time ago, I’d read about the [Heroku Review App functionality][review_apps_docs] and I though this would be a good opportunity to try it out.

Following the [Heroku documentation][review_apps_docs] I configured the Review apps for the project and, in no time, I had the ability to spin up a disposable Heroku app per pull request. The next step was populating these apps with demo data.

In this project we have a QA/UAT Heroku app with a database containing demo data. My first thought was cloning the UAT db in the review boxes, but Heroku is clear about this approach: "Copying full database contents from parent to Review Apps (similar to heroku fork) is not currently supported”. Instead, they recommend seeding the database with a post deploy script.

As this is a Rails project, I created some fixtures with demo data and loaded them using the command `rake db:fixtures:load`. Fixtures loaded fine in my local machine but in the review boxes I was getting a referential integrity violation error. What was happening?

We use PostgreSQL as database and PostgreSQL does integrity checks, like foreign key constraints. Rails fixtures are loaded in alphabetical order, so it’s common to find cases where a table that is being created has a reference to another table which hasn’t been created yet.

If we take a look at [ActiveRecord code base][fixtures_code] we can see how it calls the method `disable_referential_integrity` from `create_fixtures` method. [This method][integrity_warning] contains a warning which says "Rails needs superuser privileges to disable referential integrity". That was the reason why it worked in my machine, I had super user privileges for the local database.

As expected, in Heroku it’s not possible to change PostreSQL database privileges, so I had to search for a different approach. After a bit of researching about PostgreSQL and referencial integrity, I found a [post][drop_recreate_post] which shows how to drop and recreate PostgreSQL constraints.

Using the scripts in the [mentioned post][drop_recreate_post] I created a new [Rake task][rake_task_gist] called `populate_sample_data`. This task resets the database, drops the PostgreSQL constraints, loads the fixtures and then recreates the previous constraints.

Finally, I added the `populate_sample_data` rake task to the Review Apps post deploy script and everything started to work as expected.

Rails contributors are working on a [solution][rails_thread] for this problem, but for now it’s a work in progress and it doesn’t cover PostgreSQL versions older than 9.4.

Hopefully this post will help other people facing the same problem, or someone can come up with a better solution.

[review_apps_docs]: https://devcenter.heroku.com/articles/github-integration-review-apps
[drop_recreate_post]: http://www.hagander.net/blog/automatically-dropping-and-creating-constraints-131
[fixtures_code]: https://github.com/rails/rails/blob/master/activerecord%2Flib%2Factive_record%2Ffixtures.rb#L523
[integrity_warning]: https://github.com/rails/rails/blob/master/activerecord%2Flib%2Factive_record%2Fconnection_adapters%2Fpostgresql%2Freferential_integrity.rb#L25
[rake_task_gist]: https://gist.github.com/pvcarrera/123280c58eca51ccebe3
[rails_thread]: https://github.com/rails/rails/pull/21233/files#r38946048
