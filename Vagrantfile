Vagrant.configure(2) do |config|

  config.vm.box = "ubuntu/trusty64"
  config.vm.box_check_update = true

  config.vm.provider "virtualbox" do |vb|
    vb.memory = "8192"
  end

  config.vm.network :private_network, ip: "192.168.50.4"

  # ----------------------------------------------------------------------------
  # Global Stuff
  # ----------------------------------------------------------------------------

  config.vm.provision "shell", inline: <<-SHELL

    locale-gen en_GB.UTF-8

    apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 575159689BEFB442
    echo 'deb http://download.fpcomplete.com/ubuntu trusty main'|sudo tee /etc/apt/sources.list.d/fpco.list
    apt-get update
    apt-get -y install git
    apt-get -y install stack
    apt-get -y install hugs

    apt-get -y install zsh
    chsh -s $(which zsh) vagrant

  SHELL

  # ----------------------------------------------------------------------------
  # User Stuff
  # ----------------------------------------------------------------------------

  config.vm.provision "shell", privileged: false, inline: <<-SHELL

    sudo sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
    sudo chown -R vagrant: .

    echo 'export PATH=$PATH:~/.local/bin' >> ~/.zshrc
    echo 'alias hs="/vagrant"' >> ~/.zshrc

    rm -rf .oh-my-zsh/custom
    git clone git@github.com:grancalavera/custom-oh-my-zsh.git .oh-my-zsh/custom
    source ~/.zshrc

    stack setup
    stack install cabal-install
  SHELL

end
