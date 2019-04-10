# Natur 4.0: Sensorboxes

[![Build Status](https://travis-ci.org/Nature40/Sensorboxes.svg?branch=master)](https://travis-ci.org/Nature40/Sensorboxes)

This repo collects software and hardware descriptions and links used in Natur 4.0 Sensorboxes. It contains distribution definitions (using pimod) and configuration files for sensorboxes.

## Overview

An installation currently consist of those parts:

 - [LiftSystem](https://github.com/Nature40/Satellite-LiftSystem): stateless ESP32 Board and Motor Driver to run the Lift
 - LiftBox: various Sensors, running up and down
 - PlanetBox: various sensors, Harddrive as data sink, LTE uplink

## Configuration

To setup an installation one has to install the prepared images to the devices and configure them. The configuration mainly consists of adapting the static config (`/boot/sensorproxy.yml`) as well as the measurement cycle (`/boot/meterings.yml`).

More details can be found in [pysensorproxy Readme](https://github.com/nature40/pysensorproxy).

## Useful commands

##### pysensorproxy daemon control

```bash
# control daemon
sudo systemctl start sensorproxy
sudo systemctl stop sensorproxy
systemctl status sensorproxy

# view logs
journalctl -u sensorproxy

# follow current log
journalctl -fu sensorproxy

# run standalone instance
sudo sensorproxy -vv --config /boot/sensorproxy.yml --metering /boot/meterings.yml
```

## Build Distro

A distribution can be build by using pimod. This tutorial will use the provided `docker-compose` file.

```bash
# first update / init the submodules
git submodule update --init --recursive

# download the required raspbian image
wget http://director.downloads.raspberrypi.org/raspbian_lite/images/raspbian_lite-2018-11-15/2018-11-13-raspbian-stretch-lite.zip
unzip 2018-11-13-raspbian-stretch-lite.zip

# build the Base and Sensorbox image
docker-compose run pimod pimod.sh Base.Pifile
docker-compose run pimod pimod.sh Sensorbox.Pifile

# alternative: using the provided makefile
make Sensorbox.img
```

```bash
$ docker-compose run pimod pimod.sh Sensorbox.Pifile
### FROM 2018-11-13-raspbian-stretch-lite.img
### TO LiftCar.img
### PUMP 100
100+0 records in
100+0 records out
104857600 bytes (105 MB, 100 MiB) copied, 0.929542 s, 113 MB/s
e2fsck 1.43.4 (31-Jan-2017)
Pass 1: Checking inodes, blocks, and sizes
Pass 2: Checking directory structure
Pass 3: Checking directory connectivity
Pass 4: Checking reference counts
Pass 5: Checking group summary information
rootfs: 39646/110880 files (0.1% non-contiguous), 258346/443392 blocks
resize2fs 1.43.4 (31-Jan-2017)
The filesystem is already 443392 (4k) blocks long.  Nothing to do!

Disk /dev/loop0: 1.9 GiB, 1971322880 bytes, 3850240 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x7ee80803

Device       Boot Start     End Sectors  Size Id Type
/dev/loop0p1       8192   98045   89854 43.9M  c W95 FAT32 (LBA)
/dev/loop0p2      98304 3645439 3547136  1.7G 83 Linux
loop deleted : /dev/loop0
### RUN raspi-config nonint do_serial 0
...
```
