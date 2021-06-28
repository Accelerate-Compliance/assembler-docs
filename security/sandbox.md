# Sandbox execution security
## Overview
Assembler, as a low-code platform, has the ability to execute arbitrary Javascript, defined for an application, in a secure manner. 

A variety of technologies are implemented in order to do this in a safe manner.

### V8 Isolates

Customer supplied code (written as script actions in the platform for an application) is executed within a [V8 isolate](https://v8docs.nodesource.com/node-0.8/d5/dda/classv8_1_1_isolate.html). V8 Isolates have the ability to sandbox untrusted Javascript and execute them within entirely isolated environments. This is the same technology used by Chromium-based web browsers, such as Chrome and Edge, in order to isolate Javascript execution between tabs. This Javascript has no access to the underlying machine and has memory and CPU resource limits imposed at the isolate level.

### Pod Isolation

Purpose built Kubernetes pods are deployed to run the custom code. This further sandboxes the script execution from the rest of the platform. Network policies, RBAC Authorisation and anomaly detection protect the rest of the cluster if a breach was to occur. Where available, script pods are run using [gVisor](https://gvisor.dev/) to provide further sandboxing.