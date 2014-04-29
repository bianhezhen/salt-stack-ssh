# You need saucy or later for Ubuntu in order to get an ssh server which has AuthorizedKeysCommand
# https://github.com/mitchellh/vagrant/issues/1709
BASE_BOX="http://cloud-images.ubuntu.com/vagrant"
SAUCY64_URL="#{BASE_BOX}/saucy/current/saucy-server-cloudimg-amd64-vagrant-disk1.box"

Vagrant.configure("2") do |config|
  config.vm.box = "saucy64"
  config.vm.box_url = SAUCY64_URL

  config.ssh.forward_agent = true

  config.vm.define :"salt-master" do |master|
    master.vm.network "private_network", ip: "10.47.94.2"
    master.vm.network :forwarded_port, guest: 53, host: 53
    master.vm.hostname = "master"
    master.vm.synced_folder "srv/runners", "/srv/runners"
    master.vm.synced_folder "srv/reactor", "/srv/reactor"

    master.vm.provision :salt do |salt|
      salt.verbose = true
  
      salt.install_master = true
      salt.no_minion = true
    end
  end

  config.vm.define :"client" do |client|
    client.vm.hostname = "client"

    client.vm.provision :salt do |salt|
        salt.verbose = true
        
    end
  end
end
