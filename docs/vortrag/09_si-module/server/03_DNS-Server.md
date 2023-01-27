# DNS Server (Domain Name System)
Der DNS-Server löst die Domain-Namen von bspw. Webseiten in die entsprechende IP-Adresse auf. 
In unserem Fall löst der DNS-Server den Webseiten Namen gz-bad-erzland-p2.de in die IP-Adresse des Webservers auf.

Der Kunde sieht jedoch nur die URL und nicht die dahinterstehende IP-Adresse.
# Erstellen

Vagrantfile: 

```
# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  
  INVENTORY = "playbooks/hosts"
  PLAYBOOK = "playbooks/site.yml"  
    
  config.vm.define "centosdns", primary: true do |config|  
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
         v.vmx["ethernet0.address"] = "00:50:56:2a:c6:d8"
         v.vmx["ethernet0.vnet"] = "vmnet1"
         v.vmx["ethernet0.displayName"] = "VMnet1"
         v.vmx["ethernet0.virtualDev"] = "e1000"
         end
      end
    end  
    
	# vagrant on linux system:
    # manual [explicit] usage: vagrant provision
    config.vm.provision "ansible" do |ansible|
       ansible.playbook = PLAYBOOK
       ansible.inventory_path = INVENTORY
       ansible.skip_tags = ["ping","pkgupgrade","test"]
       end
       
    # manual [explicit] usage: vagrant provision --provision-with ping
    config.vm.provision "ping", type: "ansible", run: "never" do |ansible|
        ansible.playbook = PLAYBOOK
        ansible.inventory_path = INVENTORY
        ansible.tags = ["ping"]
        end
  end
```

# Konfiguration

Playbook: dns.yml

```
---
- name: Setup DNS
  hosts: centosdns
  become: true
  
  tasks:
    - name: Set hostname to centosdns
      become: true
      ansible.builtin.hostname:
        name: centosdns.localdomain
        use: systemd
        
    - name: 'Set keyboard layout to: de-nodeadkeys'
      become: true 
      ansible.builtin.shell: 
        cmd: |
          if localectl status | grep -q 'VC Keymap: us'; then 
             sudo localectl set-keymap de-nodeadkeys ; fi
             
    - name: Change timezone to Europe/Berlin
      become: true 
      community.general.timezone:
        name: Europe/Berlin
      
    - name: Add an Ethernet connection with static IP
      community.general.nmcli:
        conn_name: eth0
        ifname: eth0
        type: ethernet
        ip4: 192.168.98.12/24
        gw4: 192.168.98.10
        dns4: 192.168.98.12
        state: present
        
    - name: Restart NetworkManager
      become: true
      ansible.builtin.service:
        name: NetworkManager
        state: restarted
        
    - name: Check Proxy Settings
      ansible.builtin.lineinfile:
        path: /etc/environment
        regexp: '^export no_proxy'
        state: absent
      check_mode: yes
      failed_when: false
      changed_when: false
      register: proxy_check  
      tags: proxy

    - name: Set Proxy Settings if not configured
      become: true
      ansible.builtin.copy:
        src: "{{ playbook_dir }}/files/environment"
        dest: /etc/environment
        force: true
      when: not proxy_check.found
      tags: proxy
        
    - name: Upgrade all packages
      ansible.builtin.dnf:
        name: "*"
        state: latest
    - name: Install nano
      ansible.builtin.dnf:
        name: nano
        state: latest

    - name: Install dnsmasq
      ansible.builtin.dnf:
        name: dnsmasq
        state: latest
    - name: Enable dnsmasq
      ansible.builtin.service:
        name: dnsmasq
        enabled: true
        state: started
    - name: Set dnsmasq.conf
      become: true
      ansible.builtin.copy:
        src: "{{ playbook_dir }}/files/dnsmasq.conf"
        dest: /etc/dnsmasq.conf
        force: true
    - name: Set hosts
      become: true
      ansible.builtin.copy:
        src: "{{ playbook_dir }}/files/hosts"
        dest: /etc/hosts
        force: true
    - name: Restart dnsmasq
      become: true
      ansible.builtin.service:
        name: dnsmasq
        state: restarted
```

# DNS Einträge
