---
layout:     post
title:      "Hub commands"
date:       2015-05-23 20:15:00
categories: general
comments:   true
---
During these last weeks, I’ve been trying out some Hub commands and finally decided to include some of them in my day to day workflow. Hub is a command line wrapper for Git that adds some GitHub specific functionality.

To install it
{% highlight Bash shell scripts %}
$ brew install hub
{% endhighlight %}

To mix the classic Git commands with the new Hub commands, add to the bash profile
{% highlight Bash shell scripts %}
eval "$(hub alias -s)"
{% endhighlight %}


All the commands available are in the [project page](https://github.com/github/hub), but the ones I’ve incorporated into my workflow are:

- `git compare`
- `git pull-request -o -b`
- `git ci-status -v`



This is the way I use them:

Given that I’m in a feature, bug, etc. branch, when at some point I want to review my changes and compare them with the master branch, I use the command `git compare`. It launches the default browser with the GitHub diff view, comparing the current branch and master.

When I’m happy with my branch, I push my changes to remote with `git push origin my_branch`. This action triggers the CI server which runs the test suite and confirms that tests work not only on my machine. While the CI server is running, I use the Hub command `git ci-status` which returns a string with the current status of the CI server. Sometimes I run the command with the option -v, which also returns the CI server status plus the URL to CI build results.

Once the CI server runs ssuccessfully, I proceed to open a pull request. In order to do that, I use the command `git pull-request`, usually with the option -o which automatically opens the GitHub pull request page in the default browser. Another option I sometimes use is -b, which allows opening a pull request to a specific branch.


And that’s it. These are the three Hub commands I’ve incoporated to my git workflow.


