Exakat-vagrant
==============

Builds a vagrant with [exakat](http://www.exakat.io/) and all dependencies installed.

How do I use it?
----------------

Make sure you have all pre-requisite in your host machine:

* Ansible
* Vagrant
* Virtualbox
* Git


Then :

    git clone https://github.com/exakat/exakat-vagrant.git exakat-vagrant
    cd exakat-vagrant
    // Check for the Vagrant files to adapt it to your configuration
    vagrant up --provision

More details
------------

* PHP binary for exakat execution (PHP with curl, tokenizer and sqlite3).
* PHP 7.1-dev (master), 7.0, 5.6, 5.5 for exakat analysis
* Neo4j 2.2.7 and gremlin 2.0 plugin, running on Java 8

This box is sized to handle projects up to 100k LOC of PHP. Larger projects do require more RAM and adaptation in config/config.ini.

The ansible playbook may be used independantly to install exakat only : see role ['exakat'](https://github.com/exakat/exakat-vagrant/blob/master/.ansible/roles/exakat/tasks/main.yml).

Put your projects of exakat inside ./projects, this folder will be automatically shared with the VM so you can execute exakat from inside the machine on your stuff.

Author
------

This repository is the brain child of [Alexis von Glasow](https://github.com/vonglasow), with the help of contributors.
