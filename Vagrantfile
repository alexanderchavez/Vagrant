Vagrant.configure("2") do |config|

  config.vm.define "server01" do |server01|
    server01.vm.box = "Ubuntu/jammy64"
    server01.vm.network "forwarded_port", guest: 3306, host: 3306
    server01.vm.network "private_network", ip: "192.168.56.11"
    server01.vm.provision "shell", inline: <<-SHELL
      sudo apt-get update
      sudo apt-get install mysql-server -y
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
    SHELL
  end
end
