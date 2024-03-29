# DHCP Server für das blaue Netz

!!! info "Überblick"
    <span class="biggerFont">Der DHCP-Server verwaltet einen Pool von IP-Adressen und leaset eine Adresse [und weitere Konfigurationsdaten] an jeden DHCP-fähigen Client, wenn er im Netzwerk gestartet wird. (Quelle 1)</span>

## Erstellen

Vagrantfile:

```
# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  
  INVENTORY = "playbooks/hosts"
  PLAYBOOK = "playbooks/site.yml"  
    
  config.vm.define "centosdhcp", primary: true do |config|  
    config.vm.box_check_update = false   
    config.vm.box = "generic/centos9s"
    config.vm.boot_timeout = 1800
    config.vm.communicator = :ssh
    config.vm.guest = :linux
    
    config.vm.synced_folder './', '/vagrant', disabled: true
    
    config.ssh.insert_key = false
  
    # https://sanbarrow.com/vmx/vmx-network-advanced.html
  
    ["vmware_desktop"].each do |provider|
      config.vm.provider provider do |v|
         v.allowlist_verified = :disable_warning
         v.enable_vmrun_ip_lookup = true
         v.unmount_default_hgfs = true
         # v.whitelist_verified = true
         
         # required by vmx network-settings on startup ??
         v.gui = true
      
         v.vmx["cpuid.coresPerSocket"] = "1"
         v.vmx["memsize"] = "2048"
         v.vmx["numvcpus"] = "2"
         v.vmx["floppy0.present"] = "false"
		 
		 # v.vmx["svga.autodetect"] = "false"
		 # v.vmx["svga.maxWidth"] = "2560"
		 # v.vmx["svga.maxHeight"] = "2048"
		 # v.vmx["svga.vramSize"] = "20971520"
		 # v.vmx["svga.numDisplays"] = "1"
      
         v.vmx["ethernet0.present"] = "true"
         v.vmx["ethernet0.connectionType"] = "custom"
         v.vmx["ethernet0.addressType"] = "static"
         v.vmx["ethernet0.address"] = "00:50:56:2a:c6:d9"
         v.vmx["ethernet0.vnet"] = "vmnet3"
         v.vmx["ethernet0.displayName"] = "VMnet3"
         v.vmx["ethernet0.virtualDev"] = "e1000"
         end
      end
    end  
  end
  ```

## Konfiguration

Playbook:

```
---
- name: Setup DHCP
  hosts: centosdhcp
  become: true
  
  tasks:
#   VM Hostname
    - name: Set hostname to centosdhcp
      become: true
      ansible.builtin.hostname:
        name: centosdhcp.localdomain
        use: systemd
      
#   IP Configuration 
    - name: Add an Ethernet connection with static IP
      community.general.nmcli:
        conn_name: eth0
        ifname: eth0
        type: ethernet
        ip4: 192.168.91.11/24
        gw4: 192.168.91.10
        dns4: 192.168.91.10
        state: present 
    - name: Restart NetworkManager
      become: true
      ansible.builtin.service:
        name: NetworkManager
        state: restarted

#   Install Packages   
    - name: Upgrade all packages
      ansible.builtin.dnf:
        name: "*"
        state: latest
    - name: Install nano
      ansible.builtin.dnf:
        name: nano
        state: latest
    - name: Install dhcp-server
      ansible.builtin.dnf:
        name: dhcp-server
        state: latest

#   DHCP Server       
    - name: Set dhcpd.conf
      become: true
      ansible.builtin.copy:
        src: "{{ playbook_dir }}/files/dhcpd.conf"
        dest: /etc/dhcp/dhcpd.conf
        force: true     
    - name: Enable dhcp-server
      ansible.builtin.service:
        name: dhcpd
        enabled: true
        state: started    
    - name: Restart dhcp-server
      ansible.builtin.service:
        name: dhcpd
        enabled: true
        state: restarted
```

(Quelle 1: https://learn.microsoft.com/de-de/windows-server/networking/technologies/dhcp/dhcp-top, 27.02.2023)
