# IR-001 — Network Reconnaissance: SYN Scan
> **Analyst / Analista:** Ary Otero  
> **Date / Fecha:** 25/06/2026  
> **Severity / Severidad:** 🔴 HIGH  
> **Status / Estado:** ✅ Resolved  

---

## 1. Executive Summary / Resumen Ejecutivo

**[EN]** Active reconnaissance scan detected originating from 
192.168.100.10 (Kali Linux) targeting 192.168.100.20 
(Metasploitable 2). A SYN scan identified 23 open ports 
with multiple critical vulnerabilities including anonymous 
FTP, exposed root shell, and unauthenticated Samba access.

**[ES]** Se detectó escaneo de reconocimiento activo desde 
192.168.100.10 (Kali Linux) hacia 192.168.100.20 
(Metasploitable 2). Un SYN scan identificó 23 puertos 
abiertos con múltiples vulnerabilidades críticas incluyendo 
FTP anónimo, shell remota expuesta y acceso Samba sin 
autenticación.

| Field | Value |
|-------|-------|
| Incident Type | Network Reconnaissance — SYN Scan |
| Source IP | 192.168.100.10 (Kali Linux) |
| Target IP | 192.168.100.20 (Metasploitable 2) |
| Open Ports | 23 |
| Detection Time | 19:48:00 |
| Resolution Time | 20:12:00 |
| Response Time | 24 minutes |

---

## 2. Detection / Detección

**Tools used / Herramientas utilizadas:**
- Nmap 7.99 — service enumeration
- Wireshark 4.6.6 — real-time traffic capture on eth1

**Detection method / Método de detección:**
Anomalous volume of TCP SYN packets detected on eth1 — 
1000 SYN packets in under 4 seconds from single source IP.

**Wireshark filters applied / Filtros aplicados:**
Detect SYN scan (attacker side)
tcp.flags.syn == 1 && tcp.flags.ack == 0
Confirm open ports (target responses)
tcp.flags.syn == 1 && tcp.flags.ack == 1

**Results / Resultados:**
| Filter | Packets | % of total |
|--------|---------|------------|
| SYN only | 1000 | 49.3% |
| SYN-ACK | 23 | 1.1% |
| Total captured | 2029 | 100% |

---

## 3. Timeline / Línea de Tiempo

| Time | Event |
|------|-------|
| 19:48:00 | Nmap -sS scan initiated from 192.168.100.10 toward 192.168.100.20 |
| 19:48:02 | Wireshark detects anomalous SYN packet volume on eth1 |
| 19:48:04 | SYN filter applied — scan confirmed |
| 19:48:06 | SYN-ACK filter applied — 23 open ports identified |
| 19:48:08 | Nmap scan completed in 3.89 seconds |
| 20:12:00 | Forensic evidence saved — 3 files in ~/lab-evidencia/ |

---

## 4. Indicators of Compromise / Indicadores de Compromiso

| Type | Value | Description |
|------|-------|-------------|
| IP | 192.168.100.10 | Scan origin — Kali Linux |
| IP | 192.168.100.20 | Target — Metasploitable 2 |
| Port | 1524 | Bindshell — root shell exposed |
| Port | 21 | FTP vsftpd 2.3.4 — anonymous login enabled |
| Port | 139/445 | Samba 3.0.20 — guest account access |
| Port | 23 | Telnet — plaintext transmission |
| Port | 3306 | MySQL 5.0.51a — exposed to network |
| Port | 5432 | PostgreSQL 8.3 — exposed to network |

---

## 5. Technical Analysis / Análisis Técnico

### Attack Pattern / Patrón de Ataque
SYN scan (half-open scan) — Nmap flag `-sS`. Sends SYN 
packets without completing the TCP handshake. Each packet 
has `Seq=0, Len=0` — signature of reconnaissance scanning.

### Critical Findings / Hallazgos Críticos

**🔴 CRITICAL — Port 1524 Bindshell**  
Root shell open and waiting for connections. 
No authentication required. Full system compromise possible.

**🔴 CRITICAL — Port 21 FTP vsftpd 2.3.4**  
Anonymous login enabled — access without credentials.  
vsftpd 2.3.4 has a known backdoor (CVE-2011-2523).

**🔴 CRITICAL — Port 139/445 Samba 3.0.20**  
Guest account access enabled.  
Message signing disabled — vulnerable to Man-in-the-Middle.  
CVE-2007-2447 — remote command execution.

**🟠 HIGH — Port 23 Telnet**  
Transmits credentials in plaintext.  
Capturable with Wireshark in real time.

**🟠 HIGH — Port 3306/5432 Databases**  
MySQL and PostgreSQL directly exposed to network.  
Should only be accessible from localhost.

### MITRE ATT&CK Mapping
| Technique | ID | Description |
|-----------|-----|-------------|
| Network Service Discovery | T1046 | Port scanning to enumerate services |
| Active Scanning: IP Blocks | T1595.001 | Systematic IP/port scanning |
| Active Scanning: Vulnerability Scanning | T1595.002 | Service version detection |

---

## 6. Response & Containment / Respuesta y Contención

**Actions taken / Acciones tomadas:**
1. Full traffic capture with Wireshark on eth1
2. TCP filters applied to isolate and confirm scan type
3. Service enumeration with `nmap -sV -sC` for version detection
4. Forensic evidence preserved in 3 files
5. Formal incident report documented with IOCs and MITRE mapping

**Containment / Contención:**  
✅ Controlled lab environment — no external network exposure risk.

**Response time / Tiempo de respuesta:**  
Detection to full documentation: **24 minutes**

---

## 7. Lessons Learned / Lecciones Aprendidas

**[EN]**
- Learned to identify SYN scan signature using Wireshark TCP filters
- Understood difference between SYN (recon) and SYN-ACK (open port confirmation)
- Nmap and Wireshark are complementary — Nmap gives results, Wireshark shows the process
- VPN routing affects capture interface — lab traffic runs on eth1, not eth0
- Forensic evidence must be organized immediately after detection

**[ES]**
- Aprendí a identificar la firma de un SYN scan con filtros TCP en Wireshark
- Entendí la diferencia entre SYN (reconocimiento) y SYN-ACK (puerto abierto)
- Nmap y Wireshark se complementan — Nmap da el resultado, Wireshark muestra el proceso
- La VPN afecta la interfaz de captura — el tráfico del lab viaja por eth1
- La evidencia forense debe organizarse inmediatamente después de la detección

**Proposed improvements / Mejoras propuestas:**
- [ ] Configure Splunk alert for >100 SYN packets from same IP in 60 seconds
- [ ] Implement IDS rule in Security Onion for automatic SYN scan detection
- [ ] Create traffic baseline for anomaly comparison in future incidents
- [ ] Research CVEs for each exposed service version found

---

## 8. Evidence Files / Archivos de Evidencia

| File | Description |
|------|-------------|
| `escaneo_completo.txt` | Nmap full scan output — 23 open ports |
| `escaneo_syn.pcapng` | Wireshark capture — 1000 SYN packets |
| `puertos_abiertos_syn_ack.pcapng` | Wireshark capture — 23 SYN-ACK responses |

---

*Lab environment — Metasploitable 2 is an intentionally 
vulnerable VM for educational purposes only.*
