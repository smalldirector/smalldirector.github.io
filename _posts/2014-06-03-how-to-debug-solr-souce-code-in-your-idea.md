---
layout: post
title: "How to debug solr souce code in your IDEA"
date: 2014-06-03 22:10:11 +0800
comments: true
categories: [IDEA, Solr, Ant]
---
If you are puzzled by some behavior Solr was showing you, I think you should to debug your issues in Solr source code.
It's very easy to debug Solr source code in your IDEA, just following these steps:

For the default, there is no `solr.war` in your `solr-4.7.0/solr/example/webapps`, so you need to generate it firstly.
So you need run the following command in your solr folder `solr-4.7.0/solr`:

{% highlight bash %}
ant dist
{% endhighlight %}

After this command, it will generate the war file in `solr-4.7.0/solr/dist`, so move it to `solr-4.7.0/solr/example/webapps`.

Then step into `solr-4.7.0/solr/example`, and start this Solr server with debug mode:

{% highlight bash %}
java -jar -Xdebug -Xrunjdwp:transport=dt_socket,server=y,suspend=n,address=8888 start.jar
{% endhighlight %}

After this command, you can access Solr admin page [http://localhost:8983/solr/](http://localhost:8983/solr/)

The final thing is we need to configure IDEA, you need to open Remote configuration page, `Run->Edit Configurations`, and then click `+` button, choose Remote configuration page, and configure it as following figure:

![remote-debug-solr-configuration.png](/assets/img/posts/remote-debug-solr-configuration.png "remote-debug-solr-configuration.png")

click the debug button in the toolbar, And then you can set any breakpoint in your IDEA, and then debug your code:

![idea-debug-solr-view.png](/assets/img/posts/idea-debug-solr-view.png "idea-debug-solr-view.png")

If you want to know how to run solr source code in your IDEA, you can refer to blog [How to Run Solr Source Code in Your IDEA](http://www.otips.me/posts/how-to-run-solr-source-code-in-your-idea.html).

And now, please enjoy it! Have fun hacking on Solr!