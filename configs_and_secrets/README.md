configmap and secret

they are used to store key-value pairs

a secret is ... more secret and gets `base64` encrypted...

|           |                |                 |
| --------- | -------------- | --------------- |
| configmap | APP_MESSAGE    | "a fun message" |
| secret    | APP_SECRET_KEY | "swordfish"     |

## kata

1. create a configmap
2. create a secret
3. create a deployment:
   1. one replica
   2. assign `APP_MESSAGE` as an environment variable
   3. assign `APP_SECRET_KEY` as an environment variable

## validation

execute a `printenv` command inside the running nginx pod and we expect to see
the values of your `configmap` and `secret`

```bash
k exec -it -n <your pod's name> -- printenv | grep "APP_"
```

notice the `secret` value is base64 encoded

```bash
echo "c3dvcmRmaXNoCg==" | base64 -d
```
