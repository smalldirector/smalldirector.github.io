---
layout: post
title: "Rabbitmq Server Cluster Installation in Amazon EC2(Amazon Linux AMI)"
category: [Rabbitmq, EC2, Linux]
---

## Install Erlang environment
Before install Erlang, please make sure your system has the latest the update, we need to try to get the latest Erlang verson:
{% highlight bash %}
sudo yum clean all
sudo yum -y update
{% endhighlight %}
then use linux yum tool to install Erlang:
{% highlight bash %}
sudo yum clean all
{% endhighlight %}
You can use `erl -v` to check the installation and the Erlang version you installed.

## Install Rabbitmq environment
Download latest Rabbitmq installation file from its official website http://www.rabbitmq.com/install-rpm.html. For now, the latest Rabbitmq version is `3.3.5-1`:
{% highlight bash %}
wget http://www.rabbitmq.com/releases/rabbitmq-server/v3.3.5/rabbitmq-server-3.3.5-1.noarch.rpm
{% endhighlight %}
Use rpm tool to install Rabbitmq
{% highlight bash %}
sudo rpm -ivh rabbitmq-server-3.3.5-1.noarch.rpm
{% endhighlight %}
You can use start & stop command to verify your installation
{% highlight bash %}
sudo /etc/init.d/rabbitmq-server start
sudo /etc/init.d/rabbitmq-server stop
{% endhighlight %}

If you want to totally stop rabbitmq service, you need to kill the process:
{% highlight bash %}
ps aux | grep rabbitmq
{% endhighlight %}
then find the pid of rabbitmq, kill it `sudo kill -9 rabbitmq-pid`

## Configure server host mapping

You need to configure the host name and its ip address mapping, you can use `hostname` command to check current server host name.

We have created three servers, so we should add the following information to `/etc/hosts` file:
{% highlight bash %}
172.31.1.166 ip-172-31-1-166
172.31.1.167 ip-172-31-1-167
172.31.1.168 ip-172-31-1-168
{% endhighlight %}

Notice: The IP address is the EC2 instance `private IP`.

## Initialize Rabbitmq configuration file "rabbitmq.config"

For the default, after you install Rabbitmq server, you will not find the `rabbitmq.config` file in `/etc/rabbitmq/`, you need to create it by yourself.

{% highlight bash %}
sudo cp /usr/share/doc/rabbitmq-server-3.3.5/rabbitmq.config.example /etc/rabbitmq/rabbitmq.confi
{% endhighlight %}

## Configure Rabbitmq server cluster information in its configuration file

Configure the private IP in this configuration file

{% highlight bash %}
{cluster_nodes, {['rabbit@ip-172-31-5-198', 'rabbit@ip-172-31-5-195', 'rabbit@ip-172-31-5-193'], ram}}
{% endhighlight %}

if you change the configuration file, you need `stop_app` and `reset` it.

## Synchronize Erlang cookie to all the servers in this cluster
We need to make all the servers have the same cookie information, then the Rabbitmq server can communicate each other.

You can use two way to synchronize the cookie information, the cookie information in `/var/lib/rabbitmq/.erlang.cookie`. The cookie value is like `ODZLSBTZPNQIQFEAGYFV`.

* Use scp command to copy the `.erlang.cookie` file to another servers.
* Use vim to edit other servers `.erlang.cookie` file, make them have the same value.

Before you use vim to edit the value, you need to set the write permission to the `.erlang.cookie` file:

{% highlight bash %}
sudo chmod 600 /var/lib/rabbitmq/.erlang.cookie
{% endhighlight %}

If you cannot find the `.erlang.cookie`, it means you have never started Rabbitmq server, please start it and then you will found this file.

## Active Rabbitmq server cluster

You need to use the following command to active Rabbitmq server cluster:
{% highlight bash %}
sudo /etc/init.d/rabbitmq-server start
sudo rabbitmqctl stop_app
sudo rabbitmqctl reset
sudo /etc/init.d/rabbitmq-server restart
{% endhighlight %}

You can use the following command to check the cluster status:
{% highlight bash %}
sudo rabbitmqctl cluster_status
{% endhighlight %}


## Enable Rabbitmq management plugin
Use the following command to enable rabbitmq management page:
{% highlight bash %}
sudo rabbitmq-plugins enable rabbitmq_management
{% endhighlight %}

After this, please remember to restart Rabbitmq server:
{% highlight bash %}
sudo /etc/init.d/rabbitmq-server restart
{% endhighlight %}

Then you can use your server `public IP` or `public DNS` to visit this management page: `http://your-public-ip:15672`

## Create user to manage Rabbitmq server cluster
For now, we will create a test account, username is `gabriel` and password is `tkz123`.

{% highlight bash %}
sudo rabbitmqctl add_user gabriel tkz123
{% endhighlight %}

Make this account as administrator:
{% highlight bash %}
sudo rabbitmqctl set_user_tags gabriel administrator
{% endhighlight %}

Set permission for the specified vhost, this is set for the `default vhost /`:
{% highlight bash %}
sudo rabbitmqctl set_permissions -p / gabriel ".*" ".*" ".*"
{% endhighlight %}

Notice: For this command, you just need to run once in any one of the clusters, another server will synchronize this information. 

## Configure HA policy for mirror exchange and mirror queues
Visit `Admin-->Policies-->Add Policy`, and add the following configuration:
{% highlight bash %}
VHost: mail
Name: webmail.ha.policy
Pattern: ^webmail\.
Apply to: Exchanges and Queues
Definition: ha-mode=all
{% endhighlight %}
