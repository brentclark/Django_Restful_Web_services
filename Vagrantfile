Vagrant.configure("2") do |config|
  config.vm.box = "debian/stretch64"
  config.vm.network :forwarded_port, guest: 8000, host: 8081
  config.vm.synced_folder '.', '/vagrant', disabled: true      # Switch off rsync
  config.vm.synced_folder "/home/bclark/Dropbox/Vagrant_Projects/Restful/src", "/home/vagrant/src", type: "sshfs"
  config.vm.provision :shell, :inline => "sudo rm /etc/localtime && sudo ln -s /usr/share/zoneinfo/Africa/Johannesburg /etc/localtime", run: "always"  
  config.vm.provision 'shell', inline: <<-SHELL

cat <<-'EOF' >/etc/apt/apt.conf.d/01norecommend
APT::Install-Recommends "0";
APT::Install-Suggests "0";
EOF

    #export DEBIAN_FRONTEND=noninteractive
    echo 'deb http://chs-mirror02-v.jnb1.chs.hetzner.co.za/debian/ stretch main contrib non-free' > /etc/apt/sources.list
    echo 'deb http://chs-mirror02-v.jnb1.chs.hetzner.co.za/debian-security/ stretch/updates main contrib non-free' >> /etc/apt/sources.list
    echo 'deb http://chs-mirror02-v.jnb1.chs.hetzner.co.za/debian/ stretch-backports main contrib non-free' >> /etc/apt/sources.list
    echo 'deb http://chs-mirror02-v.jnb1.chs.hetzner.co.za/debian/ stretch-updates main contrib non-free' >> /etc/apt/sources.list

    /usr/bin/apt-get update

    DEBIAN_FRONTEND=noninteractive /usr/bin/apt-get -fy --no-install-recommends --no-install-suggests dist-upgrade 
    DEBIAN_FRONTEND=noninteractive /usr/bin/apt-get -fy --no-install-recommends --no-install-suggests install \
      apt-transport-https \
      build-essential \
      exim4 \
      net-tools \
      ntp \
      nginx \
      python3 \
      python3-dev \
      python3-minimal \
      python3-openssl \
      python3-pip \
      python3-setuptools \
      sqlite3 \
      tmux \
      unattended-upgrades \
      vim \
      vim-python-jedi \
      vim-scripts \
      vim-tabular 

    sudo -u vagrant vim-addons install python-jedi

    # Python modules
    sudo -u vagrant pip3 install django==1.11
    sudo -u vagrant pip3 install django-extensions
    sudo -u vagrant pip3 install djangorestframework gunicorn httpie

    sudo -u vagrant ln -s .local/bin/ 
    sudo -u vagrant ln -s src/.bash_history .bash_history

cat <<-'EOF' >/etc/apt/apt.conf.d/20auto-upgrades
APT::Periodic::Update-Package-Lists "1";
APT::Periodic::Download-Upgradeable-Packages "1";
APT::Periodic::AutocleanInterval "7";
APT::Periodic::Unattended-Upgrade "1";
EOF

    /etc/init.d/unattended-upgrades restart

    cat <<-'EOF' >/home/vagrant/.vimrc
set mouse:r
syntax on
filetype indent plugin on
set tabstop=4
set expandtab
set shiftwidth=4
set softtabstop=4
EOF
  SHELL
end

# See this:
# https://securityblog.gr/4381/apache2-and-django-installation-on-debian/
# cd ~/src/mysite/ ; python3 manage.py makemigrations ; python3 manage.py migrate ; python3 manage.py runserver 0.0.0.0:8000
