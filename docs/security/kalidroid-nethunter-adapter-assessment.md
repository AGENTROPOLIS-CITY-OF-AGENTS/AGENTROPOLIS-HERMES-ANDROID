# Mobile Security Adapter Assessment — Kalidroid / NetHunter

Status: PROPOSED / NOT DEPLOYED
Date: 2026-11-19
Owner: HERMES Android / APEX54 review
Scope: authorized defensive testing only

## Verified evidence
- Kalidroid vendor advertises Android 8+ ARM64, QEMU/pKVM, Kali guest, external adapter monitor mode/injection, USB passthrough, GPS, cloud dashboard and backup sync. These are **vendor claims**, not independently validated capabilities. Source: https://www.kalidroid.com/
- Website prominently labels build 1.3.33-debug; treat as untrusted pending provenance, source/license, signing, hashes, update-chain, privacy and independent isolation review.
- Official Kali NetHunter documentation establishes an open-source, unrooted Android Rootless edition with Kali CLI and KeX; **no Wi-Fi injection in Rootless**. Full NetHunter requires compatible kernel/hardware for injection. Sources: https://www.kali.org/docs/nethunter/ and https://www.kali.org/docs/nethunter/nethunter-rootless/
- Alternatives to evaluate: official NetHunter Rootless; Termux + proot-distro; vendor Kalidroid. Do not conflate their security models.

## Architecture (proposal)
HERMES Android UI -> Mobile Security Adapter (disabled by default) -> APEX54 capability and authorization gate -> selected local or remote read-only diagnostic runner -> evidence normalization -> signed receipt store.
CLOUD54 optional remote compute. HERMES ROAM may synchronize non-secret profile metadata only after encryption/privacy review.

## Adapter contract v0 (design only)
Input: request_id, actor_id, device_id (pseudonymous), adapter_kind, operation, target_scope, authorization_receipt, expiry, capability_manifest.
Output: request_id, status [denied|simulated|completed|failed], capability_evidence, redacted_findings, tool_digest, timestamp, receipt_digest.
Operations initially permitted: system_info, local_configuration_review, dependency_inventory, self_diagnostics.
Operations requiring separate explicit approvals: active network probes, radio capture, USB passthrough, cloud upload, location acquisition, remote command execution.
Default deny all unlisted operations. No hidden scans, persistent remote access, or auto-install.

## APEX54 gates
1. Inspect package provenance, reproducible build availability, license, SBOM and signature.
2. Simulate permissions, egress, data flows, VM escape boundaries and update rollback.
3. Preview exact command, scope, destination, data retention and costs.
4. Approve: explicit human authorization for every active target; separate approval for radio/location/cloud.
5. Execute in isolated test lab only, with scoped allowlist and bounded timeouts.
6. Verify artifact digest, command exit status, evidence redaction, no unexpected network egress.
7. Record tamper-evident success/denial receipts; never store credentials in reports.

## Comparison scorecard (not yet scored)
- Open source/license and supply-chain reproducibility
- Unrooted Android compatibility; ARM64 and storage requirements
- Real isolation, device-specific pKVM support, USB/OTG hardware behavior
- Offline capability; privacy/telemetry; update security
- Resource usage, accessibility, maintenance and ongoing cost
- APEX54 receipt integration and HERMES ROAM interoperability

## MATH54 / EVOLVE54 test plan
Record measured compatibility success rate, false-positive rate, memory/CPU overhead, p95 diagnostic latency, unexpected-egress count and denied-operation enforcement rate.
Test on an Android emulator or disposable authorized device first. Simulate hardware-incompatible, offline, revoked-authorization, tampered-package and malicious-update scenarios.
Release criteria: 100% denial of unauthorized operations, zero unexpected egress in lab, signed artifact provenance, explicit maintainer approval. No invented benchmarks.

## Decision
Prefer official NetHunter Rootless or Termux/proot as baseline for open-source-first proof of concept. Keep Kalidroid as an optional untrusted candidate pending independent review. Do not download APKs, request secrets, enable root, deploy agents, or alter production.
