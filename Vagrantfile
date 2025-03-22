Vagrant.configure("2") do |config|
  # Configuración del servidor Apache 1
  config.vm.define "web1" do |web1|
    web1.vm.box = "ubuntu/bionic64"
    web1.vm.hostname = "web1"
    web1.vm.network "private_network", ip: "192.168.56.10"
    web1.vm.provider "virtualbox" do |vb|
      vb.memory = "512"
      vb.cpus = 1
    end
    web1.vm.synced_folder "./html", "/var/www/html"
    web1.vm.provision "shell", inline: <<-SHELL
      sudo apt update
      sudo apt install -y apache2
      echo "<h1>Servidor Web 1</h1>" | sudo tee /var/www/html/index.html
      sudo systemctl enable apache2
      sudo systemctl start apache2
    SHELL
  end

  # Configuración del servidor Apache 2
  config.vm.define "web2" do |web2|
    web2.vm.box = "ubuntu/bionic64"
    web2.vm.hostname = "web2"
    web2.vm.network "private_network", ip: "192.168.56.11"
    web2.vm.provider "virtualbox" do |vb|
      vb.memory = "512"
      vb.cpus = 1
    end
    web2.vm.synced_folder "./html", "/var/www/html"
    web2.vm.provision "shell", inline: <<-SHELL
      sudo apt update
      sudo apt install -y apache2
      echo "<h1>Servidor Web 2</h1>" | sudo tee /var/www/html/index.html
      sudo systemctl enable apache2
      sudo systemctl start apache2
    SHELL
  end

  # Configuración del servidor Nginx con balanceo de carga
  config.vm.define "nginx" do |nginx|
    nginx.vm.box = "ubuntu/bionic64"
    nginx.vm.hostname = "nginx"
    nginx.vm.network "private_network", ip: "192.168.56.12"
    nginx.vm.provider "virtualbox" do |vb|
      vb.memory = "512"
      vb.cpus = 1
    end
    nginx.vm.provision "shell", inline: <<-SHELL
      sudo apt update
      sudo apt install -y nginx
      echo 'upstream backend {
        server 192.168.56.10;
        server 192.168.56.11;
      }
      server {
        listen 80;
        location / {
          proxy_pass http://backend;
        }
      }' | sudo tee /etc/nginx/sites-available/default
      sudo systemctl restart nginx
    SHELL
  end
end
