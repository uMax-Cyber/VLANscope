<div align="center">

[![English](https://img.shields.io/badge/README-English-blue)](README.md)
[![Русский](https://img.shields.io/badge/README-Русский-red)](README.ru.md)
[![Oʻzbekcha](https://img.shields.io/badge/README-Oʻzbekcha-green)](README.uz.md)

</div>

# VLAN Troubleshooting Guide
[![CI](https://github.com/uMax-Cyber/VLANscope/actions/workflows/ci.yml/badge.svg)](https://github.com/uMax-Cyber/VLANscope/actions/workflows/ci.yml)


![Demo](screenshots/demo.svg)
Practical guide to diagnosing VLAN issues in multi-vendor environments (UniFi switches, Sophos firewalls, Linux bridges). Covers native/tagged mismatch detection, trunk verification, and DHCP-across-VLAN debugging.

## ✨ Features

- **Native/tagged mismatch detection** — find ports where the native VLAN doesn't match untagged traffic
- **Trunk verification** — confirm which VLANs actually cross a trunk link
- **DHCP-across-VLAN debugging** — relay and firewall checks for UDP 67/68
- **Multi-vendor coverage** — UniFi, Sophos, and Linux bridges in one guide
- **Copy-paste diagnosis commands** — ready-to-use `tcpdump` / `ip` recipes

## Common VLAN Problems (with solutions)

### 1. Native VLAN Mismatch
**Symptom**: Devices get IPs from wrong subnet. Intermittent connectivity.
**Cause**: Switch port native VLAN ≠ device expectation.
**Fix**: Audit all ports — native VLAN should match the untagged traffic on that port.

### 2. DHCP Not Crossing VLANs
**Symptom**: Devices on one VLAN can't get DHCP from server on another.
**Cause**: Either no DHCP relay, or firewall blocking UDP 67/68 between VLANs.
**Fix**: Check relay config on gateway; verify firewall rules for DHCP ports.

### 3. Trunk Missing VLAN Tags
**Symptom**: Some VLANs work, others don't — same physical link.
**Cause**: Trunk port not allowing all needed VLANs (tagged_vlan_mgmt = block_all).
**Fix**: Explicitly allow needed VLANs on trunk ports.

### 4. Static Lease in Wrong Scope
**Symptom**: One specific device stuck on "connecting..." while others work.
**Cause**: Device has static DHCP reservation in VLAN-A's scope but connects to VLAN-B.
**Fix**: Move the reservation to the correct scope (or make it global).

## Diagnosis Tools

```bash
# Check what VLANs are actually on a trunk
tcpdump -lni <iface> vlan -c 20

# Verify DHCP relay path
# On gateway: check DHCP relay config for each VLAN interface

# Test from specific VLAN (create temp subinterface)
ip link add link eth0 name eth0.XXX type vlan id XXX
ip link set eth0.XXX up
dhclient eth0.XXX
```

## License
MIT

## 📬 Contact

Questions? Reach out: **[allumaxmail@gmail.com](mailto:allumaxmail@gmail.com)**

---

<div align="center">

[![English](https://img.shields.io/badge/README-English-blue)](README.md)
[![Русский](https://img.shields.io/badge/README-Русский-red)](README.ru.md)
[![Oʻzbekcha](https://img.shields.io/badge/README-Oʻzbekcha-green)](README.uz.md)

</div>
