# awecloud-socks

## build

```bash
# devops-go-arch
docker run -it --rm \
-w /go/src/github.com/open-beagle/awecloud-socks \
-v $PWD/:/go/src/github.com/open-beagle/awecloud-socks \
-e CI_WORKSPACE=/go/src/github.com/open-beagle/awecloud-socks \
-e PLUGIN_MAIN=src \
-e PLUGIN_BINARY=awecloud-socks \
registry.cn-qingdao.aliyuncs.com/wod/devops-go-arch:1.19-alpine

# devops-docker
docker run -it --rm \
-w /go/src/github.com/open-beagle/awecloud-socks \
-v $PWD/:/go/src/github.com/open-beagle/awecloud-socks \
-e CI_WORKSPACE=/go/src/github.com/open-beagle/awecloud-socks \
-e PLUGIN_BASE=registry.cn-qingdao.aliyuncs.com/wod/alpine:3 \
-e PLUGIN_DOCKERFILE=.beagle/dockerfile \
-e PLUGIN_REPO=wod/awecloud-socks \
-e PLUGIN_VERSION='v5.1.0' \
-e PLUGIN_ARGS='TARGETOS=linux,TARGETARCH=amd64' \
-e PLUGIN_REGISTRY=registry.cn-qingdao.aliyuncs.com \
-e REGISTRY_USER=$REGISTRY_USER \
-e REGISTRY_PASSWORD=$REGISTRY_PASSWORD \
-v /var/run/docker.sock:/var/run/docker.sock \
registry.cn-qingdao.aliyuncs.com/wod/devops-docker:1.0
```

## cache

```bash
# 构建缓存-->推送缓存至服务器
docker run --rm \
  -e PLUGIN_REBUILD=true \
  -e PLUGIN_ENDPOINT=$PLUGIN_ENDPOINT \
  -e PLUGIN_ACCESS_KEY=$PLUGIN_ACCESS_KEY \
  -e PLUGIN_SECRET_KEY=$PLUGIN_SECRET_KEY \
  -e DRONE_REPO_OWNER="open-beagle" \
  -e DRONE_REPO_NAME="awecloud-socks" \
  -e PLUGIN_MOUNT="./.git,./vendor" \
  -v $(pwd):$(pwd) \
  -w $(pwd) \
  registry.cn-qingdao.aliyuncs.com/wod/devops-s3-cache:1.0

# 读取缓存-->将缓存从服务器拉取到本地
docker run --rm \
  -e PLUGIN_RESTORE=true \
  -e PLUGIN_ENDPOINT=$PLUGIN_ENDPOINT \
  -e PLUGIN_ACCESS_KEY=$PLUGIN_ACCESS_KEY \
  -e PLUGIN_SECRET_KEY=$PLUGIN_SECRET_KEY \
  -e DRONE_REPO_OWNER="open-beagle" \
  -e DRONE_REPO_NAME="awecloud-socks" \
  -v $(pwd):$(pwd) \
  -w $(pwd) \
  registry.cn-qingdao.aliyuncs.com/wod/devops-s3-cache:1.0
```
