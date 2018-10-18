VAGRANTFILE_API_VERSION = "2"

$conda_installation = <<SCRIPT
  sudo systemctl disable firewalld
  sudo sed -i 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/selinux/config
  # sudo yum -y install bzip2
  # sudo yum -y groupinstall 'Development Tools'
  curl https://repo.continuum.io/archive/Anaconda3-4.2.0-Linux-x86_64.sh -o anaconda.sh || exit 1;
  bash anaconda.sh -b || exit 1;
  grep -q anaconda ~/.bashrc;
  if [[ ${?} -ne 0 ]]; then
    echo "export PATH=\"${HOME}/anaconda3/bin:\${PATH}\"" >> ~/.bashrc
  fi
  source ~/.bashrc
  conda info || exit 1;
  rm anaconda.sh || exit 1;
  conda clean -t || exit 1;
SCRIPT

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "ubuntu/bionic64"
  config.vm.hostname = "conda"
  config.vm.network "forwarded_port", guest: 8888, host: 8080
  config.vm.provider "virtualbox" do |v|
    v.linked_clone = true
    v.memory = "1024"
    v.cpus = "4"
  end

  # config.vm.provision "shell", inline: <<-SHELL
  #   # sudo yum -y groupinstall "GNOME Desktop"
  #   # sudo systemctl set-default graphical.target
  #   # sudo systemctl start graphical.target
  # SHELL

  # config.vm.provision "shell", inline: $conda_installation,  privileged: false
  # config.vm.provision "shell", run: "always", inline: <<-SHELL
  #   su -c 'jupyter notebook --notebook-dir=/vagrant/notebook --no-browser --ip=0.0.0.0 &' - vagrant
  # SHELL

end
