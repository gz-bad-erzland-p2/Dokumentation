# Serverüberblick

![Netzwerkplan_finalpng](https://user-images.githubusercontent.com/44226321/214246798-95457483-08bc-4875-947b-7af2355f5c04.png)


Jeder Server (außer die Firewall) wird mithilfe der Orchestrierungssoftware Ansible erstellt und ggf. konfiguriert.
Als Schnittstelle zwischen VMware und Ansible wird die Software Vagrant verwendet.
# Serveraufbau
## Aufbau der Vagrant / Ansible Ordner/Dateien

Weiterhin wird jeder Server, der mittels Vagrant und Ansible gestartet wird, mit folgender Datei und Ordnerstruktur ausgerüstet.
Diese liegen unter C:\Vagrant\CentOS9_xx (xx=Servername z. B. DB)

![grafik](https://user-images.githubusercontent.com/44226321/214226433-1f3112a2-c841-43e8-9988-cfcbdb62851c.png)

### Datei ssh-config

Die ssh-config enthält alle nötigen Informationen, um eine Verbindung mit dem Server mittels SSH herzustellen.
```Nginx
Host centosxx
  HostName 192.168.xx.xx
  User vagrant
  Port 22
  UserKnownHostsFile /dev/null
  StrictHostKeyChecking no
  PasswordAuthentication no
  IdentityFile playbooks/sshkeys/centosxx-key
  IdentityFile playbooks/sshkeys/insecure_private_key
  IdentitiesOnly yes
  LogLevel FATAL

Host lnxhost-Internal
  HostName 192.168.98.128
  User vagrant
  Port 22
  UserKnownHostsFile /dev/null
  StrictHostKeyChecking no
  PasswordAuthentication no
  IdentityFile c:/Vagrant/CentOS9-dns/.vagrant/machines/centos-dns/vmware_desktop/private_key
  IdentityFile ~/vagrant/CentOS9-dns/.vagrant/machines/centos-dns/vmware_desktop/private_key
  IdentityFile /CentOS9-dns/.vagrant/machines/centos-dns/vmware_desktop/private_key
  IdentitiesOnly yes
  LogLevel FATAL 
```

### Datei vagrantfile

Das vagrantfile enthält alle Informationen, die Vagrant benötigt, um den Server mit den entsprechenden Spezifikationen automatisiert aufzusetzen.
--> die einzelnen vagrantfiles werden separat vorgestellt
___
## Ordner vmware:

Jeder Server erhält eine feste IP-Adresse mittels DCHP Reservierung.
Dafür wird die Datei vmnetdhcp.conf in der vmware Konfiguration mithilfe des replaceDHCPconf.cmd Skripts überschrieben.

### vmnetdhcp.conf
 
```bash
#
# Configuration file for VMware port of ISC 2.0 release running on
# Windows.
#
# This file is generated by the VMware installation procedure; it
# is edited each time you add or delete a VMware host-only network
# adapter.
#
# We set domain-name-servers to make some clients happy
# (dhclient as configued in SuSE, TurboLinux, etc.).
# We also supply a domain name to make pump (Red Hat 6.x) happy.
#
allow unknown-clients;
default-lease-time 1800;                # default is 30 minutes
max-lease-time 7200;                    # default is 2 hours

# add your host reservations here
host bszetlnx {
	hardware ethernet 0a:50:56:3d:49:5e;
	fixed-address 192.168.72.4;
	option host-name "bszetlnx";
	# option domain-search "bsz-et.lan.dd-schulen.de";
}

#DMZ
host centoswebserver {
	hardware ethernet 00:50:56:2a:c6:d7;
	fixed-address 192.168.86.11;
	option host-name "centoswebserver";
}

#Intern
host centosdns {
	hardware ethernet 00:50:56:2a:c6:d8;
	fixed-address 192.168.98.12;
	option host-name "centosdns";
}

host centosca {
	hardware ethernet 00:50:56:2a:c6:b3;
	fixed-address 192.168.72.128;
}

host centosdb {
	hardware ethernet 00:50:56:2a:c6:7b;
	fixed-address 192.168.98.11;
	option host-name "centosdb";
}

host centodhcp {
	hardware ethernet 00:50:56:2a:c6:d9;
	fixed-address 192.168.91.11;
}

# Virtual ethernet segment 8
# Added at 02/07/22 11:55:42
subnet 192.168.72.0 netmask 255.255.255.0 {
range 192.168.72.128 192.168.72.254;            # default allows up to 125 VM's
option broadcast-address 192.168.72.255;
option domain-name-servers 192.168.72.2;
option domain-name "localdomain";
option netbios-name-servers 192.168.72.2;
option routers 192.168.72.2;
default-lease-time 1800;
max-lease-time 7200;
}
host VMnet8 {
    hardware ethernet 00:50:56:C0:00:08;
    fixed-address 192.168.72.1;
    option domain-name-servers 0.0.0.0;
    option domain-name "";
    option routers 0.0.0.0;
}
# End


# Virtual ethernet segment 3
# Added at 02/07/22 11:55:42
subnet 192.168.91.0 netmask 255.255.255.0 {
range 192.168.91.240 192.168.91.254;            # default allows up to 125 VM's
option broadcast-address 192.168.91.255;
option domain-name-servers 192.168.91.1;
option domain-name "localdomain";
default-lease-time 1800;
max-lease-time 7200;
}
host VMnet3 {
    hardware ethernet 00:50:56:C0:00:03;
    fixed-address 192.168.91.1;
    option domain-name-servers 0.0.0.0;
    option domain-name "";
}
# End


# Virtual ethernet segment 2
# Added at 11/29/22 10:17:53
subnet 192.168.86.0 netmask 255.255.255.0 {
range 192.168.86.128 192.168.86.254;            # default allows up to 125 VM's
option broadcast-address 192.168.86.255;
option domain-name-servers 192.168.86.1;
option domain-name "localdomain";
default-lease-time 1800;
max-lease-time 7200;
}
host VMnet2 {
    hardware ethernet 00:50:56:C0:00:02;
    fixed-address 192.168.86.1;
    option domain-name-servers 0.0.0.0;
    option domain-name "";
}
# End

# Virtual ethernet segment 1
# Added at 02/07/22 11:55:42
subnet 192.168.98.0 netmask 255.255.255.0 {
range 192.168.98.128 192.168.98.254;            # default allows up to 125 VM's
option broadcast-address 192.168.98.255;
option domain-name-servers 192.168.98.1;
option domain-name "localdomain";
default-lease-time 1800;
max-lease-time 7200;
}
host VMnet1 {
    hardware ethernet 00:50:56:C0:00:01;
    fixed-address 192.168.98.1;
    option domain-name-servers 0.0.0.0;
    option domain-name "";
}
# End
```
___

## Ordner playbooks:


![grafik](https://user-images.githubusercontent.com/44226321/214227756-7a79b81d-4ca0-4523-a590-6c7fa71c0ac2.png)


Erläuterung der einzelnen Dateien: 
---
### ansible.cfg: 

= Konfiguration die Ansible benötigt

```Nginx
# ansible configuration file.

[defaults]
inventory = hosts
force_handlers = True
host_key_checking = False

filter_version = "1.0"
module_rejectlist = [ "easy_install" ]

[ssh_connection]
ssh_args = -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -o CheckHostIP=no -o UpdateHostKeys=no -o LogLevel=quiet
scp_if_ssh = True
```

### hosts: 

enthält die Hostnamen und IP-Adressen der jeweiligen Servers
```
[centOS]
centosdb			ansible_host=192.168.98.11

[centOS:vars]
ansible_port=22
ansible_user=vagrant
ansible_ssh_private_key_file=playbooks/sshkeys/centosdb-key
ansible_ssh_common_args=-oIdentityFile=playbooks/sshkeys/insecure_private_key
```

### xx.yml: 

enthält das jeweilige Playbook für den Server -> werden einzeln genauer vorgestellt
___
## Ordner sshkeys:

enthält die einzelnen SSH-Keys, um eine SSH Verbidnung mit dem Server aufzubauen

![grafik](https://user-images.githubusercontent.com/44226321/214228103-14840b9f-ae3d-43c1-b4c4-f801b753f831.png)

___
## Ordner files:

Im Ordner _files_ liegen Dateien die in den playbooks verwendet und eingebunden werden.

Bei jedem Server liegt hier für die Proxy Konfiguration die _evironment_ Datei
```bash
export http_proxy=http://10.254.5.100:3128
export https_proxy=http://10.254.5.100:3128
export no_proxy=localhost,.bsz-et.lan.dd-schulen.de
```

Für die einzelnen Server liegen hier noch spezifische andere Dateien wie bspw. dnsmasq.conf etc ...
