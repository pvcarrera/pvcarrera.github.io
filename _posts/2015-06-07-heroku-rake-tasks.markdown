---
layout:     post
title:      "Heroku Rake tasks"
date:       2015-06-07 14:41:00
categories: general
comments:   true
---
In the project I’m currently working on, we use [Heroku][heroku] as infrasture provider. [Heroku][heroku] has its own command line operations to execute tasks like db backups, managing addons, etc.

Last March, [Heroku][heroku] [deprecated its PG Backups addon][deprecated], so the CLI syntax changed. The project I'm working on is a Rails project, so by default we already use [Rake][rake] to run tasks like database migrations. Thus, instead of learning the new [Heroku][heroku] syntax, I’ve decide to write a couple of [Rake][rake] tasks to wrap the new [Heroku][heroku] commands.

{% highlight Ruby %}
namespace :heroku do
  DEFAULT_APP = 'my_app'

  desc 'Create database backup'
  task :backup, :app do |_task, args|
    args.with_defaults(app: DEFAULT_APP)
    Bundler.with_clean_env do
      `heroku pg:backups capture --app #{args[:app]}`
    end

    puts $?.exitstatus == 0 ? 'Backup created.' : 'Backup failed.'
  end

  desc 'Download latest database backup'
  task :download_backup, :app do |_task, args|
    args.with_defaults(app: DEFAULT_APP)
    Bundler.with_clean_env do
      url = `heroku pg:backups public-url --app #{args[:app]}`
      `curl -o latest.dump "#{url}"`
    end

    puts $?.exitstatus == 0 ? 'Database downloaded.' : 'Download failed.'
  end
end
{% endhighlight %}

Next time [Heroku][heroku] updates its command line syntax, I’ll only need to update the commands in my [Rake][rake] tasks and I won't have to learn the nex syntax.

[heroku]: http://www.heroku.com
[deprecated]: https://devcenter.heroku.com/changelog-items/623
[rake]: http://docs.seattlerb.org/rake/
