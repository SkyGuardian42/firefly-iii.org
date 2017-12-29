---
layout: page
title: FAQ about installing and updating
redirect_from:
  - /installation-guide-faq/
  - /faq/
---

### Can I use it on PHP 5.x?

No. Most code has been written specifically for PHP 7.1 and higher.

### Can I use SQLite, instead of MySQL?

Yes. When you are using sqlite, the following parameters are enough to get it working (the `.env` file):

```
DB_CONNECTION=sqlite
DB_HOST=127.0.0.1
DB_PORT=3306
DB_USERNAME=homestead
DB_PASSWORD=
```

Even the host and port are technically not necessary. To get it to work, the following file must exist: `/storage/database/database.sqlite`.

You can create this file by simply running:

```
touch ./storage/database/database.sqlite
```

From your Firefly III directory.

Then, you can initialise your database by running:

```
php artisan migrate:refresh --seed
```

### Decimal points are missing, numbers are off, stuff like that

Ensure with `dpkg-reconfigure locales` that the language you want to use is installed, then reboot Apache or Nginx (webserver).

In order to make the demo site work (it's an Ubuntu server) I run these commands:

* `sudo apt-get install -y language-pack-en-base`
* `sudo apt-get install -y language-pack-nl-base`
* `sudo apt-get install -y language-pack-de-base`
* `sudo apt-get install -y language-pack-pt-base`

That should take care of most issues.

### 404 when trying to visit login page or other pages.

1. Run `sudo a2enmod rewrite`. Restart Apache.
2. Check if the database credentials in de `.env` file are correct.
3. Open your Apache config file. Find `<Directory /var/www>` (or similar). Change `AllowOverride None` to `AllowOverride All`. Restart Apache.

### 500 errors, logs are empty

If the logs are empty (``storage/logs``) Firefly can't write to them. See above for the commands. If the logs still remain empty, do you have a the ``vendor`` in your Firefly root? If not, run the Composer commands.

### Unexpected question mark

```
PHP Parse error:  syntax error, unexpected '?' in 
app/Support/Twig/General.php on line 103
```

Firefly III requires PHP 7.1 or higher.

### BCMath

```
PHP message: PHP Fatal error: Call to undefined function 
FireflyIII\Http\Controllers\bcscale() in
firefly-iii/app/Http/Controllers/HomeController.php on line 76
```

Solution: you haven't enabled or installed the BCMath module.

### intl

Errors such as these:

```
production.ERROR: exception 
'Symfony\Component\Debug\Exception\FatalErrorException' with message
'Call to undefined function FireflyIII\Http\Controllers\numfmt_create()'
in firefly-iii/app/Http/Controllers/Controller.php:55
```

Solution: You haven't enabled or installed the Internationalization extension.

If you are running FreeBSD, install ``pecl-intl``.

### I get weird Javascript errors

If you have installed the javascript-common package, please remove it. It overrides your Apache configuration and breaks Firefly III.

### Error: Call to undefined function ctype_alpha()

This may happen when you are on a NAS4free Debian installation or similar platform. This command may help:

```
pkg install php71-ctype
```

### Error: could not open input file artisan

Make sure you run the artisan commands in the `firefly-iii` directory.

### Error: call to undefined function numfmt_create()

Make sure you have installed and enabled the PHP intl extension.

### I have another question!

Please [open a ticket on Github](https://github.com/firefly-iii/firefly-iii).