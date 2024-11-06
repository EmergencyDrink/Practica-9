Vagrant.configure("2") do |config|
    # Configuración de la primera máquina virtual: Apache Server 1
    config.vm.define "apache1" do |apache1|
      apache1.vm.box = "ubuntu/bionic64"
      apache1.vm.network "private_network", ip: "192.168.33.10"
      apache1.vm.provider "virtualbox" do |vb|
        vb.memory = "512"
        vb.cpus = 1
      end
      apache1.vm.provision "shell", inline: <<-SHELL
        apt-get update
        apt-get install -y apache2
        systemctl enable apache2
        echo "<h1>Apache Server 1</h1>" > /var/www/html/index.html
      SHELL
    end
  
    # Configuración de la segunda máquina virtual: Apache Server 2
    config.vm.define "apache2" do |apache2|
      apache2.vm.box = "ubuntu/bionic64"
      apache2.vm.network "private_network", ip: "192.168.33.11"
      apache2.vm.provider "virtualbox" do |vb|
        vb.memory = "512"
        vb.cpus = 1
      end
      apache2.vm.provision "shell", inline: <<-SHELL
        apt-get update
        apt-get install -y apache2
        systemctl enable apache2
        echo "<h1>Apache Server 2</h1>" > /var/www/html/index.html
      SHELL
    end
  
    # Configuración de la tercera máquina virtual: Nginx Load Balancer
    config.vm.define "nginx" do |nginx|
      nginx.vm.box = "ubuntu/bionic64"
      nginx.vm.network "private_network", ip: "192.168.33.12"
      nginx.vm.provider "virtualbox" do |vb|
        vb.memory = "512"
        vb.cpus = 1
      end
      nginx.vm.provision "shell", inline: <<-SHELL
        apt-get update
        apt-get install -y nginx
        systemctl enable nginx
        # Configuración del balanceo de carga en Nginx
        cat <<EOF > /etc/nginx/conf.d/load_balancer.conf
        upstream apache_servers {
            server 192.168.33.10;
            server 192.168.33.11;
        }
  
        server {
            listen 80;
            location / {
                proxy_pass http://apache_servers;
            }
        }
        EOF
        systemctl restart nginx
      SHELL
    end
  end
  