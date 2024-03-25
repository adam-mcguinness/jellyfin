# Jellyfin Helm Chart

This Helm chart deploys the Jellyfin media system on a Kubernetes cluster using the Helm package manager.

## Prerequisites

- Kubernetes 1.12+
- Helm 3.0+
- PV provisioner support in the underlying infrastructure if persisting data

## Installing the Chart

To install the chart with the release name `my-jellyfin`:

```bash
helm install my-jellyfin path/to/your/chart --values path/to/your/values.yaml
```

## Uninstalling the Chart

To uninstall/delete the `my-jellyfin` deployment:

```bash
helm delete my-jellyfin
```

## Configuration

The following table lists the configurable parameters of the Jellyfin chart and their default values as per your provided values file.

| Parameter                   | Description                                      | Default                |
| --------------------------- | ------------------------------------------------ | ---------------------- |
| `app.name`                  | Application Name                                 | `jellyfin`             |
| `deployment.name`           | Deployment Name                                  | `jellyfin`             |
| `deployment.replicas`       | Number of nodes                                  | `1`                    |
| `container.name`            | Container Name                                   | `jellyfin`             |
| `container.image`           | Image location                                   | `jellyfin/jellyfin`    |
| `container.tag`             | Image tag                                        | `latest`               |
| `container.imagePullPolicy` | Image pull policy                                | `IfNotPresent`         |
| `container.port`            | Container port                                   | `8096`                 |
| `container.securityContext.runAsUser`  | UID for the container               | `1000`                 |
| `container.securityContext.runAsGroup` | GID for the container               | `1000`                 |
| `container.volumeMounts`    | Volume mounts for the container                  | See Volume Mounts      |
| `service.name`              | Service name                                     | `jellyfin-service`     |
| `service.ports.http.port`   | Service HTTP port                                | `443`                  |
| `service.ports.http.targetPort` | Container port targeted by the service     | `8096`                 |
| `service.ports.http.protocol`   | Protocol used by the service                 | `TCP`                  |
| `volumes.name`              | Volume name                                      | `data`                 |
| `volumes.nfs.server`        | NFS server IP                                    | `192.168.x.x`          |
| `volumes.nfs.path`          | Path on the NFS server                           | `/nfs_share`           |
| `ingress.enabled`           | If true, an ingress will be created              | `true`                 |
| `ingress.name`              | Ingress name                                     | `jellyfin-ingress`     |
| `ingress.className`         | Ingress class name                               | `nginx`                |
| `ingress.host`              | Host for the ingress                             | `jellyfin.domain.com`  |
| `ingress.path`              | Path for the ingress                             | `/`                    |
| `ingress.tlsSecret`         | TLS secret for the ingress                       | `jellyfin-tls`         |

### Volume Mounts

Volumes are mounted to provide persistent storage for Jellyfin's configuration, cache, and media files:

- `/config`: Path where Jellyfin's configuration data is stored. Mapped to NFS location `/location_on_nfs`.
- `/cache`: Path for Jellyfin's cache. Mapped to NFS location `/location_on_nfs`.
- `/media`: Path for Jellyfin's media files. Directly mapped to the root of the NFS share or a specified sub-directory.

Ensure that your NFS server is accessible from within your Kubernetes cluster at the specified IP address and that the paths exist and are accessible.

## Customizing Installation

To further customize the installation, you can modify the values in your `values.yaml` file and reapply the chart using the `helm upgrade` command:

```bash
helm upgrade my-jellyfin path/to/your/chart --values path/to/your/values.yaml
```

For more information on configuring the Jellyfin application itself, refer to the [official Jellyfin documentation](https://jellyfin.org/docs/).

