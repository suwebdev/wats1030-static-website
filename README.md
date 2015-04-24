# wats1030-static-website
An assignment repository that provides a simple static website students can deploy using a static file server.

In this project we will add the Apache server to our Droplet and point a subdomain to our hosted app. We will host a static website for the game [2048](https://github.com/gabrielecirulli/2048), which can be played in any HTML5 capable web browser (both desktop and mobile). The point here is not to recreate the game or to code any new elements of the site. We are simply deploying this site at a location of our choosing.

## Basic Requirements

* Set up Apache on your Droplet.
* Clone this repository to a location you can make accessible via Apache.
* Configure Apache to serve this site.

Follow these directions for the easiest way to get up and running with a custom subdomain on a Droplet. 

### Create Droplet
Digital Ocean allows you to select the operating system and the "app" that a Droplet is running when you create a new Droplet. Select these options when you make your Droplet:

* Ubuntu 14.04
* LAMP App (click on the Apps tab to see the list of apps you can choose)

(You can pick any region and use any name.)

Once you've configured your Droplet, you should login using the `ssh root@droplet.ip.address`. You will see the following message:

```
-------------------------------------------------------------------------------------
Thank you for using DigitalOcean's LAMP Application.
Your web root is located at /var/www/html and can be seen from http://droplet.ip.address/
The details of your PHP installation can be seen at http://droplet.ip.address/info.php
Your MySQL root user's password is * * * * * * * * * * *
You are encouraged to run mysql_secure_installation to ready your server for production.
-------------------------------------------------------------------------------------
```

You have actually installed PHP and mySQL in addition to Apache on this server. (We aren't using those technologies now.) You also have access to all the stuff that comes with Ubuntu.

### Install Git
This is a brand new server. You will need to install Git via apt-get:

`apt-get install git`

Since you do not plan to do any editing on the repository we are cloning, you don't actually need to configure Git. You also don't need to add your SSH key. However, since you're not trading keys between Github and your Droplet, you will need to use the HTTP clone URL when you clone repositories. You can switch to this on Github.

### Clone Website Code
This repository provides the code you will deploy on your Droplet. You should change directory (`cd`) into the "web root" of your Droplet. By default, as indicated in the message above, that is located at `/var/www/html`. This location is standard for the Digital Ocean configuration of this Droplet. Other hosting services may vary somewhat, but they will generally all still define some location as the "web root", which is where Apache is set up to look for files by default.

Clone the repository into your web root with the following command:

`git clone https://github.com/suwebdev/wats1030-static-website.git`

*Please note that you don't need to fork this repository to clone the code, so you can use that exact line of code. If you wish to make modifications to the repository, then you will need to set up Git fully and add your Droplet's SSH Key to your Github account, then fork this repository and clone your personal fork to your Droplet.*

Once you have the repo cloned, you should be able to view your files at:

[http://your.droplet.ip/wats1030-static-website/2048/](http://your.droplet.ip/wats1030-static-website/2048/)

You may now waste some time playing 2048. Come back and complete this assignment after you get the hang of it.

### Configure Subdomain
You now need to configure your subdomain to point to your game (and that means making changes in your hosting service and in your Apache configuration). Remember&mdash;Apache is the application making your website available on the web, and it is serving your site at the IP address of your Droplet. 

To configure your subdomain, go to your domain registrar and create a new subdomain. When configuring the subdomain, you want to set the CNAME for the subdomain to point to the IP address of your Droplet. This is similar to what you did to configure your main domain to point to your Github profile pages. Exactly how to accomplish this will vary depending on your domain registrar. Consult the support pages for your domain registrar to find out how to do this.

*NOTE:* Sometimes the domain registrar may force you to create an A record since you only have a numeric address. That's fine, and will also work. Again, consult your registrar's support documentation for details and reach out to your instructor for help.

**Test your subdomain** by going to `http://sub.yourdomain.com` in your web browser. You should see an Ubuntu Apache default page telling you about your Apache installation. 

Once you've set up the subdomain with a CNAME pointing to the IP address of your Droplet, you will need to modify the Apache configuration that was just created for you. This is a simple change: You must add a "virtual host" configuration for your chosen subdomain. Follow the [Digital Ocean Support instructions to configure Apache virtual hosts](https://www.digitalocean.com/community/tutorials/how-to-set-up-apache-virtual-hosts-on-ubuntu-14-04-lts/#step-four-—-create-new-virtual-host-files) to accomplish this. Your site configuration file should look something like this:

```
<VirtualHost *:80>
    ServerAdmin you@example.com
    ServerName sub.yourdomain.com
    DocumentRoot /var/www/wats1030-static-website/2048
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

Don't forget to run the commands specified in the Digital Ocean support document:

```
root@static-site:/var/www/html# sudo a2ensite sub.yourdomain.com.conf
Enabling site sub.yourdomain.com.
To activate the new configuration, you need to run:
  service apache2 reload
```

As the feedback instructs, reload Apache so your changes take effect:

```
root@static-site:/var/www/html# service apache2 reload
 * Reloading web server apache2                                                                                             *
```

You should now be able to see the 2048 game at [http://sub.yourdomain.com](http://sub.yourdomain.com).

## Stretch Requirements

* Fulfill the basic requirements, but using nginx. You can even configure an additional subdomain so that you have the Apache and nginx versions running side by side.
* Deploy another (or two or three more!) apps on your Droplet. (Here is a [list of open source HTML5 games](https://github.com/leereilly/games) you can install.)
* Create an HTML page serving as a homepage for your arcade and host it on your Droplet at a unique subdomain.
* Deploy some of your other standout static HTML pages (from WATS1010 or 1020, perhaps) at a unique subdomain.
