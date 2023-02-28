# Datenbank Server
>Als Datenbankserver werden Rechner bezeichnet, auf denen Datenbanksysteme abgelegt werden, dabei stellt der Server Datenverwaltungsdienste bereit, die von anderen Rechnern aus genutzt werden können. (Quelle 1)

Auf dem Datenbankserver werden die Daten von den Nutzern der Buchungswebsite gespeichert und verwaltet.
Als Datenbankverwaltungssystem wird MySQL verwendet.
# Erstellen

Vagrantfile:

```
# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  
  INVENTORY = "playbooks/hosts"
  PLAYBOOK = "playbooks/site.yml"  
    
  config.vm.define "centosdb", primary: true do |config|  
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
         v.vmx["ethernet0.address"] = "00:50:56:2a:c6:7b"
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

Playbook: db.yml

```
- name: Setup DB
  hosts: centosdb
  become: true
  
  tasks:
    - name: Set hostname to centosdb
      become: true
      ansible.builtin.hostname:
        name: centosdb.localdomain
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
      
    - name: Add an Ethernet connection with static IP
      community.general.nmcli:
        conn_name: eth0
        ifname: eth0
        type: ethernet
        ip4: 192.168.98.11/24
        gw4: 192.168.98.10
        dns4: 192.168.98.12
        state: present 
        
    - name: Restart NetworkManager
      become: true
      ansible.builtin.service:
        name: NetworkManager
        state: restarted
        
    - name: Open firewall port 3306 TCP
      ansible.posix.firewalld:
        port: 3306/tcp
        permanent: yes
        state: enabled
    - name: Reload Firewalld
      systemd:
        name: firewalld
        state: reloaded        
        
    - name: Upgrade all packages
      ansible.builtin.dnf:
        name: "*"
        state: latest
    - name: Install nano
      ansible.builtin.dnf:
        name: nano
        state: latest

    - name: Install DB
      ansible.builtin.dnf:
        name: mysql-server
        state: latest
        
    - name: Starten DB
      ansible.builtin.service:
        name: mysqld
        state: started
        enabled: true    
        
    - name: Install MySQL client on CentOS 8
      ansible.builtin.dnf:
        name: mysql
        state: present
    - name: Make sure mysqld service is running
      ansible.builtin.service:
        name: mysqld
        state: started
        enabled: True

    - name: Install python3-PyMySQL library
      ansible.builtin.dnf:
        name: python3-PyMySQL
        state: present
         
    - name: Set MySQL root Password
      mysql_user:
        login_host: 'localhost'
        login_user: 'root'
        login_password: ''
        name: 'root'
        password: 'TNpasswd00.'
        state: present
        
    - name: Create a new database with name 'nextjs_prisma'  
      community.mysql.mysql_db:
        name: nextjs_prisma
        state: present
        login_user: 'root'
        login_password: 'TNpasswd00.'          
    
    
    - name: Removes all anonymous user accounts
      community.mysql.mysql_user:
        name: ''
        host_all: yes
        state: absent
        login_user: 'root'
        login_password: 'TNpasswd00.'
    
    - name: Create database user with name 'nextjs' and password 'TNpasswd00' with all database privileges on nextjs_prisma
      community.mysql.mysql_user:
        name: nextjs
        password: TNpasswd00.
        host: '%'
        priv: 'nextjs_prisma.*:ALL,GRANT'
        state: present
        login_user: 'root'
        login_password: 'TNpasswd00.'

   ```

(Quelle 1: https://de.wikipedia.org/wiki/Datenbankserver, 27.02.2023)
