Vagrant::Config.run do |config|

  # Every Vagrant virtual environment requires a box to build off of.
   config.vm.box = "precise64"
   config.vm.box_url = "http://files.vagrantup.com/precise64.box"

  config.vm.customize ["modifyvm", :id, "--memory", 256]
  config.vm.share_folder("persist_cache", "/vm/persist_cache", "persist_cache", :nfs=> true, :create=> true)

  config.ssh.forward_agent = true
  
  servers = {
    :riak1 => {:network => "192.0.2.2", :nodename => "riak1@192.0.2.2"},
    :riak2 => {:network => "192.0.2.3", :nodename => "riak2@192.0.2.3"},
    :riak3 => {:network => "192.0.2.4", :nodename => "riak3@192.0.2.4"},
    :riak4 => {:network => "192.0.2.5", :nodename => "riak4@192.0.2.5"}
  }
  
  servers.each do |name, opts|
    config.vm.define name do |riak|
      # uncomment the following line if you want the basebox to start in gui mode
      # riak.vm.boot_mode = :gui
      riak.vm.network :hostonly, opts[:network]

      riak.vm.provision :puppet, :facter => {
        "riak_node_name"                 => "riak@#{opts[:network]}",
        "riak_data_dir"                  => "/vm/persist_cache",
      } do |puppet|
        puppet.manifests_path = "manifests"
        puppet.manifest_file = "riakbox.pp"
        puppet.module_path = ["modules"]
        puppet.options = "--verbose --debug"
      end      
    end
  end
end
