# -â€‹*- mode: ruby -*â€‹-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"
$install_ansible = <<SCRIPT
sudo apt-get install build-essential libssl-dev libffi-dev python-dev -yf
sudo pip install cryptography --upgrade
sudo apt-get purge -y python-pip
sudo wget https://bootstrap.pypa.io/get-pip.py
sudo python ./get-pip.py
sudo apt-get install python-pip
sudo pip install ansible --upgrade

SCRIPT

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.vm.define "default"
  config.vm.provider "virtualbox"

  if OS.mac? then
    ## Tested and works (2017-02), --provider=virtualbox (5.0 max)
    config.vm.box = "debian/jessie64"
    ## Tested and works (2017-02), --provider=virtualbox
    #    config.vm.box = "parallels/debian-8.2"
    ## Tested and failed (2017-02)
    #  config.vm.box = "parallels/debian-8.6"
  else
    config.vm.box = "debian/jessie64"
  end

  config.vm.hostname = "vagrant-exakat"

  config.vm.provider :virtualbox do |vb|
    vb.gui = false
    vb.customize ["modifyvm", :id, "--memory", "2048", "--cpus", "2"]
  end

  config.vm.network "forwarded_port", guest: 7447, host: 7447

  config.vm.provision 'shell', inline: $install_ansible
  config.vm.provision "shell" do |s|
    s.inline = '[[ ! -f $1 ]] || grep -F -q "$2" $1 || sed -i "/__main__/a \\    $2" $1'
    s.args = ['/usr/bin/ansible-galaxy', "if sys.argv == ['/usr/bin/ansible-galaxy', '--help']: sys.argv.insert(1, 'info')"]
  end
  config.vm.provision :ansible_local do |ansible|
    ansible.verbose = "vvvv"
    ansible.raw_arguments = ['--timeout=300']
    ansible.sudo = true
    ansible.playbook = ".ansible/exakat.yml"
  end

  config.vm.post_up_message = "    Welcome to Exakat !"
end

module OS
    def OS.windows?
        (/cygwin|mswin|mingw|bccwin|wince|emx/ =~ RUBY_PLATFORM) != nil
    end

    def OS.mac?
        (/darwin/ =~ RUBY_PLATFORM) != nil
    end

    def OS.unix?
        !OS.windows?
    end

    def OS.linux?
        OS.unix? and not OS.mac?
    end
end
