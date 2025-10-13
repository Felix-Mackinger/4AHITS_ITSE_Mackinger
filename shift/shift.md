5 — Die Aufgabe: Bash-Script — alle Hosts im Subnetz anpingen (nur aktive ausgeben)
Ziel: automatisch das Subnet (so gut wie möglich /24) ermitteln, sonst Benutzer-Eingabe akzeptieren. Alle aktiven Hosts ausgeben, keine Ausgabe für Offline-Hosts.
Kopiere das Script in z.B. scan_ping.sh, mach ausführbar chmod +x scan_ping.sh, und starte ./scan_ping.sh.
#!/usr/bin/env bash
# scan_ping.sh
# Zweck: prüft Hosts im Subnet (automatisch /24 erkannt, sonst manuell)
# Ausgabe: nur aktive Hosts (IP und optional Hostname)
set -o errexit
set -o pipefail
set -o nounset
# ----- helper: get interface, ip and cidr (best effort) -----
get_iface_and_cidr() {
# Versucht default route interface zu nehmen
 iface=$(ip route | awk '/default/ {print $5; exit}')
 if [ -z "$iface" ]; then
# fallback: erstes nicht-loopback
   iface=$(ip -o -4 addr show | awk '!/lo/ {print $2; exit}')
 fi
# get ip/cidr on that iface
 ipcidr=$(ip -4 -o addr show dev "$iface" | awk '{print $4; exit}')
 echo "$iface" "$ipcidr"
}
# ----- parse args or auto-detect -----
if [ "${1-}" == "--help" ] || [ "${1-}" == "-h" ]; then
 cat <<EOF
Usage:
 ./scan_ping.sh            # versucht Subnet automatisch (typ. /24)
 ./scan_ping.sh 10.0.0    # prefix (first 3 octets for /24)
 ./scan_ping.sh 10.0.0.0/24  # explicit cidr (only /24 supported automatically)
EOF
 exit 0
fi
# If user provided a prefix like 10.0.0 or 192.168.1
user_arg="${1-}"
if [ -n "$user_arg" ]; then
# normalize user input
 if [[ "$user_arg" =~ ^([0-9]{1,3}\.){2}[0-9]{1,3}$ ]]; then
   prefix="$user_arg"
 elif [[ "$user_arg" =~ ^([0-9]{1,3}\.){3}[0-9]{1,3}/24$ ]]; then
# e.g. 192.168.1.0/24 -> take first 3 octets
   prefix=$(echo "$user_arg" | cut -d. -f1-3)
 elif [[ "$user_arg" =~ ^([0-9]{1,3}\.){2}[0-9]{1,3}\.$ ]]; then
   prefix="${user_arg%?}"
 else
   echo "Unrecognized argument format. Use e.g. 192.168.1 or 192.168.1.0/24"
   exit 1
 fi
else
# auto-detect
 read -r iface ipcidr <<< "$(get_iface_and_cidr)"
 if [ -z "${ipcidr-}" ]; then
   echo "Konnte IP/CIDR nicht automatisch ermitteln. Bitte Prefix angeben, z.B. 192.168.1"
   exit 1
 fi
# ipcidr z.B. 192.168.1.23/24 -> extract first 3 octets
 prefix=$(echo "$ipcidr" | cut -d. -f1-3)
fi
# remove possible trailing dot
prefix="${prefix%.}"
echo "Scanning subnet: ${prefix}.0/24 (interface: ${iface-unknown})"
echo "Only active hosts will be shown."
# ----- build IP list (1..254) and run ping in parallel -----
# If xargs supports -P use it for concurrency
# ping params: -c1 send 1 packet, -W1 wait 1s for reply, -n numeric, -q quiet
seq 1 254 | xargs -I{} -P 60 bash -c '
 ip="$0.$1"
 if ping -c1 -W1 -n -q "$ip" >/dev/null 2>&1; then
   # optionally resolve hostname (non-blocking)
   host=$(getent hosts "$ip" | awk "{print \$2}" | head -n1 || true)
   if [ -n "$host" ]; then
     echo "$ip  $host"
   else
     echo "$ip"
   fi
 fi
' "$prefix" {}
# End
Hinweise zum Script
• Das Script nimmt an, dass dein Subnet /24 ist (üblich im Labor). Falls nicht, gib ./scan_ping.sh 10.0.2 oder ./scan_ping.sh 10.0.2.0/24 als Argument.
• Es nutzt xargs -P 60 für Parallelität (bis 60 Prozesse). Passe -P runter, falls dein Host zu sehr nach Luft ringt.
• ping -c1 -W1 → 1 Paket, 1s Timeout (schnell). Du kannst -W erhöhen, falls viele Hosts langsam antworten.
• Ausgabe: nur aktive IPs (evtl. mit Hostname, falls auflösbar). Keine Ausgabe für Offline-Hosts.
⸻
6 — Wie du das Script nutzt (Beispiel)
1. Speichern:
nano ~/repos/scan_ping.sh
# paste script, save
chmod +x ~/repos/scan_ping.sh
2. Automatisch (erkennt Interface/CIDR):
cd ~/repos
./scan_ping.sh
3. Manuell Prefix angeben:
./scan_ping.sh 192.168.10
# oder
./scan_ping.sh 10.0.0.0/24

⸻
7 — Weiterführende Ideen / Verbesserungen
• Benutze fping (viel schneller für große Netze) oder nmap -sn (host discovery mit ARP, oft besser lokal).
• Verwende ARP-Scan (arp-scan --localnet) — in einem Ethernet-LAN oft zuverlässiger, weil ICMP geblockt sein kann.
• Logge Ergebnisse mit Timestamp: ./scan_ping.sh | awk '{print strftime("%F %T"), $0}'
• Erweitere Skript, um Subnetze ≠ /24 korrekt zu handhaben (erfordert IP-Berechnungen).
⸻
