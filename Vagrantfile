# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure('2') do |config|
  config.vm.box = 'ubuntu/focal64'
  config.vm.hostname = 'powerline'

  config.vm.provider 'virtualbox' do |vb|
    vb.gui = false
    vb.memory = 1024
    vb.cpus = 1
  end

  if Vagrant.has_plugin?('vagrant-vbguest')
    config.vbguest.auto_update = true
  end

  config.vm.synced_folder 'home/', '/vagrant',
    automount: true,
    type: 'virtualbox',
    owner: 'vagrant',
    group: 'vagrant'

  config.vm.provision 'shell',
    inline: <<-'EOT'
      apt-get update
      apt-get dist-upgrade -y
      apt-get install -y \
          nodejs \
          npm \
          python3-hglib \
          python3-pip \
          python3-psutil \
          python3-pygit2 \
          python3-vcstools \
          python3-venv \
          socat \
          tree \
          vim
    EOT

  config.vm.provision 'shell',
    privileged: false,
    inline: <<-'EOT'
      set -o errexit
      set -o nounset
      set -o pipefail

      ln -fvs /vagrant/.config /home/vagrant/.config
    EOT

  #config.vm.provision 'shell',
  #  reboot: true

end
