Fork of the [Original](https://github.com/MaastrichtU-IDS/code-server). I am gonna try to keep this more maintained with VSCode Releases/CUDA Releases. What I have done is the following: 
- Switch to the cuda-devel images to be more minimal by default. You will have to install the respective frameworks yourself.
- Update the CUDA image. 
- Build again so latest available code-server is installed. Original image has an old version of code-server that does not use [open-vsx.org](https://open-vsx.org).
- Drop support for the base code-server image. That can be easily pulled from [lscr.io/linuxserver/openvscode-server:latest](https://hub.docker.com/r/linuxserver/openvscode-server) for example. 

Begin original README(I have only changed the links): 
---
**VisualStudio Code server** images based on https://github.com/cdr/code-server

* Hosted on [GitHub Container Registry](https://github.com/orgs/charitarthchugh/packages/container/package/code-server) ([ghcr.io](https://ghcr.io)) to avoid DockerHub pull limitations, and easily deploy on clusters (such as Kubernetes).
* Additionally installed on the CPU image: Python3, NodeJS (npm, yarn), Java JDK 11, PHP, Fortran

> Alternative: [jefferyb code-server image for OpenShift](https://github.com/jefferyb/code-server-openshift)

## Automatically updated

[![Publish Docker image](https://github.com/charitarthchugh/code-server/workflows/Publish%20Docker%20image/badge.svg)](https://github.com/charitarthchugh/code-server/actions) [![Publish GPU Docker image](https://github.com/charitarthchugh/code-server/actions/workflows/publish-docker-gpu.yml/badge.svg)](https://github.com/charitarthchugh/code-server/actions/workflows/publish-docker-gpu.yml)

The image on [ghcr.io](https://ghcr.io) is automatically updated every week (Monday at 3:00 GMT+1) by a GitHub Actions workflow to match the `latest` tag of [codercom/code-server](https://hub.docker.com/r/codercom/code-server)

This image extends the [`Dockerfile`](https://github.com/cdr/code-server/blob/main/ci/release-image/Dockerfile) defined at https://github.com/cdr/code-server

## Code server on CPU

### Run

```bash
docker run --rm -it -p 8080:8080 -e PASSWORD=password -v $(pwd):/home/coder/project ghcr.io/charitarthchugh/code-server:latest
```

In the container:

* User, with `sudo` privileges: `coder`
* Workspace path: `/home/coder`

You can also provide the URL of a git repository to be cloned at start, if a `requirements.txt`, `yarn.lock` or `package-lock.json` are present, they will be automatically installed

```bash
docker run --rm -it -p 8080:8080 -e PASSWORD=password -e GIT_URL=https://github.com/charitarthchugh/play-fair ghcr.io/charitarthchugh/code-server:latest
```

 

### Build

Feel free to edit the `Dockerfile` to install additional packages in the image.

```bash
docker build -t ghcr.io/charitarthchugh/code-server:latest .
```

### Push

```bash
docker push ghcr.io/charitarthchugh/code-server:latest
```

## Code server on Nvidia GPU

Images hosted on the GitHub Container Registry: https://github.com/orgs/charitarthchugh/packages/container/package/code-server-gpu

Based on Docker images provided by Nvidia:

* Tensorflow: https://ngc.nvidia.com/catalog/containers/nvidia:tensorflow
* PyTorch: https://ngc.nvidia.com/catalog/containers/nvidia:pytorch

The best way to update the images is to update the version of the environment variables `TENSORFLOW_IMAGE` and `PYTORCH_IMAGE` in the [`publish-docker-gpu.yml` workflow](https://github.com/charitarthchugh/code-server/blob/main/.github/workflows/publish-docker-gpu.yml), and push the changes to the `main` branch, the new images version will be built and published by GitHub Actions

You can also build the images locally.

Build Tensorflow:

```bash
docker build --build-arg NVIDIA_IMAGE=nvcr.io/nvidia/tensorflow:21.05-tf2-py3 -t ghcr.io/charitarthchugh/code-server-gpu:tensorflow-21.05-tf2-py3 -f Dockerfile.gpu .
```

Build PyTorch:

```bash
docker build --build-arg NVIDIA_IMAGE=nvcr.io/nvidia/pytorch:21.05-py3 -t ghcr.io/charitarthchugh/code-server-gpu:pytorch-21.05-py3 -f Dockerfile.gpu .
```


Test to run it locally:

```bash
docker run -it --rm -p 8081:8081 -e PASSWORD=password ghcr.io/charitarthchugh/code-server-gpu:tensorflow-21.05-tf2-py3
```

Push:

```bash
docker push ghcr.io/charitarthchugh/code-server-gpu:tensorflow-21.05-tf2-py3
```

