# Zertifikatsserver - CA (certifcation authority)

# Erstellen

Vagrantfile: 

```
# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  
  INVENTORY = "playbooks/hosts"
  PLAYBOOK = "playbooks/site.yml"  
    
  config.vm.define "centosca", primary: true do |config|  
    config.vm.box_check_update = false   
    config.vm.box = "generic/centos9s"
    config.vm.boot_timeout = 1800
    config.vm.communicator = :ssh
    config.vm.guest = :linux
    
    config.vm.synced_folder './', '/vagrant', disabled: true
    
    config.ssh.insert_key = true
  
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
         v.vmx["ethernet0.address"] = "00:50:56:2a:c6:b3"
         v.vmx["ethernet0.vnet"] = "vmnet8"
         v.vmx["ethernet0.displayName"] = "VMnet8"
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


(https://www.digitalocean.com/community/tutorials/how-to-set-up-and-configure-a-certificate-authority-ca-on-centos-8-de)
# Erstellen des Zertifikats

ca.crt Zertifikat:

```
-----BEGIN CERTIFICATE-----
MIIDQjCCAiqgAwIBAgIUVw4ELjTqCR8iRmDTeuF300oVOgUwDQYJKoZIhvcNAQEL
BQAwEzERMA8GA1UEAwwIaXRzZWNuZXQwHhcNMjMwMTE2MTI0MzEzWhcNMzMwMTEz
MTI0MzEzWjATMREwDwYDVQQDDAhpdHNlY25ldDCCASIwDQYJKoZIhvcNAQEBBQAD
ggEPADCCAQoCggEBALVLqgz4gllQFE+Mb968TACKt9a5hmWkZCSNUo4KWKb3RJam
7My6V358romD3c2PD+Kc97/GHlrHTVEjM84Voe0KIsGmW/QVPkude1AScNFuefNj
6LSq3GSMDltdPGRrA4ESBwJtxEQ0wsM3mS260X+MdCPbkOd02D9WfaoY5GgS4Jzv
HP6F2BiP1wL44IjR+Az/oGy9+ZzODr8nGVJgJlGoqa8EEHJkZa5yDGLkkUTfCcSz
CJnJt77kQoDfaXAwfwbVAzljF6XcutLVr6RFq1namEpP9kGibMtCoyqSHVv8dEQw
9yDQKUl8Gcu8EdGi0+ibbV4IfyDxhGlV+vYHRhUCAwEAAaOBjTCBijAdBgNVHQ4E
FgQUuHGecFp8TGVr/uOkR3kYdRMxfJUwTgYDVR0jBEcwRYAUuHGecFp8TGVr/uOk
R3kYdRMxfJWhF6QVMBMxETAPBgNVBAMMCGl0c2VjbmV0ghRXDgQuNOoJHyJGYNN6
4XfTShU6BTAMBgNVHRMEBTADAQH/MAsGA1UdDwQEAwIBBjANBgkqhkiG9w0BAQsF
AAOCAQEAh26aCLwf/cVCpgMHXAnfnUq7cYuXn62AlHHMHtMFnropUJLQPRipjp0R
U04j/FR63xBws04hoiw3QIKwTMgCb+pKrbvYJF12LrURjO9zQtJUl36Y40JL4ALY
xVIKTDYd9dUrEjgZHT8s1nWAQbmJPt+k0LbMzLD1fKLPFrIYI794dvnLmE3Yc/J2
GquzxeD2Q0GqizY9u/9stKCw+JIs5qx561h2V6XnZnJ5tD2R9cUdbSQzRqEt5EUT
v6Srp2HoC20BBvHUXk1RoJteh3KBz1YOMVWQCqSB5FiFtUWBm6IHJCXYBGFw7zO6
bQymZXyXrZsTMhmC6EKOYDM4E9emjw==
-----END CERTIFICATE-----
```

# Importieren des Zertifikates in Firefox 
    Stellen Sie sicher, dass das Zertifikat, das Sie in Firefox importieren wollen, auf Ihrem Rechner gespeichert ist.
    Starten Sie Firefox und klicken Sie dann oben rechts auf die Schaltfläche, um das Menü zu öffnen.
    Wählen Sie den Eintrag „Einstellungen“ aus.
    Wechseln Sie dann links auf das Menü „Datenschutz & Sicherheit“ und scrollen Sie auf dieser Seite nach ganz unten.
    Hier finden Sie den Abschnitt „Zertifikate“. Klicken Sie auf den Button „Zertifikate anzeigen“.
    Klicken Sie unten auf „Importieren…“ und navigieren zu Ihrer Zertifikatsdatei.
    Anschließend wird der Eintrag in Firefox aufgenommen. 
