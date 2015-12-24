This is a debianization of Dolphin with Tilka's [dolphinbar_linux](https://github.com/Tilka/dolphin/tree/dolphinbar_linux) branch applied. The debianization is based on [the official ppa](https://launchpad.net/~dolphin-emu/+archive/ubuntu/ppa).

# Prerequisites

```bash
sudo apt-get install devscripts debhelper cmake pkg-config lsb-release libao-dev libasound2-dev libavcodec-dev libavformat-dev libbluetooth-dev libenet-dev libevdev-dev libgtk2.0-dev liblzo2-dev libminiupnpc-dev libopenal-dev libpolarssl-dev libpulse-dev libreadline-dev libsdl2-dev libsfml-dev libsoil-dev libswscale-dev libudev-dev libusb-1.0-0-dev libwxbase3.0-dev libwxgtk3.0-dev libxext-dev libxrandr-dev portaudio19-dev zlib1g-dev
```

# Build

```bash
mkdir dolphin-emu
cd dolphin-emu
wget https://github.com/dolphin-emu/dolphin/archive/cfd300586a64def09e6bc2e70c354d4e1646bf42.tar.gz -O dolphin-4.0-8439.orig.tar.gz
tar xzf dolphin-4.0-8439.orig.tar.gz
cd dolphin-cfd300586a64def09e6bc2e70c354d4e1646bf42
git clone https://github.com/stefansundin/dolphin-emu.deb.git debian
debuild -i -us -uc -b
```

# See also

- https://www.reddit.com/r/emulation/comments/2lnsdt/anyone_know_how_well_the_mayflash_dolphinbar/cmxin4t
- https://github.com/Tilka/dolphin/commit/985027c920fbb50c1509ccd3ff428d302f88acdd
- https://launchpad.net/~dolphin-emu/+archive/ubuntu/ppa
- https://code.launchpad.net/~dolphin-emu/dolphin-emu/debian-master

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