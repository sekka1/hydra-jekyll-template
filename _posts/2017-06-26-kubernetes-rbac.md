---
date: 2018-06-16
title: Kubernetes RBAC - giving permissions for logging and port-forwarding
categories:
  - kubernetes
  - RBAC
  - security
author_staff_member: Garland Kan
---
I am really liking Kubernetes [RBAC](https://kubernetes.io/docs/reference/access-authn-authz/rbac/).
It is "fairly" simple to use and so powerful.

For example, a user said I can't port forward to a port and pasted me the error:

```
error: error upgrading connection: pods "selenium-node-firefox-debug-mtw7r" is forbidden: User "john" cannot create pods/portforward in the namespace "app1"
```

This basically told me all that I need.  It clearly states the user cannot perform
the action `pods/portforward`.

So I added this to his role:

```
---
kind: Role
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  namespace: gawkbox-spinnaker
  name: kube-saas:list-and-logs
rules:
- apiGroups: [""]
  resources: ["pods", "pods/log", pods/portforward]
  verbs: ["get", "list"]
```

This solved the problem.  I love it when the error tells me exactly what it is and
it is so easy to express this in the role.
