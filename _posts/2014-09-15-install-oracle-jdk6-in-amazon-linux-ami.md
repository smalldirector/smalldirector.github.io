---
layout: post
title: "Install Oracle JDK6 in Amazon Linux AMI"
category: [Rabbitmq, EC2, Linux]
---

## Download Oracle JDK6
You can download Oracle JDK6 from this link: [jdk-6u45-linux-x64.bin](http://www.oracle.com/technetwork/java/javase/downloads/java-archive-downloads-javase6-419409.html#jdk-6u45-oth-JPR), choose `jdk-6u45-linux-x64.bin` and accept license agreement, because this is a old version of Oracle JDKs, so you need input your authorization information before you download it.

## Install Oracle JDK6
After you download it, you can run the following command to extract the bin file:
{% highlight bash %}
sudo chmod u+x jdk-6u45-linux-x64.bin
sudo ./jdk-6u45-linux-x64.bin
{% endhighlight %}

Then you should create one more alternative for Java, `/opt/jdk/` is your jdk home path:
{% highlight bash %}
sudo ln -s jdk1.6.0_45/ jdk
sudo alternatives --install /usr/bin/java java /opt/jdk/bin/java 20000
{% endhighlight %}

Then you can set the default java in this Amazon Linux AMI:
{% highlight bash %}
sudo /usr/sbin/alternatives --config java
{% endhighlight %}

For now you can input `java -version` command to check your jdk version information.

