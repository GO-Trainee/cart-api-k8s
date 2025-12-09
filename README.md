# Kubernetes and Helm Deployment Requirements for Cart API

## Goal
Deploy the Cart API application to Kubernetes, first with raw manifests in a `test` namespace (showing HPA scaling), then package the solution with Helm and roll it out to `dev`, `qa`, and `prod` namespaces.

## Prerequisites for contributors
- Install Kubernetes locally (e.g., Minikube).
- Install Lens.
- Ensure kubectl is available and configured for the local cluster.

## Phase 1: Raw Kubernetes manifests (branch: `k8s`)
1. Author all Kubernetes YAML to run the app natively (no Helm) in namespace `test`.
2. Required resources:
   - Namespace `test` with resource quotas/limits.
   - Deployment for the application.
   - Service to expose the Deployment.
   - StatefulSet if any stateful component is needed.
   - PersistentVolume and PersistentVolumeClaim for storage needs.
   - Resource limits/requests set at the pod/container level for all workloads.
   - Horizontal Pod Autoscaler that scales on at least one metric (CPU or memory; disk I/O optional if supported).
3. Optional but recommended:
   - ConfigMap for non-secret configuration.
   - Secret for sensitive values.
   - Gateway API for HTTP exposure.
4. Validation:
   - Apply manifests to the `test` namespace and verify pods become Ready.
   - Demonstrate HPA scaling under load (e.g., stress test to trigger additional replicas).
5. Deliverable:
   - All YAML files committed to the `k8s` branch in this repository.
   - Evidence/demo notes showing HPA behavior for mentor review before moving to Phase 2.

## Phase 2: Helm chart (branch: `helm`)
1. Create a Helm chart that templates all resources from Phase 1.
2. Support three release targets: `dev`, `qa`, `prod` namespaces.
3. Provide environment-specific values files for `dev`, `qa`, and `prod` (e.g., replica counts, resource limits, hostnames).
4. Templates must include:
   - Namespace creation (or document expectation if created externally).
   - Deployment, Service, StatefulSet (if required), PV/PVC, HPA, resource limits.
   - Optional templates for ConfigMap, Secret, Ingress/IngressClass or controller configuration as needed.
5. Validation:
   - Install the chart into each namespace (`dev`, `qa`, `prod`) and verify the application is healthy.
6. Deliverable:
   - Helm chart and values committed to the `helm` branch.
   - Proof of successful deployments in all three namespaces for mentor review.
   - Show how helm install and helm rollback commands work

## Acceptance Criteria
- Phase 1: `k8s` branch contains working raw manifests deployed to `test` with HPA demonstrated and approved by mentor.
- Phase 2: `helm` branch contains Helm chart deployed to `dev`, `qa`, and `prod` namespaces, demonstrated to mentor.

