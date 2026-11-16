# Reviewing network access boundaries

**Environment:** AWS networking training lab

**Work recorded:** 19 October 2026

**Status:** Resolved. Access was restricted while required administrative and application connections continued to work.

## Objective

Narrow administrative and database access while preserving the application connections the lab needed.

The completed access model was:

| Traffic | Intended source |
|---|---|
| Bastion SSH | The administrator's current public IPv4 address as a `/32` |
| SSH to web, API, and database hosts | Bastion security group |
| PostgreSQL on TCP 5432 | API security group |
| ICMP to the web host | Bastion security group |

## Investigation and changes

I inspected the security-group rules and compared their sources with the actual traffic the lab required. The review covered administrative SSH, API-to-database traffic, and ICMP access to the web server.

I added the narrower access rules and confirmed the required connections worked before replacing the broad rules. The final configuration restricted bastion SSH to the administrator's current public IPv4 `/32`; the other SSH, PostgreSQL, and ICMP permissions used the appropriate source security groups.

The repository's historical security-group response shows broad access inspected during the exercise. It is an earlier snapshot, rather than the final hardened state.

## Verification and result

The completed project report records review of the hardened rule set and preservation of required connectivity, including web-to-API and API-to-database communication.

The result was narrower network access without breaking the application paths repaired in the earlier incident. Public client addresses and AWS rule identifiers are omitted from this portfolio write-up.

## What I learned

Hardening should be evaluated against the complete effective policy and the required traffic paths. Both permitted traffic and rejected traffic matter when demonstrating the result.

[Case-study index](README.md) · [Project overview](../../README.md)
