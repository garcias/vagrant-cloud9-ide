# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|

  config.vm.box = "ubuntu/trusty64"
  config.vm.network "private_network", ip: "10.0.0.222"
  config.ssh.forward_agent = true
  config.vm.provision "shell", privileged: false, inline: $INSTALL_NODEJS
  config.vm.provision "shell", privileged: false, inline: $INSTALL_CLOUD9_IDE
  config.vm.provision "shell", privileged: false, run: "always", inline: $START_CLOUD9_IDE
  config.vm.provider :virtualbox do |v|
    v.name = "Cloud9"
    v.memory = 1024
  end

end

$INSTALL_NODEJS = <<SCRIPT

sudo apt-get update -qq
sudo apt-get install -y build-essential libssl-dev curl git
curl https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh | sh
source ~/.bashrc
source ~/.profile
nvm install 8.1.3
npm config set jobs 1
npm install -g forever
npm install -g c9

SCRIPT

$INSTALL_CLOUD9_IDE = <<SCRIPT

echo "Cloning Cloud9 Core"
cd /home/vagrant
git clone https://github.com/c9/core.git Cloud9IDE
echo "Install Cloud9 IDE"
npm config set jobs 1
Cloud9IDE/scripts/install-sdk.sh

SCRIPT

$START_CLOUD9_IDE = <<SCRIPT

cd /home/vagrant/Cloud9IDE
forever start server.js -p 8181 -l 0.0.0.0 -a : -w "/vagrant/workspace"
echo "Serving Cloud9 IDE at http://10.0.0.222:8181"

SCRIPT
