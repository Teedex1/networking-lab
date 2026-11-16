# Reviewing network access boundaries

**Environment:** AWS networking training lab

**Work recorded:** 19 October 2026

**Evidence retained:** A restricted client `/32` SSH source was added. Final hardening acceptance checks are not attached.

## Objective

Narrow administrative and database access while preserving the application connections the lab needed.

The intended access model in the current AWS guide is:

| Traffic | Intended source |
|---|---|
| Bastion SSH | The administrator's current public IPv4 address as a `/32` |
| SSH to web, API, and database hosts | Bastion security group |
| PostgreSQL on TCP 5432 | API security group |
| ICMP to the web host | Bastion security group |

These are the exercise's target boundaries; the table is not a claim that every final rule was verified in my run.

## Work recorded

During the investigation I added an SSH source restricted to the client IPv4 address as a `/32` for the bastion. The actual public address and AWS rule identifiers are omitted here.

The repository also contains a historical security-group response showing broad SSH and PostgreSQL sources. That snapshot is evidence of a configuration inspected during the exercise; it does not establish the final effective rules.

## Verification still to document

Adding a narrower rule alone does not establish that a broader rule has been removed. A complete report needs the final applicable rules and both allowed and blocked connection checks.

The provider guide specifies the required checks, including preserved bastion administration, working application paths, and rejected access from unapproved sources. Their successful results are not retained in this report.

## What I learned

Hardening should be evaluated against the complete effective policy and the required traffic paths. Both permitted traffic and rejected traffic matter when demonstrating the result.

[Case-study index](README.md) · [Project overview](../../README.md)
