# -*- mode: ruby -*-
# vi: set ft=ruby :
#

$domain                   = "lab.local"
$master_hostname          = "puppet"
$master_ip                = "192.168.100.100"
#$peinstaller_url          = "http://192.168.0.7/puppet-enterprise-2015.2.0-el-7-x86_64.tar.gz"
$peinstaller_url          = "https://pm.puppetlabs.com/puppet-enterprise/2015.2.0/puppet-enterprise-2015.2.0-el-7-x86_64.tar.gz"
$peanswers_url            = "https://raw.githubusercontent.com/zoojar/classified/master/puppet.lab.local.answers"
$autosign_these_nodes     = "*"

load 'roosters'

nodes = [
  { 
    :hostname        => $master_hostname, 
    :domain          => $domain,
    :ip              => $master_ip, 
    :box             => 'puppetlabs/centos-7.0-64-nocm', 
    :ram             => 8000,
    :cpus            => 4,
    :cpuexecutioncap => 80,
    :shell_script    => $install_puppet_master_centos7, 
    :shell_args      => [$peinstaller_url, $peanswers_url, $master_hostname, $domain, $master_ip] 
  },
  { 
    :hostname        => 'web-01',
    :domain          => $domain,
    :ip              => '192.168.100.12', 
    :box             => 'puppetlabs/centos-7.0-64-nocm', 
    :ram             => 1000,
    :cpus            => 2,
    :cpuexecutioncap => 50,
    :shell_script    => $install_puppet_agent_linux, 
    :shell_args      => [$master_ip, $master_hostname, $domain] 
  }
]

Vagrant.configure("2") do |config|
  nodes.each do |node|
    config.vm.define node[:hostname] do |nodeconfig|
      nodeconfig.vm.box      = node[:box]
      nodeconfig.vm.hostname = node[:domain] ? "#{node[:hostname]}.#{node[:domain]}" : "#{node[:hostname]}" ;
      memory                 = node[:ram] ? node[:ram] : 1000 ; 
      cpus                   = node[:cpus] ? node[:cpus] : 2 ;
      cpuexecutioncap        = node[:cpuexecutioncap] ? node[:cpuexecutioncap] : 50 ;
      nodeconfig.vm.network :private_network, ip: node[:ip]
      nodeconfig.vm.provider :virtualbox do |vb|
        vb.customize [
          "modifyvm", :id,
          "--memory", memory.to_s,
          "--cpus", cpus.to_s,
          "--cpuexecutioncap", cpuexecutioncap.to_s,
        ]
      end
      nodeconfig.vm.provision :reload
      nodeconfig.vm.provision "shell" do | s |
        s.inline = node[:shell_script]
        s.args   = node[:shell_args]
      end
    end
  end
end
