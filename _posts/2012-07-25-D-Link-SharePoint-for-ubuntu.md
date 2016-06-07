---
title : D-Link SharePoint for Ubuntu
layout : post
---
So I bought a new router and this one is a D-Link DIR-657 and it has one has the abillity to have a usb printer or harddisk connected to it. Something I would like to use, but there is a problem I'm using Linux, since there is no software for it from D-link and no help from D-link (non that I could find), I began to search for an answer to get it working on Ubuntu.

I first found an forum post, that tried using FTP to access it, and from what I could read, they could'nt, but they had found some other post about it [Link 1](https://groups.google.com/forum/?fromgroups#!topic/linux.debian.user/Rof9rW0vg44) [Link 2](http://www.mail-archive.com/debian-user@lists.debian.org/msg557452.html). The second link gave the most answer, it seams that the protocol used by the SharePoint software, is supported by smbclient and therefor can be connected to it by using <b>smbclient</b>.

First I tried to connect to it as the second link says

```bash
smbclient -L 192.168.0.1
```

But I got this reply from the router

```bash
Domain=[WORKGROUP] OS=[Unix] Server=[Samba 3.0.24]Server requested LANMAN password (share-level security) but 'client lanman auth = no' or 'client ntlmv2 auth = yes'tree connect failed: NT_STATUS_ACCESS_DENIED
```
So what to do other than read some documentation on smbclient, I found that by adding "<b>-U</b>" you could connect to is as an user, so I tried this also

```bash
smbclient -L 192.168.0.1 -U admin
```

But I got the same reply as before, then back to the documentation, found that by adding an "<b>-N</b>" instead of an "<b>-U</b>" you could specify that no user, that had to be tried

```bash
smbclient -L 192.168.0.1 -N
```

And success, I got the list of devices on connected to it

```bash
Domain=[WORKGROUP] OS=[Unix] Server=[Samba 3.0.24]

Sharename       Type      Comment
---------       ----      -------
usb(A1)         Disk      Device(us) (LaCie,LaCie Device)
IPC$            IPC       IPC Service (D-Link DIR-657)
Domain=[WORKGROUP] OS=[Unix] Server=[Samba 3.0.24]

Server               Comment
---------            -------

Workgroup            Master
---------            -------

```

So now I wanted to connect to it, after installing cifs-utils (<b>sudo apt-get install cifs-utils</b>), I could mount it in a folder in my home directory.

```bash
mkdir $HOME/SharePoint
sudo mount.cifs //192.168.0.1/usb\(A1\) $HOME/SharePoint/
```

Then so I added an file, and unmounted the directory looked at it, no file, mounted it again and the file was there, so It worked.
Now I would like to have it mounted at boot, then we need to add it to the <b>fstab</b> file. Since I can't remember how to do this, I found a site that contained this information [Link 3](http://opensuse.swerdna.org/susesambacifs.html)

```fstab
# Network hdd on D-Link
//192.168.0.1/usb\(A1\) /home/kaalund/SharePoint  cifs _netdev 0 0
```

Next to mount it, one could either reboot or use the mount command "<b>sudo mount -a</b>".
So whats next, that somebody add a scripts that can do this from nautilus when a device like this on the network. But I don't think it is going to be me.
