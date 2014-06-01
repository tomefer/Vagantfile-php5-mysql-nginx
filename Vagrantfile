# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "precise64"
  config.vm.box_url = "http://files.vagrantup.com/precise64.box"

  config.vm.network :forwarded_port, guest: 80, host: 8080
  config.vm.network :forwarded_port, guest: 8000, host: 8100
  config.vm.network :private_network, ip: "192.168.66.66"
  config.vm.synced_folder "../", "/srv/hosts/myhost.dev"
  config.vm.provider :virtualbox do |vb|
    vb.name = "devhost"
    vb.customize ["modifyvm", :id, "--memory", "2048"]
    vb.customize ["modifyvm", :id, "--ostype", "Ubuntu_64"]
  end

  config.vm.provision "shell", inline: <<-shell
    apt-get update
    apt-get install python-software-properties  -y --force-yes
    add-apt-repository ppa:mapnik/boost
    add-apt-repository ppa:nginx/stable    
    add-apt-repository ppa:ondrej/php5 -y
    apt-get update
    apt-get install nginx -y --force-yes
    apt-get install php5 php5-mysql php5-pgsql php5-curl php5-mcrypt php5-cli php5-fpm php-pear -y --force-yes


    apt-get install screen vim -y --force-yes
    debconf-set-selections <<< 'mysql-server-5.5 mysql-server/root_password password apassword'
    debconf-set-selections <<< 'mysql-server-5.5 mysql-server/root_password_again password apassword'
    apt-get install mysql-server -y --force-yes
   
    cp /srv/hosts/myhost.dev/vagrant/www.conf /etc/php5/fpm/pool.d/www.conf
    cp /srv/hosts/myhost.dev/vagrant/default /etc/nginx/sites-available/default
    chown vagrant /etc/php5/fpm/pool.d/www.conf /etc/nginx/sites-available/default
   
    #OJO; He editado /etc/php5/fpm/pool.d/www.conf y he cambiado listen = /var/run/php5-fpm.sock luego restart
    #OJO; HE editado la configuración de nginx /etc/nginx/sites-enabled cutremente


  shell
end
