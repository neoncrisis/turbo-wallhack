VAGRANTFILE_API_VERSION = '2'
VAGRANT_DEFAULT_PROVIDER = 'virtualbox'

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.user.defaults = {
      # Virtualbox Provider Settings
      :virtualbox => {
         :memory => 512,
         :cpus => 1,
      },
      # Digital Ocean Provider Settings
      :digital_ocean => {
          :ssh_key => '~/.ssh/id_rsa',
          :memory => '512mb',
          :region => 'nyc2',
      },
      # Ansible Provisioner Settings
      :ansible => {
          :username => 'vagrant',
          :groupname => 'vagrant',
          :code_dir => '/code',
      },
      # Vagrant settings
      :vagrant => {
          :db_port => 8081,
          :app_port => 8080,
      },
  }

  config.vm.network :forwarded_port, guest: 5432, host: config.user.vagrant.db_port
  config.vm.network :forwarded_port, guest: 8080, host: config.user.vagrant.app_port
  config.vm.synced_folder config.user.vagrant.code_dir, config.user.ansible.code_dir,
                          owner: config.user.ansible.username, group: config.user.ansible.groupname

  config.vm.provider :virtualbox do |vbox, override|
    override.vm.box = 'ubuntu/trusty32'

    vbox.memory = config.user.virtualbox.memory
    vbox.cpus = config.user.virtualbox.cpus
  end

  config.vm.provider :digital_ocean do |ocean, override|
    override.vm.box = 'digital_ocean'
    override.vm.box_url = 'https://github.com/smdahlen/vagrant-digitalocean/raw/master/box/digital_ocean.box'

    # We don't want to use the vagrant insecure key for our droplet
    override.ssh.username = 'vagrant'
    override.ssh.private_key_path = config.user.digital_ocean.ssh_key

    ocean.image = '10.04 x32'
    ocean.token = config.user.digital_ocean.api_token
    ocean.size = config.user.digital_ocean.memory
    ocean.region = config.user.digital_ocean.region
  end

  config.vm.provision :ansible do |ansible|
    ansible.playbook = 'ansible/setup.yml'
    ansible.groups = { 'dbservers' => 'default', 'webservers' => 'default' }
    ansible.extra_vars = config.user.ansible
  end
end

# Allow for easy extension of Vagrant file
load 'DevVagrantfile' if File.exist?('DevVagrantfile')