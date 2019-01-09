hostconfig =
  {1 => {:name => "swarmmanager",
         :ip => "192.168.123.11",
         :ipv6 => "fdb9:6a94:5948:313e::11"},
   2 => {:name => "swarmworker",
         :ip => "192.168.123.12",
         :ipv6 => "fdb9:6a94:5948:313e::12"}
   }
done = 0
Vagrant.configure("2") do |config|
  hostconfig.each do |idx, cfg|
    config.vm.define cfg[:name] do |machine|
      machine.vm.hostname = cfg[:name]
      machine.vm.box = "centos/7"
      machine.vm.network :private_network,
        :ip => cfg[:ip],
        :libvirt__guest_ipv6 => "yes",
        :libvirt__ipv6_address => cfg[:ipv6],
        :libvirt__ipv6_prefix => "64"
      done = done + idx
      if done == 3 then
        machine.vm.provision :ansible do |ansible|
          ansible.limit = "all"
          ansible.become = true
          ansible.playbook = "playbook.yml"
          ansible.host_vars = hostconfig.each_with_object({}) do |(_, cf), hash|
            hash[cf[:name]] = {"ipv6" => cf[:ipv6], "ipv4" => cf[:ip]}
          end
        end
      end
    end
  end
end

# vim:sw=2:sts=2:expandtab:ft=ruby
