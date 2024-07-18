# Übungsblatt 06

## Aufgabe 1

### Sliding-Window

Was ist das Sliding-Window-Protokoll und wie funktioniert es?
Sliding-Window-Protokolle sind Protokolle, die die Datenflusskontrolle zwischen zwei Kommunikationspartnern regeln und am häufigsten im TCP-Protokoll genutzt wird. Die Sliding-Window-Strategie verbindet die Vorteile von Quittungsbetrieb
und kontinuierlichem Senden und minimiert deren Nachteile. Beim strengen Quittungsbetrieb wird nach jedem empfangenen Datenpaket eine Quittung gesendet und erst bei deren Erhalt das näcshte Paket versendet. Dadurch wird i.d.R.
nur ein Bruchteil der Bandbreite der Verbindung genutzt. Beim kontinuierlichen Senden werden Pakete ohne Acknowledgement an den Empfänger gesendet wodurch dessen Ressourcen schnell überlastet werden können.
Das Sliding Window ist ein Fenster an noch nicht quittierten Paketen, das sich mit jedem empfangenen Acknowledgement um ein Paket nach rechts verschiebt, daher der Name Sliding Window (gleitendes Fenster). Solange
dieses Fenster nicht voll ist, verschickt der Sender ununterbrochen Pakete. Erst wenn das Fenster voll ist, wartet der Sender mit dem Senden neuer Pakete, bis ein neues ACK eintrifft und somit das Fenster um eine Stelle verschoben wird.
Für die Effizienz und Effektivität des Sliding-Window-Protokolls ist es wichtig, dass die Größe des Fensters optimal gewählt wird. Ist das Fenster zu klein, wird die Bandbreite nicht optimal ausgenutzt, ist es zu groß, kann es zu Überlastungen kommen.
Fenster werden i.d.R. dynamisch angepasst, um die Bandbreite optimal auszunutzen und Überlastungen zu vermeiden.
In der konkreten Implementation des Sliding-Window unterscheiden sich die TCP-Protokolle. Tahoe, Reno und Vegas.

### Tahoe

Tahoe beginnt bei einer Fenstergrösse von 1 und erhöht diese bei jedem ACK. Kommt es jedoch zu 3 Duplicate ACKS, wird die das Fenster auf 1 zurückgesetzt. Daraus ergibt sich das Tahoe-typische Sägezahnmuster.
**Grafik Sägezahnmuster**
Als erste TCP-Implementierung hat Tahoe das Sliding-Window-Protokoll implementiert und ist sehr konservativ. Der effektive Datendurchsatz ist daher weit vom Optimum entfernt, welches sich aus der Speichergröße des Übertragungsmediums ergibt.

### Reno

Reno ist eine Weiterentwicklung von Tahoe und hat das Problem des Sägezahnmusters gelöst. Bei 3 Duplicate ACKs wird das Fenster nicht auf 1 zurückgesetzt, sondern in der Größe halbiert. Dadurch wird ein erhöhter Datendurchsatz erreicht.
Selektives Acknowledgement (SACK) und Fast Retransmit sind weitere Features von Reno. SACK ermöglicht es, mehrere Pakete in einem ACK zu bestätigen und Fast Retransmit ermöglicht es, ein Paket erneut zu senden, wenn ein ACK für ein Paket empfangen wird, das noch nicht gesendet wurde.

### Vegas

Vegas nähert sich dem Optimum des Datendurchsatzes an, indem es die Bandbreite des Übertragungsmediums anhand der gemessenen Roundtrip-Durchlaufzeit ermittelt und das Fenster dynamisch anpasst.
Vegas schafft einen um 40% erhöhten Datendurchsatz gegenüber Reno, ist jedoch sehr komplex und daher nicht so weit verbreitet wie Tahoe und Reno.

### Protokolle

#### TCP

#### UDP

#### IP

#### Ethernet

#### ICMP

#### ARP

#### DHCP

#### DNS

#### HTTP

#### HTTPS

#### FTP

#### POP3

#### IMAP

#### SSH

#### NAT

## Aufgabe 2

DHCP verwendet UDP als Transportprotokoll und Port 67 und 68.
Capture Filter für DHCP: `udp port 67 or udp port 68`

Um DHCP-Pakete zu triggern hab ich zunächst den Befehl 'ipcconfig /renew' in der CMD ausgeführt, danach 'ipconfig /release' um meine ip freizugeben und erneut 'ipconfig /renew'.
Daraufhin wurden die DHCP-Pakete im Wireshark angezeigt. ein 'Request' und ein 'Acknowledge' Paket.

dann ein Release-Paket:
Src: 192.168.178... ist der Clientrechner(AsusTekCompu) Dst: 192.168.178.1 ist der Router(Fritzbox, 'AVMAudiovisu...').
Unicast
**Grafik release-Paket**

ein Discover-Paket:
ein Broadcast-Paket von meinem Clientrechner an alle Geräte im Netzwerk.
**Grafik discover-Paket**

Daraufhin ein Offer-Paket:
ein Broadcast-Paket von meinem Router an alle Geräte im Netzwerk.

**Grafik offer-Paket**

noch je ein Request- und Discover-Paket.

Schließlich ein Acknowledge-Paket:
als Broadcast-Paket von meinem Router an alle Geräte im Netzwerk.


## Aufgabe 3

a) Wie viele Hosts befinden sich in ihrem lokalen Klasse-C-Netz?
nmap -sn 192.168.178.0/24
-sn führt einen Ping-Scan durch, der zeigt, welche Hosts im Netzwerk aktiv sind.

Starting Nmap 7.95 ( https://nmap.org ) at 2024-07-15 21:13 W. Europe Daylight Time
Nmap scan report for fritz.box (192.168.178.1)
Host is up (0.0020s latency).
MAC Address: ...(AVM Audiovisuelles Marketing und Computersysteme GmbH)
Nmap scan report for PANASONIC-TV.fritz.box (192.168.178...)
Host is up (0.0097s latency).
MAC Address: ... (Edimax Technology)
Nmap scan report for ...fritz.box (192.168.178...)
Host is up.
Nmap done: 256 IP addresses (3 hosts up) scanned in 2.16 seconds

b) Welches Betriebssystem wird von scanme.nmap.org verwendet?
nmap -O scanme.nmap.org
-O aktiviert die Betriebssystemerkennung
...
Running: Linux 4.X|5.X
OS CPE: cpe:/o:linux:linux_kernel:4 cpe:/o:linux:linux_kernel:5
OS details: Linux 4.15 - 5.19
...


c) An welchem Datum wurde die Webseite nmap.org registriert?
C:\whois>whois nmap.org

Whois v1.21 - Domain information lookup
Copyright (C) 2005-2019 Mark Russinovich
Sysinternals - www.sysinternals.com

Connecting to ORG.whois-servers.net...

WHOIS Server: whois.dynadot.com
Registrar URL: http://www.dynadot.com
Updated Date: 2023-08-31T05:05:15Z
Creation Date: 1999-01-18T05:00:00Z


d) Wie kann man möglichst effektiv eine größere Menge an Adressen nach offenen TCP-Ports scannen?
nmap -p- -T4 192.168.178.0/24
-p- scannt alle Ports, -T4 steht für aggressives Timing(schnell, kann aber zu Fehlern führen)
nmap -p- 1111-2222 -T4 192.168.178.0/24
scannt alle Ports von 1111 bis 2222

e) Wie funktioniert der SYN-Scan und für was kann man ihn verwenden?
nmap -sS 192.168.178.1

Der SYN-Scan ist ein Port-Scan, bei dem der Scanner nur SYN-Pakete an die Ziel-IP-Adresse sendet und auf die Antwort wartet. Wenn ein SYN/ACK-Paket zurückkommt, ist der Port offen, wenn ein RST-Paket zurückkommt, ist der Port geschlossen.
Dann kann eine richtige Verbindung aufgebaut werden. Der SYN-Scan wird auch als halboffener Scan bezeichnet, da er keine vollständige Verbindung aufbaut.
f) Welches sind die offenen Ports, die bei Ihren bisherigen Nmap-Scans am häufigsten auftreten, und wofür werden
sie verwendet?

53/tcp    open  domain // DNS (Domain Name System)
80/tcp    open  http // HTTP (HyperText Transfer Protocol). Wird für unverschlüsselte Web-Kommunikation verwendet.
443/tcp   open  https // HTTPS (HyperText Transfer Protocol Secure). Wird für verschlüsselte Web-Kommunikation verwendet.
5060/tcp  open  sip // SIP (Session Initiation Protocol). Wird zur Initiierung, Verwaltung und Beendigung von VoIP-Anrufen verwendet.

8181/tcp  open  intermapper // Netzwerkkartierungs- und Überwachungstool.
8182/tcp  open  vmware-fdm // VMware Fault Domain Manager (FDM). Wird von VMware für das Management von Ausfällen in virtuellen Infrastrukturen verwendet.

Weitere offene Ports:
8183/tcp  open  proremote
8184/tcp  open  itach
...
48000/tcp open  nimcontroller
49000/tcp open  matahari
...

### Aufgabe 4

Erstelle zunächst eine Matrix und merk dir die Distanzen zu deinen direkten Nachbarn.
Gbi die Informationen an die anderen Routern weiter und hol dir deren Informationen ein.
Integriere die Informationen der anderen Router in deine Matrix bis sich keine Veränderungen mehr ergeben und terminiere.

Beim Link-State-Verfahren kennt jeder Router die gesamte Topologie des Netzwerks, also den kompletten Graphen mit dazu gehörigner Distanzinformation.
Jeder Router berechnet dann den kürzesten Weg zu allen anderen Routern im Netzwerk. Beim Distanzvektor-Verfahren kennt jeder Router nur die Distanzen zu seinen direkten Nachbarn und gibt diese Informationen an seine Nachbarn weiter.

