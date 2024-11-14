# kustomize and multi-environments deployments

> kustomize lets you customize raw, template-free YAML files for multiple purposes, leaving the original YAML untouched and usable as is.
>
> kustomize targets kubernetes; it understands and can patch kubernetes style
> API objects. It's like make, in that what it does is declared in a file, and
> it's like sed, in that it emits edited text.

## challenge

### requirements

| envir | pod name suffix | replicas | custom environment variable |
| ----- | --------------- | -------- | --------------------------- |
| dev   | `-dev`          | 2        | none                        |
| prod  | `-prod`         | 5        | COPYRIGHT=2024              |

using `kubectl apply -k nginx-app/overlays/dev` deploy an nginx deployment running
2 pods in dev environment:

using `kubectl apply -k nginx-app/overlays/prod` deploy an nginx deployment
running 5 pods in prod ; each container's should include a
custom `COPYRIGHT` environment variable with the current year

### recommended directory structure

```
nginx-app/
├── base/
│   ├── pod.yaml
│   |── kustomization.yaml
└── overlays/
    ├── dev/
    │   └── kustomization.yaml
    └── prod/
        └── kustomization.yaml

```

## validation

list all pods (`kubectl get pods -n kata-ns`) and expect:

- 2 pods in `dev` envir
- 5 pods in `prod` envir

check each production pods to make sure they each have a `COPYRIGHT` year value
stored as a container's environment's variable

```bash
kubectl get pods -n kata-ns -o name | grep "\-prod\-" | xargs -I {} kubectl exec -n kata-ns {} -- printenv | grep "^COPYRIGHT="
```

## sources

[Declarative Management of Kubernetes Objects Using Kustomize](https://kubernetes.io/docs/tasks/manage-kubernetes-objects/kustomization/)
