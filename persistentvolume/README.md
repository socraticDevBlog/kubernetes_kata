# persistent volume

Create a `PersistentVolume` with a capacity of 1GB, access mode should be
`ReadWriteOnce` and it should mount to `/mnt/data`

Claim that volume with a `PersistentVolumeClaim`. Match its access mode and
request the same capacity that it offers

## validate

```bash
kubectl describe pvc <pvc-name> -n <namespace>
```

Status should be: `Bound`

Volume: shows the name of the PV to which the PVC is bound

```bash
kubectl describe pv <pv-name>
```
Status should be: `Bound`
Claim: should match the PVC name and its namespace

## sources

[Persistent Volumes](https://kubernetes.io/docs/concepts/storage/persistent-volumes/)