require 'yaml'

Vagrant.configure("2") do |config|
    # Load settings
    settings = YAML::load(File.open('provision/settings.yml'))

    # VM
    config.vm.define settings["server"]["name"] do |server|
        # Box
        server.vm.box = settings["server"]["box"]

        # Hostname
        server.vm.hostname = settings["server"]["hostname"]

        # Network
        server.vm.network :private_network, ip: settings["server"]["private_ip"]

        # Sync
        settings["nginx"].each do |site|
            server.vm.synced_folder site["local_project_path"], site["docroot"], owner: "www-data", group: "www-data"
        end
    end

    # Provider settings
    config.vm.provider :virtualbox do |v|
		v.name = settings["server"]["name"]
		v.memory = settings["server"]["memory"]
		v.cpus = settings["server"]["cpus"]
	end

    # Provision
    config.vm.provision :ansible do |ansible|
        ansible.playbook = "provision/playbook.yml"
    end
end
