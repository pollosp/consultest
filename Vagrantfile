# -*- mode: ruby -*-
# vi: set ft=ruby :
### THIS ENVIRONMENT IS JUST FOR TESTING PROUPOSES

##SERVERS BOOTSTRAP
$script = <<SCRIPT
# Update apt and get dependencies
sudo apt-get update
sudo apt-get install -y unzip tmux curl wget vim
# Installing Consul
echo Fetching Consul...
sudo su -c 'curl -L https://dl.bintray.com/mitchellh/consul/0.3.0_linux_amd64.zip > /usr/local/bin/0.3.0_linux_amd64.zip 2> /dev/null'
echo Installing Consul..
cd /usr/local/bin
sudo unzip -o 0.3.0_linux_amd64.zip
sudo rm 0.3.0_linux_amd64.zip
SCRIPT

#CLIENT BOOTSTRAP
$clientscript = <<SCRIPT
#installs consul-template
cd /usr/local/bin
sudo su -c 'curl -L -o consul-template_0.12.2_linux_amd64.zip https://releases.hashicorp.com/consul-template/0.12.2/consul-template_0.12.2_linux_amd64.zip 2> /dev/null'
sudo unzip -o consul-template_0.12.2_linux_amd64.zip
sudo rm consul-template_0.12.2_linux_amd64.zip
#downloads Web UI
sudo su -c 'curl -L -o /tmp/webui.zip https://dl.bintray.com/mitchellh/consul/0.3.0_web_ui.zip 2> /dev/null'
cd /tmp/
sudo unzip -o webui.zip
sudo rm webui.zip
#Installs nginx for testing prouposes
sudo apt-get install -y nginx
SCRIPT


Vagrant.configure(2) do |config|

 (2..4).each do |i|
   config.vm.define "consulserver#{i}" do |consulserver|
     consulserver.vm.box = "ARTACK/debian-jessie"
     consulserver.vm.box_url = "https://atlas.hashicorp.com/ARTACK/boxes/debian-jessie"
     consulserver.vm.hostname = "consulserver#{i}"
     consulserver.vm.network "private_network", ip: "192.168.50.#{i}"
     consulserver.vm.provision "shell", inline: $script, privileged: true
     consulserver.vbguest.auto_update = false
     consulserver.vm.provider "virtualbox" do |vb|
       vb.memory = "512"
     end
   end
 end

  config.vm.define "consulclient" do |consulclient|
    consulclient.vm.box = "ARTACK/debian-jessie"
    consulclient.vm.box_url = "https://atlas.hashicorp.com/ARTACK/boxes/debian-jessie"
    consulclient.vm.hostname = "consulclient"
    consulclient.vm.provision "shell", inline: $script, privileged: true
    consulclient.vm.provision "shell", inline: $clientscript, privileged: true
    consulclient.vm.network "private_network", ip: "192.168.50.5"
    consulclient.vm.network :forwarded_port, guest: 8500, host: 8500
    consulclient.vbguest.auto_update = false
    consulclient.vm.provider "virtualbox" do |vb|
        vb.memory = "512"
    end
   end
end
