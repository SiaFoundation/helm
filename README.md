# Helm charts

Charts for Sia Foundation software, published to
`oci://ghcr.io/siafoundation/charts`.

| Chart | Description |
|-------|-------------|
| [s3d](s3d/README.md) | An S3-compatible gateway for the Sia network |

A push to `main` publishes every chart whose `version` in `Chart.yaml` is not
on the registry yet, so bump the version with every change.
