# Grafana & Graphite with Ansible

This playbook let you easily setup [Grafana](http://grafana.org/), [Graphite](http://graphite.readthedocs.org/en/latest/) and [StatsD](https://github.com/etsy/statsd/).
You can use it to provison a dedicated server or even a virtual machine using the VagrantFile and Virtualbox

It uses [Ansible](http://www.ansible.com/) as a provisionner, make sure to install Ansible before (version >= 1.6).

What gets installed with this playbook:
*  NginX webserver/reverse proxy
*  Python, Pip & VirtualEnv
*  NodeJS
*  StatsD
*  Grahite with its components:
	* [Carbon](https://github.com/graphite-project/carbon)
	* [Whisper](https://github.com/graphite-project/whisper)
	* [The Graphite webapp](https://github.com/graphite-project/graphite-web)
* And finally [Grafana](http://grafana.org/)

After that you will be able to access:
- **StatsD**: http://www.grafana.dev:8125 (Using UDP)
- **Graphite**: http://www.grafana.dev:81
- **Grafana**: http://www.grafana.dev


## Running it with Vagrant

If you want to install Grafana/Graphite on a VM using Vagrant, you need to install [Vagrant](http://www.vagrantup.com/) and [VirtualBox](https://www.virtualbox.org/) first.

Change "private_ip" from Vagrantfile and vagrant_hosts.

Then hit:
```
$ vagrant up
```

## Deploying with Ansible

Before running Ansible **you have to add the IP address of your target server into the _ansible_hosts_ file**.
```
$ vi ansible_hosts
```

You also need to **change the _remote_user_ variable** in _playbook.yml_ to the actual user you will use to login into your target server:

```
$ ansible-playbook -v -i ansible_hosts playbook.yml -e target=X.X.X.X
```

Also if you are **deploying to Ubuntu**, since you won't be able to login as root, you need to pass the *--sudo -K* option:
```
$ ansible-playbook -v -i ansible_hosts playbook.yml -e target=X.X.X.X --sudo -K
```

*(You should be able to use "target" instead of X.X.X.X in the command line but right now it's not working)*

This ansible role is based on [Guillaume Montard](https://github.com/gmontard/grafana-graphite-statsd-ansible-vagrant/blob/master/LICENSE) repository.