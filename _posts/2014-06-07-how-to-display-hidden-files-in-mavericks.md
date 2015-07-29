---
layout: post
title: "How to display hidden files in Mavericks"
categories: [Mac]
---

Before Mavericks, we can use the following two commands to display/hide hidden files:

{% highlight bash %}
defaults write com.apple.Finder AppleShowAllFiles Yes && killall Finder //display system files
defaults write com.apple.Finder AppleShowAllFiles No && killall Finder //hide system files
{% endhighlight %}

But in Mavericks, we need to use the latest commads, change the first `Finder` to `finder`:

{% highlight bash %}
defaults write com.apple.finder AppleShowAllFiles Yes && killall Finder //display system files
defaults write com.apple.finder AppleShowAllFiles No && killall Finder //hide system files
{% endhighlight %}