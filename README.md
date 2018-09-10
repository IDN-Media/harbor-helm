# Helm Chart for Harbor

## Introduction

This [Helm](https://github.com/kubernetes/helm) chart installs [Harbor](https://github.com/goharbor/harbor) in a Kubernetes cluster. Welcome to [contribute](CONTRIBUTING.md) to Helm Chart for Harbor.

## Prerequisites

- Kubernetes cluster 1.8+ with Beta APIs enabled
- Kubernetes Ingress Controller is enabled
- Helm 2.8.0+

## Installing the Chart

Download Harbor helm chart code.
```bash
git clone https://github.com/goharbor/harbor-helm
cd harbor-helm
```
Download external dependent charts required by Harbor chart.
```bash
helm dependency update
```
Install the Harbor helm chart with a release name `my-release`:
```bash
helm install --name my-release .
```

The command deploys Harbor on the Kubernetes cluster with the default configuration.
The [configuration](#configuration) section lists the parameters that can be configured in values.yaml or via '--set' flag during installation.

## Uninstalling the Chart

To uninstall/delete the `my-release` deployment:

```bash
helm delete --purge my-release
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

## Configuration

The following tables lists the configurable parameters of the Harbor chart and the default values.

| Parameter                  | Description                        | Default                 |
| -----------------------    | ---------------------------------- | ----------------------- |
| **Harbor** |
| `persistence.enabled`     | Persistent data | `true` |
| `externalURL`       | Ther external URL for Harbor core service | `https://core.harbor.domain` |
| `harborAdminPassword`  | The password of system admin | `Harbor12345` |
| `authenticationMode` | The authentication mode: `db_auth` for local database, `ldap_auth` for LDAP | `db_auth` |
| `selfRegistration`               | Allows users to register by themselves, otherwise only system administrators can add users | `on` |
| `email.host` | The hostname of email server | `smtp.mydomain.com` |
| `email.port` | The port of email server | `25` |
| `email.username` | The username of email server | `sample_admin@mydomain.com` |
| `email.password` | The password for email server | `password` |
| `email.ssl` | Whether use TLS | `false` |
| `email.insecure` | Whether the connection with email server is insecure | `false` |
| `email.from` | The from address shows when send email| `admin <sample_admin@mydomain.com>` |
| `email.identity` | | |
| `ldap.url` | LDAP server URL for `ldap_auth` authentication | `ldaps://ldapserver` |
| `ldap.searchDN` | LDAP search DN | |
| `ldap.searchPassword` | LDAP search password | |
| `ldap.baseDN` | LDAP base DN | |
| `ldap.filter` | LDAP filter | `(objectClass=person)` |
| `ldap.uid` | LDAP UID | `uid` |
| `ldap.scope` | LDAP scope | `2` |
| `ldap.timeout` | LDAP timeout | `5` |
| `ldap.verifyCert` | Whether to verify HTTPS certificate | `true` |
| `secretkey` | The key used for encryption. Must be a string of 16 chars | `not-a-secure-key` |
| `harborImageTag` | The tag of Harbor images | `dev` |
| **Ingress** |
| `ingress.enabled` | Enable ingress objects | `true` |
| `ingress.hosts.core` | The host of Harbor core service in ingress rule | `core.harbor.domain` |
| `ingress.hosts.notary` | The host of Harbor notary service in ingress rule | `notary.harbor.domain` |
| `ingress.annotations` | The annotations used in ingress | `true` |
| `ingress.tls.enabled` | Enable TLS | `true` |
| `ingress.tls.secretName` | Fill the secretName if you want to use the certificate of yourself when Harbor serves with HTTPS. A certificate will be generated automatically by the chart if leave it empty | |
| **Adminserver** |
| `adminserver.image.repository` | Repository for adminserver image | `goharbor/harbor-adminserver` |
| `adminserver.image.tag` | Tag for adminserver image | `dev` |
| `adminserver.image.pullPolicy` | Pull Policy for adminserver image | `IfNotPresent` |
| `adminserver.resources` | [resources](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/) to allocate for container   | undefined |
| `adminserver.nodeSelector` | Node labels for pod assignment | `{}` |
| `adminserver.tolerations` | Tolerations for pod assignment | `[]` |
| `adminserver.affinity` | Node/Pod affinities | `{}` |
| **Jobservice** |
| `jobservice.image.repository` | Repository for jobservice image | `goharbor/harbor-jobservice` |
| `jobservice.image.tag` | Tag for jobservice image | `dev` |
| `jobservice.image.pullPolicy` | Pull Policy for jobservice image | `IfNotPresent` |
| `jobservice.secret` | jobservice secret | `not-a-secure-secret` |
| `jobservice.resources` | [resources](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/) to allocate for container   | undefined |
| `jobservice.nodeSelector` | Node labels for pod assignment | `{}` |
| `jobservice.tolerations` | Tolerations for pod assignment | `[]` |
| `jobservice.affinity` | Node/Pod affinities | `{}` |
| **UI** |
| `ui.image.repository` | Repository for ui image | `goharbor/harbor-ui` |
| `ui.image.tag` | Tag for ui image | `dev` |
| `ui.image.pullPolicy` | Pull Policy for ui image | `IfNotPresent` |
| `ui.secret` | ui secret | `not-a-secure-secret` |
| `ui.resources` | [resources](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/) to allocate for container   | undefined |
| `ui.nodeSelector` | Node labels for pod assignment | `{}` |
| `ui.tolerations` | Tolerations for pod assignment | `[]` |
| `ui.affinity` | Node/Pod affinities | `{}` |
| **Database** |
| `database.type` | If external database is used, set it to `external` | `internal` |
| `database.internal.image.repository` | Repository for database image | `goharbor/harbor-db` |
| `database.internal.image.tag` | Tag for database image | `dev` |
| `database.internal.image.pullPolicy` | Pull Policy for database image | `IfNotPresent` |
| `database.internal.password` | The password for database | `changeit` |
| `database.resources` | [resources](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/) to allocate for container   | undefined |
| `database.internal.volumes` | The volume used to persistent data |
| `database.internal.nodeSelector` | Node labels for pod assignment | `{}` |
| `database.internal.tolerations` | Tolerations for pod assignment | `[]` |
| `database.internal.affinity` | Node/Pod affinities | `{}` |
| `database.external.host` | The hostname of external database | `192.168.0.1` |
| `database.external.port` | The port of external database | `5432` |
| `database.external.username` | The username of external database | `user` |
| `database.external.password` | The password of external database | `password` |
| `database.external.coreDatabase` | The database used by core service | `registry` |
| `database.external.sslmode` | Connection method of external database (require|prefer|disable) | `disable`|
| `database.external.clairDatabase` | The database used by clair | `clair` |
| `database.external.notaryServerDatabase` | The database used by Notary server | `notary_server` |
| `database.external.notarySignerDatabase` | The database used by Notary signer | `notary_signer` |
| **Registry** |
| `registry.image.repository` | Repository for registry image | `goharbor/registry-photon` |
| `registry.image.tag` | Tag for registry image | `dev` |
| `registry.image.pullPolicy` | Pull Policy for registry image | `IfNotPresent` |
| `registry.httpSecret` | registry secret | `not-a-secure-secret` |
| `registry.logLevel` | The log level | `info` |
| `registry.storage.type` | The storage used to store images: `filesystem`, `azure`, `gcs`, `s3`, `swift`, `oss` | `filesystem` |
| `registry.resources` | [resources](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/) to allocate for container   | undefined |
| `registry.volumes` | used to create PVCs if persistence is enabled (see instructions in values.yaml) | see values.yaml |
| `registry.nodeSelector` | Node labels for pod assignment | `{}` |
| `registry.tolerations` | Tolerations for pod assignment | `[]` |
| `registry.affinity` | Node/Pod affinities | `{}` |
| **Chartmuseum** |
| `chartmuseum.enabled` | Enable chartmusuem to store chart | `true` |
| `chartmuseum.image.repository` | Repository for chartmuseum image | `goharbor/chartmuseum-photon` |
| `chartmuseum.image.tag` | Tag for chartmuseum image | `dev` |
| `chartmuseum.image.pullPolicy` | Pull Policy for chartmuseum image | `IfNotPresent` |
| `chartmuseum.resources` | [resources](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/) to allocate for container   | undefined |
| `chartmuseum.volumes` | used to create PVCs if persistence is enabled (see instructions in values.yaml) | see values.yaml |
| `chartmuseum.nodeSelector` | Node labels for pod assignment | `{}` |
| `chartmuseum.tolerations` | Tolerations for pod assignment | `[]` |
| `chartmuseum.affinity` | Node/Pod affinities | `{}` |
| **Clair** |
| `clair.enabled` | Enable Clair? | `true` |
| `clair.image.repository` | Repository for clair image | `goharbor/clair-photon` |
| `clair.image.tag` | Tag for clair image | `dev`
| `clair.resources` | [resources](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/) to allocate for container   | undefined
| `clair.nodeSelector` | Node labels for pod assignment | `{}` |
| `clair.tolerations` | Tolerations for pod assignment | `[]` |
| `clair.affinity` | Node/Pod affinities | `{}` |
| **Redis** |
| `redis.usePassword` | Whether use password | `false` |
| `redis.password` | The password for Redis | `changeit` |
| `redis.cluster.enabled` | Enable Redis cluster | `false` |
| `redis.master.persistence.enabled` | Persistent data   | `false` |
| `redis.external.enabled` | If an external Redis is used, set it to `true` | `false` |
| `redis.external.host` | The hostname of external Redis | `192.168.0.2` |
| `redis.external.port` | The port of external Redis | `6379` |
| `redis.external.databaseIndex` | The database index of external Redis | `0` |
| `redis.external.usePassword` | Whether use password for external Redis | `false` |
| `redis.external.password` | The password of external Redis | `changeit` |
| **Notary** |
| `notary.enabled` | Enable Notary? | `true` |
| `notary.server.image.repository` | Repository for notary server image | `goharbor/notary-server-photon` |
| `notary.server.image.tag` | Tag for notary server image | `dev`
| `notary.signer.image.repository` | Repository for notary signer image | `goharbor/notary-signer-photon` |
| `notary.signer.image.tag` | Tag for notary signer image | `dev`
| `notary.nodeSelector` | Node labels for pod assignment | `{}` |
| `notary.tolerations` | Tolerations for pod assignment | `[]` |
| `notary.affinity` | Node/Pod affinities | `{}` |

## Persistence

You need to create `StorageClass` before you can persist data in persistent volume.

To create a `StorageClass`, set the following value in `values.yaml`:

```yaml
persistence:
  enabled: true

```

Four PVCs will be created automatically:
- adminserver-config
- chartmuseum-data
- database-data
- registry-data

All the created PVCs need to be removed manually after Helm deletes the Chart. 

When running a cluster without persistence, this Chart uses `emptyDir` as the temporary volumes. Data does not survive the termination of a pod.
