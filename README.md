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
- vagrant amazon aws provider plugin install with `vagrant plugin install vagrant-aws` (https://github.com/mitchellh/vagrant-aws)
- (optional)vagrant HostsUpdater plugin: https://github.com/cogitatio/vagrant-hostsupdater
  install with `vagrant plugin install vagrant-hostsupdater`
- (in case of error of shared folder mount errors) vagrant vagrant-vbguest plugin (install with the command `vagrant plugin install vagrant-vbguest`)
- amazon aws account (admin or IAM account)
- Amazon VPC default created

## How to use
- download latest version [https://github.com/gmdotnet/vagrant-multi-machine-amazon-aws/releases](https://github.com/gmdotnet/vagrant-multi-machine-amazon-aws/releases)
- extract on your favorite work folder
- rename `config/config.yaml.sample` in `config/config.yaml`
- change settings in `config/config.yaml`
- run `vagrant up` on folder where is `Vagrantfile`
- (optional) make your configuration on vagrant machine entering by run `vagrant ssh`
- have fun and happy coding!

### Config file `config.yaml`

Tab indent: 4 spaces or tab

| Field                               | Type          | Description                                             | Note  |
| ----------------------------------- | ------------- | ------------------------------------------------------- | ----- |
| host                                | group field   | Each group of `host` create a machine in Amazon AWS EC2     |       |
| enable                              | boolean       | Enable or not the machine                               | Disabled machine aren't managed by Vagrant file, so if you want to destroy it you have to make this flag with `yes` |
| vagrantbox_name                     | string        | Name for vagrant software                               |       |
| hostname                            | string        | Hostname of the machine                                 |       |
|                                     |               |                                                         |       |
| **aws**                             |               |                                                         |       |
| aws > access_key_id                 | string        | The access key for accessing AWS | |
| aws > secret_access_key             | string        | The secret access key for accessing AWS | |
| aws > session_token                 | string | The session token provided by STS | Can be empty |
| aws > ami                           | string | The AMI id to boot, such as "ami-11c57862" for debian | |
| aws > region                    | string | The region to start the instance in, such as "eu-west-1a" | See [aws_regionlist.md](aws_regionlist.md) | 
| aws > availability_zone          | string | The availability zone within the region to launch the instance. If empty, it will use the default set by Amazon. | |
| aws > instance_type             | string | The type of instance, such as "m3.medium". The default value of this if not specified is "m3.medium". "m1.small" has been deprecated in "us-east-1" and "m3.medium" is the smallest instance type to support both paravirtualization and hvm AMIs | See [aws_regionlist.md](aws_regionlist.md) |  
| aws > security_groups           | array | An array of security groups for the instance. If this instance will be launched in VPC, this must be a list of security group Name. For a nondefault VPC, you must use security group IDs instead (http://docs.aws.amazon.com/cli/latest/reference/ec2/run-instances.html). | Leave empty if you want to use 'default security group', otherwise use array like ['your-security-group'] |
| aws > iam_instance_profile_arn     | string | The Amazon resource name (ARN) of the IAM Instance Profile to associate with the instance | |
| aws > iam_instance_profile_name     | string | The name of the IAM Instance Profile to associate with the instance | |
| aws > tenancy                   | string | When running in a VPC configure the tenancy of the instance. | Support: 'default' or 'dedicated'| 
| aws > enable_monitoring         | bool | Set to "true" to enable detailed monitoring. | |
| aws > terminate_on_shutdown     | bool | Indicates whether an instance stops or terminates when you initiate shutdown from the instance. | |
| aws > elb                       | string | The ELB (elastic load balancer) name to attach to the instance. | |
| aws > block_device_mapping      | array | Amazon EC2 Block Device Mapping Property | "[{ 'DeviceName' => '/dev/sda1', 'Ebs.VolumeSize' => 50 }]" copy to increase disk size |
| aws > user_data                 | string | You can specify user data for the instance being booted. | |
| aws > tags                 | array | A hash of tags to set on the machine. | |
| aws > vpc > private_ip_address    | ipv4 | The private IP address to assign to an instance within a [VPC](http://aws.amazon.com/vpc/) | |
| aws > vpc > elastic_ip            | ipv4 | Can be set to 'true', or to an existing Elastic IP address. If true, allocate a new Elastic IP address to the instance. If set to an existing Elastic IP address, assign the address to the instance. | |
| aws > vpc > subnet_id            | string | The subnet to boot the instance into, for VPC. | | 
| aws > vpc > associate_public_ip            | boot | If true, will associate a public IP address to an instance in a VPC. | |
| aws > ssh > keypair_name          | string | The name of the keypair to use to bootstrap AMIs which support it. | | 
| aws > ssh > private_key_path      | string | Absolute or relative path (from Vagrantfile) to your ssh private key | | 
| aws > ssh > override_root_username    | string | Ovverride root with custom user | |
|                                     |               |                                                         |       |
| **provision**                       |               |                                                         |       |
| provision > ansible > enable                  | boolean       | Enable Ansible provisioning                             |       |
| provision > ansible > playbook_path           | string        | Relative path from Vagrantfile of your Ansible Playbook |       |
| provision > script > enable                   | boolean       | Enable script provisioning                              |       |
| provision > script > path                     | string        | Relative path from Vagrantfile of your script           |       |
|                                     |               |                                                         |       |
| **plugins**                          |               |                                                         |       |
| plugins > hostsupdater > enable             | boolean       | Enable or not hostsupdater plugin                       | https://github.com/cogitatio/vagrant-hostsupdater |
| plugins > hostsupdater > permanent          | boolean       | Your changes to /etc/hosts will be permanent            | Only if you destroy the machine, entries in /etc/hosts will be removed |
| plugins > hostsupdater > aliases            | array         | domain aliases for the same ip                          | Leave blank for no aliases |
|                                     |               |                                                         |       |
| **rsync**                           |               |                                                         |       |
| rsync > folder                            | group field   | Group of sync folder via rsync                          |       |
| rsync > folder > host_folder              | string        | Path of the folder on your machine                      |       |
| rsync > folder > vagrant_folder           | string        | Path of the folder inside vagrant machine               |       |
| rsync > folder > options                  | group field   | Rsync parameters. One per line                          | https://www.vagrantup.com/docs/synced-folders/rsync.html |
| rsync > folder > exclude                  | group field   | Exclude folders from rsync                              | https://www.vagrantup.com/docs/synced-folders/rsync.html#rsync__exclude |

## Find out GMdotnet Vagrant projects
- Vagrant multi machine for **Virtualbox**: [vagrant-multi-machine-virtualbox](https://github.com/gmdotnet/vagrant-multi-machine-virtualbox)
- Vagrant multi machine for **Digital Ocean**: [vagrant-multi-machine-digital-ocean](https://github.com/gmdotnet/vagrant-multi-machine-digital-ocean)

## Contribution
Any contribution is highly appreciated. The best way to contribute code is to open a [pull request on GitHub](https://help.github.com/articles/using-pull-requests).  
Please create your pull request against the `develop` branch

### Credits
Giuseppe Morelli - [giuseppemorelli.net](https://www.giuseppemorelli.net)
