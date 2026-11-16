# Internal service names did not resolve

**Environment:** AWS networking training lab

**Work recorded:** 19 October 2026

**Status:** Resolved. Internal service names returned the expected private addresses.

## Symptom

The lab's service-discovery names, `web.internal.test`, `api.internal.test`, and `db.internal.test`, were not resolving as required.

## Investigation

The Route 53 inspection established that:

- `internal.test` was a private hosted zone.
- The zone was associated with the lab VPC.
- The record listing contained only the zone's NS and SOA records.
- The three application A records were absent from that listing.

This narrowed the investigation from a general DNS failure to the service records within an existing, associated private zone.

## Change made

I added or updated the three application A records in Route 53 using UPSERT changes:

| Name | Destination |
|---|---|
| `web.internal.test` | Web server's private IPv4 address |
| `api.internal.test` | API server's private IPv4 address |
| `db.internal.test` | Database server's private IPv4 address |

The records used a TTL of 60 seconds. Service discovery remained within the private hosted zone and used the services' private addresses.

## Verification and result

The completed project notes record queries through the VPC DNS resolver returning the expected private IP for each name. A system-resolver lookup for `api.internal.test` also confirmed normal application-side name resolution.

This restored the internal service discovery used by the lab's applications.

## What the investigation demonstrates

Route 53 private DNS, VPC hosted-zone association, DNS record management, cloud-resolver queries, and system-resolver verification.

[Case-study index](README.md) · [Project overview](../../README.md)
