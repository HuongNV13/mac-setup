---
sidebar_position: 7
---

# PHP
macOS comes with PHP pre-installed, however, it is not a simple task to use this version with Homebrew.

The solution is to install PHP from [@shivammathur](https://github.com/shivammathur/homebrew-php).
## Installation
```bash
brew tap shivammathur/php
```
We will proceed by installing various versions of PHP and using a simple script to switch between them as we need. Feel free to exclude any versions you don't want to install.
```bash
brew install shivammathur/php/php@8.0
brew install shivammathur/php/php@8.1
brew install shivammathur/php/php@8.2
brew install shivammathur/php/php@8.3
```
Also, you may have the need to tweak configuration settings of PHP to your needs. A common thing to change is the memory settings or the date.timezone configuration. The php.ini files for each version of PHP are located in the following directories:
```
$(brew --prefix)/etc/php/8.0/php.ini
$(brew --prefix)/etc/php/8.1/php.ini
$(brew --prefix)/etc/php/8.2/php.ini
$(brew --prefix)/etc/php/8.3/php.ini
```
We have installed but **not linked** these PHP versions. To switch to PHP ```8.2``` for example we can type:
```bash
brew unlink php && brew link --overwrite --force php@8.2
```
Quick test that we're in the correct version:
```bash
php -v
```
```bash
PHP 8.2.11 (cli) (built: Sep 26 2023 11:11:58) (NTS)
Copyright (c) The PHP Group
Zend Engine v4.2.11, Copyright (c) Zend Technologies
    with Zend OPcache v8.2.11, Copyright (c), by Zend Technologies
```

## Make Apache to use PHP
Get the Homebrew prefix
```bash
echo $(brew --prefix)
```
You will again need to edit the ```$(brew --prefix)/etc/httpd/httpd.conf``` file scroll to the bottom of the LoadModule entries.
If you have been following this guide correctly, the last entry should be your ```mod_rewrite``` module:
```
LoadModule rewrite_module lib/httpd/modules/mod_rewrite.so
```
Below this add the following libphp modules:
```
#LoadModule php_module [homebrew_prefix]/opt/php@8.0/lib/httpd/modules/libphp.so
#LoadModule php_module [homebrew_prefix]/opt/php@8.1/lib/httpd/modules/libphp.so
LoadModule php_module [homebrew_prefix]/opt/php@8.2/lib/httpd/modules/libphp.so
#LoadModule php_module [homebrew_prefix]/opt/php@8.3/lib/httpd/modules/libphp.so
```
We can only have one module processing PHP at a time, so for now, so we have left our ```php@8.2``` entry uncommented while all the others are commented out. This will tell Apache to use PHP 8.2 to handle PHP requests.
Also you must set the Directory Indexes for PHP explicitly, so search for this block:
```
<IfModule dir_module>
    DirectoryIndex index.html
</IfModule>
```
and replace it with this:
```
<IfModule dir_module>
    DirectoryIndex index.php index.html
</IfModule>

<FilesMatch \.php$>
    SetHandler application/x-httpd-php
</FilesMatch>
```
**Save** the file and **stop Apache then start again**, now that we have installed PHP:
```bash
brew services stop httpd
brew services start httpd
```
## Validating PHP Installation
Simply create a file called ```info.php``` in your ```Sites/``` folder you created earlier with this one-liner.
```bash
echo "<?php phpinfo();" > ~/Sites/info.php
```
Point your browser to [http://localhost/info.php](http://localhost/info.php) and you should see a shiny PHP information page:
If you see a similar **phpinfo** result, congratulations! You now have Apache and PHP running successfully. You can test the other PHP versions by commenting the ```LoadModule ... php@8.2 ...``` entry and uncommenting one of the other ones. Then simply restart apache and reload the same page.