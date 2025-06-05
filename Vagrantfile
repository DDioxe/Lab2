# -*- mode: ruby -*-
# vi: set ft=ruby :
#Установка переменной окружения, задающей российское зеркало для box-файлов
ENV['VAGRANT_SERVER_URL'] = 'https://vagrant.elab.pro'
Vagrant.configure("2") do |config|
# Создание виртуальной машины веб-сервера
config.vm.define "webserver" do |web|
web.vm.box = "bento/ubuntu-24.04"
web.vm.network "private_network", ip: "192.168.50.10"
# Установка Apache
web.vm.provision "shell", inline: <<-SHELL
sudo apt-get update
sudo apt-get install apache2 -y
sudo systemctl enable apache2
SHELL
end
# Создание виртуальной машины базы данных
config.vm.define "dbserver" do |db|
db.vm.box = "bento/ubuntu-24.04"
db.vm.network "private_network", ip: "192.168.50.11"
# Установка MySQL
db.vm.provision "shell", inline: <<-SHELL
sudo apt-get update
sudo apt-get install -y mysql-server
sudo systemctl enable mysql
SHELL
end
# Создание виртуальной машины балансировщика нагрузки
config.vm.define "loadbalancer" do |lb|
lb.vm.box = "bento/ubuntu-24.04"
lb.vm.network "public_network"
# Установка HAProxy
lb.vm.provision "shell", inline: <<-SHELL
sudo apt-get update
sudo apt-get install -y haproxy
sudo systemctl enable haproxy
SHELL
end
end
