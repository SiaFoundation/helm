# Helm charts

Charts for Sia Foundation software, published to
`oci://ghcr.io/siafoundation/charts`.

| Chart | Install |
|-------|---------|
| [s3d](s3d/README.md) | `helm install s3d oci://ghcr.io/siafoundation/charts/s3d` |

A push to `main` publishes every chart whose `version` in `Chart.yaml` is not
on the registry yet, so bump the version with every change.
