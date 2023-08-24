Vagrant.configure("2") do |config|

  config.vm.define "server01" do |server01|
    server01.vm.box = "Ubuntu/jammy64"
    server01.vm.network "forwarded_port", guest: 3306, host: 3306
    server01.vm.network "private_network", ip: "192.168.56.11"
    server01.vm.provision "shell", inline: <<-SHELL
      sudo apt-get update
      sudo apt-get install mysql-server -y
      sudo mysql -e "CREATE USER 'alex'@'%' IDENTIFIED BY 'alex';"
      sudo mysql -e "GRANT ALL PRIVILEGES ON your_database_name.* TO 'alex'@'%';"
      sudo mysql -e "FLUSH PRIVILEGES;"
      cd /etc/mysql/mysql.conf.d/mysqld.cnf
      
    SHELL
  end

  config.vm.define "server02" do |server02|
    server02.vm.box = "Ubuntu/jammy64"
    server02.vm.network "forwarded_port", guest: 80, host: 80
    server02.vm.network "private_network", ip: "192.168.56.12"
    server02.vm.provision "shell", inline: <<-SHELL
      sudo apt-get update
      sudo apt-get install apache2 -y 
      sudo apt-get install php libapache2-mod-php php-mysql -y
      sudo apt-get install unzip
      curl https://files.phpmyadmin.net/phpMyAdmin/5.2.1/phpMyAdmin-5.2.1-all-languages.zip --output phpmyadmin.zip
      sudo unzip phpmyadmin.zip
      sudo mv phpMyAdmin-5.2.1-all-languages /var/www/html/phpmyadmin
      sudo apt-get update
      cd /var/www/html/phpmyadmin
      rm rm config.sample.inc.php
      contenido=" 
/* Servers configuration*/
$i = 0;

/**
 * First server
 */
$i++;
/* Authentication type */
$cfg['Servers'][$i]['auth_type'] = 'config';
/* Server parameters */
$cfg['Servers'][$i]['host'] = '192.168.56.11';
$cfg['Servers'][$i]['port'] = '3306';
$cfg['Servers'][$i]['user'] = 'alex';
$cfg['Servers'][$i]['password'] = 'alex';
//$cfg['Servers'][$i]['compress'] = false;
//$cfg['Servers'][$i]['AllowNoPassword'] = false;

/**
 * Directories for saving/loading files from server
 */
$cfg['UploadDir'] = '';
$cfg['SaveDir'] = '';
?>
"
nombre_archivo="config.inc.php"
echo "$contenido" | sudo tee "$nombre_archivo" > /dev/null
sudo systemctl restart apache2 
    SHELL
  end
end
