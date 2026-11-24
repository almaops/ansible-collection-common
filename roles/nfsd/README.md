# almaops.common.nfsd

Deployment of a dedicated NFS server on Debian 13 for integration with Kubernetes using the upstream NFS CSI driver.

## Overview

The solution intended for providing shared persistent storage for Kubernetes workloads requiring `ReadWriteMany` (RWX) access.

Components:

- **NFS server:** Debian 13 with `nfs-kernel-server`.
- **Server provisioning:** Ansible, using the `almaops.common.nfsd` role.
- **CSI driver:** Kubernetes NFS CSI (`nfs.csi.k8s.io`).
- **CSI deployment:** Helm, managed by OpenTofu/Terraform.
- **Storage backend:** Local filesystem on a dedicated NFS server.
- **Protocol:** NFSv4.1 over TCP.

The CSI driver dynamically provisions persistent volumes by creating individual subdirectories within an existing NFS export.

## Requirements

### NFS server

- Debian 13.
- Static IP address or stable DNS hostname.
- Dedicated storage filesystem recommended.
- TCP port `2049` accessible from Kubernetes nodes.
- Ansible access with root privileges.

### Kubernetes cluster

- Connectivity to the NFS server from all nodes running CSI components.
- NFS client support.
- Helm 3.
- OpenTofu/Terraform with the Helm provider.

For Debian-based Kubernetes nodes, install the NFS client package:

```bash
apt-get install -y nfs-common
```

## NFS Server Deployment

### Ansible role

```yaml
roles:
  - role: almaops.common.nfsd
```

### Role variables

| Variable | Default | Description |
|---|---|---|
| `nfsd_server_packages` | `nfs-kernel-server`, `nfs-common` | Required packages |
| `nfsd_server_service_name` | `nfs-kernel-server` | Systemd service name |
| `nfsd_server_service_enabled` | `true` | Enable service on boot |
| `nfsd_server_service_state` | `started` | Desired service state |
| `nfsd_server_threads` | `16` | Number of NFS server threads |
| `nfsd_server_host` | `0.0.0.0` | Listen address |
| `nfsd_server_port` | `2049` | NFS service port |
| `nfsd_server_vers3` | `false` | Enable NFSv3 |
| `nfsd_server_vers4` | `true` | Enable NFSv4 |
| `nfsd_server_vers4_0` | `false` | Enable NFSv4.0 |
| `nfsd_server_vers4_1` | `true` | Enable NFSv4.1 |
| `nfsd_server_vers4_2` | `true` | Enable NFSv4.2 |
| `nfsd_server_exports` | `[]` | List of NFS exports |

### Example configuration

```yaml
---
nfsd_server_threads: 32

nfsd_server_exports:
  - path: /srv/nfs/k8s
    owner: nobody
    group: nogroup
    mode: '0777'
    clients:
      - host: 10.10.20.0/24
        options: rw,sync,no_subtree_check,root_squash
```

Replace the client subnet with the actual Kubernetes node network.

Multiple exports and client networks can be configured:

```yaml
nfsd_server_exports:
  - path: /srv/nfs/k8s-prod
    owner: nobody
    group: nogroup
    mode: '0777'
    clients:
      - host: 10.10.20.0/24
        options: rw,sync,no_subtree_check,root_squash

  - path: /srv/nfs/k8s-preprod
    owner: nobody
    group: nogroup
    mode: '0777'
    clients:
      - host: 10.10.30.0/24
        options: rw,sync,no_subtree_check,root_squash
```

### Managed configuration files

The role manages the following files:

```text
/etc/nfs.conf.d/90-ansible.conf
/etc/exports.d/ansible.exports
```

Changes to NFS daemon configuration trigger a service restart. Changes to export definitions trigger an export reload using `exportfs -ra`.

### Deployment playbook

```yaml
---
- name: Provision NFS servers
  hosts: nfsd_servers
  become: true
  gather_facts: true

  roles:
    - role: almaops.common.nfsd
```

Run the playbook:

```bash
ansible-playbook \
  -i inventory/hosts.yml \
  playbooks/nfs-server.yml
```

## NFS Export Options

| Option | Description |
|---|---|
| `rw` | Allow read and write operations |
| `sync` | Commit writes to stable storage before acknowledging them |
| `no_subtree_check` | Disable subtree verification |
| `root_squash` | Map remote root to the anonymous UID/GID |
| `no_root_squash` | Preserve remote root privileges; not recommended by default |
| `anonuid` | Override anonymous user ID |
| `anongid` | Override anonymous group ID |

### Permissions

With `root_squash` enabled, remote root operations are mapped to the anonymous user, typically UID/GID `65534`.

The export directory must be writable by this identity to allow the CSI controller to provision persistent volumes.

The example configuration uses `nobody:nogroup` ownership and mode `0777` for compatibility with workloads using different UIDs and GIDs.

This configuration assumes a trusted Kubernetes environment. It does not provide strong isolation between mutually untrusted workloads.

Application-specific permissions should be configured and tested separately.

## Kubernetes NFS CSI Driver

### Helm repository

```bash
helm repo add csi-driver-nfs \
  https://kubernetes-csi.github.io/csi-driver-nfs

helm repo update csi-driver-nfs
```

List available chart versions:

```bash
helm search repo csi-driver-nfs --versions
```

Retrieve upstream chart defaults:

```bash
helm show values csi-driver-nfs/csi-driver-nfs \
  --version 4.13.6
```

Pin the chart version and verify the configuration against the selected release before deployment.

### Helm values

File: `templates/helm-nfs-csi-values.yaml`

```yaml
---
driver:
  name: nfs.csi.k8s.io
  mountPermissions: 0

feature:
  enableFSGroupPolicy: true
  fsGroupPolicy: File

controller:
  replicas: 1
  enableSnapshotter: false
  defaultOnDeletePolicy: retain

node:
  maxUnavailable: 1

externalSnapshotter:
  enabled: false

storageClass:
  create: false

storageClasses:
  - name: nfs-csi
    parameters:
      server: 10.10.10.20
      share: /srv/nfs/k8s
      subDir: ${pvc.metadata.namespace}/${pv.metadata.name}
      mountPermissions: "0"
      onDelete: delete
    reclaimPolicy: Delete
    volumeBindingMode: Immediate
    mountOptions:
      - nfsvers=4.1
      - hard

  - name: nfs-csi-retain
    parameters:
      server: 10.10.10.20
      share: /srv/nfs/k8s
      subDir: ${pvc.metadata.namespace}/${pv.metadata.name}
      mountPermissions: "0"
      onDelete: retain
    reclaimPolicy: Retain
    volumeBindingMode: Immediate
    mountOptions:
      - nfsvers=4.1
      - hard
```

Replace the NFS server address and export path with environment-specific values.

### StorageClasses

| StorageClass | Reclaim policy | Directory deletion | Purpose |
|---|---|---|---|
| `nfs-csi` | `Delete` | Automatic | General-purpose storage |
| `nfs-csi-retain` | `Retain` | Manual | Persistent or critical data |

Neither StorageClass is intended to replace the existing default StorageClass.

### CSI configuration options

| Parameter | Value | Description |
|---|---|---|
| `controller.replicas` | `1` | Number of CSI controller replicas |
| `controller.enableSnapshotter` | `false` | Disable snapshot sidecar |
| `controller.defaultOnDeletePolicy` | `retain` | Default directory deletion behavior |
| `driver.mountPermissions` | `0` | Disable automatic permission changes |
| `feature.fsGroupPolicy` | `File` | Enable filesystem group permission handling |
| `node.maxUnavailable` | `1` | Limit unavailable node plugins during updates |
| `externalSnapshotter.enabled` | `false` | Disable external snapshot controller |

The `fsGroupPolicy` setting does not bypass NFS server-side permissions. Applications using `securityContext.fsGroup` must be tested with the configured export permissions.

### OpenTofu/Terraform deployment

```hcl
resource "helm_release" "nfsd_csi" {
  name       = "csi-driver-nfs"
  namespace  = "kube-system"
  repository = "https://kubernetes-csi.github.io/csi-driver-nfs"
  chart      = "csi-driver-nfs"
  version    = "4.13.6"

  atomic          = true
  cleanup_on_fail = true
  wait            = true
  timeout         = 600

  values = [
    file("${path.module}/templates/helm-nfs-csi-values.yaml")
  ]
}
```


When using `templatefile()` instead of `file()`, escape CSI placeholders to prevent OpenTofu/Terraform interpolation:

```yaml
subDir: $${pvc.metadata.namespace}/$${pv.metadata.name}
```

## Storage Directory Layout

Persistent volume directories are organized by Kubernetes namespace and PV name.

```text
/srv/nfs/k8s/
├── default/
│   └── pvc-xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/
├── monitoring/
│   └── pvc-xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/
└── production/
    ├── pvc-xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/
    └── pvc-yyyyyyyy-yyyy-yyyy-yyyy-yyyyyyyyyyyy/
```

Use separate exports for different Kubernetes clusters to simplify access control, backup, and storage management.

## Verification

### NFS server

Verify service status:

```bash
systemctl status nfs-kernel-server
```

List active exports:

```bash
exportfs -v
```

Reload exports:

```bash
exportfs -rav
```

Check enabled protocol versions:

```bash
cat /proc/fs/nfsd/versions
```

Verify listening port:

```bash
ss -lnt | grep ':2049'
```

### NFS client

Run from a Kubernetes node:

```bash
mkdir -p /mnt/nfs-test

mount -t nfs4 \
  -o nfsvers=4.1,hard \
  10.10.10.20:/srv/nfs/k8s \
  /mnt/nfs-test

touch /mnt/nfs-test/testfile
ls -ln /mnt/nfs-test/testfile
rm /mnt/nfs-test/testfile

umount /mnt/nfs-test
```

### Kubernetes CSI components

```bash
kubectl -n kube-system get pods -o wide | grep csi-nfs
kubectl get csidrivers
kubectl get storageclass
```

## Test Workload

### PersistentVolumeClaim

```yaml
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: nfs-test
spec:
  accessModes:
    - ReadWriteMany
  storageClassName: nfs-csi
  resources:
    requests:
      storage: 1Gi
```

### Test deployment

```yaml
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nfs-test
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nfs-test
  template:
    metadata:
      labels:
        app: nfs-test
    spec:
      containers:
        - name: busybox
          image: busybox:1.37
          command:
            - /bin/sh
            - -c
            - |
              while true; do
                date >> /data/$(hostname).log
                sleep 10
              done
          volumeMounts:
            - name: storage
              mountPath: /data
      volumes:
        - name: storage
          persistentVolumeClaim:
            claimName: nfs-test
```

Apply the manifests:

```bash
kubectl apply -f pvc.yaml
kubectl apply -f deployment.yaml
```

Verify provisioning:

```bash
kubectl get pvc nfs-test
kubectl get pv
kubectl get pods -l app=nfs-test -o wide
```

Verify shared storage:

```bash
kubectl exec deployment/nfs-test -- ls -l /data
```

Both replicas should write to the same persistent volume.

Remove the test workload:

```bash
kubectl delete deployment nfs-test
kubectl delete pvc nfs-test
```

With the `nfs-csi` StorageClass, the backing directory should be removed automatically.

## Monitoring

Monitor the following server-side parameters:

- Filesystem capacity and available space.
- Filesystem inode utilization.
- Disk I/O latency and throughput.
- Network utilization.
- NFS request rate and errors.
- NFS service availability.
- Backup execution status.

Useful diagnostic commands:

```bash
nfsstat -s
cat /proc/net/rpc/nfsd
df -h /srv/nfs/k8s
df -i /srv/nfs/k8s
```

Prometheus Node Exporter can collect filesystem, disk, network, and NFS server metrics.

Example filesystem utilization alert:

```yaml
- alert: NFSFilesystemSpaceLow
  expr: |
    (
      1 -
      node_filesystem_avail_bytes{mountpoint="/srv/nfs/k8s"}
      /
      node_filesystem_size_bytes{mountpoint="/srv/nfs/k8s"}
    ) > 0.85
  for: 15m
  labels:
    severity: warning
  annotations:
    summary: NFS filesystem utilization exceeds 85%
    description: "NFS storage on {{ $labels.instance }} is running out of space."
```

Adjust the mountpoint selector to match the actual dedicated storage filesystem.

## Troubleshooting

### No active NFS exports

```bash
ls -la /etc/exports.d/
cat /etc/exports.d/ansible.exports
exportfs -rav
exportfs -v
```

Verify that:

- `/etc/exports.d` exists.
- The configuration filename ends with `.exports`.
- Export directories exist.
- `nfsd_server_exports` is not empty.
- Ansible inventory variables are loaded correctly.

### PVC remains Pending

```bash
kubectl describe pvc <pvc-name>
kubectl -n kube-system get pods | grep csi-nfs
kubectl -n kube-system logs deployment/csi-nfs-controller -c nfs
```

Common causes:

- Incorrect NFS server address or export path.
- Firewall blocking TCP port `2049`.
- Missing NFS client support.
- Insufficient export directory permissions.
- Kubernetes node addresses not included in the export ACL.
- Unsupported NFS protocol version.

### Permission denied

Verify directory ownership and permissions:

```bash
ls -ldn /srv/nfs/k8s
exportfs -v
```

Check whether `root_squash` prevents CSI directory creation, ownership changes, or application write operations.

### NFS server unavailable

```bash
systemctl status nfs-kernel-server
journalctl -u nfs-kernel-server -n 100 --no-pager
ss -lnt | grep ':2049'
```

Hard-mounted NFS clients may block filesystem operations until server connectivity is restored.

## Backup and Recovery

The NFS server filesystem must be backed up independently of Kubernetes.

Recommended practices:

- Back up the entire export filesystem.
- Preserve numeric UID/GID ownership and file permissions.
- Preserve ACLs and extended attributes where applicable.
- Ensure application consistency before backing up active workloads.
- Back up Kubernetes PV and PVC definitions.
- Store backups outside the NFS server.
- Regularly test restoration.

Example filesystem synchronization:

```bash
rsync -aHAX --numeric-ids \
  /srv/nfs/k8s/ \
  backup-server:/srv/backups/nfs/k8s/
```

This command provides filesystem synchronization, not an application-consistent or versioned backup by itself.

## Security Considerations

- Restrict TCP port `2049` to authorized Kubernetes nodes.
- Enable `root_squash` by default.
- Disable unused NFS protocol versions.
- Avoid exposing NFS to public networks.
- Use separate exports for different Kubernetes clusters.
- Restrict access to the NFS server itself.
- Treat all authorized NFS clients as trusted systems.
- Do not rely on NFS subdirectories as a strong multi-tenant security boundary.

Standard NFS with `AUTH_SYS` does not provide encrypted transport or cryptographically authenticated user identities.

## Known Limitations

- A standalone NFS server is a single point of failure.
- PVC capacity requests do not enforce filesystem quotas.
- All PVCs on the same filesystem share its available capacity and performance.
- Storage performance depends on network connectivity and server disk performance.
- NFS CSI does not provide storage-level replication.
- Hard mounts may block application I/O during server outages.
- Application ownership changes may be restricted by `root_squash`.
- CSI snapshots are disabled in this deployment.
- NFS is not recommended as a universal replacement for block storage.

## Recommended Use Cases

Suitable workloads:

- Shared application files.
- Build artifacts.
- Static content.
- Document storage.
- Shared configuration files.
- File processing pipelines.
- Applications requiring RWX persistent volumes.

For databases, message brokers, and latency-sensitive workloads, evaluate dedicated block storage or application-native replication instead.

## References

- [Kubernetes NFS CSI Driver](https://github.com/kubernetes-csi/csi-driver-nfs)
- [NFS CSI Helm Chart](https://github.com/kubernetes-csi/csi-driver-nfs/tree/master/charts)
- [NFS CSI Driver Parameters](https://github.com/kubernetes-csi/csi-driver-nfs/blob/master/docs/driver-parameters.md)
- [Debian NFS Server Package](https://packages.debian.org/trixie/nfs-kernel-server)
- [Kubernetes Persistent Volumes](https://kubernetes.io/docs/concepts/storage/persistent-volumes/)
