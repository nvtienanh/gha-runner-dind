[Enhance ARC runner image dockerfiles for RunnerScaleSet compatibility](https://github.com/actions/actions-runner-controller/pull/2616)

# Whatever version you want to use for this
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
# Add `--load ` or `--push` if you're using buildx!