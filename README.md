![Leigh](https://raw.githubusercontent.com/paulshryock/Leigh/master/favicon.ico)

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

## Setup Instructions

### Operating System
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
- [Apache 2.4](https://httpd.apache.org/docs/2.4/)
	- Virtual Hosts
	- SSL
- NginX

### DNS

#### hosts file

If you are testing a server that is not Internet-accessible, you can put host names in your hosts file in order to do local resolution.* For example, you might want to put a record in your hosts file to map a request for `www.example.com` to your local system, for testing purposes. This entry would look like:

```shell
127.0.0.1 www.example.com
```

_\* https://httpd.apache.org/docs/2.4/getting-started.html_

##### hosts file location

- MacOS: `/etc/hosts`
- Windows: `C:\Windows\system32\drivers\etc\hosts`
- Linux: `/etc/hosts`

#### DNS masking

### Database
  - MySQL
  - MariaDB
  - SQLite
  - PostgreSQL

#### Database Management
  - Sequel Pro
  - Heidi SQL
  - [OmniDB](https://www.omnidb.org/en/)
  - [phpMyAdmin](https://www.phpmyadmin.net/)

### Programming Language
- PHP
	- 7.3
- Ruby

### Version Control
- Git
	- GitHub

### Package Management
- Homebrew
- npm
- Chocolatey
- apt-get

### Cloud Storage
- OneDrive
- Google Drive
- DropBox
- Box

## Roadmap

View upcoming changes in the [Roadmap](https://github.com/paulshryock/Leigh/blob/master/CHANGELOG.md).

## Contributing

If you'd like to contribute, please read the [Code of Conduct](https://github.com/paulshryock/Leigh/blob/master/CODE_OF_CONDUCT.md), then fork the repository and use a feature
branch. Pull requests are welcome.

### Your First Contribution

Working on your first Pull Request? You can learn how from this *free* series, [How to Contribute to an Open Source Project on GitHub](https://egghead.io/series/how-to-contribute-to-an-open-source-project-on-github).

### Team

- [Paul Shryock](https://github.com/paulshryock) -- Lead Front End Developer

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