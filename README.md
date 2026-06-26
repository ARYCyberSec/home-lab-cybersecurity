# Home Lab — Cybersecurity Blue Team

**[English]** Personal cybersecurity laboratory focused on 
Blue Team operations, network traffic analysis, and incident response.

**[Español]** Laboratorio personal de ciberseguridad enfocado 
en operaciones Blue Team, análisis de tráfico de red y respuesta 
a incidentes.

---

## Infrastructure / Infraestructura
- **Attacker / Atacante:** Kali Linux 2026.1 — 192.168.100.10
- **Target / Objetivo:** Metasploitable 2 — 192.168.100.20
- **Hypervisor:** VirtualBox — red interna aislada / isolated internal network
- **VPN:** ProtonVPN (WireGuard) activa en eth0

---

## Tools / Herramientas
| Tool | Purpose / Uso |
|------|--------------|
| Nmap 7.99 | Network scanning & service enumeration / Escaneo y enumeración |
| Wireshark 4.6.6 | Network traffic analysis / Análisis de tráfico |
| Splunk | SIEM — próximamente / coming soon |
| Security Onion | IDS/IPS — próximamente / coming soon |

---

## Documented Incidents / Incidentes Documentados
| ID | Type / Tipo | Severity / Severidad | Status / Estado |
|----|-------------|----------------------|-----------------|
| IR-001 | Network Reconnaissance — SYN Scan | HIGH | Resolved |

---

## Week 1 / Semana 1 — Findings / Hallazgos
- 23 puertos abiertos identificados en el target / 23 open ports identified
- Servicios críticos expuestos / Critical services exposed:
  - FTP anónimo habilitado / Anonymous FTP enabled
  - Shell remota con permisos root en puerto 1524 / Root shell exposed on port 1524
  - Samba 3.0.20 sin autenticación / Samba unauthenticated
  - Telnet en texto plano / Telnet plain text transmission
  - MySQL y PostgreSQL expuestos a la red / Databases exposed to network
- Evidencia recolectada / Evidence collected:
  - Nmap output (.txt)
  - Wireshark captures (.pcapng)
  - Formal incident report IR-001 (.pdf)

---

## MITRE ATT&CK Techniques Covered
- T1046 — Network Service Discovery
- T1595.001 — Active Scanning: Scanning IP Blocks
- T1595.002 — Active Scanning: Vulnerability Scanning

---

## Roadmap
- [x] Escaneo de red con Nmap / Network scanning
- [x] Análisis de tráfico con Wireshark / Traffic analysis
- [x] Reporte de incidente IR-001 / Incident report
- [ ] Análisis de logs — /var/log/
- [ ] Configuración Splunk SIEM
- [ ] Security Onion IDS
- [ ] Honeypot con Cowrie

---

## Certifications in Progress / Certificaciones en Curso
- Cisco Junior Cybersecurity Analyst (en curso / in progress)
- ISC2 Certified in Cybersecurity — CC (próximo / next)
- Cisco CyberOps Associate (próximo / next)
- CompTIA Security+ (planificado / planned)

---

## Author / Autor
**Ary Otero**  
Cybersecurity Analyst in Training | Blue Team | SOC  
[LinkedIn](https://www.linkedin.com/in/arymatiasotero)
