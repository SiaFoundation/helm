# s3d

Chart for [s3d](https://github.com/SiaFoundation/s3d). The `config` value is
written to `s3d.yml`, see s3d README for details.

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

Clients that address buckets as `<bucket>.s3.example.com` need
`config.s3.hostBases` set to the domain and a wildcard host on the ingress.

Bucket policy conditions on `aws:SourceIp` and `aws:SecureTransport` need
`config.s3.trustedProxies` set to the CIDR the ingress controller dials from,
otherwise every request looks like plain HTTP from the controller.
