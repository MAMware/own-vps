# Server privado virtual - Primeros pasos

Servicio cloud server provisto por donweb.com

Servicio 1vCPU 1 GB RAM 10 GB SSD

Paso 1, crear vps debian11-64-min

Paso 2, crear usuario limitado

adduser usuariolimitado
usermod -aG sudo usuariolimitado
  /var/log/auth.log registro de operaciones
  
Paso 3, asegurar el SSH

cd /etc/ssh
sudo cp sshd_config  sshd_config.orig

sudo nano sshd_config
#habilitar LoginGraceTime, MaxAuthTries y MaxSessions

sudo systemctl restart sshd

Paso 4, instalar un stack LEMP

sudo apt update &&% sudo apt upgrade

sudo apt install nginx mysql-server php-fpm php-mysql

#debian no tiene los repos para mysql 
#ubicar ultima version de mysql en https://dev.mysql.com/downloads/repo/apt/

seudo wget ultimaversion
sudo dpkg -i ultimaversion
sudo apt update
sudo apt-get install 

login a mysql console

sudo mysql -u root -p

 mysql> CREATE USER 'usuario'@'localhost' IDENTIFIED WITH mysql_native_password BY 'Abc1234';

Crear archivo de estado de mysql

cd /var/www/html 
sudo touch status-mysql.php 
sudo nano status-mysql.php

<?php 
$servidor = "localhost"; 
$usuario = "usuario"; 
$pass = "Abc1234"; 
// Crear conexión $con = new mysqli($servidor, $usuario, $pass); 
// Confirmar if ($con->connect_error) { die("La conexión falló: " . $con->connect_error);
 } else { 
$estado = explode(' ', $con->stat()); 
print_r($estado); } 
// Cerrar conexión 
$con->close();

Instalacion lets encrypt

sudo apt install certbot python3-certbot-nginx -y


#editar /etc/nignx/conf.d/nombredesitio

server { 
listen 80 default_server; 
listen [::]:80 default_server;
root /var/www/nombredesitio; 
server_name nombredesitio.com www.nombredesitio.com; 
}

sudo rm /etc/nginx/sites-enabled/default

sudo mkdir /var/www/nombredesitio
sudo nginx -t && sudo nginx -s reload

Integracion de nginx con certbot

sudo certbot --nginx -d cloudme.fun -d nombredesitio

systemctl status certbot.timer

