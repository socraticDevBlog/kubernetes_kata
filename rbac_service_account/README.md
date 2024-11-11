# rbac and service accounts

1. deploy an nginx web server in a `Deployment` manifest. Make sure it's
   configured to log requests and responses

## 1 - validate

```bash
# 1- forward nginx pod's port to your localhost
kubectl port-forward <podname> 8080:80

# 2- visit URL http://localhost:8080 and hit reload a few times
```

2. create a ServiceAccount that has no RBAC linked to it

- to make sure you are not allowed to read logs because locally, you are a super-admin!

```bash
kubectl create serviceaccount limited-user -n kata-ns
```

run command to read logs from your pod and expect to be forbidden

```bash
kubectl logs <your pod> -n kata-ns --as=system:serviceaccount:kata-ns:limited-user

#expect access is forbidden
```

0. Creat another `ServiceAccount` named "logs-svc"

1. create a Role that has these rules:

- apiGroups: [""]
- resources: ["pods", "pods/log"]
- verbs: ["get", "list"]

2. bind the Role to the ServiceAccount

```bash
kubectl logs <your pod> -n kata-ns --as=system:serviceaccount:kata-ns:logs-svc

#expect to be able to read the pod's logs

```
