# Chamilo 1.10.x

[![Build Status](https://travis-ci.org/chamilo/chamilo-lms.svg?branch=1.10.x)](https://travis-ci.org/chamilo/chamilo-lms)
[![Scrutinizer Code Quality](https://scrutinizer-ci.com/g/chamilo/chamilo-lms/badges/quality-score.png?b=1.10.x)](https://scrutinizer-ci.com/g/chamilo/chamilo-lms/?branch=1.10.x)
[![Code Coverage](https://scrutinizer-ci.com/g/chamilo/chamilo-lms/badges/coverage.png?b=1.10.x)](https://scrutinizer-ci.com/g/chamilo/chamilo-lms/?branch=1.10.x)
[![Bountysource](https://www.bountysource.com/badge/team?team_id=12439&style=raised)](https://www.bountysource.com/teams/chamilo?utm_source=chamilo&utm_medium=shield&utm_campaign=raised)
[![Code Consistency](https://squizlabs.github.io/PHP_CodeSniffer/analysis/chamilo/chamilo-lms/grade.svg)](http://squizlabs.github.io/PHP_CodeSniffer/analysis/chamilo/chamilo-lms/)

## Installation

This installation guide is for development environments only.

### Install PHP, a web server and MySQL/MariaDB

To run Chamilo, you will need at least a web server (we recommend Apache2 for commodity reasons), a database server (we recommend MariaDB but will explain MySQL for commodity reasons) and a PHP interpreter (and a series of libraries for it). If you are working on a Debian-based system (Debian, Ubuntu, Mint, etc), just
type
```
sudo apt-get install libapache2-mod-php mysql-server php5-gd php5-intl php5-curl php5-json
```

### Install Git

The development version 1.10.x requires you to have Git installed. If you are working on a Debian-based system (Debian, Ubuntu, Mint, etc), just type
```
sudo apt-get install git
```

### Install Composer

To run the development version 1.10.x, you need Composer, a libraries dependency management system that will update all the libraries you need for Chamilo to the latest available version.

Make sure you have Composer installed. If you do, you should be able to launch "composer" on the command line and have the inline help of composer show a few subcommands. If you don't, please follow the installation guide at https://getcomposer.org/download/

### Download Chamilo from GitHub

Clone the repository

```
sudo mkdir chamilo-1.10
sudo chown -R `whoami` chamilo-1.10
git clone -b 1.10.x --single-branch https://github.com/chamilo/chamilo-lms.git chamilo-1.10
```

Checkout branch 1.10.x

```
cd chamilo-1.10
git checkout --track origin/1.10.x
git config --global push.default current
```

### Update dependencies using Composer

From the Chamilo folder (in which you should be now if you followed the previous steps), launch:

```
composer update
```

If you face issues related to missing JS libraries, you might need to ensure
that your web/assets folder is completely re-generated.
Use this set of commands to do that:
```
rm composer.lock
rm -rf web/ vendor/
composer clear-cache
composer update
```
This will take several minutes in the best case scenario, but should definitely
generate the missing files.

### Change permissions

On a Debian-based system, launch:
```
sudo chown -R www-data:www-data app main/default_course_document/images main/lang  
```

### Start the installer

In your browser, load the Chamilo URL. You should be automatically redirected 
to the installer. If not, add the "main/install/index.php" suffix manually in 
your browser address bar. The rest should be a matter of simple
 OK > Next > OK > Next...

## Upgrade from 1.9.x

1.10.0 is a major version. It contains a series of new features, that
also mean a series of new database changes in regards with versions 1.9.x. As 
such, it is necessary to go through an upgrade procedure when upgrading from 
1.9.x to 1.10.x.

When we published 1.10.0, we didn't catch a series of important upgrade errors.
We released a warning a few days later asking people not to upgrade from 1.9.x
to 1.10.0 for the time being. Version 1.10.2 fixed that, and you can now
safely upgrade from any 1.9.x version to any 1.10.x version higher than 1.10.0.

The upgrade procedure is relatively straightforward. If you have a 1.9.x 
initially installed with Git, here are the steps you should follow 
(considering you are already inside the Chamilo folder):
```
git fetch --all
git checkout origin 1.10.x
```

Then load the Chamilo URL in your browser, adding "main/install/index.php" and 
follow the upgrade instructions. Select the "Upgrade from 1.9.x" button to 
proceed.

If you have previously updated database rows manually, you might face issue with
FOREIGN KEYS during the upgrade process. Please make sure your database is
consistent before upgrading. This usually means making sure that you have to delete
rows from tables referring to rows which have been deleted from the user or access_url tables.
Typically:
<pre>
    DELETE FROM access_url_rel_course WHERE access_url_id NOT IN (SELECT id FROM access_url);
</pre>

# For developers and testers only

This section is for developers only (or for people who have a good reason to use
a development version of Chamilo), in the sense that other people will not 
need to update their Chamilo portal as described here.

## Updating code

To update your code with the latest developments in the 1.10.x branch, go to
your Chamilo folder and type:
```
git pull origin 1.10.x
```
If you have made customizations to your code before the update, you will have
two options:
- abandon your changes (use "git stash" to do that)
- commit your changes locally and merge (use "git commit" and then "git pull")

You are supposed to have a reasonable understanding of Git in order to
use Chamilo as a developer, so if you feel lost, please check the Git manual
first: http://git-scm.com/documentation

## Updating your database from new code

Since the 2015-05-27, Chamilo offers the possibility to make partial database
upgrades through Doctrine migrations.

To update your database to the latest version, go to your Chamilo root folder
and type
```
php bin/doctrine.php migrations:migrate --configuration=app/config/migrations.yml
```

If you want to proceed with a single migration "step" (the steps reside in
src/Chamilo/CoreBundle/Migrations/Schema/V110/), then check the datetime of the
version and type the following (assuming you want to execute Version20150527120703)
```
php bin/doctrine.php migrations:execute 20150527120703 --up --configuration=app/config/migrations.yml
```

## Contributing

If you want to submit new features or patches to Chamilo, please follow the
Github contribution guide https://guides.github.com/activities/contributing-to-open-source/
and our CONTRIBUTING.md file.
In short, we ask you to send us Pull Requests based on a branch that you create
with this purpose into your repository forked from the original Chamilo repository.

# Documentation
For more information on Chamilo, visit https://stable.chamilo.org/documentation
