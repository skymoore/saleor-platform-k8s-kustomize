# Saleor Platform Kubernetes Kustomize Deployment Manifests

## **Docker Images**

> See [slr.rwx.dev kustomization](environments/slr.rwx.dev/kustomization.yml) for example use of images.

### Build Images

#### saleor example
```bash
# repo setup
git clone git@github.com:saleor/saleor.git
cd saleor
git checkout $DESIRED_RELEASE_TAG
# build
docker build . \
-t $YOUR_DOCKER_USER/saleor:$DESIRED_RELEASE_TAG
```

#### saleor-storefront example
```bash
# repo setup
git clone git@github.com:saleor/react-storefront.git
cd react-storefront
git checkout $DESIRED_RELEASE_TAG
# build
docker build . \
-f Dockerfile.base \
-t $YOUR_DOCKER_USER/saleor-storefront:$DESIRED_RELEASE_TAG \
--build-arg SALEOR_API_URL=https://api.slr.rwx.dev/graphql/ \
--build-arg STOREFRONT_URL=https://store.slr.rwx.dev \
--build-arg CHECKOUT_APP_URL=https://co.slr.rwx.dev \
--build-arg CHECKOUT_STOREFRONT_URL=https://co.slr.rwx.dev/checkout-spa/ \
--build-arg CLOUD_DEPLOYMENT_URL=
```

#### saleor-dashboard example
```bash
# repo setup
git clone git@github.com:saleor/saleor-dashboard.git
cd saleor-dashboard
git checkout $DESIRED_RELEASE_TAG
# build
docker build . \
-f Dockerfile \
-t $YOUR_DOCKER_USER/saleor-dashboard:$DESIRED_RELEASE_TAG \
--build-arg API_URI=https://api.slr.rwx.dev/graphql/ \
--build-arg APP_MOUNT_URI=/ \
--build-arg STATIC_URL=/ \
--build-arg MARKETPLACE_URL=https://apps.saleor.io/ \
--build-arg SALEOR_APPS_ENDPOINT=https://apps.saleor.io/api/saleor-apps \
--build-arg IS_CLOUD_INSTANCE=false
```

## **Kubernetes Manifests**

> See [Makefile](Makefile) for full commands.

### Create Manifest

#### make:
```bash
# build yaml manifest
make build
```

### Deploy Manifest

#### make:
```bash
# preview
make dry-apply
# deploy
make apply
```
### Diff Manifest

#### make:
```bash
make diff
```
### Delete Resources

#### make:
```bash
# preview
make dry-delete
# delete
make delete
```

## **Setup**

> See [Makefile](Makefile) for full commands.

### Create SuperUser

#### make:
```bash
make superuser
```

### Update Site Domain

```bash
export TOKEN="someReallyLongTokenGoesHere"
# set the name and domain in the mutation
curl 'https://api.slr.rwx.dev/graphql/' \
    -X POST \
	-H 'Content-Type: application/json' \
	-H "authorization-bearer: $TOKEN" \
    --data-raw '{"operationName":"ShopDomainUpdate","variables":{"input":{"name":"saleor","domain":"api.slr.rwx.dev"}},"query":"mutation ShopDomainUpdate($input:SiteDomainInput!){shopDomainUpdate(input:$input){__typename}}"}'
```