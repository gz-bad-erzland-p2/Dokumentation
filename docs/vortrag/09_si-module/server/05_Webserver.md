# Webserver

# Erstellen

Vagrantfile:

```
# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  
  INVENTORY = "playbooks/hosts"
  PLAYBOOK = "playbooks/site.yml"  
    
  config.vm.define "centoswebserver", primary: true do |config|  
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
         v.vmx["ethernet0.address"] = "00:50:56:2a:c6:d7"
         v.vmx["ethernet0.vnet"] = "vmnet2"
         v.vmx["ethernet0.displayName"] = "VMnet2"
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

Playbooks: 

**webserver.yml**

```
---
- name: Setup Webserver
  hosts: centoswebserver
  become: true
  
  tasks:
    - name: Set hostname to centoswebserver
      become: true
      ansible.builtin.hostname:
        name: centoswebserver.localdomain
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
        ip4: 192.168.86.11/24
        gw4: 192.168.86.10
        dns4: 192.168.86.10
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
        update_cache: true
        state: latest
    - name: Install NPM
      ansible.builtin.dnf:
        name: npm
        update_cache: true
        state: latest
    - name: Install httpd
      ansible.builtin.dnf:
        name: httpd
        update_cache: true
        state: latest
    - name: Install GIT
      ansible.builtin.dnf:
        name: git
        update_cache: true
        state: latest
 
    - name: Open firewall port 3000 TCP
      ansible.posix.firewalld:
        port: 3000/tcp
        permanent: yes
        state: enabled
    - name: Reload Firewalld
      systemd:
        name: firewalld
        state: reloaded

    - name: Git clone
      shell: git clone https://github.com/gz-bad-erzland-p2/NextJS-Office-Sharing
      args:
        chdir: /var/www/
    - name: NPM Install
      shell: npm install
      args:
        chdir: /var/www/NextJS-Office-Sharing
    - name: NPM Run Build
      shell: npm run build
      args:
        chdir: /var/www/NextJS-Office-Sharing
    
    - name: Set .env File
      become: true
      ansible.builtin.copy:
        src: "{{ playbook_dir }}/files/.env"
        dest: /var/www/NextJS-Office-Sharing/.env
        force: true
        
    - name: NPM Prisma Generate
      shell: npx prisma generate
      args:
        chdir: /var/www/NextJS-Office-Sharing
    - name: NPM Prisma Deploy
      shell: npx prisma migrate deploy
      args:
        chdir: /var/www/NextJS-Office-Sharing
        
    - name: NPM Run Start
      shell: npm run start
      args:
        chdir: /var/www/NextJS-Office-Sharing
```

**react.yml:**
```
---
- name: Setup React App
  hosts: centoswebserver
  become: true
  
  tasks:
    - name: Install npm
      ansible.builtin.dnf:
        name: npm
        state: latest
    - name: Create Project Directory
      ansible.builtin.file:
        path: /home/vagrant/Test-App
        state: directory
        mode: '0755'
 ```
**git.yml:**
```
---
- name: Setup Webserver
  hosts: centoswebserver
  become: true
  
  tasks:
    - name: Git clone
      shell: git clone https://github.com/gz-bad-erzland-p2/NextJS-Office-Sharing
      args:
        chdir: /var/www/
    - name: NPM Install
      shell: npm install
      args:
        chdir: /var/www/NextJS-Office-Sharing
    - name: NPM Run Build
      shell: npm run build
      args:
        chdir: /var/www/NextJS-Office-Sharing
    - name: NPM Run Start
      shell: npm run start
      args:
        chdir: /var/www/NextJS-Office-Sharing
```
