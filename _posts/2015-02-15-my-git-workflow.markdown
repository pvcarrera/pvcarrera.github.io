---
layout:     post
title:      "My git workflow"
date:       2015-02-15 19:56:00
categories: general
---
In this post, I'm going to write down my git workflow so that the next time I find myself talking/thinking about it, I'll be able to use my blog as a reminder and visual support.

I usually follow the [feature branch workflow][branch_flow], creating a branch per feature:

{% highlight Bash shell scripts %}
git checkout -b new-feature master
{% endhighlight %}

Then, I work and commit code on my local branch as often as I need

{% highlight Bash shell scripts %}
git add -A
git commit
{% endhighlight %}

Sometimes, I also save my branch in the remote repository

{% highlight Bash shell scripts %}
git push -u origin new-feature
{% endhighlight %}

When I think the feature is ready to go, I review the commits, [reorder][reorder_commits] and [squash][squash_commits] some of them.

Then I open a pull request. When I get comments on the code, I add temporary commits in response to them. If these commits are fixes to previous ones, I like to use fixup

{% highlight Bash shell scripts %}
git commit --fixup=SHA
{% endhighlight %}

Once the merging of the branch is approved, I rebase master and fixup the temporary commits into the original ones

{% highlight Bash shell scripts %}
git rebase master -i --autosquash
{% endhighlight %}

Finally, I merge the feature branch into master.

{% highlight Bash shell scripts %}
git checkout master
git merge new-feature master
git push origin master
{% endhighlight %}

Ah, I've forgotten something! I also follow some practices to elaborate my commit messages. Here you have a [post] [commits_guide] I use to refresh my memory when I need to remember the best practices for commit messages.

[branch_flow]: https://www.atlassian.com/git/tutorials/comparing-workflows/feature-branch-workflow
[squash_commits]:  http://gitready.com/advanced/2009/02/10/squashing-commits-with-rebase.html
[reorder_commits]: http://gitready.com/advanced/2009/03/20/reorder-commits-with-rebase.html
[commits_guide]: http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html
