# GITHUB Workflow for Container Lifecycles

A github shared workflow to build a set of container images and refresh them only when real changes happen

## Usage example

Add to your project .github/workflows/containers.yml, editing v1 to match the api version. Do not use main, it is where development happens
and may break at any time.
```yaml
name: Build
on:
  schedule:
    - cron: '0 8 * * *'

  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:

permissions:
  contents: read
  packages: write
  id-token: write
  
jobs:
  build:
    uses: kfox1111/github-workflow-container-lifecycle/.github/workflows/containers.yml@v1
```

You may limit the set of architectures to use like:
```
jobs:
  build:
    uses: kfox1111/github-workflow-container-lifecycle/.github/workflows/containers.yml@v1
    with:
      platforms: "linux/arm64 linux/amd64"

```

Add a containers directory. Make a subdirectory for each container image you want, and add a Dockerfile to each.

It will output images in `ghcr.io/<yourorg>/<yourrepo>/<containername>`

By default, it will produce a latest tag that will point at a multiarch index to the most recent builds.

It will produce immutable multiarch images with a tag of the timestamp built on. It also produces individual immutable images tagged with the architecture.

example tags:
```
latest
latest-linux-arm64
latest-linux-amd64
20260302-012712
20260302-011508
linux-arm64-20260302-012712
linux-amd64-20260302-012712
linux-arm64-20260302-011508
linux-amd64-20260302-011508
```
