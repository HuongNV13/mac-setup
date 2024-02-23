---
sidebar_position: 6
---

# Apache
macOS comes with Apache pre-installed, however, it is no longer a simple task to use this version with Homebrew because Apple has removed some required scripts in this release.

The solution is to install Apache via Homebrew and then configure it to run on the standard ports (80/443).
## Installation
If you already have the built-in Apache running, it will need to be shutdown first, and any auto-loading scripts removed. It really doesn't hurt to just run all these commands in order - even if it's a fresh installation:
```bash
sudo apachectl -k stop
sudo launchctl unload -w /System/Library/LaunchDaemons/org.apache.httpd.plist 2 > /dev/null
```
Install the version provided by Homebrew:
```bash
brew install httpd
```
Start our new Apache server:
```bash
brew services start httpd
```
You now have installed Homebrew's Apache, and configured it to auto-start with a privileged account. It should already be running, so you can try to reach your server in a browser by pointing it at [http://localhost:8080](http://localhost:8080), you should see a simple header that says **"It works!"**.

Some useful commands
```bash
brew services stop httpd
brew services start httpd
brew services restart httpd
```

## Configuration
In the latest version of Homebrew Apache, you have to manually set the listen port from the default of 8080 to 80, so we will need to edit Apache's configuration file ```httpd.conf```.

You can usse **Visual Studio Code** to edit the file
```bash
code $(brew --prefix)/etc/httpd/httpd.conf
```
Find the line that says
```
Listen 8080
```
and change it to ```80```:
```
Listen 80
```
Search for the term ```DocumentRoot``` and change this to point to your user directory where your_user is the name of your user account:
```
DocumentRoot "/Users/your_user/Sites"
```
You also need to change the ```<Directory>``` tag reference right below the DocumentRoot line. This should also be changed to point to your new document root also:
```
<Directory "/Users/your_user/Sites">
```
In that same ```<Directory>``` block you will find an ```AllowOverride``` setting, this should be changed as follows:
```
#
# AllowOverride controls what directives may be placed in .htaccess files.
# It can be "All", "None", or any combination of the keywords:
#   AllowOverride FileInfo AuthConfig Limit
#
AllowOverride All
```
Also we should now enable mod_rewrite which is commented out by default. Search for mod_rewrite.so and uncomment the line by removing the leading # by pushing ⌘ + / on the line (this is a quick way to uncomment and comment a single or multiple lines:
```
LoadModule rewrite_module lib/httpd/modules/mod_rewrite.so
```
Change these to match your user account (replace your_user with your real username), with a group of staff:
```
User your_user
Group staff
```
## Sites Folder
You need to create a ```Sites``` folder in the root of your home directory. You can do this in your terminal, or in Finder. In this new ```Sites``` folder create a simple ```index.html``` and put some dummy content in it like: ```<h1>My User Web Root</h1>```.
```bash
mkdir ~/Sites
echo "<h1>My User Web Root</h1>" > ~/Sites/index.html
```
Restart Apache to ensure your configuration changes have taken effect:
```bash
brew services stop httpd
brew services start httpd
```
Pointing your browser to [http://localhost](http://localhost/) should display your new message. If you have that working, we can move on!