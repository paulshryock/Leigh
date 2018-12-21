# Leigh
> Start local development from scratch without reinventing the wheel

<!-- ## Quick Start -->

## Table of Contents

- Operating System
- Server
- DNS
- Database
- Database Management
- Programming Language
- Version Control
- Package Management
- Cloud Storage
- Symbolic Links

## Setup Instructions

### Operating System

TODO: Add Operating System content

- MacOS
	- Mojave
	- High Sierra
- Windows
	- 10
	- 7
- Linux
	- CentOS
	- Ubuntu

### Server
- [Apache](https://httpd.apache.org/docs/)
	1. Stop and unload pre-installed Apache (MacOS)
	
	    ```shell
	    $ sudo apachectl stop
	    $ sudo launchctl unload -w /System/Library/LaunchDaemons/org.apache.httpd.plist 2>/dev/null
	    ```
	    
	1. Install and start Apache
		- via Homebrew ([MacOS](https://getgrav.org/blog/macos-mojave-apache-multiple-php-versions))
	
		    ```shell
		    $ brew install httpd
		    $ sudo brew services start httpd
		    ```
	    
		- [Download Latest Version](http://httpd.apache.org/download.cgi), then Install Apache ([Windows](https://httpd.apache.org/docs/2.4/platform/windows.html))
			- Install as a service
		
			    ```shell
			    $ httpd.exe -k install
			    ```
		    
		- [Download Latest Version](http://httpd.apache.org/download.cgi), then Install Apache ([Linux](https://httpd.apache.org/docs/2.4/install.html))
		- Visit `http://localhost:8080` and see "**It works!**"
	1. Configure Apache, add Virtual Hosts and SSL
		- Edit Apache Config File: `/conf/httpd.conf`
			- Add `Define localhost_location "path/to/folder"`
			- Replace `Listen 8080` with `Listen 80`
			- Uncomment `LoadModule socache_shmcb_module lib/httpd/modules/mod_socache_shmcb.so`
			- Uncomment `LoadModule brotli_module lib/httpd/modules/mod_brotli.so`
			- Uncomment `LoadModule ssl_module lib/httpd/modules/mod_ssl.so`
			- Uncomment `LoadModule http2_module lib/httpd/modules/mod_http2.so`
			- Uncomment `LoadModule vhost_alias_module lib/httpd/modules/mod_vhost_alias.so`
			- Uncomment `LoadModule rewrite_module lib/httpd/modules/mod_rewrite.so`
			- Replace `User` with local User name
			- Replace `Group` with local Group name
				- `Group staff` (MacOS)
			- Replace `#ServerName www.example.com:8080` with `ServerName localhost`
			- Update `DocumentRoot` and `<Directory>` locations with `"${localhost_location}"`
			- Replace `AllowOverride None` with `AllowOverride All`
			- Find and replace:

				```shell
				<IfModule dir_module>
					DirectoryIndex index.html
				</IfModule>
				```

				with:

				```shell
				<IfModule dir_module>
					DirectoryIndex index.php index.html
				</IfModule>

				<FilesMatch \.php$>
					SetHandler application/x-httpd-php
				</FilesMatch>
				```

			- Uncomment `Include /usr/local/etc/httpd/extra/httpd-vhosts.conf`
			- Uncomment `Include /usr/local/etc/httpd/extra/httpd-ssl.conf`
		- Edit Virtual Hosts Config File: `/conf/extra/httpd-vhosts.conf`

		    ```shell
		    # Replace Your_Domain_Name with your domain (i.e. example.test)
		    # Replace Project_Name with your project folder name
		    <VirtualHost *:80>
			    DocumentRoot "${localhost_location}"
			    ServerName localhost
			    <Directory "${localhost_location}">
			        Require all granted
			    </Directory>
		    </VirtualHost>
		    
		    <VirtualHost *:80>
			    DocumentRoot "${localhost_location}/Project_Name"
			    ServerName Your_Domain_Name
			    <Directory "${localhost_location}/Project_Name">
			        Require all granted
			    </Directory>
			</VirtualHost>  
	    
		    <VirtualHost *:443>
			    Protocols h2 http/1.1
			    DocumentRoot "${localhost_location}/Project_Name"
			    ServerName Your_Domain_Name
			    <Directory "${localhost_location}/Project_Name">
			        Require all granted
			    </Directory>
			    SSLEngine on
			    SSLCertificateFile "${localhost_location}/Project_Name/certs/server.crt"
			    SSLCertificateKeyFile "${localhost_location}/Project_Name/certs/server.key"
		    </VirtualHost>
		    ```

		- Generate SSL Certificate files

		    ```shell
		    # Replace Your_Domain_Name with your domain (i.e. example.test)
		    # Replace Project_Name with your project folder name
		    cd Project_Name
		    mkdir certs
		    cd certs
		    openssl req -x509 -out server.crt -keyout server.key \
			  -newkey rsa:2048 -nodes -sha256 \
			  -subj '/CN=Your_Domain_Name' -extensions EXT -config <( \
			   printf "[dn]\nCN=Your_Domain_Name\n[req]\ndistinguished_name = dn\n[EXT]\nsubjectAltName=DNS:Your_Domain_Name\nkeyUsage=digitalSignature\nextendedKeyUsage=serverAuth")
		    ```

		- Edit SSL Config File: `/conf/extra/httpd-ssl.conf`
			- Find/replace `8443` with `443`
	1. Control Apache (MacOS, Linux)
	
	    ```shell
	    sudo apachectl start
	    sudo apachectl stop
	    sudo apachectl -k restart
	    ```
	1. Visit `http://localhost`
- NginX

TODO: Add NginX content

### DNS

#### hosts file

- Open `hosts` file and add local websites

    ```shell
    127.0.0.1 localhost
    127.0.0.1 example.com
    127.0.0.1 www.example.com
    ```

##### hosts file location

- MacOS: `/etc/hosts`
- Windows: `C:\Windows\system32\drivers\etc\hosts`
- Linux: `/etc/hosts`

#### Local DNS Masking

- [dnsmasq](http://www.thekelleys.org.uk/dnsmasq/doc.html) (MacOS and Linux)
	- [Configure dnsmasq](https://askubuntu.com/a/743051)
	
		```shell
		brew install dnsmasq
		echo 'address=/.test/127.0.0.1' > /usr/local/etc/dnsmasq.conf
		sudo brew services start dnsmasq
		sudo mkdir -v /etc/resolver
		sudo bash -c 'echo "nameserver 127.0.0.1" > /etc/resolver/test'
		```
	
- [Acrylic DNS Proxy](https://stackoverflow.com/questions/138162/wildcards-in-a-windows-hosts-file?answertab=votes#tab-top) (Windows)
	- [Configure Acrylic](https://www.orbitale.io/2017/12/05/setup-a-dnsmasq-equivalent-on-windows-with-acrylic.html)
- [DNSAgent](https://github.com/stackia/DNSAgent)

#### DNS Flushing

1. Flush DNS
	- MacOS: `sudo killall -HUP mDNSResponder`
	- Windows 7: `ipconfig /flushdns`
1. Clear browser cache

### Database
  - MySQL
  - MariaDB
	  - Install MariaDB
	  
	  ```shell
	  brew update
	  brew install mariadb
	  brew services start mariadb
	  ```
	  
	  To stop the server:
	  
	  ```shell
	  brew services stop mariadb
	  ```
	  
  - SQLite
  - PostgreSQL

#### Database Management
  - Sequel Pro
	  - [Download SequelPro](http://www.sequelpro.com/)
  - Heidi SQL
  - [OmniDB](https://www.omnidb.org/en/)
  - [phpMyAdmin](https://www.phpmyadmin.net/)

### Programming Language

#### PHP
- Install PHP
    - Install XCode Command Line Tools
    
	    ```shell
	    xcode-select --install
	    ```

    - Install Homebrew (see below)
    - Install Mojave Required Libraries (MacOS Mojave)
    
        ```shell
        $ brew install openldap libiconv

        ```
    - Install Apache (see above)
    - Install PHP
    
	    ```shell
	    $ brew install php@5.6
	    $ brew install php@7.0
	    $ brew install php@7.1
	    $ brew install php@7.2
	    ```
	    
		- TODO: See if this also works:
	    
    
		    ```shell
		    $ brew install php@7.3
		    ```
	    
    - Switch to PHP 5.6
    
        ```shell
        $ brew unlink php@7.2 && brew link --force --overwrite php@5.6
        ```
        
- Check PHP version: `php -v`
    
#### Ruby

### Version Control
- Git
	- GitHub

### Package Management
- Homebrew

    - Is Homebrew installed?
    
	    ```shell
	    $ brew --version
	    ```
    - Install Homebrew
    
	    ```shell
	    $ ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
 	   ```
    
    - Fix/correct Homebrew
    
	    ```shell
	    $ brew doctor
	    ```
    
    - [Update Homebrew](https://getgrav.org/blog/macos-mojave-apache-upgrade-homebrew)
    
- npm
- Chocolatey
- apt-get

### Cloud Storage
- OneDrive
- Google Drive
- DropBox
- Box

### Symbolic Links

#### MacOS, Linux

> Otherwise known as **symlinks**, they are like pointers to another place. While you don't have to _actually move_ the folder you are referencing, you can create a pointer to it that behaves as if you did.
> &mdash;[Chris Coyier](https://css-tricks.com/symbolic-links-for-easier-multi-folder-local-development/)

```shell
ln -s /path/to/original/ /path/to/link
```

#### Windows

> You can create symbolic links using the mklink command in a Command Prompt window as Administrator. To open one, locate the “Command Prompt” shortcut in your Start menu, right-click it, and select “Run as Administrator”.
> &mdash;[How-To Geek](https://www.howtogeek.com/howto/16226/complete-guide-to-symbolic-links-symlinks-on-windows-or-linux/)

```shell
# Without any extra options, mklink creates a symbolic link to a file. The below command creates a symbolic, or “soft”, link at Link pointing to the file Target :
mklink Link Target

# Use /D when you want to create a soft link pointing to a directory. like so:
mklink /D Link Target

# Use /H when you want to create a hard link pointing to a file:
mklink /H Link Target

# Use /J to create a hard link pointing to a directory, also known as a directory junction:
mklink /J Link Target
```

## Roadmap

View upcoming changes in the [Roadmap](https://github.com/paulshryock/Leigh/blob/master/CHANGELOG.md).

## Contributing

If you'd like to contribute, please read the [Code of Conduct](https://github.com/paulshryock/Leigh/blob/master/CODE_OF_CONDUCT.md), then fork the repository and use a feature
branch. Pull requests are welcome.

### Your First Contribution

Working on your first Pull Request? You can learn how from this *free* series, [How to Contribute to an Open Source Project on GitHub](https://egghead.io/series/how-to-contribute-to-an-open-source-project-on-github).

### Team

- [Paul Shryock](https://github.com/paulshryock): Lead Front End Developer

<!-- ### Thanks -->

## Links

- Project homepage: [https://paulshryock.github.io/Leigh/](https://paulshryock.github.io/Leigh/)
- Repository: [https://github.com/paulshryock/Leigh](https://github.com/paulshryock/Leigh)
- Releases:
		- [v0.1.0 - Roadmap](https://github.com/paulshryock/Leigh/releases/tag/v0.1.0)
- Issue tracker: [https://github.com/paulshryock/Leigh/issues](https://github.com/paulshryock/Leigh/issues)
  - In case of sensitive bugs like security vulnerabilities, please contact
    [paul@pshry.com](mailto:paul@pshry.com) directly instead of using issue tracker. We value your effort
    to improve the security and privacy of this project!
- Changelog: [https://github.com/paulshryock/Leigh/blob/master/CHANGELOG.md](https://github.com/paulshryock/Leigh/blob/master/CHANGELOG.md)
- Related projects:
  - **Eustace**: [https://github.com/paulshryock/Eustace](https://github.com/paulshryock/Eustace): _Start from scratch without reinventing the wheel_
  - **Myrtle**: [https://github.com/paulshryock/Myrtle](https://github.com/paulshryock/Myrtle): _Start Cockpit from scratch without reinventing the wheel_
  - **Brimbly**: [https://github.com/paulshryock/Brimbly](https://github.com/paulshryock/Brimbly): _Start WordPress from scratch without reinventing the wheel_

## Licensing

The code in this project is licensed under [GNU General Public License v3.0](https://github.com/paulshryock/Leigh/blob/master/LICENSE).