# Cisco PSIRT-flagged hardening pack

These rules implement **vendor-published hardening recommendations**
that go beyond the CIS Level-1 baseline. They are not CVE matching —
CVE / firmware vulnerability correlation lives in Centraleyezer
Platform per PLAN.md §0.

Each rule references the Cisco PSIRT advisory or hardening guide
that originated it. The advisories are mostly stable hardening
recommendations (e.g. "disable Smart Install"); when a specific CVE
exists it's listed in the references for traceability but the rule
itself checks the *configuration* signature, not the CVE.

Pack id: `psirt-cisco`.
