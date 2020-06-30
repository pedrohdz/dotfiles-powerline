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
          python3-pip \
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

      POWERLINE_HOME="$HOME/.local/opt/python/powerline"
      LOCAL_BIN="$HOME/.local/bin"

      mkdir -p "$POWERLINE_HOME"
      mkdir -p "$LOCAL_BIN"

      python3 -m venv "$POWERLINE_HOME"
      $POWERLINE_HOME/bin/pip install -U --upgrade-strategy eager pip
      $POWERLINE_HOME/bin/pip install -U --upgrade-strategy eager \
          powerline-status \
          psutil \
          pygit2 \
          python-hglib \
          pyuv \
          vcstools

      ln -svf ../opt/python/powerline/bin/powerline $LOCAL_BIN/powerline
      ln -svf ../opt/python/powerline/bin/powerline-config $LOCAL_BIN/powerline-config
      ln -svf ../opt/python/powerline/bin/powerline-daemon $LOCAL_BIN/powerline-daemon
      ln -svf ../opt/python/powerline/bin/powerline-lint $LOCAL_BIN/powerline-lint
      ln -svf ../opt/python/powerline/bin/powerline-render $LOCAL_BIN/powerline-render
      ln -svf ../opt/python/powerline/lib/python3.8/site-packages/powerline/bindings/tmux/powerline.conf $LOCAL_BIN/powerline.conf
      ln -svfn /vagrant/.config /home/vagrant/.config

      [ -d "$HOME/.homesick/repos/homeshick" ] \
          || git clone https://github.com/andsens/homeshick.git "$HOME/.homesick/repos/homeshick"

      HOMESHICK_LINE='source "$HOME/.homesick/repos/homeshick/homeshick.sh"'
      grep -qxF "$HOMESHICK_LINE" "$HOME/.bashrc" \
          || echo -e "\n$HOMESHICK_LINE\n" >> "$HOME/.bashrc"

      source "$HOME/.homesick/repos/homeshick/homeshick.sh"

      [ -d "$HOME/.homesick/repos/dr-tmux" ] \
          || homeshick clone -b 'https://github.com/pedrohdz/dr-tmux.git'
      homeshick symlink -bf dr-tmux
    EOT

  #config.vm.provision 'shell',
  #  reboot: true

end
