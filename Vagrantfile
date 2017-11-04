# -*- mode: ruby -*-
# vi: set ft=ruby :

########################################
##                                    ##
## GMdotnet                           ##
## Vagrant Multi Machine Amazon AWS   ##
## Version 1.0.0                      ##
##                                    ##
########################################

# Import configuration
require 'yaml'

# Load config.yaml file
vagrantconfig = YAML.load_file('config/config.yaml')

Vagrant.configure("2") do |config|
    vagrantconfig['gmdotnet'].each do |machine|
        # switch from machine to host variable
        host = machine['host']
        if host['enable'] == true
            config.vm.define host['vagrantbox_name'] do |vmhost|
                # hostname
                vmhost.vm.hostname = host['hostname']
                # dummy box
                vmhost.vm.box = "dummy"

                # Hostsupdater plugin
                if host['plugins']['hostsupdater']['enable'] == true

                    # Hostsupdater aliases
                    if host['plugins']['hostsupdater']['aliases'] != nil
                        vmhost.hostsupdater.aliases = host['plugins']['hostsupdater']['aliases']
                    end

                    # Hostsupdater permanent role
                    if host['plugins']['hostsupdater']['permanent'] == true
                        vmhost.hostsupdater.remove_on_suspend = false
                    end

                    # Private IP Network
                    vmhost.vm.network "private_network", ip: host['private_ip'], hostsupdater: true
                else
                    # Private IP Network - without hostsupdater plugin
                    vmhost.vm.network "private_network", ip: host['private_ip']
                end
                ## -*- end hostsupdater plugin folders -*-

                # Rsync folders
                if host['rsync'] != nil
                    host['rsync'].each do |rsync|
                      rsyncoptions = []
                      rsyncexclude = []
                      # rsync folders - owner
                      owner = "vagrant"
                      group = "vagrant"
                      if rsync['folder']['owner'] != nil
                          owner = rsync['folder']['owner']
                      end
                      if rsync['folder']['group'] != nil
                          group = rsync['folder']['group']
                      end
                      rsync['folder']['options'].each do |options|
                          rsyncoptions.push(options)
                      end
                      if rsync['folder']['exclude'] != nil
                          rsync['folder']['exclude'].each do |options|
                              rsyncexclude.push(options)
                          end
                      end
                      vmhost.vm.synced_folder rsync['folder']['host_folder'], rsync['folder']['vagrant_folder'], type: "rsync",
                          rsync__args: rsyncoptions,
                          rsync__exclude: rsyncexclude,
                          owner: owner,
                          group: group
                    end
                end
                ## -*- end rsync folders -*-

                # Amazon AWS options
                vmhost.vm.provider :aws do |provider, override|

                    override.vm.box_url = "https://github.com/mitchellh/vagrant-aws/raw/master/dummy.box"
                    provider.access_key_id = host['aws']['access_key_id']
                    provider.secret_access_key = host['aws']['secret_access_key']
                    provider.session_token = host['aws']['session_token']
                    provider.keypair_name = host['aws']['ssh']['keypair_name']
                    provider.ami = host['aws']['ami']
                    provider.region = host['aws']['region']
                    provider.availability_zone = host['aws']['availability_zone']
                    provider.instance_type = host['aws']['instance_type']
                    provider.security_groups = host['aws']['security_groups']
                    provider.iam_instance_profile_name = host['aws']['iam_instance_profile_name']
                    provider.iam_instance_profile_arn = host['aws']['iam_instance_profile_arn']
                    provider.monitoring = host['aws']['enable_monitoring']
                    provider.terminate_on_shutdown = host['aws']['terminate_on_shutdown']
                    provider.private_ip_address = host['aws']['vpc']['private_ip_address']
                    provider.elastic_ip = host['aws']['vpc']['elastic_ip']
                    provider.subnet_id = host['aws']['vpc']['subnet_id']
                    provider.associate_public_ip = host['aws']['vpc']['associate_public_ip']
                    provider.elb = host['aws']['elb']
                    if host['aws']['block_device_mapping'] != nil
                        provider.block_device_mapping = host['aws']['block_device_mapping']
                    end
                    provider.user_data = host['aws']['user_data']
                    provider.tenancy = host['aws']['tenancy']
                    provider.tags = host['aws']['tags']
                    override.ssh.username = host['aws']['ssh']['override_root_username']
                    override.ssh.private_key_path = host['aws']['ssh']['private_key_path']
                end

                # Shell provision
                if host['provision']['script']['enable'] == true
                    vmhost.vm.provision "shell", path: host['provision']['script']['path']
                end

                # Permanent Shell provision
                if host['provision']['permanent_script']['enable'] == true
                    vmhost.vm.provision "shell", path: host['provision']['permanent_script']['path'],
                    run: 'always'
                end

                # Ansible provision
                if host['provision']['ansible']['enable'] == true
                    vmhost.vm.provision "ansible_local" do |ansible|
                        ansible.verbose = "vv"
                        ansible.playbook =  host['provision']['ansible']['playbook_path']
                    end
                end
            end
            ## -*- end vmhost -*-
        end
        ## -*- end host enable -*-
    end
    ## -*- end machine -*-
end
## -*- end Vagrant -*-
