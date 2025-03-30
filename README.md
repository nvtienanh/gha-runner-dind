# Overview

This docker image based on PR: [Enhance ARC runner image dockerfiles for RunnerScaleSet compatibility](https://github.com/actions/actions-runner-controller/pull/2616) to make GitHub ARC run dind mode directly, without using sidecar container.

# Build docker image
```bash
RUNNER_ASSETS_DIR=/home/runner
TARGETPLATFORM=linux/amd64
RUNNER_VERSION=2.323.0
RUNNER_CONTAINER_HOOKS_VERSION=0.6.2
DOCKER_VERSION=25.0.5
BUILDX_VERSION=v0.14.1

docker build \
  --build-arg RUNNER_ASSETS_DIR=$RUNNER_ASSETS_DIR \
  --build-arg TARGETPLATFORM=$TARGETPLATFORM \
  --build-arg RUNNER_VERSION=$RUNNER_VERSION \
  --build-arg RUNNER_CONTAINER_HOOKS_VERSION=$RUNNER_CONTAINER_HOOKS_VERSION \
  --build-arg DOCKER_VERSION=$DOCKER_VERSION \
  --build-arg BUILDX_VERSION=$BUILDX_VERSION \
  -t nvtienanh/actions-runner:dind-$RUNNER_VERSION \
  .
```

# Deploy on Kubernetes

Please do following offical document: [Deploying runner scale sets with Actions Runner Controller](https://docs.github.com/en/actions/hosting-your-own-runners/managing-self-hosted-runners-with-actions-runner-controller/deploying-runner-scale-sets-with-actions-runner-controller)

And [Updating the pod specification for the runner pod](https://docs.github.com/en/actions/hosting-your-own-runners/managing-self-hosted-runners-with-actions-runner-controller/deploying-runner-scale-sets-with-actions-runner-controller#updating-the-pod-specification-for-the-runner-pod) following yaml:

```yaml
...
  template:
    spec:
      containers:
        - name: runner
          image: ghcr.io/nvtienanh/gha-runner-dind:2.323.0
          securityContext:
            privileged: true
          resources:
            requests:
              memory: 192Mi
              cpu: 150m
            limits:
              memory: 384Mi
              cpu: 375m
          volumeMounts:
            - name: hostedtoolcache
              mountPath: /opt/hostedtoolcache
      volumes:
        - name: hostedtoolcache
          persistentVolumeClaim:
            claimName: hostedtoolcache-pvc
...
```