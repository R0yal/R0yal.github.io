---
layout: post
title:  "Installing SickRage on Alpine linux"
date:   2015-12-08
categories: alpine linux usenet server sickrage sickbeard automated ash
---
## SickRage
SickRage is an automatic video library manager for TV shows. 
It is quite popular in the Usenet crowd and is often seen as the successor to sickbeard or at minimum still in active development.

This tutorial will show you have to manually or Automatically configure SickRage on Alpine Linux

## Automatic configuration
{% highlight bash %}
apk add curl && curl -sSl https://raw.githubusercontent.com/R0yal/alpine_sickrage/master/alpine_sickrage.sh | ash -s
{% endhighlight %}
## Manual Setup

### Install git and python.
{% highlight bash %}
apk add git python
{% endhighlight %}
### Create the sickrage user as a service account.
{% highlight bash %}
adduser -S sickrage
{% endhighlight %}
### Create the /opt directory for sickbeard to live in.
{% highlight bash %}
mkdir /opt
{% endhighlight %}
### Clone sickrage into the opt directory.
{% highlight bash %}
git clone https://github.com/SiCKRAGETV/SickRage.git /opt/sickrage
{% endhighlight %}
### Run sickrage once manualy to generate the 'config.ini'. Press ctrl-c to quit the program.
{% highlight bash %}
python /opt/sickbeard/SickBeard.py
{% endhighlight %}
### Copy init script to init.d
{% highlight bash %}
cp /opt/sickrage/runscripts/init.gentoo /etc/init.d/sickrage
{% endhighlight %}
### Create conf file for the sickrage init file
{% highlight bash %}
echo 'SICKRAGE_USER=sickrage
SICKRAGE_GROUP=nogroup
SICKRAGE_DIR=/opt/sickrage
SICKRAGE_DATADIR=/opt/sickrage
SICKRAGE_CONFDIR=/opt/sickrage
PATH_TO_PYTHON_2=/usr/bin/python2.7' > /etc/conf.d/sickrage
{% endhighlight %}
### Change ownership of /opt/sickrage to sickrage 
{% highlight bash %}
chown -R sickrage:nogroup /opt/sickrage
{% endhighlight %}
### Start and enable sickrage on boot
{% highlight bash %}
rc-service sickrage start && rc-update add sickrage default
{% endhighlight %}
