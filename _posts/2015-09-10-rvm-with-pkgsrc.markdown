---
layout: post
title:  "RVM and non-standard include and lib directories"
date:   2015-09-10
---
This quick post is about installing rubies with [RVM](https://rvm.io) if the build dependencies aren't installed in the usual places (`/usr/local/[include,lib]`).  In my particular case, I use [pkgsrc](https://www.pkgsrc.org/) in unprivileged mode for package management, so anything I "install" gets put under `$HOME/pkg/`.  When installing a ruby with `rvm install`, we just need to add the `-C` flag which allows to specify options that `rvm` will forward to the `configure` script that is run before compiling the ruby and the default gems.  The two options we want to pass `--with-opt-include` and `--with-opt-lib`, which obviously are the directories we want the compiler to (also) look in for headers and library files.  Here's a sample command:

{% highlight bash %}
rvm install 2.2.3 -C --with-opt-include=/home/dagnir/pkg/include,--with-opt-lib=/home/dagnir/pkg/lib
{% endhighlight %}

Note that when specifying multiple options with the `-C` flag in RVM, you need to separate them with a comma.

Note that you do the same exact thing later on when you want to install a gem that might have other dependencies:

{% highlight bash %}
gem install GEM -- --with-opt-include=INCLUDE_DIR --with-opt-lib=LIB_DIR
{% endhighlight %}
This time, you don't need to separate them with a comma.  Again, these flags will be forwarded to the configure script(s).

That's it! Hopefully this will be helpful for people running similar setups (e.g. Homebrew with non-standard install dir).
