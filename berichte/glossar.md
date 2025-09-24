# Glossar – Kali Linux & Metasploitable

**BackTrack / Kali Linux**  
Debian-basierte Linux-Distribution, speziell für Penetration-Testing  
und digitale Forensik. Enthält Hunderte Security-Tools (z. B. Nmap, Metasploit, Wireshark).

**Metasploitable**  
Absichtlich verwundbare Linux-VM (meist Ubuntu-basiert),  
entwickelt zum Testen von Penetrationstools und Exploits.

**Metasploit Framework**  
Open-Source Exploitation-Framework für das Auffinden, Testen  
und Ausnutzen von Sicherheitslücken.

**Exploit**  
Programm oder Code, der eine Sicherheitslücke gezielt ausnutzt,  
um ungewollten Zugriff oder Rechte zu erhalten.

**Payload**  
Der „Nutzinhalt“ eines Exploits, z. B. ein Reverse Shell Script  
oder ein Meterpreter-Session-Loader.

**Meterpreter**  
Erweiterter Metasploit-Payload, der nach erfolgreichem Exploit  
eine interaktive Remote-Shell mit vielen Post-Exploitation-Features bietet.

**Nmap**  
Netzwerkscanner zum Auffinden von Hosts, offenen Ports und  
laufenden Diensten. Basis für viele Reconnaissance-Phasen.

**Service Enumeration**  
Technik zum Ermitteln von Versionsinformationen (z. B. Apache 2.4.6)  
für gezielte Exploit-Auswahl.

**Brute Force**  
Automatisiertes Ausprobieren vieler Passwort-/Schlüssel-Kombinationen,  
bis ein Zugang gefunden wird.

**Dictionary Attack**  
Variante von Brute Force, nutzt vorbereitete Wortlisten  
(z. B. rockyou.txt), um Passwörter schneller zu erraten.

**Privilege Escalation (PrivEsc)**  
Techniken zum Erlangen höherer Rechte (z. B. von normalem Benutzer  
zu Root), nachdem ein System bereits kompromittiert wurde.

**Reverse Shell**  
Verbindung, bei der sich das Zielsystem **aktiv** zurück zum Angreifer verbindet,  
um Firewalls/NAT zu umgehen.

**Bind Shell**  
Umgekehrtes Prinzip zur Reverse Shell: Der Angreifer verbindet sich  
zu einem offenen Port des Zielsystems.

**Armitage**  
GUI-Frontend für Metasploit zur Visualisierung von Angriffen  
und Team-Collaboration.

**Hydra**  
Tool für automatisierte Brute-Force-Angriffe auf Login-Dienste  
(SSH, FTP, HTTP-Basic, etc.).

**SQL Injection (SQLi)**  
Einschleusen von SQL-Befehlen in unsichere Web-Formulare,  
um Datenbanken zu manipulieren oder auszulesen.

**Cross-Site Scripting (XSS)**  
Einbetten von schädlichem JavaScript in Webinhalte,  
das bei anderen Nutzern ausgeführt wird.

**VM Snapshot**  
Speicherabbild des aktuellen Zustands einer virtuellen Maschine –  
nützlich, um nach Tests schnell auf den Ausgangszustand zurückzuspringen.

**Ethical Hacking**  
Legales Penetration Testing mit Zustimmung des Besitzers,  
um Schwachstellen zu finden, bevor echte Angreifer sie ausnutzen.

---

*Hinweis:* Dieses Glossar deckt zentrale Begriffe für Laborübungen mit  
**Kali Linux** & **Metasploitable** ab. Es kann nach Bedarf um weitere  
Tools (z. B. Burp Suite, Nikto, Gobuster) erweitert werden.
