# -*- mode: ruby -*-
# vi: set ft=ruby :
 
Vagrant.configure("2") do |config|
  config.vm.box = "debian/buster64"
  config.vm.box_check_update = false
 
  config.vm.define "recette" do |recette|
    recette.vm.hostname = "recette"
    recette.vm.network "forwarded_port", guest: 8000, host: 8010
    recette.vm.network "private_network", ip: "192.168.34.10" 
    recette.vm.synced_folder ".", "/vagrant", type: "nfs"
  end

  config.vm.define "production" do |production|
    production.vm.hostname = "production"
    production.vm.network "forwarded_port", guest: 8000, host: 8080
    production.vm.network "private_network", ip: "192.168.34.20" 
    production.vm.synced_folder ".", "/vagrant", type: "nfs"
  end

  config.vm.provision "shell", inline: <<-SHELL
    apt-get update
    apt upgrade -y
    apt install curl wget unzip -y
    # Install Gitlab
    curl -L https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.deb.sh | sudo bash
    apt-get install gitlab-runner -y
    # Install requirements for Symfony
    apt install -y php php-bcmath php-cli php-curl php-zip php-sqlite3 php-mysql php-xml php-mbstring
    curl -sS https://getcomposer.org/installer -o composer-setup.php
    php composer-setup.php --install-dir=/usr/local/bin --filename=composer
 SHELL
end

