# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "debian/bookworm64"
  config.vm.provision "shell", inline: <<-SHELL
    apt-get update -y
    apt-get upgrade -y
    apt-get install -y bind9 dnsutils
  SHELL

    config.vm.define "tierra" do |master|
      master.vm.network "private_network", ip: "192.168.57.103"
<<<<<<< HEAD
      master.vm.hostname = "tierra.sistema.test"

      master.vm.provision "shell", name: "master-dns", inline: <<-SHELL
        cp -v /vagrant/named /etc/default
        cp -v /vagrant/named.conf.options /etc/bind
        cp -v /vagrant/tierra/named.conf.local /etc/bind
        cp -v /vagrant/sistema.test.rev /var/lib/bind/
        cp -v /vagrant/sistema.test.dns /var/lib/bind/
        chown :bind /var/lib/bind/*
=======
      master.vm.hostname = "tierra"

      master.vm.provision "shell", name: "tierra", inline: <<-SHELL
        cp -v /vagrant/named /etc/default/
        cp -v /vagrant/named.conf.options /etc/bind
        cp -v /vagrant/tierra/named.conf.local /etc/bind
        cp -v /vagrant/sistema.test.dns /var/lib/bind/
        cp -v /vagrant/sistema.test.rev /var/lib/bind/
        chown bind:bind /var/lib/bind/*
>>>>>>> b10a46123b4d01771d68bad9c7fb49a84b94f110
        systemctl restart named
      SHELL
  end

  config.vm.define "venus" do |slave|
      slave.vm.network "private_network", ip: "192.168.57.102"
<<<<<<< HEAD
      slave.vm.hostname = "venus.sistema.test"

      slave.vm.provision "shell", name: "slave-dns", inline: <<-SHELL
=======
      slave.vm.hostname = "venus"

      slave.vm.provision "shell", name: "venus", inline: <<-SHELL
>>>>>>> b10a46123b4d01771d68bad9c7fb49a84b94f110
        cp -v /vagrant/named /etc/default/
        cp -v /vagrant/named.conf.options /etc/bind
        cp -v /vagrant/venus/named.conf.local /etc/bind
        systemctl restart named
      SHELL
  end
end
