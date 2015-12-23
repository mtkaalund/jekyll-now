---
layout: post
title:  "XBMC and packaging"
date:   2011-11-07 12:52:59
categories: xmbc mediacenter
---
So I\'ve got this media centre, which is in reality just an \"old\" Intel motherboard, with an Intel Atom processor and 2 GB ram.
About two weeks ago, I reinstalled XBMC  on it, at first I was running an gentoo with XBMC installed on top. I really liked it, because it was an minimal installation, but with me an gentoo which I always fuck it up. But that\'s life, always updating the system when I don\'t have the time to take care of it.

So this time I installed the live cd provided  from the xbmc-live team, nice and easy to run and install. But this is not the neweste version of the xbmc media centre, but again xbmc, has a packaging team, I must say that I think that they have done a fine job of making the scripts user friendly.
But there is not any instructions on how getting started with these scripts or how to setting an build environment up.

So there is many ways to setting the build environment up, but as far I can see there is two methods to do this. The first method is to create an new user, which handles all the building of the xbmc packaging, the other way is to use your xbmc user to build and package the files.

I always ssh into my media centre, so I will assume, if you want to follow this, that you also do this.

## So here is the first way to do this

This way we create an new user, you can call it what you want, but I have called it \"buildd\". So when you have login in to your xbmc mediacenter. 
{% highlight bash %}
xbmc@XBMCLive ~: sudo adduser buildd
{% endhighlight %}

Just enter all the informations that it will ask about. This will create an user home directory in \"**/home**\", this will become our build environment.
The next we want to do is give our new user superuser access, so as I just assumes that we are on an debian based system (like the xbmc live cd is), so now we are going to modify sudoers.
{% highlight bash %}
xmbc@XBMCLive ~: sudo visudo
{% endhighlight %}
In that file, you have to be really carefull on what you are doing, because it can remove your user from the sudo list, which is not good. Bus just add this at the end of the file:
{% highlight bash %}
## lines to be added in order to build XBMC
buildd ALL=NOPASSWD: ALL
{% endhighlight %}

Be aware that this command is not secure, **it will grant root access for anybody** that is login as `buildd`. After this we are going login as the new user:
{% highlight bash %}
xbmc@XBMCLive ~: su buildd && cd
{% endhighlight %}
and do the rest from the new user. The next thing we need to do is setup a GPG key for signing the packages, which we going to build.
{% highlight bash %}
buildd@XBMCLive ~: gpg --cert-digest-algo=SHA256 --default-preference-list="h10 h8 h9 h11 s9 s8 s7 s3 z2 z3 z1 z0" --gen-key
{% endhighlight %}
This is how I have done it, you can alway take a look at [Ubuntu community GnuPrivacyGuardHowto](https://help.ubuntu.com/community/GnuPrivacyGuardHowto)

* Select it the one that is **\"RSA (sign only)\"**
* The default key size should be **2048** if not choose **2048**
* Then enter your name, e-mail, comments according to the `buildd` user
* I have not entered a password

So right about now it could be useful to install **git** and **pbuilder**, as of we are going to use them in the next section.
{% highlight bash %}
buildd@XBMCLive ~: sudo apt-get update && sudo apt-get install git-core pbuilder
{% endhighlight %}
When done installing, we can setup come directories for our build environment
{% highlight bash %}
buildd@XBMCLive ~: mkdir -vp {tmp,pbuilder/{aptcache,build,result}}
buildd@XBMCLive ~: sudo rm -rf /var/cache/pbuilder
buildd@XBMCLive ~: sudo ln -s /home/buildd/pbuilder /var/cache/pbuilder
buildd@XBMCLive ~: sudo ln -s /home/buildd/tmp /tmp/buildd
{% endhighlight %}
Now, the build environment is done, then we want to clone the `xbmc/xbmc-packaging` repository
{% highlight bash %}
buildd@XBMCLive ~: git clone git://github.com/xbmc/xbmc-packaging.git
buildd@XBMCLive ~: cd xbmc-packaging
{% endhighlight %}

Now before we can compile xbmc, we need the source for it. You can setup your own clone repository for xbmc, but there is an script that can handle that for us, so that is we want to do. This script can get the stable source or the development (unstable) source code, by setting the variable **\"--stable\"** after the script. 

To get the **development version**:

{% highlight bash %}
buildd@XBMCLive ~/xbmc-packaging: ./xbmc-get-orig-source
{% endhighlight %}

To get the **stable version**:

{% highlight bash %}
buildd@XBMCLive ~/xbmc-packaging: ./xbmc-get-orig-source --stable
{% endhighlight %}

**pbuilder** calls its compressed build environment for images, so lets us create an image. As of writing the newest version of ubuntu is not supported (oneiric), but the it is fortune that XBMC live is lucid, and that is what we are going to make packages for. The `xbmc-packaging` repository provides us with an script that handle all the pbuilder commands and etc. So to first we need to create and build image for **pbuilder**.

{% highlight bash %}
buildd@XBMCLive ~/xbmc-packaging: ./pbuilder-dist lucid create
{% endhighlight %}

This creates the \"build environment\" for us, and it is going take some time to do it. It will create an chroot environment, which is an nice way to say that is going to download a lot of packages. If you want to know more about chroot environments, you can search [google](https://www.google.com/search?client=ubuntu&channel=fs&q=chroot+environment&ie=utf-8&oe=utf-8), or take a look at the [ubuntu documentations](https://help.ubuntu.com/community/BasicChroot).
When it is done with downloading the packages an compressed the image, we now can begin to build our first package (first make sure the there is an **\*.dsc** file in the directory)

{% highlight bash %}
buildd@XBMCLive ~/xbmc-packaging: ./pbuilder-dist lucid build xbmc-11.0.dsc
{% endhighlight %}

Now this is also going to take some time, so watch some tv, drink some coffee, the, beer or cola pick your poison. Now when the script is done, the resulting package should be in the \"/home/buildd/pbuilder/results/\" directory.
This package you should be able to install using the dpkg, with the install switch.

{% highlight bash %}
buildd@XBMCLive ~/pbuilder/results: sudo dpkg -i \*.deb
{% endhighlight %}

And it now you should have the newest compiled package installed. But now you are thinking about setting up your own ubuntu repository, and this will not be coveret at this time.
The next time, I will show you how do the same but without create an new user.

### References:

* [XBMC.org forum](http://forum.xbmc.org/archive/index.php/t-60967.html)
* [Github xbmc-packaging](https://github.com/xbmc/xbmc-packaging)
