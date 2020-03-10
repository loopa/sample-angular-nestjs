Vagrant.configure('2') do |config|
  config.vm.box = 'ubuntu/xenial64'

  config.vm.hostname = 'dev-vm'

  config.vm.network :private_network, ip: '192.168.50.10'

  config.vm.provider :virtualbox do |vb|
    vb.gui = false
    vb.cpus = 4
    vb.memory = 8192
    vb.customize ['modifyvm', :id, '--natdnsproxy1', 'off']
    vb.customize ['modifyvm', :id, '--natdnshostresolver1', 'off']
  end

  config.disksize.size = '30GB'
  config.mutagen.orchestrate = true

  config.vm.synced_folder './', '/home/vagrant/app', type: "rsync",
    rsync_auto: true,
    rsync__exclude: ['.git/', 'node_modules/', 'log/', 'tmp/']

  config.vm.provision 'shell', inline: <<-SHELL
    apt-get update && upgrade -y
  
    # install nodejs , npm
    curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -
    sudo apt-get install -y nodejs
     
    # install docker , docker-compose
    curl -fsSL https://get.docker.com -o get-docker.sh
    sh get-docker.sh
    usermod -aG docker vagrant

    curl -L "https://github.com/docker/compose/releases/download/1.25.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
    chmod +x /usr/local/bin/docker-compose

    # after 
    # cd app
    # npx create-nx-workspace demo --appName=web --style=scss --preset=angular-nest
    # yes... yes...
    # something...

    # vagrant halt && vagrant destroy -f && vagrant up
  SHELL
end