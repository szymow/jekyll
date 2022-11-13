---
layout: post
title:  "Nextcloud installation"
date:   2022-11-10 10:54:43 +0100
categories: installation nextcloud
permalink: /:year/:categories/:title.html
---

<h1>Example installation on Ubuntu 20.04 LTS</h1>

        sudo apt update
        sudo apt install apache2 mariadb-server libapache2-mod-php7.4
        sudo apt install php7.4-gd php7.4-mysql php7.4-curl php7.4-mbstring php7.4-intl
        sudo apt install php7.4-gmp php7.4-bcmath php-imagick php7.4-xml php7.4-zip

Now you need to create a database user and the database itself by using the MySQL command line interface.
The database tables will be created by Nextcloud when you login for the first time.
To start the MySQL command line mode use the following command and press the enter key when prompted
for a password:

        sudo /etc/init.d/mysql start
        sudo mysql -uroot -p

Then a MariaDB [none]> prompt will appear. Now enter the following lines, replacing username and
password with appropriate values, and confirm them with the enter key:

        CREATE USER 'username'@'localhost' IDENTIFIED BY 'password';
        CREATE DATABASE IF NOT EXISTS nextcloud CHARACTER SET utf8mb4 COLLATE
        utf8mb4_general_ci;
        GRANT ALL PRIVILEGES ON nextcloud.* TO 'username'@'localhost';
        FLUSH PRIVILEGES;

You can quit the prompt by entering:

        quit;

Now download the archive of the latest Nextcloud version:
Go to the <a href="https://nextcloud.com/install">Nextcloud Download Page</a>.
Go to Download Nextcloud Server > Download > Archive file for server owners and download
either the tar.bz2 or .zip archive.
This downloads a file named nextcloud-x.y.z.tar.bz2 or nextcloud-x.y.z.zip (where x.y.z is the version
number).
Download its corresponding checksum file, e.g. nextcloud-x.y.z.tar.bz2.md5, or nextcloud-
x.y.z.tar.bz2.sha256.
Verify the MD5 or SHA256 sum:

        md5sum -c nextcloud-x.y.z.tar.bz2.md5 < nextcloud-x.y.z.tar.bz2
        sha256sum -c nextcloud-x.y.z.tar.bz2.sha256 < nextcloud-x.y.z.tar.bz2
        md5sum -c nextcloud-x.y.z.zip.md5 < nextcloud-x.y.z.zip
        sha256sum -c nextcloud-x.y.z.zip.sha256 < nextcloud-x.y.z.zip

You may also verify the PGP signature:
        wget https://download.nextcloud.com/server/releases/nextcloud-
        x.y.z.tar.bz2.asc
        wget https://nextcloud.com/nextcloud.asc
        gpg --import nextcloud.asc
        gpg --verify nextcloud-x.y.z.tar.bz2.asc nextcloud-x.y.z.tar.bz2

Now you can extract the archive contents. Run the appropriate unpacking command for your archive
type:
        tar -xjvf nextcloud-x.y.z.tar.bz2
        unzip nextcloud-x.y.z.zip

 Copy the Nextcloud directory to its final destination. When you are running the Apache HTTP server you may safely install Nextcloud in your Apache document root:
        cp -r nextcloud /var/www

<h1>Apache Web server configuration</h1>
Configuring Apache requires the creation of a single configuration file. On Debian, Ubuntu, and their
derivatives, this file will be <b>/etc/apache2/sites-available/nextcloud.conf</b>. On Fedora, CentOS, RHEL, and similar systems, the configuration file will be <b>/etc/httpd/conf.d/nextcloud.conf</b>.

You can choose to install Nextcloud in a directory on an existing webserver, for example
https://www.example.com/nextcloud/, or in a virtual host if you want Nextcloud to be accessible from its own
subdomain such as https://cloud.example.com/.
To use the directory-based installation, put the following in your nextcloud.conf{.file .docutils .literal
.notranslate} replacing the <b>Directory</b> and <b>Alias</b> filepaths with the filepaths appropriate for your system:

        Alias /nextcloud "/var/www/nextcloud/"
        <Directory /var/www/nextcloud/>
            Require all granted
            AllowOverride All
            Options FollowSymLinks MultiViews

            <IfModule mod_dav.c>
                Dav off
            </IfModule>
        </Directory>

To use the virtual host installation, put the following in your nextcloud.conf replacing <b>ServerName</b>, as well as the <b>DocumentRoot</b> and <b>Directory</b> filepaths with values appropriate for your system:

        <VirtualHost *:80>
            DocumentRoot /var/www/nextcloud/
            ServerName your.server.com
            <Directory /var/www/nextcloud/>
                Require all granted
                AllowOverride All
                Options FollowSymLinks MultiViews
                
                <IfModule mod_dav.c>
                    Dav off
                </IfModule>
            </Directory>
        </VirtualHost>

On Debian, Ubuntu, and their derivatives, you should run the following command to enable the configuration:

        a2ensite nextcloud.conf

Additional Apache configurations
For Nextcloud to work correctly, we need the module mod_rewrite.
Enable it by running:
        a2enmod rewrite env dir mime

Now restart Apache:
        service apache2 restart

Enabling SSL
You can use Nextcloud over plain HTTP, but we strongly encourage you to use SSL/TLS to encrypt all of your
server traffic, and to protect user's logins and data in transit.
Apache installed under Ubuntu comes already set-up with a simple self-signed certificate. All you have to do
is to enable the ssl module and the default site. Open a terminal and run:

        a2enmod ssl
        a2ensite default-ssl
        service apache2 reload

Installation wizard
After restarting Apache you must complete your installation by running either the graphical Installation
Wizard, or on the command line with the occ command. To enable this, change
the ownership on your Nextcloud directories to your HTTP user:
        chown -R www-data:www-data /var/www/nextcloud/

<h1>Quick start</h1>
When Nextcloud prerequisites are fulfilled and all Nextcloud files are installed, the last step to completing the
installation is running the Installation Wizard. This is just three steps:
1. Point your Web browser to http://localhost/nextcloud
2. Enter your desired administrator's username and password.
3. Click Finish Setup.

Trusted domains
All URLs used to access your Nextcloud server must be whitelisted in your config.php file, under the trusted_domains setting. Users are allowed to log into Nextcloud only when they point their browsers to a URL that is listed in the trusted_domains setting. You may use IP addresses and domain names. A typical configuration looks like this:

        'trusted_domains' =>
        array (
        0 => 'localhost',
        1 => 'server1.example.com',
        2 => '192.168.1.50',
        3 => '[fe80::1:50]',
        ),

<h1>Extra remark for getting access to the database</h1>
Error: Host Is Not Allowed to Connect to This MySQL Server

This error occurs due to the default configuration the MySQL database is currently using. This configuration
allows connections only from the 'root' user when coming from 'localhost' and not other IP address ranges.
Resolution
Create a new administrator user or modify the root user to allow connections from 'vScopeServerIP'.
To create a new administrator user, run the following queries in a mysql prompt:

        > CREATE USER 'vScopeUserName'@'localhost' IDENTIFIED BY 'password';
        > GRANT ALL PRIVILEGES ON *.* TO 'vScopeUserName'@'localhost' WITH GRANT OPTION;
        > CREATE USER 'vScopeUserName'@'vScopeServerIP' IDENTIFIED BY 'password';
        > GRANT ALL PRIVILEGES ON *.* TO 'vScopeUserName'@'vScopeServerIP' WITH GRANT
        OPTION;
        > FLUSH PRIVILEGES;

To modify the root user (not recommended), run the following queries in a mysql prompt:

        > GRANT ALL ON *.* to root@'vScopeServerIP' IDENTIFIED BY 'password';
        > FLUSH PRIVILEGES;
