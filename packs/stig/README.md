# STIG rule pack

This directory ships Netzer rule files modelled on DISA STIG checks for
Cisco network devices. Each rule carries a `references.stig: [V-XXX]`
list. Rules are normal Rule files (run through the evaluator with
positive/negative fixtures), not overlays.

## V-ID realignment status

DISA reshuffles V-IDs between STIG releases, so the IDs in this pack
must be re-verified periodically against the current XCCDF.

| Pack | V-ID source | Status |
|---|---|---|
| `cisco-ios/` (NDM subset: ssh_v2 / telnet / banner / logging / NTP / SNMP / passwords / service-hardening / aux) | `OpenSTIG-Ansible/cisco_ios-xe_router_ndm_stig` main.yml | Realigned to live V-215807..V-220139 range |
| `cisco-ios/l3_stp_bpduguard_default.yaml`, `l3_access_ports_bpduguard.yaml`, `l3_dhcp_snooping.yaml` | `ansible-lockdown/CISCO-IOS-L2S-STIG` | Realigned to V-220630 / V-220633 |
| `cisco-ios/icmp_rate_limit.yaml`, `ssh_source_iface.yaml` | (no clean source) | **Placeholder V-220157 / V-220158** — verify against current STIG |
| `cisco-ios/l3_access_ports_port_security.yaml`, `l3_vtp_transparent.yaml`, `l3_ip_cef.yaml` | (no L2S equivalent) | **Placeholder V-220601 / V-220604 / V-220605** — these may not be STIG-mandated; reclassify as PSIRT-hardening if so |
| `cisco-ios/rtr_*.yaml` (Router RTR) | (no Ansible mirror found) | **Placeholder V-220500..V-220506** — needs DISA Cisco IOS XE Router RTR STIG XCCDF |
| `cisco-nxos/*.yaml` (NDM) | (no Ansible mirror found) | **Placeholder V-220300..V-220315** — needs DISA Cisco NX-OS Switch NDM STIG XCCDF |
| `cisco-asa/*.yaml` (NDM / FW / VPN / IDPS) | (no Ansible mirror found) | **Placeholder V-220400+ / V-220700+ / V-220800+ / V-220900+** — needs DISA Cisco ASA NDM / Firewall / VPN / IDPS STIG XCCDFs |

## How to realign once you have the XCCDF

1. Download the relevant STIG zip from public.cyber.mil (requires a
   browser session — DISA pages are JavaScript-rendered).
2. Unzip to get the `*Manual_STIG.xml` (XCCDF format).
3. Run a one-shot mapper:

       go run ./tools/stig-realign \
           --xccdf path/to/Cisco_IOS_XE_NDM_STIG.xml \
           --pack  internal/rules/packs/stig/cisco-ios/

   (The `tools/stig-realign` helper does not exist yet — it would
   match Netzer rule titles against XCCDF `<title>` fragments and
   emit a proposed rename table for review before applying.)

4. After re-tagging, run `make rules-lint` to confirm no duplicate
   IDs were introduced (the dedupe lint pass catches collisions
   across `<vid>.<discriminator>` slugs).

## Compound V-IDs

A single STIG V-ID often covers multiple distinct config knobs (e.g.
V-215823 "disable nonsecure services" covers HTTP server, source
routing, proxy ARP, PAD, finger, AUX, bootp). Netzer keeps these as
separate rule files for granular findings; the slug carries a
discriminator after the V-ID — `cisco.ios.stig.v-215823.no_pad`,
`cisco.ios.stig.v-215823.http_off`, etc — and all share
`references.stig: ["V-215823"]`. This is intentional and audited by
the duplicate-ID lint pass.
