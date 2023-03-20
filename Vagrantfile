# -*- mode: ruby -*-
# vi: set ft=ruby :

NODES = [
  {:role => "control-plane", :ip => "10.10.10.11", :mem => "8000", :cpus => 8},
  {:role => "worker", :ip => "10.10.10.20", :mem => "16000", :cpus => 8},
  {:role => "worker", :ip => "10.10.10.21", :mem => "16000", :cpus => 8}
]

def get_hostname(role, idx)
  node_name = role == "worker" ? "worker-#{idx}" : role
  "lfd259-#{node_name}"
end

def get_hosts_content(nodes)
  res = "["
  nodes.each.with_index do |node, idx|
    role = node[:role]
    ip = node[:ip]
    if idx != 0 then
      res += ","
    end
    res += "'#{ip} #{get_hostname(role, idx)}'"
  end
  res += "]"
  res
end

Vagrant.configure("2") do |config|
  hosts_table = get_hosts_content(NODES)

  NODES.each.with_index do |node, idx|
    role = node[:role]

    config.vm.box = "ubuntu/focal64"
    config.vm.define get_hostname(role, idx) do |c|
      c.vm.hostname = get_hostname(role, idx)
      c.vm.network "private_network", ip: node[:ip]

      c.vm.provider "virtualbox" do |box|
        box.memory = node[:mem]
        box.cpus = node[:cpus]
        box.name = "LFD-259 (node: #{idx + 1} #{role})"
      end

      c.vm.provision :ansible do |ansible|
        ansible.playbook = "#{role}.yaml"
        ansible.extra_vars = {
          node_ip: node[:ip],
          hosts_table: hosts_table
        }
      end
    end
  end
end
