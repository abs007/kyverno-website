---
title: "Minimum Backup Retention"
category: Kasten K10 by Veeam
version: 1.6.2
subject: Policy
policyType: "mutate"
description: >
    K10 Policy resources can be validated to adhere to common compliance retention standards.  Uncomment the regulation/compliance standards you want to enforce for according to GFS retention. This policy deletes the retention value in the backup operation and replaces it with the specified retention. Note: K10 Policy uses the GFS retention scheme and export operations default to use the retention of the backup operation. To use different  This policy can also be used go reduce retentions lengths to enforce cost optimization.
---

## Policy Definition
<a href="https://github.com/kyverno/policies/raw/main//kasten/k10-minimum-retention/k10-override-minimum-retentions.yaml" target="-blank">/kasten/k10-minimum-retention/k10-override-minimum-retentions.yaml</a>

```yaml
apiVersion: kyverno.io/v1
kind: ClusterPolicy
metadata:
  name: k10-policy-minimum-retention
  annotations:
    policies.kyverno.io/title: Minimum Backup Retention
    policies.kyverno.io/category: Kasten K10 by Veeam
    kyverno.io/kyverno-version: 1.6.2
    policies.kyverno.io/minversion: 1.6.2
    kyverno.io/kubernetes-version: "1.21-1.22"
    policies.kyverno.io/subject: Policy
    policies.kyverno.io/description: >-
      K10 Policy resources can be validated to adhere to common compliance retention standards. 
      Uncomment the regulation/compliance standards you want to enforce for according to GFS retention.
      This policy deletes the retention value in the backup operation and replaces it with the specified retention.
      Note: K10 Policy uses the GFS retention scheme and export operations default to use the retention of the backup operation.
      To use different 
      This policy can also be used go reduce retentions lengths to enforce cost optimization.
spec:
  rules:
  - name: k10-policy-minimum-retention
    match:
      any:
      - resources:
          kinds:
          - config.kio.kasten.io/v1alpha1/Policy
    mutate: 
      # Federal Information Security Management Act (FISMA): 3 Years 
      #patchesJson6902: |-
      #  - path: "/spec/retention"
      #    op: replace
      #    value: {"hourly":24,"daily":30,"weekly":4,"monthly":12,"yearly":3}
      
      # Health Insurance Portability and Accountability Act (HIPAA):  6 Years   
      #patchesJson6902: |-
      #  - path: "/spec/retention"
      #    op: replace
      #    value: {"hourly":24,"daily":30,"weekly":4,"monthly":12,"yearly":6}
      
      # National Energy Commission (NERC): 3 to 6 Years  
      #patchesJson6902: |-
      #  - path: "/spec/retention"
      #    op: replace
      #    value: {"hourly":24,"daily":30,"weekly":4,"monthly":12,"yearly":3}
      
      # Basel II Capital Accord: 3 to 7 Years 
      #patchesJson6902: |-
      #  - path: "/spec/retention"
      #    op: replace
      #    value: {"hourly":24,"daily":30,"weekly":4,"monthly":12,"yearly":3}
      
      # Sarbanes-Oxley Act of 2002 (SOX): 7 Years
      #patchesJson6902: |-
      #  - path: "/spec/retention"
      #    op: replace
      #    value: {"hourly":24,"daily":30,"weekly":4,"monthly":12,"yearly":7}
      
      # National Industrial Security Program Operating Manual (NISPOM): 6 to 12 Months 
      #patchesJson6902: |-
      #  - path: "/spec/retention"
      #    op: replace
      #    value: {"hourly":24,"daily":30,"weekly":4,"monthly":6}
      
      # Cost Optimization (Maximum Retention: 3 Months)
      patchesJson6902: |-
        - path: "/spec/retention"
          op: replace
          value: {"hourly":24,"daily":30,"weekly":4,"monthly":3}

```
