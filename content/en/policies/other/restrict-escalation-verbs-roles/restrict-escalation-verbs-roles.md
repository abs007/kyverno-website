---
title: "Restrict Escalation Verbs in Roles"
category: Security
version: 1.6.0
subject: Role, ClusterRole, RBAC
policyType: "validate"
description: >
    The verbs `impersonate`, `bind`, and `escalate` may all potentially lead to privilege escalation and should be tightly controlled. This policy prevents use of these verbs in Role or ClusterRole resources. In order to fully implement this control, it is recommended to pair this policy with another which also prevents use of the wildcard ('*') in the verbs list.
---

## Policy Definition
<a href="https://github.com/kyverno/policies/raw/main//other/restrict-escalation-verbs-roles/restrict-escalation-verbs-roles.yaml" target="-blank">/other/restrict-escalation-verbs-roles/restrict-escalation-verbs-roles.yaml</a>

```yaml
apiVersion: kyverno.io/v1
kind: ClusterPolicy
metadata:
  name: restrict-escalation-verbs-roles
  annotations:
    policies.kyverno.io/title: Restrict Escalation Verbs in Roles
    policies.kyverno.io/category: Security
    policies.kyverno.io/severity: medium
    policies.kyverno.io/subject: Role, ClusterRole, RBAC
    kyverno.io/kyverno-version: 1.6.2
    policies.kyverno.io/minversion: 1.6.0
    kyverno.io/kubernetes-version: "1.23"
    policies.kyverno.io/description: >-
      The verbs `impersonate`, `bind`, and `escalate` may all potentially lead to
      privilege escalation and should be tightly controlled. This policy prevents
      use of these verbs in Role or ClusterRole resources. In order to
      fully implement this control, it is recommended to pair this policy with another which
      also prevents use of the wildcard ('*') in the verbs list.
spec:
  validationFailureAction: audit
  background: true
  rules:
    - name: escalate
      match:
        any:
        - resources:
            kinds:
              - Role
              - ClusterRole
      validate:
        message: "Use of verbs `escalate`, `bind`, and `impersonate` are forbidden."
        deny:
          conditions:
            any:
            - key: ["escalate","bind","impersonate"]
              operator: AnyIn
              value: "{{ request.object.rules[].verbs[] }}"
```
