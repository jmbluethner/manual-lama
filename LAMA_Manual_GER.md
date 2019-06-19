# **>> LAMA <<**
**LA**N-**Ma**nager
## Eine PHP basierte Webanwendung zur Verwaltung von Gameservern auf der Source Engine  

##### Impressum:  
Dev(s): Jan-Morris Blüthner - The Nighttimedev  
Kontakt: develop@nighttimedev.com  
Weitere Projekte: https://die-lan.party  
<br>
https://nighttimedev.com  
##### Notes:
*Der Markdown-Viewer von Github zeigt die geschachtelte Liste des Index falsch an.*
<hr>

### Look
<br>

![Screenshot4](https://i.imgur.com/kfplPAT.png)  
![Screenshot3](https://i.imgur.com/0Bmilmj.jpg)  
![Screenshot1](https://cdn.nighttimedev.com/images/lama/lama1.png)  
![Screenshot2](https://i.imgur.com/uEDBh6Q.png)  

<hr>

### Was in diesem Manual zu finden ist
##### Für Anwender
1. **Allgemein**
  1. Was ist LAMA
  1. Was kann LAMA
  1. Was kann LAMA nicht
  1. Was es in Zukunft können soll / Roadmap
1. **Installation**
  1. Benötigte Elemente
    1. Webserver
    1. PHP
    1. Datenbank
  1. Repository Klonen
  1. LAMA einrichten / installieren
1. **Verwendung**
  1. Anmeldung
  1. Was sind Plugins?
  1. Die Standard-Plugins erklärt
    1. Home
    1. Servers
    1. Customize
    1. Bans

##### Für Coder
1. **Struktur**
1. **Plugins entwickeln**

<hr>

Im folgenden steht 'ich' für die Perspektive des Verfassers, also Nighttimedev.

### Für Anwender
##### **1. Allgemein**
###### 1.1 Was ist LAMA?
LAMA wurde für die LAN-Party ["DIE-LAN"](https://die-lan.party) entwickelt. Hier liegt auch bis heute der Fokus. Allerdings habe ich im Laufe der Zeit bemerkt, dass eine Web-Basierte Applikation zur Verwaltung von Gameservern eine generelle Nachfrage birgt.  
Auf Grund dessen wurde LAMA so entwickelt, dass jeder Nutzer, der Grundkenntnisse von HTML/CSS (Optional PHP) birgt, Plugins für eigene Bedürfnisse entwickeln kann.  
Mein Ziel war es auch, dass LAMA selbst von Personen ohne Kenntnisee im Webdevelopment benutzt werden kann.  
Die eigentliche Idee zu LAMA kam mir durch das CS:GO Server-Plugin [get5](https://github.com/splewis/get5) von splewis und dem damit zusammenhängenden Proof-Of-Concept [get5-web](https://github.com/splewis/get5-web), ebenfalls von splewis.  
###### 1.2 Was kann LAMA?
- Verwalten von Gameservern auf der Source-Engine
  - Einsehen von Meta-Infos wie Playerzahlen und aktuelle Map
  - Ausführen von RCON Befehlen / Remote Execution
- Nutzerverwaltung
- Themes
- Anlegen von Profilen und eigenen Game Configs
- Eigener Installer
- Logging von Anmeldungen und Anmeldungsversuchen mit IP Tracking
- Schönes aber trotzdem sauberes Frontend
- Einbindung von FontAwesome zur Vebesserung der Intuitivität
- Keine Serverseitigen Plugins benötigt  
- Alle Passwörter werden stets nur verschlüsselt in der Datenbank gespeichert  

###### 1.3 Was kann LAMA nicht?  
- Überwachung von Performance der Zielserver  

###### 1.4 Was LAMA in Zukunft können soll  
- Hosting der Anwendung auf https://lama.nighttimedev.com sodass Server auch ohne eigene Installation verwaltet werden können.
- Erstellen von Matches mit Brackets, Teams usw.  
<br>  
<br>  

##### **2. Installation**
*Note: Für das Development wurden stets virtuelle Maschinen auf Debian 9, 64 bit verwendet. Auf anderen Distros kann die Funktionalität abweichen.*  
<br>
Wenn bereits ein funktionierender Webserver mit SQL und PHP (7.x) exisitert kann direkt zu Punkt 2.3 gesprungen werden.
###### 2.1 Benötigte Elemente
Bevor Anwendungen installiert werden, wird stets empfohlen vor den Installationen den Server zu Updaten / Upgraden, um Komplikationen oder fehlende Quellen zu vermeiden:  
```sh
  sudo apt-get update && sudo apt-get upgrade -y
```
###### 2.1.1 Webserver
Über die gesamte Entwicklung hinweg wurden Apache2 Wesberver verwendet. Über die Verwendung von z.B. nginx können keine Aussagen getroffen werden.  
Installation von Apache2:  
```sh
sudo apt-get install apache2
```
###### 2.1.2 PHP
PHP Installation:  
```sh
sudo apt install lsb-release apt-transport-https ca-certificates
sudo wget -O /etc/apt/trusted.gpg.d/php.gpg https://packages.sury.org/php/apt.gpg
echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" | sudo tee /etc/apt/sources.list.d/php7.3.list
sudo apt update
sudo apt-get install php7.3 php-pear
```
###### 2.1.3 Datenbank
Für die Verwendung der Anwendung ist eine SQL Datenbank nötig.  
SQL kann wie folgt installiert werden:  
```sh
  sudo apt-get install mysql-server
```
Um bei der Verwendung von IPTables einen Zugriff von außen zu erlauben:  
```sh
sudo ufw allow mysql
```
Damit das Arbeiten mit SQL einfacher und angenehmer wird, wird stark empfohlen phpMyAdmin zu installieren:  
```sh
  sudo apt-get install phpmyadmin php-mbstring php-gettext
  sudo phpenmod mbstring
  sudo systemctl restart apache2
```
Nach der Installation kann das Interface über <code>https://your_domain_or_IP/phpmyadmin</code>  aufgerufen werden.
Die Anweisungen zum Installieren von phpMyAdmin sind von [digitalocean.com](https://www.digitalocean.com/community/tutorials/how-to-install-and-secure-phpmyadmin-on-debian-9) übernommen.  
###### 2.2 Klonen der Repository
*Note: LAMA kann auch in Höheren Verzeichnissen des Webroots installiert werden, da sämtliche Verlinkungen intern relativ erfolgen.*  
<br>
Als erstes bewegen Sie sich zu dem Verzeichnis in dem LAMA installiert werden soll. Falls LAMA im Webroot installiert werden soll (was sich in den meisten Fällen empfiehlt), ist das Vorgehen wie folgt:  
```sh
  cd /var/www/html
  git clone https://github.com/nighttimedev/lama
  sudo mv ./lama/* ./ && sudo rm -rf ./lama
```
Hier gehen wir in den Webroot, klonen die Repository und verschieben anschließend alle Dateien direkt in den Rootpath. Danach wird der leere Ordner gelöscht.  
###### 2.3 Installation von LAMA
Dank dem eigenen Installers kann sich LAMA zu fast 100% selbst konfigurieren. Hierdurch kann, oft auch nervenaufreibende, Arbeit mit SQL / phpMyAdmin vermieden werden. Hierduch werden auch potentielle Fehlerquellen ausgeschlossen.  
Die Automatische Installation kann ohne Probleme mehrmals ausgeführt werden, da dieser niemals Daten löscht, sondern nur anlegt. Überschreiben ist auch nicht möglich, da der Installer jeden bereits vorhandenen Datensatz sofort überspringt.  
Der Installer kann sehr einfach gestartet werden, indem <code>https://your_domain_or_IP/install</code> aufgerufen wird.  
Direkt nach dem Aufruf wird der Installer gestartet. Dieser richtet dann sämtliche SQL Tabellen sowie deren Default-Werte und Spalten ein.  
Sollte es beim Installer zu einem Error kommen, zeigt er diesen an. Hierdurch kann der Fehler schnell gefunden werden.  
Wenn die Installation erfolgreich abgeschlossen ist, wurde, sofern es eine saubere Installation ist, ein Standard-User angelegt.  
<br>
Die Login-Daten hier sind:  
*root@root.com* : *lamaManager!*  
<br>
*Note: Fast immer liegt der Fehler bei der config.php, wenn hier z.B. bei der Adresse ein Tippfehler oder ein ungewolltes Leerzeichen unterkommen sind.*  
<br>  
##### **3. Verwendung**
Die Verwendung von LAMA ist i.d.R sehr einfach. Wirklich.  
LAMA soll so einfach wie möglich die Interaktion zwischen Admin und Gameserver ermöglichen. Auch wenn die Person in der Rolle des Admins keine Development Fähigkeiten besitzt.  
###### 3.1 Anmeldung
Wenn LAMA zum ersten mal installiert wurde, sind die default-Anmeldedaten unter Punkt 2.3 zu finden.  
Die Anmeldung erfolgt einfach mit der EMail Adresse des Users sowie dessen Passwort. User ohne Account können sich natürlich nicht einfach so einen anlegen, da das Interface nur für Admins zugänglich sein soll.  
Admins können über das (aktuell noch nicht implementierte) Plugin 'Usermanagement' Accounts anlegen oder entfernen.
###### 3.2 Was sind Plugins?
Nach dem erfolgreichen Login sind auf der Linken Seite innerhalb eines Menübereiches einzelne Reiter zu sehen. Dies sind die Plugins. Jeder Reiter repräsentiert ein eigenständiges Plugin, welches vom eigentlichen Dashboard eingebunden, bzw. entbunden, wird.  
Plugins sind sozusagen die einzelnen Werkzeuge, die der Admin zur Verfügung hat.  
Plugins können recht einfach selbst erstellt werden. Mehr hierzu weiter unten unter 'Für Coder'.
###### 3.3 Die elementaren Plugins
Hier werden die wichtigsten Plugins von LAMA erklärt.
###### 3.3.1 Home
Das 'Home' Plugin dient als kleiner Startbildschirm. Hier wird das aktuelle Changelog (per showdown.js von changelog.md konvertiert), sowie eine kurze Übersicht über alle gespeicherten Server angezeigt.  
###### 3.3.2 Servers
Hier können Gameserver hinzugefügt, entfernt und verwaltet werden. Hier sitzt sozusagen der eigentliche Kern von LAMA. Von hier können alle Server gesteuert und überwacht werden.
###### 3.3.3 Customize
Hier kann der User ein Theme wählen. Die Auswahl wird in JS Cookies gespeichert.
###### 3.3.4 Bans
Hier sind alle getätigten Bans von allen Gameservern zu finden. Dieses Plugin ist nützlich, wenn zum Beispiel alle Spieler auf einmal entbannt werden sollen, oder man schnell den letzten Ban zurückziehen möchte.
<hr>

### Für Coder
##### **1. Struktur**
Die Dateistruktur von LAMA ist simpel gehalten.  
Der Root folder enthält alle Seiten auf die der User Zugriff benötigt (abgesehen Plugins).  
<code>/assets</code> enthält kleine Codefragmente, das gesamte JavaScript sowie sonstige Daten wie Bilder.  
<code>/install</code> enthält den Installer ( <code>/install/index.php</code> )
<code>/plugins</code> ist der Ordner in dem alle Plugins abgelegt werden. Dieser Ordner wird bei jedem page Load indexiert.  
<code>/scq</code> enthält die [PHP-Source-Query](https://github.com/xPaw/PHP-Source-Query) von [xPaw](https://github.com/xpaw)
##### **2. Plugins entwickeln**
Die Entwicklung von Plugins ist sehr einfach, sofern man Kenntnisse in HTML und CSS hat. Damit eine funktionelle Dynamik genutzt werden kann ist zudem natürlich auch PHP notwendig.  
Jedes Plugin muss, unabhängig von seiner Funktion, folgende Grundstruktur einhalten:  
```php
<!DOCTYPE html>
<html>
  <head>
    <?php include '../assets/php/sessioncheck.php' ?>
    <meta charset="utf-8"/>
    <link href="https://fonts.googleapis.com/css?family=Roboto" rel="stylesheet"/>
    <link rel="stylesheet" href="../assets/css/dashboard.css"/>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.8.1/css/all.css" type="text/css">
    <script src="../assets/js/functions.js"></script>
  </head>
  <body class="framebody">

    <h1>Your Plugin Title here</h1>
    <span id="loadingstatus"></span>

    <div class="pane" id="pane_PANENAME">
      <h3>Title of the Pane</h3>
      <button class="pane_collapse" onclick="collapsePane('pane_PANENAME')">
        <i class="fas fa-chevron-up" id="pane_PANENAME_chevron"></i>
      </button>
      <div style="height: 35px; width: 1px;"></div>
    </div>
  </body>
</html>
```
Zur Beachtung: Wichtig ist es, die drei Codestellen an welchen *'PANENAME'* steht mit dem selben Wert (dem Namen des Panes) zu füllen. Sonst kann die Fläche nicht über das Chevron auf und zu geklappt werden.  
<br>
Bei jedem Aufruf der Seite wird <code>/plugins</code> neu indexiert. Dies passiert in <code>/dashboard.php</code>.  
- Es werden nur dateien mit Endung *.php* indexiert
- Dateien, welche mit einem Punkt beginnen, werden nicht indexiert
- *.php* Files in höher liegenden Ordnern werden nicht berücksichtigt  

Um ein einheitliches Design, sowie die 'Theme' Funktion aus <code>/plugins/customize.php</code> zu erhalten, ist es strikt notwendig, nicht mit eigenen Farben zu arbeiten. <code>/assets/css/dashboard.css</code> sollte alle nötigen Klassen liefern.  
Ist dies nicht der Fall, also muss eine neue Klasse definiert werden, müssen hier die root Farben der <code>/assets/css/dashboard.css</code> verwendet werden.
<hr>

## Für weitere Fragen:
##### Impressum:  
Dev(s): Jan-Morris Blüthner - The Nighttimedev  
Kontakt: develop@nighttimedev.com  
Weitere Projekte: https://die-lan.party  
<br>
https://nighttimedev.com  
