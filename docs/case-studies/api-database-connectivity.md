# API could not connect to PostgreSQL

**Environment:** AWS networking training lab

**Work recorded:** 19 October 2026

**Recorded outcome:** I reported connectivity working after removing an explicit network ACL deny. The successful command output is not attached.

## Symptom

Connections from the API instance to the database's private address on TCP 5432 timed out. The recorded PostgreSQL readiness check returned **no response**.

## Investigation

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

I removed the identified inbound network ACL deny rule from the deployed lab using the AWS CLI.

## Verification and result

After the change, I reported that it **worked**. That supports a reported resolution of the API-to-database issue.

The retained notes do not contain the successful post-change `pg_isready` or `nc` output. This report therefore does not present an invented acceptance transcript or extend the result to the separate web-to-API path.

## What I learned

- Check the service, host, and network controls separately.
- A security-group allow does not override a network ACL deny.
- Record the exact successful command output immediately after a repair so someone else can assess the result.

## Skills demonstrated

PostgreSQL connectivity diagnostics, Linux service and socket inspection, security-group analysis, network ACL evaluation, and layered troubleshooting.

[Case-study index](README.md) · [Project overview](../../README.md)
