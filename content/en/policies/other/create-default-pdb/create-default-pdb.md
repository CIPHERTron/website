---
title: "Add Pod Disruption Budget"
category: Sample
version: 1.6.0
subject: Deployment
policyType: "generate"
description: >
    A PodDisruptionBudget limits the number of Pods of a replicated application that are down simultaneously from voluntary disruptions. For example, a quorum-based application would like to ensure that the number of replicas running is never brought below the number needed for a quorum. As an application owner, you can create a PodDisruptionBudget (PDB) for each application. This policy will create a PDB resource whenever a new Deployment is created.
---

## Policy Definition
<a href="https://github.com/kyverno/policies/raw/main//other/create-default-pdb/create-default-pdb.yaml" target="-blank">/other/create-default-pdb/create-default-pdb.yaml</a>

```yaml
apiVersion: kyverno.io/v1
kind: ClusterPolicy
metadata:
  name: create-default-pdb
  annotations:
    policies.kyverno.io/title: Add Pod Disruption Budget
    policies.kyverno.io/category: Sample
    kyverno.io/kyverno-version: 1.6.2
    policies.kyverno.io/minversion: 1.6.0
    policies.kyverno.io/subject: Deployment
    policies.kyverno.io/description: >-
      A PodDisruptionBudget limits the number of Pods of a replicated application that
      are down simultaneously from voluntary disruptions. For example, a quorum-based
      application would like to ensure that the number of replicas running is never brought
      below the number needed for a quorum. As an application owner, you can create a PodDisruptionBudget (PDB)
      for each application. This policy will create a PDB resource whenever a new Deployment is created.
spec:
  rules:
  - name: create-default-pdb
    match:
      any:
      - resources:
          kinds:
          - Deployment
    generate:
      apiVersion: policy/v1
      kind: PodDisruptionBudget
      name: "{{request.object.metadata.name}}-default-pdb"
      namespace: "{{request.object.metadata.namespace}}"
      data:
        spec:
          minAvailable: 1
          selector:
            matchLabels:
              "{{request.object.metadata.labels}}"
```
