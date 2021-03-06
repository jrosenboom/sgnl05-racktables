# racktables

#### Table of Contents

1. [Overview](#overview)
2. [Module Description - What the module does and why it is useful](#module-description)
3. [Setup - The basics of getting started with racktables](#setup)
    * [What racktables affects](#what-racktables-affects)
    * [Beginning with RackTables module](#beginning-with-racktables-module)
4. [Usage - Configuration options and additional functionality](#usage)
5. [Reference - An under-the-hood peek at what the module is doing and how](#reference)
5. [Limitations - OS compatibility, etc.](#limitations)
6. [Development - Guide for contributing to the module](#development)

## Overview

This Puppet module installs the latest development version of RackTables, along with Apache, PHP and MySQL.

## Module Description

RackTables is nifty and robust solution for datacenter and server room asset management. It helps document hardware assets, network addresses, space in racks, networks configuration and much much more.

Use this module to install the latest development version of RackTables. The module will also install Apache, PHP, MySQL and some other dependencies for RackTables (php packages mostly).


WARNING:
This is not ment for production servers. Use this module on NEW servers for development or demo purposes only! 
Existing Apache, MySQL and RackTables installations might/will be overwritten.


## Setup

### What racktables affects

* Apache - Any existing config will be replaced and/or purged.
* MySQL - Existing databases and settings might get overwritten.

### Beginning with RackTables module

To install RackTables with the default parameters

```puppet
   class { '::racktables':
     secretwriteable => true,
     vhost           => 'racktables.example.com',
   }
```

As soon as puppet is done installing, go to vhost address and append ?module=installer to the URL (Example URL: http://racktables.example.com/?module=installer). From there follow the RackTables installation steps, 7 in total.

Default database settings are:
* database: racktables_db
* username: racktables_user
* password: racktables_pass

These values can be changed by adding parameters mysqldb, mysqluser and mysqluserpw to the ::racktables class (See more [examples](#examples)).

When you get to RackTables installation step 4 (of 7), remove the `secretwriteable` parameter from the ::racktables class (or set it to false). Next, re-run puppet and then finnish the rest of the installation steps.

#### Examples

Set a root password for your mysql installation (default password is "strongpassword" if undefined):

```puppet
   class { '::racktables':
     secretwriteable => true,
     mysqlrootpw     => 'change.me123XXXabc',
     vhost           => 'racktables.example.com',
   }
```

Proper installation with recommended parameters:

```puppet
   class { '::racktables':
     secretwriteable => true,
     vhost           => 'racktables.example.com',
     mysqlrootpw     => 'strongpassword123.XXXabc',
     mysqldb         => 'racktables',
     mysqluser       => 'racktables',
     mysqluserpw     => 'otherstrongpasswordXXX123XXX.abc',
     mysqlhost       => 'localhost',
   }
```

When installations done, dont forget to set secretwriteable to false (or remove the whole parameter). You can also remove the mysqlrootpw parameter, as it is now stored in /root/.my.cnf on the local server.

In other words, after installation is done your class should look like this:

```puppet
   class { '::racktables':
     vhost           => 'racktables.example.com',
     mysqldb         => 'racktables',
     mysqluser       => 'racktables',
     mysqluserpw     => 'otherstrongpasswordXXX123XXX.abc',
     mysqlhost       => 'localhost',
   }
```
## Usage

###Classes and Defined Types

####Class: `racktables`


#####`secretwriteable`

Makes the inc/secret.php file writeable for apache during setup. Set this parameter to true while installing racktables and false after installation is done. Defaults to 'false'.

#####`vhost`

The virtual host address to use for your racktables installation. Requires a valid DNS entry. Defaults to 'racktables.example.com'.

#####`mysqlrootpw`

Sets the root password on MySQL. Defaults to 'strongpassword'.

#####`mysqluser`

Sets the mysql user for the racktables database. Defaults to 'racktables_user'.

#####`mysqluserpw`

Sets the password for the "mysqluser". Defaults to 'racktables_pass'. 

#####`mysqldb`

Sets the name of the database for racktables. Defaults to 'racktables_db'.

#####`mysqlhost`

Sets the name of the database to connect to. Defaults to 'localhost'.

#####`datadir`

Specifies the installation path of RackTables. Defaults to '/usr/local/share/RackTables'.

#####`apacheuser`

Specifies the apache user. Used for setting permissions to inc/secret.php. Defaults to 'apache' for RedHat/CentOS and 'www-data' for Debian/Ubuntu.

#####`vcsprovider`

Defines what vcs system to use for downloading RackTables. Defaults to 'git'.

#####`source`

Path to RackTables source. Defaults to ''https://github.com/RackTables/racktables.git'.

## Reference

###Classes

####Public Classes

* `racktables`: Starts the module and calls racktables::install, racktables::apache and racktables::mysql

####Private Classes

* `racktables::install`: Pulls and installs RackTables from https://github.com/RackTables/racktables.git
* `racktables::apache`: Installs Apache and a vhost for Racktables
* `racktables::mysql`: Install MySQL and sets up a database for Racktables


## Limitations

###Ubuntu 12.04 and 14.04

Should work, but has not been tested

## Development

###Contributing

Please use the issue tracker (https://github.com/sgnl05/sgnl05-racktables/issues) for any type of contribution. 
