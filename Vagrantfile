# this vagrant-file needs the following plugins:
# * vagrant-hostmaster

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"
Vagrant.require_version ">= 1.5"
require 'yaml'

path = "#{File.dirname(__FILE__)}"
yamlFile = path + '/fabfile.yaml'
if !File.exist?(yamlFile)
  print "* No yaml configuration file found * \n"
  exit
end

yamlConfig = YAML.load_file(yamlFile)
#sitename
sitename = yamlConfig['hosts']['local']['host']
# The project name is base for directories & the docker image name
project_name = yamlConfig['name']


# IP Address for the host only network, change it to anything you like
# but please keep it within the IPv4 private network range
ip_address = "33.33.33.18"


# Check hostmanager required plugin
REQUIRED_PLUGINS = %w(vagrant-hostmanager)
exit unless REQUIRED_PLUGINS.all? do |plugin|
  Vagrant.has_plugin?(plugin) || (
    puts "The #{plugin} plugin is required. Please install it with:"
    puts "$ vagrant plugin install #{plugin}"
    false
  )
end



Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "phusion-open-ubuntu-14.04-amd64"
  config.vm.box_url = "https://oss-binaries.phusionpassenger.com/vagrant/boxes/latest/ubuntu-14.04-amd64-vbox.box"
  # Or, for Ubuntu 12.04:
  #config.vm.box = "phusion-open-ubuntu-12.04-amd64"
  #config.vm.box_url = "https://oss-binaries.phusionpassenger.com/vagrant/boxes/latest/ubuntu-12.04-amd64-vbox.box"

  config.vm.provider :vmware_fusion do |f, override|
    override.vm.box_url = "https://oss-binaries.phusionpassenger.com/vagrant/boxes/latest/ubuntu-14.04-amd64-vmwarefusion.box"
    #override.vm.box_url = "https://oss-binaries.phusionpassenger.com/vagrant/boxes/latest/ubuntu-12.04-amd64-vmwarefusion.box"
  end

  config.vm.network :private_network, ip: ip_address

  # Use hostonly network with a static IP Address and enable
  # hostmanager so we can have a custom domain for the server
  # by modifying the host machines hosts file
  config.hostmanager.enabled = true
  config.hostmanager.manage_host = true
  config.vm.hostname = sitename
  config.hostmanager.aliases = [ "www." + sitename ]
  config.vm.provision :hostmanager

  config.ssh.forward_agent = true

  config.vm.synced_folder ".", "/vagrant", nfs: true

  #increase memory
  config.vm.provider "virtualbox" do |v|
    v.customize ["modifyvm", :id, "--cpuexecutioncap", "80"]
    v.customize ["modifyvm", :id, "--memory", 4536]
    v.cpus = 2
  end

  config.vm.provider "vmware_fusion" do |v|
    v.vmx["memsize"] = "1536"
    v.vmx["numvcpus"] = "2"
  end

  # Configure The Public Key For SSH Access
  #config.vm.provision "shell" do |s|
  #  s.inline = "echo $1 | tee -a /home/vagrant/.ssh/authorized_keys"
  #  s.args = [File.read(File.expand_path('~/.ssh/id_rsa.pub'))]
  #end

  if Dir.glob("#{File.dirname(__FILE__)}/.vagrant/machines/default/*/id").empty?
    # Install Docker
    pkg_cmd = "wget -q -O - https://get.docker.io/gpg | apt-key add -;" \
      "echo deb http://get.docker.io/ubuntu docker main > /etc/apt/sources.list.d/docker.list;" \
      "apt-get update -qq; apt-get install -q -y --force-yes lxc-docker; "
    # Add vagrant user to the docker group
    pkg_cmd << "usermod -a -G docker vagrant; "
    config.vm.provision :shell, :inline => pkg_cmd
  end

  # run or rebuild docker image
  config.vm.provision "shell",run: "always" do |s|
    if ARGV[0] == 'provision'
      s.inline = "cd /vagrant/_tools/docker ; bash ./run.sh $1 $2 --webRoot $3 --http 80 --ssh 222  --vhost $4 --rebuild"
    else
      s.inline = "cd /vagrant/_tools/docker ; bash ./run.sh $1 $2 --webRoot $3 --http 80 --ssh 222  --vhost $4"
    end
      s.privileged = true
    s.args = [project_name, '/vagrant', yamlConfig['hosts']['local']['rootFolder'], sitename]
  end

  if File.file?('vagrant.local')
    load "./vagrant.local"
  end
end
