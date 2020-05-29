# gitops-profiles

Welcome to the gitops-profiles plugin!
This plugin is for creating GitOps-managed Kubernetes clusters. Currently, it supports provisioning EKS clusters on GitHub via GitHub Actions.

_This plugin was created through the Backstage CLI_

## Plugin Development

Your plugin has been added to the example app in this repository, meaning you'll be able to access it by running `yarn start` in the root directory, and then navigating to [/gitops-clusters](http://localhost:3000/gitops-profiles).

You can also serve the plugin in isolation by running `yarn start` in the plugin directory.
This method of serving the plugin provides quicker iteration speed and a faster startup and hot reloads.
It is only meant for local development, and the setup for it can be found inside the [/dev](/dev) directory.

## Deployment

Please follow these steps to build Docker images for deployment.

At Backstage directory,

- Replace Backstage's NGINX config with this plugin's config

```bash
  $ cp plugins/gitops-profiles/deploy/default.conf.template docker/default.conf.template
```

- Create Backstage images with

```bash
  $ yarn docker-build
  $ export REPO=<your_docker_id>
  $ docker tag spotify/backstage $REPO/gitops-app-backstage
  $ docker push $REPO/gitops-app-backstage
```

- Change container image inside `plugins/gitops-profiles/deploy/gitops-app-manifest.yaml` to point to your image abd deploy to Kubernetes with the follow command.

```bash
  $ sed "s/SPOTIFY_BACKSTAGE_IMAGE/$REPO\/gitops-app-backstage:latest/" plugins/gitops-profiles/deploy/gitops-app-manifest.yaml \
    | kubectl apply -f -
```
