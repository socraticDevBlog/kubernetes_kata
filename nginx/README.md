# nginx deployment

## deployment

- running `nginx:latest` image
- app name is `kata-app`
- 3 replicas for High availability!
- run on containerPort 80

## service

we want to access the nginx default page on either of the 3 replicas

create a `service` of type `ClusterIP` running on port 80 with a targetPort 80
of the `TCP` protocol

## validation 

```bash
kubectl port-forward svc/kata-app-service 8080:80 -n kata-ns
```

open a browser and navigate to url `localhost:8080` and expect to see nginx
default page