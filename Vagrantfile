# -â€‹*- mode: ruby -*â€‹-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.vm.define "default"
  config.vm.provider "virtualbox"
  vagrant_dir = File.expand_path(File.dirname(__FILE__))
  config.vm.synced_folder "#{vagrant_dir}/projects", "/home/vagrant/projects"

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

  config.vm.provision :ansible_local do |ansible|
    ansible.verbose = "vvvv"
    ansible.install_mode = "pip"
    ansible.raw_arguments = ['--timeout=300']
    ansible.become = true
    ansible.playbook = ".ansible/exakat.yml"
    ansible.version = "latest"
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
