# IR-001 — Network Reconnaissance SYN Scan

## Evidence Files / Archivos de Evidencia
- `escaneo_completo.txt` — Nmap full scan output
- `escaneo_syn.pcapng` — SYN packets capture (1000 packets)
- `puertos_abiertos_syn_ack.pcapng` — Open ports confirmation (23 ports)
- `IR-001_reporte.pdf` — Full incident report

## Summary / Resumen
- **Date / Fecha:** 25/06/2026
- **Severity / Severidad:** HIGH
- **Status / Estado:** Resolved
- **Open ports / Puertos abiertos:** 23
- **Critical findings:** FTP anónimo, root shell puerto 1524, Samba sin autenticación

## MITRE ATT&CK
- T1046 — Network Service Discovery
- T1595.001 — Active Scanning: Scanning IP Blocks
- T1595.002 — Active Scanning: Vulnerability Scanning
