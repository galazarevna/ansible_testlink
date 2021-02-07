# Ansible Playbook for TestLink

An Ansible playbook of Deploy TestLink with Nginx, PHP 7.4 (php-fpm), and MySQL 5.6+ on CentOS 8.

## What is TestLink?
TestLink is an open-source web-based test management system that facilitates software quality assurance.
The platform offers support for test cases, test suites, test plans, test projects and user management, as well as various reports and statistics.

http://testlink.org/

## What is Ansible?
Ansible is an open-source software provisioning, configuration management, and application-deployment tool enabling infrastructure as code.

https://www.ansible.com/

## What is Vagrant?
Vagrant is an open-source software product for creating and configuring lightweight, reproducible, and portable development environments.

https://www.vagrantup.com/

## Structure
```
ansible_testlink
├── ansible.cfg
├── hosts
├── README.md
├── roles
│   ├── company_settings
│   │   └── tasks
│   │       └── main.yml
│   ├── mysql
│   │   ├── handlers
│   │   │   └── main.yml
│   │   └── tasks
│   │       └── main.yml
│   ├── nginx
│   │   ├── handlers
│   │   │   └── main.yml
│   │   ├── tasks
│   │   │   └── main.yml
│   │   └── templates
│   │       └── nginx.conf.j2
│   ├── php-fpm
│   │   ├── handlers
│   │   │   └── main.yml
│   │   └── tasks
│   │       └── main.yml
│   ├── system
│   │   └── tasks
│   │       └── main.yml
│   └── testlink
│       ├── handlers
│       │   └── main.yml
│       └── tasks
│           └── main.yml
├── site.yml
└── vars
    └── main.yml
```

## Prerequisites

Install [Vagrant](https://www.vagrantup.com/) and [Virtual Box](https://www.virtualbox.org/) on a control node with a package manager.<br/>
For Mac users who use a homebrew package manager run:

  ```sh
  brew install virtual box
  brew install vagrant
  ```

In your projects directory create a TestLink directory

  ```sh
  mkdir TestLink
  cd TestLink
  ```

Copy the patched [Vagrantfile](https://github.com/galazarevna/ansible_testlink/blob/master/Vagrantfile) to the TestLink directory. Then run: 

  ```sh
  vagrant up
  ```

Check if you are able to ssh to the VM by running

  ```sh
  vagrant ssh
  ```
You might want to open VirtualBox for debugging purposes (if needed).

## Requirements
* Navigate to your projects directory and clone the repository<br/>

  ```sh
  git clone https://github.com/galazarevna/ansible_testlink.git
  cd ansible_testlink
  ```

* Create and activate a [python3 virtual environment](optional)

* Install Ansible on a control node with ```pip``` or a package manager.<br/>
For Mac users who use a homebrew package manager run:

  ```sh
  brew install ansible
  ```

* Put mysql root password of your choice into the `vars/main.yml` file.

* Put the path to your TestLink directory `/Users/<user_name>/<your_projects>/TestLink` into the 'hosts' inventory file.<br/>
You can change the hosts settings in the 'hosts' inventory file (localhost:2222 is used in the playbook) 

* Check the ssh connection to your managed node (vagrant CentOS 8 VM)

  ```sh
  ansible all -m ping
  ```
Once you have "pong" in the response you are good to go!

## Playbook Usage

* To run the playbook on your local (vagrant) machine

  ```sh
  ansible-playbook -i hosts site.yml
  ```

## TestLink Installation
* Once you provisioned the VM, navigate to the TestLink Installation page http://127.0.0.1:8080/install/index.php<br/>
You might want to forward the port 2222 by running:

  ```sh
  ssh vagrant@127.0.0.1 -p 2222 -i "/Users/<user_name>/<your_projects>/TestLink/.vagrant/machines/default/virtualbox/private_key" -L 8080:127.0.0.1:80
  ```

* Follow the installation steps:
  * provide the mysql root password set in the Reqiurements section
  * set any username and password for the "testlink" db user

* Remove password for the "testlink" db user created in the previous step from the `/var/www/testlink/config_db.inc.php` <br/>
`define('DB_PASS', '');`

* Navigate to http://localhost:8080/testlink/ <br/>
Log into the TestLink with the default credentials - login: "admin", password: "admin"
