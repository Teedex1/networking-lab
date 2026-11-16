# AWS troubleshooting case studies

These reports document four completed investigations from my October 2026 AWS lab work. Each explains the symptom, diagnostic reasoning, change made, and result.

The infrastructure and exercises were provided by [Learn to Cloud](https://github.com/learntocloud/networking-lab). These case studies document my troubleshooting of the deployed AWS environment.

## Completed investigations

| Report | Main diagnosis | Outcome |
|---|---|---|
| [Internal DNS](internal-dns.md) | Application A records were missing from an existing private zone. | Restored private service-name resolution. |
| [Application and database connectivity](api-database-connectivity.md) | Application security-group paths needed correction; a network ACL explicitly denied database traffic. | Restored web-to-API and API-to-PostgreSQL communication. |
| [Private API internet egress](private-api-egress.md) | The private subnet's route table lacked a default route to the NAT gateway. | HTTPS requests from the API returned HTTP 200. |
| [Access hardening](access-hardening.md) | Administrative and database access needed narrower permitted sources. | Restricted access while preserving required connectivity. |

## Record of the work

The reports draw on my October 2026 troubleshooting notes and the detailed project report compiled on 27 October 2026. They summarise the investigation, repairs, and results for all four completed incidents.

## Reading the reports

- Reports are named by symptom because the lab's incident labels have changed over time. The current [AWS guide](../../aws/README.md) is authoritative for the current exercise queue.
- Public client addresses, AWS account identifiers, and resource IDs are omitted. Network roles and service ports carry the technical explanation.
- The individual reports explain the checks used to confirm the repairs.

The source lab's setup, acceptance checks, and cleanup instructions remain in the provider guides. These case studies disclose findings from the exercise and are intended for portfolio review after attempting the lab.

[Return to the project overview](../../README.md)
