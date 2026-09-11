# Native VLAN Audit Procedure

## What to Check
For every access port (port connecting to an end device):
- The native (untagged) VLAN should match the device's expected network
- For AP uplink ports: native VLAN = management network, tagged VLANs = all client networks

## Common Findings
1. **Port set to wrong native VLAN** — device gets IP from unexpected subnet
2. **Port in "All" profile instead of specific VLAN** — works but unpredictable
3. **AP uplink missing tagged VLANs** — clients on some SSIDs can't get DHCP
4. **Phone port with voice VLAN but no data VLAN** — phone works, PC doesn't

## Script Approach
```python
# Pseudocode for VLAN audit
for switch in switches:
    for port in switch.ports:
        if port.is_uplink: continue  # trunk ports are complex
        native_vlan = port.native_networkconf.vlan
        expected = get_expected_vlan(port.connected_device)
        if native_vlan != expected:
            report(f"Mismatch on {switch.name}:{port.idx}: {native_vlan} != {expected}")
```
