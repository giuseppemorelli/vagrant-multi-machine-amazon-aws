[![stable version](https://img.shields.io/badge/stable%20version-1.0.0-green.svg?style=flat-square)](https://github.com/gmdotnet/vagrant-multi-machine-amazon-aws/releases/tag/1.0.0)
[![develop](https://img.shields.io/badge/beta%20version-branch%20develop-oran.svg?style=flat-square)](https://github.com/gmdotnet/vagrant-multi-machine-amazon-aws/tree/develop)
[![license](https://img.shields.io/badge/license-OSL--3-blue.svg?style=flat-square)](https://github.com/gmdotnet/vagrant-multi-machine-amazon-aws/blob/master/LICENSE.txt)
[![gitter](https://img.shields.io/gitter/room/nwjs/nw.js.svg)](https://gitter.im/GMdotnet/Lobby?utm_source=share-link&utm_medium=link&utm_campaign=share-link)

# Vagrant Multi Machine Amazon AWS

This is a Multi Machine system for vagrant with Amazon AWS provider.  

## Features
- vagrant multi machine: create multiple machine at the same time
- YAML config file. No more Vagrantfile to edit!
- vagrant provision with script or ansible (configurable via yaml)
- integration - vagrant plugin HostsUpdater: no more /etc/hosts file to edit!

## Requirements
- virtualbox 5.x
- vagrant >1.8.4
- vagrant amazon aws provider plugin
  install with `vagrant plugin install vagrant-aws`
- (optional)vagrant HostsUpdater plugin: https://github.com/cogitatio/vagrant-hostsupdater
  install with `vagrant plugin install vagrant-hostsupdater`
- (in case of error of shared folder mount errors) vagrant vagrant-vbguest plugin (install with the command `vagrant plugin install vagrant-vbguest`)

## How to use
- download https://github.com/gmdotnet/vagrant-multi-machine-amazon-aws/archive/master.zip
- unzip on your favorite work folder
- rename `config/config.yaml.sample` in `config/config.yaml`
- change settings in `config/config.yaml`
(if you need more information about rsync folder just have a look here: https://www.vagrantup.com/docs/synced-folders/basic_usage.html)
- run `vagrant up` on folder where is `Vagrantfile`
- (optional) make your configuration on vagrant machine entering by run `vagrant ssh`
- have fun and happy coding!

## Contribution
Any contribution is highly appreciated. The best way to contribute code is to open a [pull request on GitHub](https://help.github.com/articles/using-pull-requests).  
Please create your pull request against the `develop` branch

### Credits
Giuseppe Morelli - [giuseppemorelli.net](https://www.giuseppemorelli.net)
