# Incident Response: Network Traffic & Malware Forensics (Wireshark)

## Objective
Analyze a network packet capture (PCAP) to investigate an automated security alert, identify the compromised internal endpoint, extract Indicators of Compromise (IoCs), and trace malicious HTTP payload distribution.

## Tools Used
* **Wireshark** (Network protocol analyzer & packet forensics)

## Executive Summary
An internal host on the network was compromised after navigating to a legitimate website (`www.ciniholland.nl`). The connection was redirected to external exploit kit infrastructure, which served an encrypted payload from server IP `37.200.69.143`.

## Forensic Findings & Indicators of Compromise (IoCs)

### Victim Endpoint Details
* **Internal IP:** `172.16.165.165`
* **Hostname:** `K34EN6W3N-PC`
* **MAC Address:** `f0:19:af:02:9b:f1`

### Threat Infrastructure Details
* **Compromised Referrer Host:** `www.ciniholland.nl` (`82.150.140.30`)
* **Attacker C2 / Payload Server IP:** `37.200.69.143`
* **Delivery Method:** HTTP GET request with obfuscated URI parameters (`/index.php?req=mp3...`)

---

## Technical Investigation

### Phase 1: Host Identification
Using NetBIOS Name Service (`nbns`) display filtering, internal broadcast traffic was analyzed to map the IP address `172.16.165.165` directly to system hostname `K34EN6W3N-PC` and physical MAC address `f0:19:af:02:9b:f1`.

![Host Identification Screenshot](nbns.png)

### Phase 2: Malicious Traffic & IoC Extraction
By filtering HTTP host requests (`http.host`), outbound traffic originating from `172.16.165.165` to external destination `37.200.69.143` was isolated. Packet #1569 confirms an unencrypted HTTP GET request attempting to fetch an exploit payload.

![HTTP Traffic Analysis Screenshot](http.jpg)
