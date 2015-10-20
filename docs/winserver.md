
## Website files

The latest image with needed website files and binaries is:
AAM-win-image-version-4-2015-10-20 (AMI id:ami-ab4a18ce). It should be visible in Darko's account as I shared it with him.

What needs to be copied from there are two folders:
c:\var\website-v1 and c:\var\webiste-binaries

Also the vhost.conf file in C:\Apache24\conf is very useful as a template.

Not completely tested but based on Server Installation wiki and my Win installation notes:

Assuming that Apache and PHP are installed in C:\Apache24 and C:\php respectively

Add following lines to the bottom of c:\Apache24\conf\httpd.conf
'
LoadModule php5_module "c:/php/php5apache2_4.dll"
AddHandler application/x-httpd-php .php
 configure the path to php.ini
PHPIniDir "C:/php"
Add index.php to DirectoryIndex line, it should look like:

<IfModule dir_module>
    DirectoryIndex index.html index.php
</IfModule>
'

Uncomment:
'
LoadModule rewrite_module modules/mod_rewrite.so
LoadModule vhost_alias_module modules/mod_vhost_alias.so
Include conf/extra/httpd-vhosts.conf
'
In c:\php\php.ini uncomment the following lines:
'
extension=php_curl.dl
extension=php_openssl.dll
'


If any of the above php modules is missing it should be installed.

In httpd.conf remove the hash ‘#’ from the line

'#LoadModule rewrite_module modules/mod_rewrite.so'

Based on the vhost.conf file in the provided image you should create your own vhost.conf. The only things that should be changed are the name of the server and location of website-v1 and website-binaries folders (and appropriate subfolders).
Copy website-binaries and website-v1 folders from the shared image.

Finally edit website-v1/php/libs/setup.inc.php
The following line should point to your installation (website-v1):
define('IM_ROOT_PATH', '/var/www/website/');
Restart Apache.

At this point API access should work. If it doesn't please contact me. 

However if I am unavailable, and you are impatient you try following steps:
- Install composer:

 download composer installer using the following:
php.exe -r "readfile('https://getcomposer.org/installer');" | php.exe
After that call the installer to generate phar file. Once composer.phar file is generated successfully, you need to create a batch file to wrap it, and put it to PATH. Here's the example how that file can look like

@c:\xampp\php\php.exe c:\opt\bin\composer.phar %*
This is called in website-v1 folder.

- Install Bower: http://bower.io/

Than in website-v1 folder execute the following code:
@echo "Installing composer dependencies in php"
cd php
call composer self-update
call composer install
echo "Installing composer dependencies in zfapp"
cd ../zfapp
call composer install
@echo "==================================="
cd ../webroot/lib/

        echo "Installing bower (browser JavaScript) dependencies in webroot/lib"
        call bower install
This should take care of any missing dependencies, although those should be already resolved, so installing those tools is most likely unnecessary.

As a last step if at this point it still doesn't work you can try installing node.js:
http://blog.teamtreehouse.com/install-node-js-npm-windows

And again in website-v1 run:
call npm install
API calls example are in following repo:
https://github.com/insidemaps-org/api-demo

