# Tor Browser Launcher

Tor Browser Launcher is intended to make the Tor Browser Bundle (TBB) easier to maintain and use for GNU/Linux users. You install ```torbrowser-launcher``` from your distribution's package manager and it handles everything else, including:

* Downloading the most recent version of TBB for you, in your language and for your architecture
* Automatically updating (while preserving your bookmarks and preferences)
* Verifying the TBB's [GnuPG signature](http://www.gnupg.org/gph/en/manual/x135.html)
* Adding a "Tor Browser" application launcher to your desktop environment's menu

If you use Ubuntu, you can install it now from my PPA (see "Installing in Ubuntu" below). [Soon](https://github.com/micahflee/torbrowser-launcher/issues/31) it will be in Debian. To install it in any other distribution, follow the "Quick Start" instructions.

## Quick Start

First, clone the repository:

    git clone https://github.com/micahflee/torbrowser-launcher.git
    cd torbrowser-launcher

Then install dependencies, build a package, and install:

### Debian, Ubuntu, Linux Mint, etc.

    sudo apt-get install build-essential python-all python-stdeb python-gtk2 python-psutil python-twisted python-pygame python-lzma python-parsley python-txsocksx wmctrl gnupg fakeroot
    ./build_deb.sh
    sudo dpkg -i deb_dist/torbrowser-launcher_*.deb

### Red Hat, Fedora, CentOS, etc.

    sudo yum install python-psutil python-twisted pygame wmctrl gnupg fakeroot
    ./build_rpm.sh
    sudo yum install dist/torbrowser-launcher-*.rpm

## Installing in Ubuntu

I've created a PPA where I'm maintaining torbrowser-launcher binaries. You can install in an Ubuntu-based distribution like this:

    sudo add-apt-repository ppa:micahflee/ppa
    sudo apt-get update
    sudo apt-get install torbrowser-launcher

## Enabling AppArmor Profiles

AppArmor is a Linux kernel hardening technology that lets you confine specific programs to only use specific resources. For example, if you visit a website that exploits a bug in the Tor Browser to get arbitrary code execution, the attacker won't be able to access files in ~/.gnupg. Here's how you turn on the AppArmor profiles for Tor Browser Launcher and for the programs bundled in the TBB.

    sudo apt-get install apparmor-utils
    sudo aa-enforce /etc/apparmor.d/usr.bin.torbrowser-launcher
    sudo aa-enforce /etc/apparmor.d/torbrowser.start-tor-browser
    sudo aa-enforce /etc/apparmor.d/torbrowser.Browser.firefox
    sudo aa-enforce /etc/apparmor.d/torbrowser.Tor.tor

# Building in Debian

## Dependencies

You need to have ```stdeb``` installed. If you're using Debian or Ubuntu you can install it like this:

    apt-get install python-stdeb

Also install the ```torbrowser-launcher``` dependencies:

    apt-get install python-gtk2 python-psutil python-twisted python-lzma wmctrl gnupg fakeroot

## Debian packages

To build a Debian source and binary packages:

    python setup.py --command-packages=stdeb.command sdist_dsc bdist_deb

Once you've made a ```.deb```, you can install by running this as root:

    dpkg -i deb_dist/torbrowser-launcher_VERSION_all.deb
