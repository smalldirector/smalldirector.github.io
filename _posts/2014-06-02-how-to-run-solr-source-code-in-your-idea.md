---
layout: post
title: "How to run solr source code in your IDEA"
categories: [IDEA, Solr, Ant]
---

You need to do is to download the source code of solr, choose the version you want to use in [Apache Solr Archieve](http://archive.apache.org/dist/lucene/solr/).
For myself, I choose the Solr-4.7.0:

{% highlight bash %}
wget http://archive.apache.org/dist/lucene/solr/4.7.0/solr-4.7.0-src.tgz
{% endhighlight %}

Download and install [Apache Ant](http://mirror.nus.edu.sg/apache//ant/binaries/apache-ant-1.9.4-bin.tar.gz):

{% highlight bash %}
wget http://mirror.nus.edu.sg/apache//ant/binaries/apache-ant-1.9.4-bin.tar.gz
{% endhighlight %}

And set path environment for Apache Ant:
{% highlight bash %}
# set Apache Ant Environment
ANT_HOME=/Users/gabriel/Programs/ant
export PATH=$ANT_HOME/bin:$PATH
{% endhighlight %}

For the default, Apache Ant doesn't include [Apache Ivy](http://ant.apache.org/ivy/), so we need to install it firstly. There are two ways to install Apache Ivy, the first way is use command `ant ivy-bootstrap`, another way is to download `ivy-xxx.jar`, and put it in the lib folder of Apache Ant. 

If you don't install Apache Ivy firstly, you will encounter this problem:

![apache-ivy-problem.png](/assets/img/posts/apache-ivy-problem.png "apache-ivy-problem.png")

Generate IDE project configurations, If you want to use Eclipse project, you can use command `ant eclipse`, if you want to use IDEA project, you can use command `ant idea`, if you want to use Netbeans project, you can use command `ant netbeans`.

Then we run this command:

{% highlight bash %}
ant idea
{% endhighlight %}

After soooo long time(Ivy will download all the dependencies at the first time), we will get the successful message:

![ant-idea-command.png](/assets/img/posts/ant-idea-command.png "ant-idea-command.png")

Then we can use IDEA to import solr source code project.

Find class `org.apache.solr.client.solrj.StartSolrJetty`, it in the package `solr/solrj/src/test`. Then we need to set solr home firstly:

{% highlight java %}
System.setProperty("solr.solr.home", "solr/example/solr"); 
{% endhighlight %}

and change the war path:

{% highlight java %}
bb.setWar("solr/webapp/web")
{% endhighlight %}

At last, run `StartSolrJetty`, and then you can visit the [Solr Admin Page](http://localhost:8983/solr/#/) directly.

More information you can learn from Solr wiki, [How To Compile Solr](http://wiki.apache.org/solr/HowToCompileSolr)
