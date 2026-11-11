# s3d

```sh
helm install s3d oci://ghcr.io/siafoundation/charts/s3d \
  --set adminPassword=<password> \
  --set recoveryPhrase="<12 words>" \
  --set preauthorizedKey=<hex>
```

The recovery phrase and the pre-authorized key register the instance on its
first start and can be removed from the values afterwards.

Create a user and an access key once the pod is ready:

```sh
kubectl exec s3d-0 -- s3d users create <username>
kubectl exec s3d-0 -- s3d keys create <username>
```
