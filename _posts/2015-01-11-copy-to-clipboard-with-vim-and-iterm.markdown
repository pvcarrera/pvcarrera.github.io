---
layout:     post
title:      "Copy to clipboard with Vim and iTerm"
date:       2015-01-12 19:35:00
categories: general
---
Last week I was trying to configure my [Vim][vim] in order to allow it to access my OSX clipboard.
I tried all the combinations:
{% highlight Vim Script %}
set clipboard=unnamed
set clipboard=unnamedplus,unnamed,autoselect
{% endhighlight %}

And so, but any of them were functioning properly. They either worked for a short period of time, or I got
 truncated copies.

Then I started to think about other configurations which could interfere with [Vim][vim] accessing the
 system clipboard. At some point, I realised that "copy to clipboard" started to fail when I play
 around with the mouse on my [Vim][vim], so I tried to disable the mouse in [Vim][vim] by removing the
 option:
{% highlight Vim Script %}
set mouse=a
{% endhighlight %}

 from my ``.vimrc``. After that change, "copy to clipboard"
 started to work fine.

Despite the problem being solved, and I don't really use the mouse in [Vim][vim],
 I wasn't happy about the idea of leaving its configuration aside, without finding a solution.

I went back to my old ``.vimrc`` version and, before the "enable mouse" command, I found this comment:

> In many terminal emulators the mouse works just fine, thus enable it.

And I thought, maybe the problem it had to do with my terminal [iTerm][iterm]

The last step I took was looking for more information about it on the web, were I found this [post][post],
 which describes the same problem I had, and how to configure [iTerm][iterm] to work nicely with
 [Vim][vim] mouse reporting plus clipboard functionalities.

That's it for today, now I don't have to remember this configuration any more, it's saved in my blog.

[post]: http://aubreypwd.com/copy-paste-not-working-in-iterm2-using-Vim-and-system-keyboard-fix/
[iterm]: http://iterm2.com/
[vim]: http://www.vim.org/
