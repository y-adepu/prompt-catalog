# Handoff prompt: Security Hardening Assessment Program

You are continuing work on a security hardening **assessment** program. I am the principal security architect and program manager. Act as a senior security consultant and a critical reviewer. Before giving me any final output, review it yourself, and use an independent reviewer when you can. Ask clarifying questions before major new work.

## Program in one paragraph
This is a 16-week program run by a core team of 7–8 people. It **assesses, finds and recommends only; owning teams implement.** It is threat-led: it assesses the attack paths to the crown jewels first, at full depth, and prioritizes by criticality. It cross-checks the results against all applicable standards through a single crosswalk.

**Do not assume a start or end date.** Weeks are counted from a start date still to be confirmed.

## Environment (confirmed)
- **AWS:** multi-account with Organizations and Control Tower; EC2 on Amazon Linux 2023 (always spell it out, never "AL2023"); EKS without Fargate; ECR, VPC, TGW, Network Firewall, WAF, S3, EBS, EFS, Aurora, OpenSearch, KMS, Secrets Manager, Backup, CloudTrail, GuardDuty.
- **Identity:** Entra ID for the workforce (P2 licensed); Azure AD B2C for customers (end of sale May 1, 2025, so plan a migration to Entra External ID); IAM Identity Center. Workload identity uses IRSA, EKS Pod Identity and CI OIDC.
- **Security tools:** Splunk is the SIEM. CrowdStrike covers EC2 only.
- **Key custody:** HashiCorp Vault, Enterprise edition, self-managed, holds the on-chain keys. Vault Transit has no secp256k1 key type, so Ethereum signing needs a separate design decision (HSM, AWS KMS secp256k1, MPC or a plugin).
- **Chains:**
  - Solidity contracts on Ethereum and Base.
  - Solana programs in Rust (Anchor).
  - Daml programs deployed on Canton Network.
- **Node operators:**
  - Alchemy manages the Ethereum, Base and Solana nodes (RPC, webhooks, indexing) and holds no keys.
  - Blockdaemon operates the Canton validator node, which hosts our Daml parties and, by default, their party keys and contract data.

## Structure
Five workstreams:
- WS1 Governance, scope and architecture: the overarching view, incident response and resilience, and the crosswalk.
- WS2 OS and containers, including Vault Enterprise platform hardening.
- WS3 Identity.
- WS4 Cloud, network, data, logging and detection, AppSec, SDLC and supply chain. It runs as two tracks: platform in weeks 3–7 and application in weeks 5–13.
- WS5 Blockchain and smart contracts.

Phases and gates:
- Phase 1, mobilize and scope: weeks 1–2, ending at G1 (access to Tier 1 ≥ 90%, inventory reconciled, scenarios approved, rules of engagement signed).
- Phase 2, assess Tier 1: weeks 3–7, ending at G2 (Tier 1 fully assessed, criticals escalated, containment status known). The flash readout is in week 4.
- Buffer: weeks 8–9 (internal analysis only; to be placed over the main holiday period).
- Phase 3, Tiers 2–3 and detection validation: weeks 10–13, ending at G3.
- Phase 4, recommendations and handover: weeks 14–16, ending at G4 (report signed, owner decisions recorded, access removed).

Capacity: about 100–150 person-weeks are needed against about 94 usable, so the estimate is re-checked at G1. Twelve weeks is feasible only with 2–3 more people and a narrower crosswalk.

## Terminology rule (adopted, "option A")
- **Crown jewel** is an *impact* label: what an attacker wants. Use it for targets, attack scenarios and ratings.
- **Tier 1** is a *scope* label: what we assess first and in full depth. It covers every crown jewel plus any asset two steps or fewer from one, including internet-facing entry points on those paths.
- Crown jewels include:
  - signing keys, Vault root and unseal material;
  - Canton party keys on the Blockdaemon validator;
  - admin and upgrade multisigs, treasury contracts;
  - the Entra Global Admin and Identity Center trust;
  - the B2C login-to-withdrawal path;
  - the management and delegated-admin accounts, deploy paths;
  - PII and KYC stores;
  - the dApp front end and DNS.

## Methods
- **Crosswalk:** NIST CSF 2.0 is the reporting hub; CIS is the test layer. Link standards directly (never inferred through the hub), using NIST IR 8477 relationship types. Give full credit only for "equal" or "superset" relationships. Start from published mappings, labeled "candidate" until reviewed, and use a second reviewer. The strictest requirement wins, and conflicts go to Legal. Store control IDs and paraphrases only. Versions: v1 in week 2, v2 in week 7, v3 (full catalogue) in week 13; cross-check report in week 14.
- **Standards:** NIST CSF 2.0, SP 800-53, ISO 27001, SOC 2, CIS (AWS, Amazon Linux 2023, EKS), PCI DSS (only if cardholder data is confirmed in scope), DORA, MiCA, NYDFS Part 500, OWASP ASVS 5.0, SLSA v1.2, OWASP SAMM. BSSC: Node Operation Standard v2, Token Integration Standard v1, Key Management Standard v1, Smart Contract Security Standard v1, General Security and Privacy Guidelines v2. Also OWASP SCSVS, SC Top 10, Web3 Attack Vectors Top 15, EthTrust, SEAL and SEAL 911, and CCSS.
- **Rating model:** a proximity (P4–P1) × impact (I4–I1) matrix. A finding moves up one level if it is unauthenticated and internet-facing, listed in CISA KEV, exploited during testing, or has a CVSS v4.0 score ≥ 9.0 with public exploit code. Regulatory findings can be accepted or deferred but never rejected.
- **Handover pack:** 13 items, each with a receiving owner, accepted at G4.

## Existing deliverables (attached)
- **Security Hardening Program — Charter & 16-Week Roadmap** (draft v0.6)
- **Security Hardening Assessment Program – Executive Briefing:** 19 slides, consulting style.
  - Style: IBM Plex Sans; navy #14213D, teal #1F7A6E, light #F8F7F3, amber #E8B04B.
  - Footer: "Security Hardening Assessment Program" plus the page number.
  - The appendix has the references and 96 acronyms. Every acronym used must appear there.
- **Program Reference Guide — Terms, Tiering, Templates and Samples:** key terms, crown jewels, tiering, the rating model, crosswalk and cross-check templates, the handover pack, samples (identity report, privileged-access map, container-escape and credential-compromise playbooks) and the Entra External ID migration plan.
- **Control registers:**
  - Unified_Blockchain_Control_Register.csv (386 controls)
  - Azure_CIS_Control_Register.csv

## Open items
- The start date.
- Named workstream leads and WS5 skills.
- Sizing (accounts, clusters, codebases).
- PCI scope, MiCA and NYDFS licences, and the ISO or SOC 2 target.
- Any further NIST publications.
- AI/LLM scope.
- Any cross-chain bridge in use.
- Whether we hold any Daml party keys ourselves.
- Where Vault runs and who holds the admin and upgrade keys today.
- The CI/CD platform.

## Working rules
- Verify present-day facts (regulations, versions, product status) against primary sources and cite them. Don't rely on memory.
- Keep the deck, charter and guide consistent. Any change in one is checked in the others.
- Write in plain English, keep it concise, and flag assumptions explicitly.

**My next request:** [describe the task here]
