Vagrant.configure("2") do |config|

    # PostgreSQL Instances
    (1..2).each do |n|
        config.vm.define "pgsql-#{n}" do |define|

            define.vm.box = "ubuntu/focal64"

            define.ssh.insert_key = false
            define.vm.hostname = "pgsql-#{n}"
            define.vm.network :private_network, ip: "172.16.1.1#{n}"
            # define.vm.network :forwarded_port, guest: 5432, host: "543#{n}"

            define.vm.provider :virtualbox do |v|
                v.cpus = 1
                v.memory = 512
                v.name = "pgsql-#{n}"
            end

            if n == 2
                define.vm.provision :ansible do |ansible|
                    ansible.limit = "all"
                    ansible.playbook = "provisioning/pgsql.yaml"

                    ansible.host_vars = {
                        "pgsql-1" => {:connection_host => "172.16.1.11",
                                    :node_id => 1,
                                    :role => "primary" },

                        "pgsql-2" => {:connection_host => "172.16.1.12",
                                    :node_id => 2,
                                    :role => "slave" },

                    }
                    ansible.verbose = "v"
                end
            end

        end
    end

    # Nginx Instances
    (1..2).each do |n|
        config.vm.define "web-#{n}" do |define|
            
            define.vm.box = "ubuntu/focal64"

            define.ssh.insert_key = false
            define.vm.hostname = "web-#{n}"
            define.vm.network :private_network, ip: "172.16.1.2#{n}"
            # define.vm.network :forwarded_port, guest: 80, host: "808#{n}"

            define.vm.provider :virtualbox do |v|
                v.cpus = 1
                v.memory = 512
                v.name = "web-#{n}"
            end

            if n == 2
                define.vm.provision :ansible do |ansible|
                    ansible.limit = "all"
                    ansible.playbook = "provisioning/web.yaml"

                    ansible.host_vars = {
                        "web-1" => {:connection_host => "172.16.1.21",
                                    :node_id => 1,
                                    :role => "web1" },

                        "web-2" => {:connection_host => "172.16.1.22",
                                    :node_id => 2,
                                    :role => "web2" },

                    }
                    ansible.verbose = "v"
                end
            end

        end
    end

    # Odoo Instances
    (1..3).each do |n|
        config.vm.define "odoo-#{n}" do |define|

            define.vm.box = "odoo"

            define.ssh.insert_key = false
            define.vm.hostname = "odoo-#{n}"
            define.vm.network :private_network, ip: "172.16.1.3#{n}"
            # define.vm.network :forwarded_port, guest: 8069, host: "806#{n}"

            define.vm.provider :virtualbox do |v|
                v.cpus = 1
                v.memory = 512
                v.name = "odoo-#{n}"
            end

            if n == 3
                define.vm.provision :ansible do |ansible|
                    ansible.limit = "all"
                    ansible.playbook = "provisioning/odoo.yaml"

                    ansible.host_vars = {
                        "odoo-1" => {:connection_host => "172.16.1.31",
                                    :node_id => 1,
                                    :role => "odoo1" },

                        "odoo-2" => {:connection_host => "172.16.1.32",
                                    :node_id => 2,
                                    :role => "odoo2" },

                        "odoo-3" => {:connection_host => "172.16.1.33",
                                    :node_id => 3,
                                    :role => "odoo3" },

                    }
                    ansible.verbose = "v"
                end
            end

        end
    end

    # Monitoring Instance

    config.vm.define "monitor" do |define|

        define.vm.box = "ubuntu/focal64"

        define.ssh.insert_key = false
        define.vm.hostname = "monitor"
        define.vm.network :private_network, ip: "172.16.1.41"
        # define.vm.network :forwarded_port, guest: 8069, host: "806#{n}"

        define.vm.provider :virtualbox do |v|
            v.cpus = 2
            v.memory = 1024
            v.name = "monitor"
        end

        define.vm.provision :ansible do |ansible|
            ansible.limit = "all"
            ansible.playbook = "provisioning/monitor.yaml"

            ansible.host_vars = {
                "monitor" => {:connection_host => "172.16.1.41",
                            :node_id => 1,
                            :role => "monitor" },

            }
            ansible.verbose = "v"
        end

    end


end