# Allgemeines
Überblick über alle Server:

![Netzwerkplan_finalpng](https://user-images.githubusercontent.com/44226321/214246798-95457483-08bc-4875-947b-7af2355f5c04.png)


Jeder Server (außer die Firewall) wird mittlhilfe der Orchestrierungssoftware Ansible erstellt und ggf. konfiguriert.
Als Schnittsnitstelle zwischen VMware und Ansible wird die Software Vagrant verwendent.

# Aufbau der Vagrant / Ansible Ordner/Datein

Weiterhin wird jeder Server der mittels Vagrant und Ansible gestartet wird mit folgender Datei und Ordnerstruktur ausgerüstet.
Diese liegen unter C:\Vagrant\CentOS9_xx (xx=Servername z.B. DB)

![grafik](https://user-images.githubusercontent.com/44226321/214226433-1f3112a2-c841-43e8-9988-cfcbdb62851c.png)

Datei ssh-config
---
Enthält alle nötigen Informationen um eine Verbindung mit dem Server mittels SSH herzustellen.

![grafik](https://user-images.githubusercontent.com/44226321/214228453-54d71da7-5e40-481a-9a40-5a046a2cd99e.png)

___
Datei vagrantfile
---
Enthält alle Informationen die Vagrant benötigt um den Server mit den entsprechenden Spezifiaktionen automatisiert aufzusetzten.
--> die einzelnen Vagrantfiles werden seperat vorgestellt
___
Ordner vmware:
---
Jeder Server erhält eine feste IP-Addresse mittels DCHP Reservierung.
Dafür wird die Datei vmnetdhcp.conf in der vmware Konfiguration mitthilfe des replaceDHCPDconf.cmd Skrips überschrieben.

vmnetdhcp.conf
---

![grafik](https://user-images.githubusercontent.com/44226321/214229399-473191a8-dfd6-404a-9d16-c85d10c9a7d5.png)
![grafik](https://user-images.githubusercontent.com/44226321/214229468-a734b8e7-c9ea-4729-856f-77a40ed694e4.png)

___

Ordner playbooks:
---

![grafik](https://user-images.githubusercontent.com/44226321/214227756-7a79b81d-4ca0-4523-a590-6c7fa71c0ac2.png)


Erläuterung der einzelnen Datein: 
---
ansible.cfg: 
----
!!!ERKLÄRUNG!!!

![grafik](https://user-images.githubusercontent.com/44226321/214227026-4ffa34ce-3276-4181-a810-6df31ae1cd78.png)


hosts: 
---
enthält die Hostnamen und IP Adressen des jeweiligen Servers

![grafik](https://user-images.githubusercontent.com/44226321/214227468-08882814-e487-4737-8928-1724f2560398.png)


xx.yml: 
---
enthält das jeweilige Playbook für den Server -> werden einzeln genauer vorgestellt
___
Ordner sshkeys:
---
enthält die einezelnen SSH-Keys um eine SSH Verbidnung mit dem Server aufzubauen

![grafik](https://user-images.githubusercontent.com/44226321/214228103-14840b9f-ae3d-43c1-b4c4-f801b753f831.png)



