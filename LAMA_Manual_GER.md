# >> LAMA <<
**LA**N-**Ma**nager
## Eine PHP basierte Webanwendung zur Verwaltung von Gameservern auf der Source Engine  
<br>

##### Impressum:  
Dev(s): Jan-Morris Blüthner - The Nighttimedev  
Kontakt: develop@nighttimedev.com  
Weitere Projekte: https://die-lan.party  
<br>
https://nighttimedev.com  
##### Impressum:
Der Markdown-Viewer von Github zeigt die geschachtelte Liste des Index falsch an.
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
##### 1. Allgemein
###### 1.1 Was ist LAMA?
LAMA wurde für die LAN-Party ["DIE-LAN"](https://die-lan.party) entwickelt. Hier liegt auch bis heute der Fokus. Allerdings habe ich im Laufe der Zeit bemerkt, dass eine Web-Basierte Applikation zur Verwaltung von Gameservern eine generelle Nachfrage birgt.  
Auf Grund dessen wurde LAMA so entwickelt, dass jeder Nutzer, der Grundkenntnisse von HTML/CSS (Optional PHP) birgt, Plugins für eigene Bedürfnisse entwickeln kann.  
Mein Ziel war es auch, dass LAMA selbst von Personen ohne Kenntnisee im Webdevelopment benutzt werden kann.  
Die eigentliche Idee zu LAMA kam mir durch das CS:GO Server-Plugin [get5](https://github.com/splewis/get5) von splewis und dem damit zusammenhängenden Proof-Of-Concept [get5-web](https://github.com/splewis/get5-web), ebenfalls von splewis.
###### 1.2 Was kann LAMA?
- Verwalten von Gameservern auf der Source-Engine
- Nutzerverwaltung
- Themes
- Anlegen von Profilen und eigenen Game Configs
- Eigener Installer
- Logging von Anmeldungen und Anmeldungsversuchen mit IP Tracking
- Schönes aber trotzdem sauberes Frontend
- Einbindung von FontAwesome zur Vebesserung der Intuitivität
- Keine Serverseitigen Plugins benötigt  

###### 1.3 Was kann LAMA nicht?  
- Überwachung von Performance der Zielserver
