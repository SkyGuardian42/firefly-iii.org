---
layout: page
title: Installation guide
redirect_from:
  - /installation-guide/
  - /about-ff/installation-guide/
  - /about-ff/system-requirements/
  - /server-prep-ubu-16/
  - /server-prep-ubu-14/
  - /server-prep-centos/
---

## Introduction
This guide focuses on the installation of [Firefly III](https://github.com/firefly-iii/firefly-iii) only. The guide is just three steps!
Check out [the FAQ]({{ 'faq-technical.html' | absolute_url }}) when things are not working.

You can also use [Docker]({{ 'using-docker.html' | absolute_url }}), [Sandstorm]({{ 'using-sandstorm.html' | absolute_url }}), [Heroku]({{ 'using-heroku.html' | absolute_url }}), 
[Softaculous]({{ 'using-softaculous.html' | absolute_url }}) or [AMPPS]({{ 'using-ampps.html' | absolute_url }}) to run and use Firefly III.

### Prerequisites
You need a working LAMP, LEMP or WAMP stack. If you don't have one, search the web to find out how to get one. Make sure you're running PHP 7.1. There are many tutorials that will help you install one. For example:

1. [A guide to install a LAMP stack](https://www.digitalocean.com/community/tutorials/how-to-install-linux-apache-mysql-php-lamp-stack-on-ubuntu)
2. [A guide to update to PHP 7.1](https://www.digitalocean.com/community/questions/how-do-i-update-my-lamp-stack-to-php7)
3. [A guide to install PHP7.1 on a Raspberry Pi](https://raspberrypi.stackexchange.com/questions/70388/how-to-install-php-7-1)

You also need an email address and the associated password and server settings. You can safely use your own email address. If you prefer not to, you can register a free email account at [GMail](https://accounts.google.com/SignUp?service=mail) or [Outlook](https://outlook.live.com/owa/). This isn't really mandatory but it could proof useful.

If you wish to use another database such as SQLite, please check out [the FAQ]({{ 'faq-technical.html' | absolute_url }}).

### 1. Preparing your server

#### Extra packages

Install the following PHP modules:

* PHP BCMath Arbitrary Precision Mathematics
* PHP Internationalization extension
* PHP MBstring
* PHP Curl
* PHP Zip
* PHP GD

You can search the web to find out how to install these modules. Some may be installed already depending on your system.

#### Optional: Apache configuration

If you run Apache, open the `httpd.conf` or `apache2.conf` configuration file (its location differs, but it is probably in `/etc/apache2`).

Find the line that starts with `<Directory /var/www>`. If you see `/`, keep looking!

You will see the text `AllowOverride None` right below it. Change it to `AllowOverride All`.

Also run the following commands:

```
sudo a2enmod rewrite
sudo service apache2 restart
```

#### Optional: nginx configuration

If you run nginx, the configuration should be fine. You'll just have to search the web on how to run PHP7.1 in nginx. You can check out [my own nginx configuration](/static/nginx.txt).

#### Installing composer

If you have sudo rights (try `sudo ls`) you can install composer using the following command:

```
curl -sS https://getcomposer.org/installer | sudo php -- --install-dir=/usr/local/bin --filename=composer
```

Verify the installation of composer using the following command.

```
composer -v
```

##### Composer without sudo

If you have no sudo rights, you can simply download composer [here](https://getcomposer.org/download/). See the header "manual download".

This concludes the server preparations!

### 2. Installing Firefly

#### Main command

Browse to `/var/www` which is probably the directory where your web server is configured to find its files.

Enter the following command. 

```
composer create-project grumpydictator/firefly-iii --no-dev --prefer-dist firefly-iii <latest>
```

You should replace `<latest>` with the latest version, which you can find on the [Github release list](https://github.com/firefly-iii/firefly-iii/releases).

If this gives an error because of access rights, prepend the command with `sudo `. We'll fix the access rights later. The output of this command must look something like [this example](/static/installation-output.txt).

#### Configuration

In the `firefly-iii` directory you will find a `.env` file. Open this file using your favorite editor.

In the list below you will see each value in this file and what you must put there. If a value from the `.env` file is not mentioned, you should ignore it.

Field | Meaning
----- | -------
`APP_DEBUG` | Set this to true when you file a bug. It will enable detailed errors.
`APP_FORCE_SSL` | Firefly III knows if you use http or https. To force https, set this to true.
`APP_FORCE_ROOT` | Firefly III knows its own web address. If it is mistaken, fill in this field
`APP_LOG_LEVEL` | Change this to get more detailed logging.
`DB_*` | Change this to match your database settings.
`COOKIE_PATH` | Tie the cookies to a specific path if necessary
`COOKIE_DOMAIN` | Tie the cookies to a specific domain if necessary
`COOKIE_SECURE` | Only use cookies over https (off by default)
`MAIL_*` | Update this to reflect your email settings.
`MAPBOX_API_KEY` | You [need to fill this in](https://www.mapbox.com/signup/) to be able to set the location for tags.
`ANALYTICS_ID` | If you want to see analytics for your own Firefly III installation
`SITE_OWNER` | **Important** Fill in your email address.

Other fields in this file are not relevant now for the installation, but you can tweak them later.

#### Initialize the database

This step is very important, because Firefly III needs a database to work with and it will tell you whether or not your configuration is correct. Run the following command in the Firefly III directory.

```
php artisan migrate:refresh --seed
```

If this command does not work, and it says "could not open input file", do this first:

```
cd firefly-iii
```

#### Make sure the web server user has access rights.
Especially when you install Firefly III using `sudo`, the web server may not have (write) access to the Firefly III directory. To make sure that the webserver can run Firefly, run the following commands:

```
sudo chown -R www-data:www-data /var/www/firefly-iii
sudo chmod -R 775 /var/www/firefly-iii/storage
```

Now you should be able to visit [http://localhost/firefly-iii**/public**](http://localhost/firefly-iii/public) and see Firefly.

### 3. Accessing Firefly

#### Browsing to site

Browsing to the site should be easy. You should see a login screen. If you see empty pages, or "Whoops" errors, open the `.env` file again and change `APP_DEBUG` from `false` to `true`. Also change `APP_LOG_LEVEL` to `debug`. This will give you some insight in what is happening. If you don't know what to do, [open an issue](https://github.com/firefly-iii/firefly-iii/issues) and I will help you.

#### Registering an account
You cannot login yet. Click on "Register a new account" and fill in the form.

#### Your first accounts
You will be logged in automatically. Follow the instructions and you are done!

Is it not working? Check out [the FAQ]({{ 'faq-technical.html' | absolute_url }})!

