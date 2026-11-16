# Internal service names did not resolve

**Environment:** AWS networking training lab

**Work recorded:** 19 October 2026

**Evidence retained:** Private hosted-zone configuration and record inspection. Final DNS resolution outputs are not included.

## Symptom

The lab's service-discovery names, `web.internal.test`, `api.internal.test`, and `db.internal.test`, were not resolving as required.

## Investigation

The Route 53 inspection established that:

- `internal.test` was a private hosted zone.
- The zone was associated with the lab VPC.
- The record listing contained only the zone's NS and SOA records.
- The three application A records were absent from that listing.

This narrowed the investigation from a general DNS failure to the service records within an existing, associated private zone.

## Evidence boundary

The retained inspection supports the missing-record diagnosis. Final record-change output, resolver responses from the four hosts, and a validator result are not attached to this report, so it does not mark the incident as verified resolved.

## Evidence to add to the completed report

- The service records actually created or updated, with sensitive identifiers removed.
- Successful cloud-DNS and system-resolver results from the bastion, web, API, and database hosts, following the provider guide's acceptance checks.
- The relevant validator result.

## What the investigation demonstrates

Checking a hosted zone's existence, its VPC association, and its actual records as separate parts of DNS service discovery.

[Case-study index](README.md) · [Project overview](../../README.md)
