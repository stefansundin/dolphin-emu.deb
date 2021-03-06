This is a debianization of Dolphin with Tilka's [dolphinbar_linux](https://github.com/Tilka/dolphin/tree/dolphinbar_linux) patch applied.

For an easy to use PPA, use: https://launchpad.net/~stefansundin/+archive/ubuntu/dolphin-emu

If you had the DolphinBar connected before installing this package, remember to unplug it and plug it back in for the udev rule to take effect.

Also includes an udev rule for the GameCube controller adapter.

# Prerequisites

```bash
apt-get install -y git devscripts debhelper libcurl4-openssl-dev libegl1-mesa-dev libsdl2-dev cmake pkg-config libao-dev libasound2-dev libavcodec-dev libavformat-dev libbluetooth-dev libenet-dev libgtk2.0-dev liblzo2-dev libminiupnpc-dev libopenal-dev libpulse-dev libsfml-dev libsoil-dev libsoundtouch-dev libswscale-dev libusb-1.0-0-dev libwxbase3.0-dev libwxgtk3.0-dev libxext-dev libxrandr-dev portaudio19-dev libudev-dev libevdev-dev libmbedtls-dev libreadline-dev
```

# Build

```bash
mkdir dolphin-emu
cd dolphin-emu
wget https://github.com/dolphin-emu/dolphin/archive/e31a4d1f0731603d0396e592e23cf7acd0457397.tar.gz -O dolphin-emu_5.0-34.orig.tar.gz
tar xzf dolphin-emu_5.0-34.orig.tar.gz
cd dolphin-e31a4d1f0731603d0396e592e23cf7acd0457397
git clone https://github.com/stefansundin/dolphin-emu.deb.git debian
debuild -i -us -uc -b
```

Add `-nc` to `debuild` if you are building the files several times. It saves time by not recompiling everything.

## Vagrant

If you are familiar with [Vagrant](https://www.vagrantup.com/), you can simply run this to build the deb file inside a VM:

```shell
git clone https://github.com/stefansundin/dolphin-emu.deb.git
cd dolphin-emu.deb
vagrant up
```

Follow the instructions printed by the Vagrantfile.

# See also

- https://www.reddit.com/r/emulation/comments/2lnsdt/anyone_know_how_well_the_mayflash_dolphinbar/cmxin4t
- https://github.com/Tilka/dolphin/commit/985027c920fbb50c1509ccd3ff428d302f88acdd

Package based on the official PPA:
- https://launchpad.net/~dolphin-emu/+archive/ubuntu/ppa
- https://code.launchpad.net/~dolphin-emu/dolphin-emu/debian-master

# Rebase patch

Debian packages doesn't allow patches with fuzz, so because Tilka's patch doesn't cleanly apply anymore, I had to rebase it like this:

```bash
git clone -b dolphinbar_linux https://github.com/Tilka/dolphin.git
cd dolphin
git remote add upstream https://github.com/dolphin-emu/dolphin
git pull --rebase upstream master
git diff upstream/master..HEAD > ../dolphinbar_linux.patch
```

# Other useful commands

```
wget https://github.com/dolphin-emu/dolphin/archive/master.tar.gz
wget https://github.com/Tilka/dolphin/commit/985027c920fbb50c1509ccd3ff428d302f88acdd.patch -O dolphinbar_linux.patch
tar xzf master.tar.gz
cd dolphin-master
patch -p 1 < ../dolphinbar_linux.patch
mkdir Build
cd Build
cmake ..
make

bzr branch lp:~dolphin-emu/dolphin-emu/debian-master debian
```
