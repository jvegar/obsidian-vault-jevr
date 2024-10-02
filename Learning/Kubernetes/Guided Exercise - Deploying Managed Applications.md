## Outcomes

You should be able to:

- Deploy an application container with several replicas.
    
- Review the structure of the `Deployment` resource manifest.
    
- Update the application to a new version without losing availability.

## Prerequisites

You need a working Kubernetes cluster, and your `kubectl` command must be configured to communicate with the cluster.

Make sure your `kubectl` context refers to a namespace where you have enough permissions, usually `_username_-dev` or `_username_-stage`. Use the `kubectl config set-context --current --namespace=_namespace_` command to switch to the appropriate namespace.