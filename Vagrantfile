Vagrant.configure(2) do |config|
    boxes = [
        {
            :name      => "lab",
            :box       => "ubuntu/trusty64",
            :cpu       => "2",
            :mem       => "4096",
            :net       => "virtio",
            :sync_dir  => [ {
                "/tmp/vagrant" => "/vagrant",
             } ],
            :forward   => [ {
                "8080" => "8080",
                "6379" => "6379",
                "5432" => "5432",
             } ],
        },
    ]

    boxes.each do |opts|
        config.vm.define opts[:name] do |config|

            config.vm.hostname = opts[:name]
            config.vm.box = opts[:box]
            config.vm.box_check_update = false
            config.vm.network "private_network", type: "dhcp"
            config.vm.provision "shell",
                path: "https://raw.githubusercontent.com/radar-aol-pl/dotfiles/master/install.sh",
                privileged: false

            unless opts[:sync_dir].nil?
                opts[:sync_dir].each do |dir|
                    dir.each do |src, dst|
                        config.vm.synced_folder src, dst, create: true
                    end
                end
            end

            unless opts[:forward].nil?
                opts[:forward].each do |port|
                    port.each do |guest, host|
                        config.vm.network "forwarded_port", guest: guest, host: host
                    end
                end
            end

            config.vm.provider "virtualbox" do |virtualbox|
                virtualbox.customize ["modifyvm", :id, "--name", opts[:name]]
                virtualbox.customize ["modifyvm", :id, "--cpus", opts[:cpu]]
                virtualbox.customize ["modifyvm", :id, "--memory", opts[:mem]]
                virtualbox.customize ["modifyvm", :id, "--nictype1", opts[:net]]
            end

        end
    end
end

# vim: set ft=ruby :
