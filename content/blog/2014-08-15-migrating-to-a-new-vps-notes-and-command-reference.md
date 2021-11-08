---
title:  "Migrating to a new VPS - notes and command reference"
date:   2014-08-15 15:40:30 +0530
tags: ["ops"]
---
I tried to setup this site on my VPS late at night. Sleepiness lead to stupidity and I ended up breaking various things on my site, that I couldn't quite restore. Frustration eventually gave way, and I decided to set up a fresh VPS and finally make the move to nginx webserver. Previously, Apache2 tweaked for low memory was used, but it was still poor.

VPS migration can be fast and easy when done properly. This was my second time, but I didn't save links or references from last time, so I had to figure out a lot of things by trial and error and reading up all over again. I'm writing this for future reference (when I inevitably screw up things :P ), but here's hoping others will find it useful too. Feel free to ask any questions, I'll try to help.

### Setting up the new VPS
I'm using a Debian 7 'Wheezy' 32 bit 512 MB VPS. <strong>Why Debian?</strong> It has a lower footprint than Ubuntu, and is suitable for low memory VPS.

<strong>Why nginx?</strong> nginx is known to be fast and capable of more concurrent connections. In the rare occasions when traffic spikes, I do not want to be caught off guard. WordPress works well with nginx now, so there's no real reason to stay on Apache. 

<strong>Looking for an affordable VPS provider?</strong> I set this up on a [Digital Ocean](https://www.digitalocean.com/?refcode=7f10667e7318) VPS. If you sign up with my affiliate link, I'll get a couple of months of free hosting, but I'd recommend them even without that. They're awesome (so far).

They are one of the fastest growing cloud VPS providers, and start at $5. They also have detailed tutorials, which I found helpful. Affordable and decent support, and they're active on [Twitter](https://twitter.com/digitalocean) too. They have an API, and many developers have made apps for iOS and Android.

Chris Black has a detailed tutorial to [setup LEMP stack](http://thatchrisblack.com/2013/06/wordpress-debian-wheezy-nginx-and-you/) (nginx, PHP, MySQL) and WordPress.

Alternatively, follow one of the [guides at Digital Ocean](https://www.digitalocean.com/community/articles/initial-server-setup-with-ubuntu-12-04) for a different combination of Linux flavor and software stack. The instructions aren't tailored for Digital Ocean - they work well with any VPS.

Now, follow the SFTP setup from [this article](https://www.digitalocean.com/community/articles/how-to-launch-your-site-on-a-new-ubuntu-12-04-server-with-lamp-sftp-and-dns). That link is for Ubuntu 12.04, but it works for Debian 7 too.

Next, <a href="https://www.digitalocean.com/community/articles/how-to-set-up-ssh-keys--2">set up SSH keys</a> which is a more secure and convenient authentication than passwords. Also disable root login and passwords by editing /etc/ssh/sshd_config (PermitRootLogin no).

I started [password protecting wp-admin](http://weavervsworld.com/docs/other/passprotect.html) (with htpasswd) to keep out hacks thru frequent plugin vulnerabilities and wp-admin. Haven't had a hack since, <em>touchwood</em>.

### Migrating data
You can use either the powerful rsync, or move things by hand. I prefer the latter because of fine grained control. Also, I did not need some of the data from the old VPS, so a manual move made sense. But this is not as manual as you'd think - a few commands and you're done.

Backup [databases with mysqldump](http://www.thegeekstuff.com/2008/09/backup-and-restore-mysql-database-using-mysqldump/) on your old VPS.

To migrate files, first create a tar file (which is a container). Keep adding files to it, and when you're done, gzip it ([tar reference](http://www.tecmint.com/18-tar-command-examples-in-linux/), [gzip reference](http://www.computerhope.com/unix/gzip.htm))

Here's how I did it: starting from domain root, add all folders. So, my tarball contained , etc. Don't forget to include the previously created MySQL dump.

Transfer the archive to new VPS, move it to the domain root and unzip it. Directory structure will be intact, so you don't have to change anything.

Move the archive from old VPS to new VPS quickly with secure copy over SSH, aka SCP ([reference](http://www.ozpersia.net/blog/?p=20)).