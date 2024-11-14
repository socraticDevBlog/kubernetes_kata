![Kubernetes Image](kubernetes.webp)

# kubernetes_kata

various katas to whet my kubernetes skills

## namespace

before provisionning any resource to your cluster, create a `kata-ns` and
provision all resources to this namespace

when you're finish working, delete all resources to keep your lab cluster clean

```bash
kubectl delete -f ns.yml
```

## coming soon

- helm
- horizontal pod scaling
- network policies
- custom resource definition (CRD)
- logging and monitoring
- ci/cd with kubernetes
- statefulset
