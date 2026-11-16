# AWS troubleshooting case studies

These reports organise my October 2026 AWS lab work by the failure being investigated. They focus on the observed symptom, diagnostic reasoning, action taken, and evidence of the result.

The infrastructure and exercises were provided by [Learn to Cloud](https://github.com/learntocloud/networking-lab). Completing and documenting a troubleshooting exercise is distinct from authoring the underlying lab infrastructure.

## Evidence retained

| Report | Evidence available | Boundary of the claim |
|---|---|---|
| [Private API internet egress](private-api-egress.md) | A missing private default route, a successful route-creation response, and an HTTPS response status of 200 after the change. | A successful connectivity check; no saved all-incidents validator transcript or NAT-address comparison is included. |
| [API to PostgreSQL connectivity](api-database-connectivity.md) | A timeout, a listening PostgreSQL service, an inactive host firewall, a permitted API security-group source, an explicit network ACL deny, and my report that connectivity worked after removing the deny. | A reported resolution; the successful `pg_isready` or `nc` output is not retained here. |
| [Internal DNS](internal-dns.md) | The private zone, its VPC association, and a listing containing only NS and SOA records. | A documented diagnosis; final record changes and resolver outputs are not included. |
| [Access hardening](access-hardening.md) | A restricted client `/32` SSH source was added during the work. | A configuration change; final effective rules and allowed/blocked traffic checks are not included. |

The absence of a retained final output does not establish that a task was never completed. It limits what this portfolio currently substantiates.

## Reading the reports

- Reports are named by symptom because the lab's incident labels have changed over time. The current [AWS guide](../../aws/README.md) is authoritative for the current exercise queue.
- Prose summarises the lab notes. No synthetic terminal transcripts, completion tokens, performance metrics, or production impact figures have been added.
- Public client addresses, AWS account identifiers, and resource IDs are omitted. Network roles and service ports carry the technical explanation.
- No current live deployment, final all-four-incidents validator pass, or completed cleanup is asserted by these historical notes.

The source lab's setup, acceptance checks, and cleanup instructions remain in the provider guides. These case studies disclose findings from the exercise and are intended for portfolio review after attempting the lab.

[Return to the project overview](../../README.md)
