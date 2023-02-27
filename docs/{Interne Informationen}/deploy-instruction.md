# Hinweise zur NRE

## Betriebssystem

- [ ] Debian
- [ ] CentOS

### Benötigte Pakete

- mysql
- npm

### Optional Packages

- apache2
- nginx
- yarn
- nvm

!!! tip "Hinweis zur Installation"
    Es ist möglicherweise sinnvoll `nvm` zu installieren, um die Node Versionen zu verwalten. `nvm` bringt auch `npm` mit.
    Bevorzugt benutzen wir erstmal die neuste Version von `Node`.

## Beispiel Installation für Debian

**Das NextJS Projekt herunterladen: https://github.com/gz-bad-erzland-p2/NextJS-Office-Sharing**

### NodeJS

```bash
# https://tecadmin.net/how-to-install-nvm-on-debian-10/
sudo apt install curl 
curl https://raw.githubusercontent.com/creationix/nvm/master/install.sh | bash 
source ~/.profile
nvm install node
```

### MySQL

```bash
# https://www.cloudbooklet.com/how-to-install-mysql-on-debian-11/
sudo apt install wget
wget https://dev.mysql.com/get/mysql-apt-config_0.8.24-1_all.deb
sudo apt install ./mysql-apt-config_0.8.24-1_all.deb
sudo apt update
sudo apt install mysql-server
sudo service mysql status

sudo mysql_secure_installation
```

### Yarn (optional)

```bash
# https://linuxize.com/post/how-to-install-yarn-on-debian-10/
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
sudo apt update
sudo apt install yarn
yarn --version
```

## Anwendung starten

### NPM Pakete installieren (ohne Yarn)

```bash
cd /path/to/project
npm install --production
```

### NPM Pakete installieren (mit Yarn)

```bash
cd /path/to/project
yarn install --production
```

### Datenbank einrichten
#### Voraussetzungen
- MySQL Server läuft
- MySQL User mit allen Rechten auf der Datenbank

!!! warning "Achtung!"
    Das Passwort für den MySQL User muss in der Datei `.env` hinterlegt werden. Standardmäßig ist der Benutzer `root` und das Passwort `toor`.

#### Datenbank erstellen
```bash
mysql -u root -p
CREATE DATABASE nextjs_prisma;
```
Überprüfen, ob die Datenbank erstellt wurde:
```bash
SHOW DATABASES;
```

#### Migrationen ausführen
```bash
npx prisma generate
npx prisma migrate deploy
```

!!! info
    Überprüfen, ob die Tabellen erstellt wurden.

#### Bauen und Starten
```bash
npm run build
npm run start
```
oder
```bash
yarn run build
yarn run start
```

Die Webseite sollte nun unter `http://localhost:3000` erreichbar sein.
