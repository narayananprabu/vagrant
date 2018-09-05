# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!

$script = <<SCRIPT
echo provisioning the vm...
echo I am user:
whoami

echo HOME location:
echo $HOME

sudo DEBIAN_FRONTEND=noninteractive apt-get -y -o DPkg::options::="--force-confdef" -o DPkg::options::="--force-confold"  install grub-pc

echo ==========> installing git and subversion start
sudo apt-get install git subversion -y
echo ==========> installing git and subversion end

echo ==========> installing ruby start
git clone https://github.com/rbenv/rbenv.git ~/.rbenv
git clone https://github.com/rbenv/ruby-build.git ~/.rbenv/plugins/ruby-build
git clone https://github.com/sstephenson/rbenv-gem-rehash.git ~/.rbenv/plugins/rbenv-gem-rehash
sleep 5
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
echo 'export PATH="~/.rbenv/plugins/ruby-build/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(rbenv init -)"' >> ~/.bashrc
source ~/.bashrc
sleep 5
sudo apt-get -y install autoconf bison build-essential libssl-dev libyaml-dev libreadline6-dev zlib1g-dev libncurses5-dev libffi-dev libgdbm3 libgdbm-dev
sudo -H -u vagrant bash -i -c 'rbenv install 2.5.0'
sudo -H -u vagrant bash -i -c 'rbenv global 2.5.0'
sudo -H -u vagrant bash -i -c 'rbenv rehash'
sudo -H -u vagrant bash -i -c 'gem install bundler --no-ri --no-rdoc'
sudo -H -u vagrant bash -i -c 'ruby -v'
echo ==========> installing ruby end

echo ==========> installing Node Js start
curl -sL https://deb.nodesource.com/setup_8.x -o nodesource_setup.sh
sudo bash nodesource_setup.sh
sudo rm -rf /var/lib/apt/lists/partial
sudo apt-get update -y -o Acquire::CompressionTypes::Order::=gz
sudo apt-get install -y nodejs
sudo -H -u vagrant bash -i -c 'nodejs -v'
echo ==========> installing Node Js end

echo ==========> installing rails start
sudo -H -u vagrant bash -i -c 'gem install rails -v 4.2.7'
sudo -H -u vagrant bash -i -c 'rails -v'
echo ==========> installing rails end

echo ==========> installing libmysqlclient and libsqlite3...
sudo apt-get install libaio1 -y
sudo apt-get install libmysqlclient-dev -y
sudo apt-get install libsqlite3-dev -y

echo ==========> installing terminator start
sudo add-apt-repository ppa:gnome-terminator -y
sudo apt-get update -y
sudo apt-get install terminator -y
echo ==========> installing terminator end

sudo apt-get update -y

echo "\n----- Installing Java 8 start ------\n"
sudo add-apt-repository ppa:webupd8team/java -y
sudo apt-get update -y
echo debconf shared/accepted-oracle-license-v1-1 select true | sudo debconf-set-selections
echo debconf shared/accepted-oracle-license-v1-1 seen true | sudo debconf-set-selections
sudo apt-get install oracle-java8-installer -y
sudo apt-get install oracle-java8-set-default -y
echo "\n----- Installing Java 8 end ------\n"

echo ==========> installing Docker and Docker Compose...
#Docker and compose
echo "[vagrant provisioning] Installing Docker start"
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
sudo apt-get update -y
apt-cache policy docker-ce
sudo apt-get install -y docker-ce
sudo usermod -aG docker vagrant
#Add vagrant user to docker group
sudo gpasswd -a vagrant docker
echo "[vagrant provisioning] Installing Docker end"

echo "[vagrant provisioning] Installing docker-compose start"
sudo curl -L https://github.com/docker/compose/releases/download/1.19.0/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
docker-compose --version
echo "[vagrant provisioning] Installing docker-compose end"

echo ==========> Setting Time zone start
echo "[vagrant provisioning] Configuring Timezone & Language..."
sudo timedatectl set-timezone Asia/Kolkata
#sudo apt-get install -y language-pack-ca language-pack-gnome-ca language-pack-ca-base language-pack-gnome-ca-base xinit
#sudo update-locale LANG=ca_ES.UTF-8 LC_MESSAGES=POSIX
echo ==========> Setting Time zone end
	
echo ==========> installing maven 3 start
sudo wget http://www-eu.apache.org/dist/maven/maven-3/3.5.3/binaries/apache-maven-3.5.3-bin.tar.gz
sudo tar xvf apache-maven-3.5.3-bin.tar.gz
sudo mv apache-maven-3.5.3 /opt/
#Add Maven to PATH
echo 'export PATH="/opt/apache-maven-3.5.3/bin:$PATH"' >> ~/.bashrc
echo ==========> installing maven 3 end

sudo apt-get update -y

echo "Installing Apache Web Server start"
sudo apt-get install -y apache2
echo "Installing Apache Web Server end"

echo  ==========> installing Tomcat start
sudo groupadd tomcat
sudo useradd -s /bin/false -g tomcat -d /opt/tomcat tomcat
curl -O https://archive.apache.org/dist/tomcat/tomcat-8/v8.5.5/bin/apache-tomcat-8.5.5.tar.gz
sudo mkdir /opt/tomcat
sudo tar xvf apache-tomcat-8*tar.gz -C /opt/tomcat --strip-components=1
sudo chgrp -R tomcat /opt/tomcat
sudo chmod g+x /opt/tomcat/conf
sudo chmod -R g+r /opt/tomcat/conf
sudo chown -R tomcat /opt/tomcat/webapps /opt/tomcat/work/ /opt/tomcat/temp/ /opt/tomcat/logs/
sudo curl -L https://gist.githubusercontent.com/mamukundan/97be9ef76e52fbfbbd55e2393ad9c62b/raw/15a9b71463631602142135f109e43332c05bcbd5/Apache-Tomcat-8.5-systemctl -o /etc/systemd/system/tomcat.service
sudo systemctl daemon-reload
sudo systemctl start tomcat
sudo systemctl status tomcat
echo  ==========> installing Tomcat end

echo ==========> installing STS start
curl -O http://download.springsource.com/release/STS/3.9.2.RELEASE/dist/e4.7/spring-tool-suite-3.9.2.RELEASE-e4.7.2-linux-gtk-x86_64.tar.gz
sudo tar -zxvf spring-tool-suite*.tar.gz
sudo rm spring-tool-suite*.tar.gz
sudo mv sts-bundle /opt/
sudo ln -s /opt/sts-bundle/sts-3.9.2.RELEASE/STS  /home/vagrant/Desktop
echo ==========> installing STS end

echo ==========> installing groovy start
curl -s get.sdkman.io | bash
source "$HOME/.sdkman/bin/sdkman-init.sh"
sleep 5
sdk install groovy -y
groovy -version
echo ==========> installing groovy end

echo ==========> installing Google Chrome start
wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | sudo apt-key add -
echo 'deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main' | sudo tee /etc/apt/sources.list.d/google-chrome.list
sudo apt-get update -y
sudo apt-get install -y google-chrome-stable
echo ==========> installing Google Chrome end

echo ==========> installing Postman start
wget https://dl.pstmn.io/download/latest/linux64 -O postman.tar.gz
sudo tar -xzf postman.tar.gz -C /opt
rm postman.tar.gz
sudo ln -s /opt/Postman/Postman /usr/bin/postman
sudo ln -s /opt/Postman/Postman /home/vagrant/Desktop
echo ==========> installing Postman end

echo ==========> installing PHP...
sudo add-apt-repository ppa:ondrej/php
sudo apt-get update
sudo apt-get install php7.1 php7.1-mcrypt php7.1-xml php7.1-gd php7.1-opcache php7.1-mbstring
sudo apt-get update
sudo apt-get install apache2 libapache2-mod-php7.1

echo ==========> installing MONGODB...
sudo apt-get install -y mongodb-org
echo "mongodb-org hold" | sudo dpkg --set-selections
echo "mongodb-org-server hold" | sudo dpkg --set-selections
echo "mongodb-org-shell hold" | sudo dpkg --set-selections
echo "mongodb-org-mongos hold" | sudo dpkg --set-selections
echo "mongodb-org-tools hold" | sudo dpkg --set-selections
sudo service mongod start
sleep 10
mongo --host 127.0.0.1:27017

echo ============> Installed Apps
java -version
sudo shutdown -r now
echo provisioning complete.
SCRIPT

VAGRANTFILE_API_VERSION = "2"
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "calthorpeanalytics/ubuntu-desktop-16.04"
  config.vm.box_check_update = false
  config.vm.provision :shell, privileged: false, :inline => $script  
  config.vm.provider "virtualbox" do |vb|
		vb.gui = true
		vb.memory = "4096"
		vb.cpus = 2
		vb.customize ["modifyvm", :id, "--cableconnected1", "on"]
		vb.name = "TVSNEXT-Vagrant-1.0.0"
  end
end
		