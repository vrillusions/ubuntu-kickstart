# ubuntu-kickstart

Kickstart files to automate the build of Ubuntu servers. This originally existed as a [gist](https://gist.github.com/vrillusions/d292953ff9bc0e2041d9) but is now a full project to make it easier to find.

## Overview

To automate the installation of Ubuntu you have two options. First is Ubuntu's native format which is called preseeding. It seemed overly complex when I just wanted something to build a VM quickly. The other option is using a kickstart file. Kickstart files are used by the various RedHat based distros. Ubuntu implements a subset of the options and then lets you add preseed commands throughout it. So that's what I went with.

Each folder contains a readme with usage information and what is installed.

The goal of these kickstart files is to be secure by default, with proper partitions and with a minimal install of software to get you going.  Pretty much all of this is geared towards running in a VM although these should work with only a couple modifications.

## Basic setup

To use any of these files you'll need some way for the server to pull down the config. When you use the `ks` option at boot Ubuntu will first bring up the networking using DHCP and once it has a network connection fetch the config.  At that point it will reset things and follow the config.  This means the config has to be someone on the network.  The config can be obtained via `http://`, `https://`, `ftp://`, or `nfs://`, at leaast based off the [initrd-kickseed](https://bazaar.launchpad.net/~ubuntu-installer/kickseed/master/view/head:/initrd-kickseed) file.  While you may be able to point directly to this git repo DO NOT DO THIS.  In doing so you are pulling a file from an external site and letting it run whatever it so desires on your fresh new server.  It's highly unlikely something bad is in there but a config could change in some way that you don't expect and then wonder why you server isn't building right.  This leave running your own web site.  If you have an external web page somewhere you can host it there.  I have a local web server I use.  This gives me some level of privacy in the config.  I still use a dummy password and then change it when I first connect.  Setting up a web server is out of scope for this project but you can find plenty of information on google on how to run a basic web server.

If you are going to be installing several VMs I recommend setting up a proxy server for apt.  After the first download it will cache it locally which will speed up additional installs.  Can use it in final server as well to speed up updates.  For the most part just choose a local server with some disk space and run `apt-get install apt-cacher-ng`.  Then update the kickstart files with the proxy which should be running at `http://your.server.name:3142/`. You can verify this by going to that page in your browser and you'll be presented with a management interface.

## Troubleshooting

Some tips in case the install doesn't go to plan

- `ctrl-alt-F1` is the installer screen you normally see (to get back to it from other screens)
- `ctrl-alt-F2` will let you bring up a terminal session
- `ctrl-alt-F4` is a detail activity log
- The `%post` section runs when you see `Running kickseed...` (at least in 16.04)
- Any `echo` you have in `%post` will show in log but won't print to installer screen
- After entering the `ks=` option, add `DEBCONF_DEBUG=5` which will print out additional info to log
- After the install the logs will be available in system at `/var/log/installer`.

## References

- https://help.ubuntu.com/lts/installation-guide/amd64/ch04s06.html#kickstart
- https://help.ubuntu.com/lts/installation-guide/amd64/apbs04.html
- https://help.ubuntu.com/community/KickstartCompatibility
- https://help.ubuntu.com/community/StricterDefaults

## License

Creative Commons CC0 (public domain)

To the extent possible under law, Todd Eddy has waived all copyright and related or neighboring rights to [Ubuntu Kickstart](https://github.com/vrillusions/ubuntu-kickstart). The full license text is available at <https://creativecommons.org/publicdomain/zero/1.0/>. This work is published from: United States
