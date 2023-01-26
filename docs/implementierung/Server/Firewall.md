# Firewall
Auf der Firewall läuft die freie Linux-Distribution IPFire, die als Router und Firewall fungiert.
# Einzelnen Netzwerkadapter

Schnittstellen:

![grafik](https://user-images.githubusercontent.com/44226321/214235061-958b0814-98f0-4932-ad29-de1805b198b4.png)
___
![grafik](https://user-images.githubusercontent.com/44226321/214235193-a2385c18-dd2f-4617-94fb-001dbd606b13.png)
___
![grafik](https://user-images.githubusercontent.com/44226321/214235267-2a707dde-9625-45a8-b093-b47f201a3bb0.png)
___
![grafik](https://user-images.githubusercontent.com/44226321/214235395-432edfee-8d00-4ac7-9454-0e79f942703e.png)
___
![grafik](https://user-images.githubusercontent.com/44226321/214235491-17bc1b71-8a21-4455-8c49-5bf9f29c6030.png)
___
![grafik](https://user-images.githubusercontent.com/44226321/214235543-122f6bf2-a061-40fc-9158-7740355dceb6.png)


# Konfiguration
Unter der IP-Adresse https://192.168.98.10:444/ ist das Webinterface der Firewall zu erreichen.
Hier wurde die Konfiguration der Firewall vorgenommen.

**Firewall Optionen:**

![grafik](https://user-images.githubusercontent.com/44226321/214234670-908fe9c0-6581-42be-a7b1-e6066de2cdd4.png)

Generell werden alle Aufragen von außen (also aus dem roten Netz) in die anderen Netze durch die Firewall verworfen. 
Andersherum dürfen alle Netze Anfragen ins rote Netz senden und diese werden zugelassen
So kann der Zugriff genau gesteuert werden

**Firewall Regeln:**

Anfragen aus allen Netzen 
