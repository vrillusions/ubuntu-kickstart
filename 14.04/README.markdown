# Kickstart configs for Ubuntu 14.04

- **Status**: inactive

This folder contains kickstart configs designed for Ubuntu 14.04

## No longer making changes

I'm no longer adding or updating these configs and have moved on to other versions

## Configuration

Here's what gets setup based on the distro and specific kickstart file you use.  Files are heavily commented so any of this can be changed fairly easily.

### ks-1404minimalvm.cfg

- Intended to be used when creating a virtual machine
- Requires minimum 8GB disk
- Initial user is `ubuntu` with password `ChangeMe`
- Installs the following
    - openssl
    - wget
    - curl
    - tcpd
    - openssh-server
    - screen
    - vim
- Set vim background to dark
- Change default umask from `022` to `027`, meaning files created are not world readable
- Turns off installation of recommended packages
- Enables automatic security updates
- All partitions are `EXT4`
- All partitions except `/boot` are in a LVM
- After install, with 8GB disk, leaves about 1GB left to allocate to whatever partition you want using `lvextend`
- See below for partition layout.

| mount | size  | options         |
| ----- | ----- | --------------- |
| /boot | 512MB |                 |
| /     | 1GB   | `noatime`       |
| /usr  | 2GB   | `noatime,nodev` |
| /var  | 1.5GB | `noatime,nodev` |
| /home | 512MB | `noatime,nodev` |
| swap  | 2GB   |                 |

## Usage

See the individual config file for usage information
