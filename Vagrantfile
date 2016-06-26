$dolphin_version = "5.0-34"
$dolphin_hash = "e31a4d1f0731603d0396e592e23cf7acd0457397"

# Put a sources.list with mirrors near you in this directory for faster provisioning
# https://wiki.dolphin-emu.org/index.php?title=Building_Dolphin_on_Linux#16.04_.28LTS.29

$root_provision = <<SCRIPT
[[ -f /vagrant/sources.list ]] && cp /vagrant/sources.list /etc/apt/sources.list

export DEBIAN_FRONTEND=noninteractive
apt-get update
apt-get install -y gnupg2 vim devscripts debhelper libcurl4-openssl-dev libegl1-mesa-dev libsdl2-dev
apt-get install -y git cmake pkg-config libao-dev libasound2-dev libavcodec-dev libavformat-dev libbluetooth-dev libenet-dev libgtk2.0-dev liblzo2-dev libminiupnpc-dev libopenal-dev libpulse-dev libsfml-dev libsoil-dev libsoundtouch-dev libswscale-dev libusb-1.0-0-dev libwxbase3.0-dev libwxgtk3.0-dev libxext-dev libxrandr-dev portaudio19-dev libudev-dev libevdev-dev libmbedtls-dev libreadline-dev
SCRIPT

$user_provision = <<SCRIPT
git config --global user.email "vagrant"
git config --global user.name "vagrant"

wget -q https://github.com/dolphin-emu/dolphin/archive/#{$dolphin_hash}.tar.gz -O dolphin-emu_#{$dolphin_version}.orig.tar.gz
tar xzf dolphin-emu_#{$dolphin_version}.orig.tar.gz
ln -s /vagrant dolphin-#{$dolphin_hash}/debian
SCRIPT


Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/xenial64"
  # config.vm.hostname = "dolphinbar-linux"
  config.vm.synced_folder ".", "/vagrant"

  # The default RAM size is not enough, the compiler will crash
  # You might want to increase the numbers of CPUs in the VirtualBox GUI after running 'vagrant up' and 'vagrant halt'
  config.vm.provider :virtualbox do |vb|
    vb.memory = 3072
  end

  config.vm.provision "shell", inline: $root_provision
  config.vm.provision "shell", inline: $user_provision, privileged: false
  config.vm.post_up_message = <<EOF
Run `vagrant ssh`, then run:
cd dolphin-#{$dolphin_hash}
debuild -i -us -uc -b

To copy the finished deb file out of the VM, run:
cp ../dolphin-emu_*_amd64.deb /vagrant/
EOF
end
