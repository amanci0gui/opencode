---
name: sre-engineer
description: Defines service level objectives, creates error budget policies, designs incident response procedures, develops capacity models, and produces monitoring configurations and automation scripts for production systems. Use when defining SLIs/SLOs, managing error budgets, building reliable systems at scale, incident management, chaos engineering, toil reduction, or capacity planning.
license: MIT
compatibility: opencode
metadata:
  author: https://github.com/Jeffallan
  version: "1.1.0"
  domain: devops
  triggers: SRE, site reliability, SLO, SLI, error budget, incident management, chaos engineering, toil reduction, on-call, MTTR
  role: specialist
  scope: implementation
  output-format: code
  related-skills: devops-engineer, cloud-architect, kubernetes-specialist
---

# SRE Engineer

## Core Workflow

1. **Assess reliability** - Review architecture, SLOs, incidents, toil levels
2. **Define SLOs** - Identify meaningful SLIs and set appropriate targets
3. **Verify alignment** - Confirm SLO targets reflect user expectations before proceeding
4. **Implement monitoring** - Build golden signal dashboards and alerting
5. **Automate toil** - Identify repetitive tasks and build automation
6. **Test resilience** - Design failure scenarios and evaluate recovery against RTO/RPO targets. Execute fault injection only in an isolated test/staging environment after explicit approval of the exact target, scope, timing, and rollback plan.

## Reference Guide

Load detailed guidance based on context:

| Topic | Reference | Load When |
|-------|-----------|-----------|
| SLO/SLI | `references/slo-sli-management.md` | Defining SLOs, calculating error budgets |
| Error Budgets | `references/error-budget-policy.md` | Managing budgets, burn rates, policies |
| Monitoring | `references/monitoring-alerting.md` | Golden signals, alert design, dashboards |
| Automation | `references/automation-toil.md` | Toil reduction, automation patterns |
| Incidents | `references/incident-chaos.md` | Incident response, chaos engineering |

## Operational Safety

- Treat operational commands and automation examples as templates, not authorization to run them.
- Use read-only inspection first. Any restart, scale, rollback, failover, deletion, configuration change, or fault injection requires explicit user approval for the exact environment and target.
- Keep chaos experiments in an isolated, disposable test/staging environment. Never target production by default; require a tested abort and rollback plan.
- Remediation examples must default to dry-run or recommendation-only. Do not enable unattended production restarts or failovers without an explicitly approved design and safeguards.

## Constraints

### MUST DO
- Define quantitative SLOs (e.g., 99.9% availability)
- Calculate error budgets from SLO targets
- Monitor golden signals (latency, traffic, errors, saturation)
- Write blameless postmortems for all incidents
- Measure toil and track reduction progress
- Automate repetitive operational tasks
- Consider failure scenarios and test them only in an isolated test/staging environment with explicit approval and a tested abort/rollback plan
- Balance reliability with feature velocity

### MUST NOT DO
- Set SLOs without user impact justification
- Alert on symptoms without actionable runbooks
- Tolerate >50% toil without automation plan
- Skip postmortems or assign blame
- Implement manual processes for recurring tasks
- Deploy without capacity planning
- Ignore error budget exhaustion
- Build systems that can't degrade gracefully

## Output Templates

When implementing SRE practices, provide:
1. SLO definitions with SLI measurements and targets
2. Monitoring/alerting configuration (Prometheus, etc.)
3. Automation scripts (Python, Go, Terraform)
4. Runbooks with clear remediation steps
5. Brief explanation of reliability impact

## Concrete Examples

### SLO Definition & Error Budget Calculation

```
# 99.9% availability SLO over a 30-day window
# Allowed downtime: (1 - 0.999) * 30 * 24 * 60 = 43.2 minutes/month
# Error budget (request-based): 0.001 * total_requests

# Example: 10M requests/month → 10,000 error budget requests
# If 5,000 errors consumed in week 1 → 50% budget burned in 25% of window
# → Trigger error budget policy: freeze non-critical releases
```

### Prometheus SLO Alerting Rule (Multiwindow Burn Rate)

```yaml
groups:
  - name: slo_availability
    rules:
      # Fast burn: 2% budget in 1h (14.4x burn rate)
      - alert: HighErrorBudgetBurn
        expr: |
          (
            sum(rate(http_requests_total{status=~"5.."}[1h]))
            /
            sum(rate(http_requests_total[1h]))
          ) > 0.014400
          and
          (
            sum(rate(http_requests_total{status=~"5.."}[5m]))
            /
            sum(rate(http_requests_total[5m]))
          ) > 0.014400
        for: 2m
        labels:
          severity: critical
        annotations:
          summary: "High error budget burn rate detected"
          runbook: "https://wiki.internal/runbooks/high-error-burn"

      # Slow burn: 5% budget in 6h (1x burn rate sustained)
      - alert: SlowErrorBudgetBurn
        expr: |
          (
            sum(rate(http_requests_total{status=~"5.."}[6h]))
            /
            sum(rate(http_requests_total[6h]))
          ) > 0.001
        for: 15m
        labels:
          severity: warning
        annotations:
          summary: "Sustained error budget consumption"
          runbook: "https://wiki.internal/runbooks/slow-error-burn"
```

### PromQL Golden Signal Queries

```promql
# Latency — 99th percentile request duration
histogram_quantile(0.99, sum(rate(http_request_duration_seconds_bucket[5m])) by (le, service))

# Traffic — requests per second by service
sum(rate(http_requests_total[5m])) by (service)

# Errors — error rate ratio
sum(rate(http_requests_total{status=~"5.."}[5m])) by (service)
  /
sum(rate(http_requests_total[5m])) by (service)

# Saturation — CPU throttling ratio
sum(rate(container_cpu_cfs_throttled_seconds_total[5m])) by (pod)
  /
sum(rate(container_cpu_cfs_periods_total[5m])) by (pod)
```

### Toil Monitoring Example (Python, dry-run)

```python
#!/usr/bin/env python3
"""Recommend a restart when an error threshold is exceeded; do not mutate by default."""
import subprocess, sys, json

ERROR_THRESHOLD = 0.05  # 5% error rate triggers restart

def get_error_rate(service: str) -> float:
    """Query Prometheus for current error rate."""
    import urllib.request
    query = f'sum(rate(http_requests_total{{status=~"5..",service="{service}"}}[5m])) / sum(rate(http_requests_total{{service="{service}"}}[5m]))'
    url = f"http://prometheus:9090/api/v1/query?query={urllib.request.quote(query)}"
    with urllib.request.urlopen(url) as resp:
        data = json.load(resp)
    results = data["data"]["result"]
    return float(results[0]["value"][1]) if results else 0.0

def restart_deployment(namespace: str, deployment: str, *, execute: bool = False) -> None:
    command = ["kubectl", "rollout", "restart", f"deployment/{deployment}", "-n", namespace]
    if not execute:
        print(f"DRY RUN: would run {command!r}; obtain approval for this exact target first")
        return
    subprocess.run(command, check=True)
    print(f"Restarted {namespace}/{deployment}")

if __name__ == "__main__":
    service, namespace, deployment = sys.argv[1], sys.argv[2], sys.argv[3]
    rate = get_error_rate(service)
    print(f"Error rate for {service}: {rate:.2%}")
    if rate > ERROR_THRESHOLD:
        print("Threshold exceeded; review the incident and obtain explicit approval before remediation.")
        restart_deployment(namespace, deployment)  # dry-run unless execute=True is explicitly set
    else:
        print("Within SLO threshold — no action required")
```

[Documentation](https://jeffallan.github.io/claude-skills/skills/devops/sre-engineer/)
