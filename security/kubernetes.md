# Kubernetes security model
## Overview

By utilising inbuilt Kubernetes security features, Assembler can a provide mature, community supported and widely reviewed security model. This section covers how Assembler leverages these features.

## Center for Internet Security Kubernetes Benchmark

The CIS Kubernetes benchmark is the community standard for security Kubernetes administration. Assembler clusters are measured against the latest CIS Kubernetes Benchmark on a regular basis and automatically scanned using [aquasecurity/kube-bench](https://github.com/aquasecurity/kube-bench).

## Application organisation

In Assembler, all application environments (Dev/Staging/Prod/Other) live within their own [Kubernetes Namespace](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/). All application infrastructure (App server, database etc) exist within the environment namespace. This allows environments to be treated as a single entity and separated from other workloads from both a resource consumption and a data separation perspective. 

## Network policies

All environments, by default, only expose Port 443 to the application server via a Kubernetes [Ingress](https://kubernetes.io/docs/concepts/services-networking/ingress/). All internal communication between namespace components is transmitted through the Kubernetes private network for the cluster. 

Components within a namespace are protected from other namespaces through Kubernetes [Network Policies](https://kubernetes.io/docs/concepts/services-networking/network-policies/). This makes all components within a  namespace unable to send or receive any requests to any other pod outside of the environment namespace.

## RBAC Authorisation

RBAC authorisation limits Kubernetes API access to the limited interface required within the cluster to administer the application. More information about RBAC Authorisation can be found [here](https://kubernetes.io/docs/reference/access-authn-authz/rbac/).