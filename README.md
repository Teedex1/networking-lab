# AWS Network Troubleshooting Portfolio

**Tunde Ogunmodede · AWS · Linux · Networking**

I investigated connectivity failures in an AWS training environment containing a bastion host, web server, private API server, and PostgreSQL database. My work focused on tracing failed connections through route tables, DNS, security groups, and network ACLs, then checking the effect of each change.

This is my personal portfolio fork of [Learn to Cloud's networking lab](https://github.com/learntocloud/networking-lab). The lab infrastructure and incident scenarios come from Learn to Cloud. The case studies below document my troubleshooting work on the deployed AWS environment.

## Start with the case studies

| Investigation | What I found and changed | Recorded outcome |
|---|---|---|
| [Private API could not reach external HTTPS](docs/case-studies/private-api-egress.md) | Found a missing default route in the private subnet's route table and added a route through the NAT gateway. | The route operation succeeded and the API's HTTPS check returned **HTTP 200**. |
| [API could not connect to PostgreSQL](docs/case-studies/api-database-connectivity.md) | Checked the database service, listening socket, host firewall, security group, and subnet network ACL. Found and removed an inbound ACL deny for TCP 5432. | I reported the connection working after the change; a successful readiness-command transcript is not attached. |

**What these demonstrate:** investigating across several network layers, distinguishing a running service from a reachable service, and checking results after a targeted change.

## Other investigations

- [Internal DNS service discovery](docs/case-studies/internal-dns.md): the private hosted zone existed and was associated with the VPC, but its record listing contained only NS and SOA records.
- [Network access hardening](docs/case-studies/access-hardening.md): work on narrowing SSH access and reviewing the permitted sources for application and database traffic.

Each report states the evidence retained and the checks still needed to substantiate its final state. These reports do not claim an all-incidents validation pass.

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

The case studies are historical lab reports, rather than a replacement setup guide. See the [evidence notes](docs/case-studies/README.md) for the limits of the retained results.

## Run the original lab

The upstream lab intentionally deploys broken networking. Use its provider guides for prerequisites, setup, validation, and cleanup:

| Provider | Lab guide |
|---|---|
| AWS — the environment used in these reports | [AWS guide](aws/README.md) |
| Azure — included upstream | [Azure guide](azure/README.md) |
| GCP — included upstream | [GCP guide](gcp/README.md) |

The [original lab overview](docs/lab-overview.md) is preserved separately. Terraform, setup scripts, intentional faults, and validators are unchanged by this portfolio documentation.

The lab creates billable resources. Follow the provider guide's cleanup procedure when finished. Contribution guidance remains in [CONTRIBUTING.md](CONTRIBUTING.md).
