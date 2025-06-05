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
# Установка MySQLServer
sudo apt-get install -y mysql-client
# Установка модулей для работы из PHP
sudo apt install php libapache2-mod-php php-mysql -y
# Рестарт веб-сервера
sudo systemctl restart apache2
# Создание файла с текстом
cd /var/www/html/
echo '<?php
$host = "192.168.50.11";
$user = "vagrant_test";
$pass = "Tusur123";
$db = "testdb";
$conn = new mysqli($host, $user, $pass, $db);
if ($conn->connect_error) {
die("Connection failed: " . $conn->connect_error);
}
echo "Connected to MySQL successfully!";
?>' > test_db.php
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
sudo mysql -u root
sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
sudo sed -i 's/bind-address = 127.0.0.1/bind-address = 192.168.50.11/' /etc/mysql/mysql.conf.d/mysqld.cnf
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
