This is a debianization of Dolphin with Tilka's [dolphinbar_linux](https://github.com/Tilka/dolphin/tree/dolphinbar_linux) patch applied.

For an easy to use PPA, use: https://launchpad.net/~stefansundin/+archive/ubuntu/dolphin-emu

If you had the DolphinBar connected before installing this package, remember to unplug it and plug it back in for the udev rule to take effect.

# Prerequisites

```bash
sudo apt-get install devscripts debhelper cmake pkg-config lsb-release libao-dev libasound2-dev libavcodec-dev libavformat-dev libbluetooth-dev libenet-dev libevdev-dev libgtk2.0-dev liblzo2-dev libminiupnpc-dev libopenal-dev libpolarssl-dev libpulse-dev libreadline-dev libsdl2-dev libsfml-dev libsoil-dev libswscale-dev libudev-dev libusb-1.0-0-dev libwxbase3.0-dev libwxgtk3.0-dev libxext-dev libxrandr-dev portaudio19-dev zlib1g-dev
```

# Build

```bash
mkdir dolphin-emu
cd dolphin-emu
wget https://github.com/dolphin-emu/dolphin/archive/05e431d5b5b351d67e6d202ad5960e48f7612a58.tar.gz -O dolphin-emu-master_4.0-8961.orig.tar.gz
tar xzf dolphin-emu-master_4.0-8961.orig.tar.gz
cd dolphin-05e431d5b5b351d67e6d202ad5960e48f7612a58
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
