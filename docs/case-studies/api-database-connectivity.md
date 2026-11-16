# Restoring web, API, and database connectivity

**Environment:** AWS networking training lab

**Work recorded:** 19 October 2026

**Status:** Resolved. The web-to-API health check and API-to-PostgreSQL connection worked after the repairs.

## Symptom

Two application paths were failing:

- The web server could not reach the API service on TCP 8080, even though the API health endpoint worked locally.
- The API could not connect to the database's private address on TCP 5432. The initial PostgreSQL readiness check returned **no response**.

## Investigation

### Web to API

I checked the direction of the connection and the security-group rules at both ends. The web server needed permitted outbound TCP 8080 traffic to the API security group, and the API needed an inbound TCP 8080 rule from the web security group.

The troubleshooting included correcting how the destination security group was specified for egress. An outbound rule describes where traffic is allowed to go; an inbound rule describes where traffic is allowed to come from.

### API to PostgreSQL

I worked through the service and network layers:

| Layer | Finding | What it told me |
|---|---|---|
| Database service | PostgreSQL was running. | The database process was available for further checks. |
| Listening socket | PostgreSQL listened on `0.0.0.0:5432` and `[::]:5432`. | It was not restricted to loopback-only connections. |
| Host firewall | UFW was inactive. | UFW was not the source of this block. |
| Database security group | A rule permitted TCP 5432 from the API security group. | An appropriate security-group rule alone had not restored the connection. |
| Routing | The inspected route tables included local VPC routes. | I continued investigating other controls on the private traffic path. |
| Subnet network ACL | Inbound rule 100 explicitly denied TCP 5432 from all IPv4 sources. | This rule blocked the database traffic despite the security-group permission. |

The important step was continuing beyond the security group when the connection still failed. The running service, listening socket, and security-group rule did not exclude a subnet-level network block.

## Change made

Using the AWS CLI, I corrected the application traffic rules, removed the identified inbound network ACL deny, and restricted database TCP 5432 access to the API security group. Broad database access was removed after the required narrower path worked.

## Verification and result

The completed project notes record successful TCP and PostgreSQL readiness checks from the API to the database. The web-side health check also became healthy after the application traffic path was repaired.

Both failed paths were restored: **web → API on TCP 8080**, and **API → PostgreSQL on TCP 5432**.

## What I learned

- Check the service, host, and network controls separately.
- A security-group allow does not override a network ACL deny.
- Record the exact successful command output immediately after a repair so someone else can assess the result.

## Skills demonstrated

Application health checks, PostgreSQL connectivity diagnostics, Linux service and socket inspection, security-group ingress and egress, network ACL evaluation, and layered troubleshooting.

[Case-study index](README.md) · [Project overview](../../README.md)
