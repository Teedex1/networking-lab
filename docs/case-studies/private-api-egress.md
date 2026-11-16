# Private API could not reach external HTTPS

**Environment:** AWS networking training lab

**Work recorded:** 19 October 2026

**Status:** Resolved. HTTPS connectivity restored; the check returned HTTP 200.

## Symptom

The API instance in the private subnet could not reach an external HTTPS endpoint. Requests timed out.

## Investigation

I inspected the private subnet's route table and the NAT gateway's placement. The NAT gateway existed in the public subnet, but the private route table did not have a default route directing internet-bound traffic to it.

That explained why having a NAT gateway in the VPC had not, by itself, provided internet egress for the API instance. The instance's traffic still needed a route to that gateway.

## Change made

Using the AWS CLI, I added a `0.0.0.0/0` destination to the affected private route table, targeting the NAT gateway. The route-creation operation returned success.

The change addressed the private instance's outbound network path. It did not require making the API instance publicly addressable.

## Verification and result

After the route change, the HTTPS check from the API instance returned **HTTP 200**. This is the retained result supporting the connectivity claim.

This incident was completed as part of the four-incident lab. The separate DNS, application-connectivity, and hardening reports describe the other repairs.

## What I learned

- A resource can exist without being part of the traffic path that needs it.
- Route-table selection matters as much as gateway placement.
- A targeted configuration change should be followed by a check from the instance that experienced the failure.

## Skills demonstrated

VPC routing, private-subnet egress, NAT gateways, AWS CLI inspection and configuration, and HTTP-based connectivity verification.

[Case-study index](README.md) · [Project overview](../../README.md)
