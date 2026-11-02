# s3d Helm chart

Runs one `s3d` instance as a StatefulSet with a persistent data volume.

```sh
helm install s3d oci://ghcr.io/siafoundation/charts/s3d \
  --set adminPassword=<password> \
  --set recoveryPhrase="<12 words>" \
  --set preauthorizedKey=<hex>
```

The image tag defaults to the `appVersion` in `Chart.yaml`, the s3d release
the chart was written against. Every value is documented in `values.yaml`.

On the first start the pod registers with the indexer using the pre-authorized
key and the recovery phrase, then stores the app key on the data volume. Later
starts use the stored key and no longer read the phrase or the pre-authorized
key, so both can be removed from the values once the pod has come up. Helm
keeps earlier revisions around, so they stay readable through the release
history until it is pruned or the release is uninstalled. The key is minted on
the indexer, expires and is good for a limited number of uses.

The data volume holds the metadata database and the objects waiting to be
uploaded to Sia. It is not deleted by `helm uninstall`, a reinstall with the
same release name picks it up again, and `persistence.size` should cover
`sia.diskUsageLimit` in the config, 10 GiB by default.

Run a single instance per app key. The chart pins the replica count to one for
that reason, see the project README.

The admin API listens on port 8001 of the service, behind the password and
without TLS, so leave it inside the cluster. The S3 API is on port 8000 and is
the only port the Ingress exposes. Add the wildcard host to `ingress.hosts`
together with `config.s3.hostBases` for virtual-hosted-style buckets.

## Users and access keys

```sh
kubectl exec s3d-0 -- s3d users create <username>
kubectl exec s3d-0 -- s3d keys create <username>
```

The secret key is only shown once.
