---
title: "Restrict NGINX Ingress annotation values"
category: Security, NGINX Ingress
version: 1.6.0
subject: Ingress
policyType: "validate"
description: >
    This policy mitigates CVE-2021-25746 by restricting `metadata.annotations` to safe values. See: https://github.com/kubernetes/ingress-nginx/blame/main/internal/ingress/inspector/rules.go. This issue has been fixed in NGINX Ingress v1.2.0. For NGINX Ingress version 1.0.5+ the  "annotation-value-word-blocklist" configuration setting is also recommended.  Please refer to the CVE for details. 
---

## Policy Definition
<a href="https://github.com/kyverno/policies/raw/main//nginx-ingress/nginx_annotation_checks/restrict-annotations.yaml" target="-blank">/nginx-ingress/nginx_annotation_checks/restrict-annotations.yaml</a>

```yaml
apiVersion: kyverno.io/v1
kind: ClusterPolicy
metadata:
  name: restrict-annotations
  annotations:
    policies.kyverno.io/title: Restrict NGINX Ingress annotation values 
    policies.kyverno.io/category: Security, NGINX Ingress
    policies.kyverno.io/severity: high
    policies.kyverno.io/subject: Ingress
    policies.kyverno.io/minversion: "1.6.0"
    kyverno.io/kyverno-version: "1.6.0"
    kyverno.io/kubernetes-version: "1.23"
    policies.kyverno.io/description: >-
      This policy mitigates CVE-2021-25746 by restricting `metadata.annotations` to safe values.
      See: https://github.com/kubernetes/ingress-nginx/blame/main/internal/ingress/inspector/rules.go.
      This issue has been fixed in NGINX Ingress v1.2.0. For NGINX Ingress version 1.0.5+ the 
      "annotation-value-word-blocklist" configuration setting is also recommended. 
      Please refer to the CVE for details. 
spec:
  validationFailureAction: enforce
  rules:
    - name: check-ingress
      match:
        any:
        - resources:
            kinds:
            - networking.k8s.io/v1/Ingress
      validate:
        message: "spec.rules[].http.paths[].path value is not allowed"
        deny:
          conditions:
            any:
            - key: "{{request.object.metadata.annotations.values(@)[].regex_match('\\s*alias\\s*.*;', @)}}"
              operator: AnyIn
              value: [true]
            - key: "{{request.object.metadata.annotations.values(@)[].regex_match('\\s*root\\s*.*;', @)}}"
              operator: AnyIn
              value: [true]    
            - key: "{{request.object.metadata.annotations.values(@)[].regex_match('/etc/(passwd|shadow|group|nginx|ingress-controller)', @)}}"
              operator: AnyIn
              value: [true]     
            - key: "{{request.object.metadata.annotations.values(@)[].regex_match('/var/run/secrets', @)}}"
              operator: AnyIn
              value: [true]  
            - key: "{{request.object.metadata.annotations.values(@)[].regex_match('.*_by_lua.*', @)}}"
              operator: AnyIn
              value: [true]


```
