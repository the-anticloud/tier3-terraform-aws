# L5 Narrow / L2 General Classification — terraform-aws
**Platform:** Anticloud | **PAX:** 27B | **IP:** USPTO pending 2026, Anticloud FZ LLE
**Domain:** Sovereign AWS deployment: air-gapped VPC with no external internet (outpost/GovCloud)

## L5 Narrow
terraform-aws specializes in sovereign aws deployment: air-gapped vpc with no external internet (outpost/govcloud) within the Anticloud sovereign deployment boundary. All operations stay local — no cloud services, no external APIs, no data exfiltration. The narrow scope ensures deterministic, auditable behavior that PAX 27B can reason about precisely.

## L2 General
L2 General means terraform-aws is available to all 9 Anticloud tiers without per-tier configuration. The same API serves hospital, defense, robotics, and research deployments.

## PAX Integration
PAX 27B validates Terraform plans before apply: verifies no Internet Gateway is attached, all S3 buckets use VPC endpoints, all EBS volumes are encrypted with customer-managed KMS keys.

## AIOSS Audit Relevance
Every Terraform apply event (plan hash + resource changes + state hash) is AIOSS-chained: H_n = SHA3-256(H_{n-1} || entry_hash_n || timestamp_n).
Tamper-evident, offline-verifiable, zero cloud dependency.

## Regulatory / Compliance
FedRAMP High, NIST SP 800-53, AWS GovCloud compliance, FIPS 140-2
