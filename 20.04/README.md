# Autoinstall configs for Ubuntu 20.04

- **Status**: WIP - expect things to change

As you can guess this is all quite different from previous setups so expect it to still be a bit messy for a while.

This folder contains autoinstall configs designed for Ubuntu 20.04

## Notable changes since 18.04

- Biggest change is preseed and kickstart files are deprecated in favor of `cloud-init`.
- Using `ks=http://example.com/install.ks` on boot command requires using the `legacy-server` image.
- Alternate install is deprecated, renamed to `legacy-server` install and all installs are supposed to use the full install iso.
- It appears the new installer copies a base disk image to target system which makes the previous hack of trimming down the install by not installing recommended packages ineffective.

## Using kickstart files

It is still possible to use kickstart files.  Keep in mind Canonical is making it clear that it's going away soon so may want to get used to the new isntaller.  Because of it being deprecated and because 18.04 kickstarts seem to still just work I'm only providing an example `minimal vm` kickstart file

- Download the latest [ubuntu-legacy-server](http://cdimage.ubuntu.com/ubuntu-legacy-server/releases/20.04/release/) image
- Place `ks-2004-minimalvm.cfg` on a web server the vm can see.  Testing the kickstart file from 18.04 seems to work without modification.
    - Only modification I've made to 20.04 kickstart file is default size of `/usr` is now 3GB.
- TBD but do all the stuff mentioned in 18.04 for creating destination vm.
- After creating vm boot from legacy server iso.
- Choose language.
- Press `F6` and then press `ESC` to get to command line.
- Enter the following at end of command, replacing the URL with the address to where the above files are located
    - `ks=http://example.com/ks-2004-minimalvm.cfg`
- Press enter to start the install process.

## Basic usage

- Create a path for a given install type on a webserver.  I have been using `/uai/vm/` (for Ubuntu Auto Install / VM install).
- Create an empty `meta-data` file in that directory.
- Copy one of the user-data files to that directory and make sure it is named `user-data`.
- Boot from Ubuntu 20.04 ISO.
- Once you see the keyboard at bottom of screen press escape or space or probably any key.
- Choose language.
- Press `F6` and then press `ESC` to get to command line.
- Enter the following at end of command, replacing the URL with the address to where the above files are located
    - `autoinstall ds=nocloud-net;s=http://example.com/uai/vm/`
- Press enter to start the install process.
- If you are taken to the install menu double check you entered the path correctly including the trailing `/`.

## Troubleshooting

- `CTRL`-`ALT`-`F2` will still bring up an alternate shell.  The `sudo` command won't ask for a password.
- Everything is logged to `/var/log` during install and then at the end is copied to target system and is available at `/var/log/install`.

## Templates

### user-data.min

The bare needed for auto install to work.  Password of user is `ubuntu`.

### user-data.full

A more complete auto install file.

- Include a place to enter proxy server that will be used by `apt`.
- While it mimics the default the file system names have been changed to make them easier to read in final system.
- Show how to run some early or late commands.
- Will update the installer before continuing.
