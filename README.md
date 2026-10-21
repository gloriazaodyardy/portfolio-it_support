---
title: IT Support Portfolio
type: readme
status: Active
author: Gloriaza Y.L.O
owner: Gloriaza Y.L.O
created: 2026-10-21
updated: 2026-10-21
version: "1.0"
tags:
  - it-support
  - portfolio
  - documentation
  - installation
  - templates
language: en
confidentiality: Redact sensitive data before publishing
---

# IT Support Portfolio

Documentation portfolio for IT Support, covering assistance/provisioning, incident handling,
installation, and troubleshooting.

## Structure

```text
portfolio-it-support/
├── Installation/              # installation documentation
│   ├── windows/
│   ├── linux/
│   ├── printer/
│   └── network-device/
├── SOPs/                       # standard operating procedures
├── Troubleshooting/            # problem analysis and solutions
├── scripts/                    # helper and automation scripts
|   templates/                  # ticket, SOP, RCA, and installation templates
└── README.md                   # You are here!            
```

## Documentation Rules

- Each folder has its own `assets/`.
- File names use lowercase with dashes: `windows-11-baseline.md`.
- Sensitive files must be redacted (passwords, tokens, public IPs, serial numbers).
- Every document must include: Purpose, Scope, Prerequisites, Steps, Verification, Rollback.
