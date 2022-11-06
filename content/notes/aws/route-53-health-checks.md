---
title: "Route 53 Health Checks"
tags:
- aws
- route-53
disableToc: false
---

### Related Notes
- [Route 53](/notes/aws/route-53.md)

## Route 53 Health Checks
- Health checks are separate from, but are used by records
- They're configured separately
- Health checkers located globally
- Health checkers check every 30s (every 10s costs extra)
- TCP, HTTP/S, HTTP/S with String Matching
- Moves between either Healthy or Unhealthy state
- Endpoint, CloudWatch Alarm, Checks of Checks (Calculated)