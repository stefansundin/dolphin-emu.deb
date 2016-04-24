$dolphin_version = "4.0-9237"
$dolphin_hash = "3033096223de1a212d475cd670705c00daeba88d"

# Put a sources.list with mirrors near you in this directory for faster provisioning

$root_provision = <<SCRIPT
[[ -f /vagrant/sources.list ]] && cp /vagrant/sources.list /etc/apt/sources.list

export DEBIAN_FRONTEND=noninteractive
apt-get update
apt-get install -y git gnupg2 vim
apt-get install -y devscripts debhelper cmake pkg-config lsb-release libao-dev libasound2-dev libavcodec-dev libavformat-dev libbluetooth-dev libenet-dev libevdev-dev libgtk2.0-dev liblzo2-dev libminiupnpc-dev libopenal-dev libpulse-dev libreadline-dev libsdl2-dev libsfml-dev libsoil-dev libswscale-dev libudev-dev libusb-1.0-0-dev libwxbase3.0-dev libwxgtk3.0-dev libxext-dev libxrandr-dev portaudio19-dev zlib1g-dev
SCRIPT

$user_provision = <<SCRIPT
git config --global user.email "vagrant"
git config --global user.name "vagrant"

wget -q https://github.com/dolphin-emu/dolphin/archive/#{$dolphin_hash}.tar.gz -O dolphin-emu-master_#{$dolphin_version}.orig.tar.gz
tar xzf dolphin-emu-master_#{$dolphin_version}.orig.tar.gz
ln -s /vagrant dolphin-#{$dolphin_hash}/debian
SCRIPT


Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/xenial64"
  config.vm.hostname = "dolphinbar-linux"

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
cp ../dolphin-emu-master_*_amd64.deb /vagrant/
EOF
end
