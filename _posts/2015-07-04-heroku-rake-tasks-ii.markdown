---
layout:     post
title:      "Heroku Rake tasks II"
date:       2015-07-04 14:41:00
categories: general
comments:   true
---
In the [previous post][previous], I wrote a couple of [Rake][rake] tasks to manage [Heroku][heroku] db backups. Once I had this tasks in place I realised that it would be handy to have another one to load them into my local [Postgres][postgress] database, so I wrote this in my `database.rake` file:

{% highlight Ruby %}
namespace :db do
  desc 'Load remote database backup to local database'
  task :load_backup do
    fail 'latest.dump does not exist' unless File.exist?('latest.dump')
    Bundler.with_clean_env do
      `pg_restore --verbose --clean --no-acl --no-owner \
      -h localhost -U postgres_dev -d development latest.dump`
    end
    puts 'Load completed.'
  end
end
{% endhighlight %}

This worked fine, but if you take a look at the [Rake documentation][rake_docs] it's easy to spot that [Rake][rake] uses a dependency based style of computation rather than the usual imperative style.This means you can add dependent tasks and [Rake][rake] will evaluate and run them first. Thanks to this [Rake][rake] feature I rewrote my task to look like this:

{% highlight Ruby %}
namespace :db do
  desc 'Load remote database backup to local database'
  task :load_backup => 'heroku:download_backup' do
    Bundler.with_clean_env do
      `pg_restore --verbose --clean --no-acl --no-owner \
      -h localhost -U postgres_dev -d development latest.dump`
    end
    puts 'Load completed.'
  end
end
{% endhighlight %}

With these changes, the task `heroku:download_backup`, which was created in the [previous post][previous], runs before the `load_backup` task, so I can ensure that the `lastest.dump` file is going to be in place. As a side effect I don't need to check for the dump file existence anymore.

It looks good, but after using these tasks a couple of times I felt the inconvenience of having to wait for the db dump to download. The solution to this problem was using the [Rake file taks][file_task]. It's like a normal [Rake][rake] task, but it checks for the file timestamps. This is how I rewrote my [Heroku][heroku] [Rake][rake] task

{% highlight Ruby %}
desc 'Download database backup'
task :download_backup, [:app] =>  latest.dump

file 'latest.dump' do |task, args|
  args.with_defaults(app: DEFAULT_APP)
  Bundler.with_clean_env do
    url = `heroku pg:backups public-url --app #{args[:app]}`
    `curl -o latest.dump "#{url}"`
  end
  puts $?.exitstatus == 0 ? 'Database downloaded.' : 'Download failed.'
  end
end
{% endhighlight %}

The [Rake file task][file_task] only runs if the file doesn't exist, so I can now run `rake db:load_backup` and it'll only download the database dump file the first time.

It would also be nice if the `heroku:download_backup` ran when there is a new backup available in [Heroku][heroku], but I'll leave that for the future since this post is long enough.

[heroku]: http://www.heroku.com
[rake]: http://docs.seattlerb.org/rake/
[previous]: {% post_url 2015-06-07-heroku-rake-tasks %}
[rake_docs]: https://github.com/ruby/rake#resources
[file_task]: http://ruby-doc.org/stdlib-2.2.2/libdoc/rake/rdoc/Rake/FileTask.html
[postgress]:  http://www.postgresql.org
