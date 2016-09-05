# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|

  config.vm.box = "ubuntu/trusty64"
  config.vm.network "private_network", ip: "10.0.0.222"
  config.vm.provision "shell", privileged: true, inline: $INSTALL_NODEJS
  config.vm.provision "shell", privileged: false, inline: $INSTALL_CLOUD9_IDE
  config.vm.provision "shell", privileged: false, run: "always", inline: $START_CLOUD9_IDE

end

$INSTALL_NODEJS = <<SCRIPT

echo "~~~~~ Build n ~~~~~"
apt-get update -qq
apt-get install -y build-essential curl git
git clone https://github.com/tj/n.git
cd n
make install
cd ..
rm -rf n
echo "~~~~~ Install Node LTS ~~~~~"
n lts
npm config set jobs 1
echo "~~~~~ Install forever ~~~~~"
npm install -g forever

SCRIPT

$INSTALL_CLOUD9_IDE = <<SCRIPT

echo "Cloning Cloud9 Core"
cd /home/vagrant
git clone https://github.com/c9/core.git Cloud9IDE
echo "Install Cloud9 IDE"
cd Cloud9IDE
npm config set jobs 1
scripts/install-sdk.sh

SCRIPT

$START_CLOUD9_IDE = <<SCRIPT

echo "Start Cloud9 IDE Standalone application"
cd /home/vagrant/Cloud9IDE
forever start server.js -p 8181 -l 0.0.0.0 -a : -w "/vagrant/workspace"

SCRIPT
