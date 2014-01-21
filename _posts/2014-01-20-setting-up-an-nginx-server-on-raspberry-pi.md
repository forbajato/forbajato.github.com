---
layout: post
title: "Setting up an Nginx server on Raspberry Pi"
description: "Notes on how to set up a RPi as a webserver using nginx."
summary: "Got a new raspberry pi for Christmas, need to learn webserver deployment in a safe environment.  Seems like a match made in geek heaven."
category: "Lessons"
tags: [raspberrypi, nginx, sysadmin, diy]
---
{% include JB/setup %}

#Introduction

I don't know how he knew I would love this but my nephew got me a raspberry pi for Christmas.  I have been working on my VPS and want to deploy a webserver on it but am a little reticent to deploy to the Internet without experimenting in a safe environment first.  The RPi solves this problem very neatly - just install Raspbian, fire up a package manager to install nginx and off we go!

#Goals

1. Install and configure Raspbian.
1. Install nginx webserver on RPi.
1. Set up RPi server to host static pages blog.
1. Set up local computer to deploy static pages blog to RPi server.

#Install and configure Raspbian

Start by downloading the Raspbian distro from [here](http://www.raspberrypi.org/downloads).  Page down until you see the Raspbian section, it has a link for both a direct download of the image file and a link for the torrent.  I went with the torrent so I could help seed the distro afterwards (gotta be a good netizen!).  If you are a total newbie to Linux and Raspberry Pi you may want to go with the NOOBS install - it will enable you to choose which distro to install and even change it later.  I know my VPS, which I am trying to model, runs Ubuntu so I don't need to mess around with the other versions of the RPi operating systems and went straight with the Raspbian.

Get your SD card ready - I used gparted to clear out an 8G SD card, you will need at least a 4G card to install Raspbian and have something workable to run it afterwards.

Flash the SD card with the Raspbian image.  This is simple in Linux:

`$sudo dd bs=4M if=./raspbian.img of=/dev/location_of_the_sd_card`

Replace the `./raspbian.img` with the path to and name of the image file, replace `location_of_the_sd_card` with the spot where the SD card is in your system.  In Linux you can find this by plugging in the SD card and then doing `dmesg` in the terminal, it will show you where the SD card went in your file system (for me it was `/dev/mmcblk0`).  This process may take a minute or two.  When it is finished you are ready to fire up your RPi.

Insert the newly flashed SD card into the RPi, plug in the video (either using composite to the TV or the HDMI cable), a keyboard and (ideally) an ethernet cable or wireless USB card.  Finally plug in the power supply to boot up the RPi.

On first boot Raspbian will run raspi-config, a tool to get things up and running quickly.  Your user name will be `pi` and the password will be `raspberry`, this user has sudo access.

First you will want to expand the filesystem.  When you flashed the SD card you only installed a boot and system partition, there may well be lots of SD card that is not being used, you don't want it going to waste!  

Next you can work through the various options on the rasp-config screen - reseting the password (the pi user has admin access so you will not want to leave that user with the default password if this thing is going to be exposed to the Internet), setting up networking, etc.  There are even options to set the machine to boot to an X server or directly to Scratch - I will have to come back an play with those sometime.  One thing you want to be sure to set up is the SSH server, once that is done you can administer the RPi from any with an ssh client - i.e. you won't need the mouse, keyboard and monitor (or TV in my case) anymore.

After everything is set up the first thing to do is to update the sources so you can get to installing things.

`$sudo apt-get update`

#Install nginx webserver on RPi.

To use this as a webserver we will need to install one, there are several choices but we are only hosting static pages on this so nginx is a good choice.  Nginx seems pretty simple without too much to get it up and running yet hefty enough to handle huge loads without blinking.

First install the server:

`$sudo apt-get install nginx`

If you installed from the Raspbian repos you already have the www-data user and www-data group.  To be sure (or if you installed in another way):

    $sudo useradd www-data
    $sudo groupadd www-data
    $sudo usermod -g www-data www-data

Now you need to check the nginx configuration files - I made a copy before I started messing with this, always a good idea to have a back up just in case you do something ... ill-advised (aka, "stupid").  The first config file to adjust is /etc/nginx/sites-enabled/default.  If you are not using IPV6 you will need to comment out (add a `#` sign to the beginning of the line) the line that looks like:

    listen   [::]:80 default_server ipv6only=on;

My file started with both this and the line above it commented out, I uncommented (removed the `#`) the line above it `listen 80;` so that the server knew where to listen.

Next you will need to set up where you are going to put your website files.  I plan to use a subdirectory of the pi user `/srv/web` but nginx comes ready to use `/usr/share/nginx/www`, I am not sure of the advantage of this approach, will look into it and may end up changing my set up in the future.  For now we are just wanting to get things running so you need to make a change to the default file to add the following line:

    root /home/pi/srv/web;

You will also want to comment out (put a `#` at the beginning) the line that says:

    root /usr/share/nginx/www;

Now you need some stuff in /var/www and you need to be sure that directory belongs to the proper user.  First make the directory:

    $cd /var
    $sudo mkdir www
    $sudo chown www-data:www-data www/

Now you need something in that directory to serve so you can test things out.  Fire up a text editor and write some simple HTML, save it as `index.html` in the `/var/www` directory.  Finally start up the nginx server:

    $sudo service nginx start

If you don't already know the RPi's IP address issue this at the command line:

    $ifconfig

In the block eth0 (if you have an ethernet cable connected to the RPi) or the wlan0 (if your RPi is using wireless) will have an IP address, you will use that to test the server.  From another computer on your network simply point the browser at the IP address of your RPi and you should see the webpage you just created.  If so, so far so good!

Much thanks to the tutorial [here](http://en.joscandreu.com/blog/install-nginx-on-raspberry-pi/) which is a great resource for doing this little project.  Also the [Linode](https://library.linode.com/web-servers/nginx/installation/debian-6-squeeze#sph_installing-nginx-from-debian-packages) documentation was invaluable.

#Deploying the pages

Now you have your server set up, all it needs is some content.  I fiddled around with using git to publish (like I do to post to github) but it turns out to be a bit complicated.  Turns out good 'ole rsync can send files over to the RPi just fine with a simple

    $rsync -avx "_site/" pi@rpi_ip_address:/home/pi/srv/web/

You can even automated it with rake by setting a new rake task.  All you have to do is edit your Rakefile (I have one for making new posts, pages, etc. in my git repo directory where the source files for the static blog live) to include the following (doesn't seem to matter where you put it in the file):

    desc "rsync the contents of _site directory to the raspberry pi"
    task :syncpi do
	     puts '* Publishing files to RaspberryPi'
		  puts `rsync -avz "_site/" pi@rpi_ip_address:/home/pi/srv/web/`
    end

Those are backticks around the rsync command.  Now to post your blog all you have to do is fire up Jekyll's server, let it make the `_site/` directory, then do a `rake syncpi` to send it to the RPi for hosting.

Thanks to [this blogger](http://arjanvandergaag.nl/blog/publishing-a-jekyll-website-to-a-server.html) for help with the rsync and rake file tips on posting.


