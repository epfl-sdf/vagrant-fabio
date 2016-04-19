# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu/trusty64"
   config.vm.provision "shell", inline: <<-SHELL
     sudo apt-get update
     sudo apt-get upgrade --yes
     sudo apt-get install golang-1.6 git --yes
     mkdir /opt/gopath || echo "/opt/gopath already exists"
     export GOPATH=/opt/gopath
     mkdir /opt/fabio || echo "/opt/fabio already exists"
     pushd /opt/fabio
     /usr/lib/go-1.6/bin/go get github.com/eBay/fabio
     popd
     mkdir /opt/consul|| echo "/opt/consul already exists"
     pushd /opt/consul
     /usr/lib/go-1.6/bin/go get github.com/hashicorp/consul
     popd
     
     sudo apt-get install supervisor --yes
cat << EOF | sudo tee '/etc/supervisor/conf.d/consul.conf'
[program:consul]
command=/opt/gopath/bin/consul agent -bootstrap -server --data-dir /opt/consul

EOF

    cat << EOF | sudo tee '/etc/supervisor/conf.d/fabio.conf'
[program:fabio]
command=/opt/gopath/bin/fabio
EOF

   /etc/init.d/supervisor restart

   SHELL
end
