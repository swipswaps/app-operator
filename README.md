# App Operator

Kubernetes operator for GRPC or HTTP applications. Simplifies and reduces
boilerplate by creating an ingress, service and deployment under the hood for
you.

Are you tired of copying around complex Helm configuration for deployments?
The `app-operator` is a Helm chart with best practices for deploying http based
services to Kubernetes. This is born out of copying similar configuration around
to make helm charts that look quite alike one another.

An added benefit of this approach is removing the dependency on a server side
Tiller for applying charts.

STATUS: Experimental.

## Example

Create's a deployment resource using the image `myrepo:v1` to Kubernetes with
a service and a corresponding ingress resource.

```yaml
apiVersion: deliverybot.dev/v1alpha1
kind: Application
metadata:
  name: example-application
spec:
  app:
    name: example-application
    version: v1
  image:
    repository: myrepo
    tag: v1
  ingress:
    enabled: true
    hosts:
      - host: example.com
        paths: ["/"]
```

## Installation

```bash
kubectl apply -k https://github.com/deliverybot/app-operator/deploy/
```

## Spec

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| affinity | object | `{}` | Kubernetes affinity resource. |
| app.name | string | `"nginx"` | Application name: Can be consistent between tracks. (required) |
| app.version | string | `"v1"` | Application version: Unique tag for this release. (required) |
| env | list | `[]` | Environment variables for the application. |
| image.pullPolicy | string | `"IfNotPresent"` |  |
| image.repository | string | `"nginx"` | Docker image repository. |
| image.tag | string | `"latest"` | Docker image tag. |
| imagePullSecrets | list | `[]` | Defines secrets to use for pulling docker images. |
| ingress.annotations | object | `{}` | Configures annotations for the ingress. |
| ingress.enabled | bool | `false` | Enable ingress resource. |
| ingress.hosts[0].host | string | `"chart-example.local"` | Host name for routing traffic. |
| ingress.hosts[0].paths | list | `["/"]` | Array of routable paths. |
| ingress.tls | list | `[]` | Kubernetes ingress tls resource. |
| livenessProbe | object | `{"httpGet":{"path":"/","port":"http"}}` | Customize the livenessProbe. |
| migrate.command | string | `"rails db:migrate"` | Command to run on the migrate pod. |
| migrate.enabled | bool | `false` | Run a pod before the application is released to migrate. |
| nodeSelector | object | `{}` | Kubernetes node selectors for Deployment resources. |
| readinessProbe | object | `{"httpGet":{"path":"/","port":"http"}}` | Customize the readiness probe. |
| replicaCount | int | `1` | Replica count for deployments. |
| resources | object | `{}` | Kubernetes resources for Deployment resources. |
| secret | string | `nil` | If defined will pull all secrets from this resource using envFrom. |
| service.enabled | bool | `true` | Enable service resource. |
| service.internalPort | int | `80` | Deployment internal port. |
| service.port | int | `80` | Kubernetes service port. |
| service.type | string | `"ClusterIP"` | Kubernetes service type. |
| tolerations | list | `[]` | Kubernetes tolerations for Deployment resources. |
| workers | list | `[]` | Deploy background jobs with the same config as the main app. |
