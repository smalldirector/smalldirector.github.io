---
layout: post
title: "Degrade Java 7 to Java 6 on an Amazon EC2 Linux"
comments: true
categories: [Java, EC2, Linux, Solr] 
---
Amazon EC2 has updated its default java version with Java 7 in each EC2 server, but some featurs for solr 4.7 only work fine with Java 6.
ex, it will cause the exception for solr 4.7 with Java 7:

`java.lang.NoClassDefFoundError: org/apache/lucene/analysis/cn/smart/SentenceTokenizer`

So we have to change the Java version, you only need to follow two steps:

1, We need to install `java-1.6.0-openjdk` firstly, the command is :

{% highlight bash %}
sudo yum install java-1.6.0-openjdk
{% endhighlight %}

2, Choose your default java version:

{% highlight bash %}
sudo alternatives --config java
{% endhighlight %}

And then you need to choose the number, then you can use the `java -version` to check the latest environment.

![sudo-alternatives-config-java.png](/assets/img/posts/sudo-alternatives-config-java.png "sudo-alternatives-config-java.png")

Of course, if you want to upgrade the new Java version, for example, from Java 7 to Java 8, you can do it in the same way.
