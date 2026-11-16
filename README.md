# AWS Network Troubleshooting Lab

**Tunde Ogunmodede · AWS · Linux · Networking**

**Four troubleshooting incidents resolved.** I investigated and repaired connectivity failures in an AWS training environment containing a bastion host, web server, private API server, and PostgreSQL database. My work covered private DNS, application and database traffic, internet egress, and network access hardening.

This is my personal portfolio fork of [Learn to Cloud's networking lab](https://github.com/learntocloud/networking-lab). The lab infrastructure and incident scenarios come from Learn to Cloud. The case studies below document my troubleshooting work on the deployed AWS environment.

## Start with the case studies

| Investigation | What I found and changed | Result |
|---|---|---|
| [Internal service names did not resolve](docs/case-studies/internal-dns.md) | Inspected the private hosted zone and VPC association, found missing application A records, and added records pointing to the services' private addresses. | Restored internal service discovery; DNS queries returned the expected private IPs. |
| [Web, API, and database connections timed out](docs/case-studies/api-database-connectivity.md) | Corrected application security-group traffic paths and removed an inbound network ACL deny blocking PostgreSQL. | Restored web-to-API health checks and API-to-database connectivity. |
| [Private API could not reach external HTTPS](docs/case-studies/private-api-egress.md) | Found a missing default route in the private subnet's route table and added a route through the NAT gateway. | The route operation succeeded and the API's HTTPS check returned **HTTP 200**. |
| [Network access was too broad](docs/case-studies/access-hardening.md) | Restricted bastion SSH to the administrator's `/32`, internal SSH to the bastion security group, database access to the API security group, and web ICMP to the bastion. | Tightened access while preserving administrative access and the required application connections. |

**What these demonstrate:** investigating across several network layers, distinguishing a running service from a reachable service, and checking results after a targeted change.

## What I achieved

- Restored name-based communication between private services.
- Repaired the web → API → PostgreSQL application path.
- Restored outbound HTTPS from the private API through the NAT gateway.
- Replaced broad network access with source-specific rules while keeping required connections working.

## Environment

| Component | Role |
|---|---|
| Bastion | SSH entry point into the lab |
| Web server | Public application entry point |
| API server | Private application service on TCP 8080 |
| PostgreSQL database | Private database service on TCP 5432 |
| NAT gateway and route tables | Outbound internet path for private resources |
| Route 53 private hosted zone | Internal service-name resolution |
| Security groups and network ACLs | Network access controls at different layers |

The intended application path is **web → API → database**. Administrators enter through the bastion, and the private API uses the NAT gateway for external HTTPS. The [AWS lab guide](aws/README.md) contains the full topology.

## How I approached troubleshooting

1. Identify the source, destination, port, and exact failure symptom.
2. Check whether the destination service is running and listening.
3. Inspect the relevant DNS, routes, and network access controls.
4. Make a targeted change in the deployed environment using the AWS CLI.
5. Retest the failing path and record the result.

The case studies document completed lab investigations. See the [report index](docs/case-studies/README.md) for the record of the work and the original provider guides for setup instructions.

## Run the original lab

The upstream lab intentionally deploys broken networking. Use its provider guides for prerequisites, setup, validation, and cleanup:

| Provider | Lab guide |
|---|---|
| AWS — the environment used in these reports | [AWS guide](aws/README.md) |
| Azure — included upstream | [Azure guide](azure/README.md) |
| GCP — included upstream | [GCP guide](gcp/README.md) |

The [original lab overview](docs/lab-overview.md) is preserved separately. Terraform, setup scripts, intentional faults, and validators are unchanged by this portfolio documentation.

The lab creates billable resources. Follow the provider guide's cleanup procedure when finished. Contribution guidance remains in [CONTRIBUTING.md](CONTRIBUTING.md).
