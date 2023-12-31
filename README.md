# flux-v1-helm-controller
Testing with flux-v1-helm-controller

# Install Flux by providing your GitHub repository URL:


```

helm repo add fluxcd https://charts.fluxcd.io

kubectl create ns fluxcd

helm upgrade -i flux fluxcd/flux --wait \
  --namespace fluxcd \
  --set registry.pollInterval=1m \
  --set git.pollInterval=1m \
  --set git.url=git@github.com:${GHUSER}/flux-v1-helm-controller \
  --set git.branch=main
  --set syncGarbageCollection.enabled=true
```


# Paste the result of command in a Deploy key under the GitHub repository (Settings > Deploy leys) [enable write]

```
FLUX_FORWARD_NAMESPACE=fluxcd fluxctl identity

```
# Install Flux Helm Operator in the fluxcd namespace:
```
helm upgrade -i helm-operator fluxcd/helm-operator --wait \
            --namespace fluxcd \
            --create-namespace \
            --set git.ssh.secretName=flux-git-deploy \
            --set git.pollInterval=1m \
            --set chartsSyncInterval=1m \
            --set helm.versions=v3
```


# Flux installs following Pods:

- flux
- flux-memcached
- helm-operator


# Uninstall

```
$ helm uninstall helm-operator -n fluxcd
$ helm uninstall flux -n fluxcd

```

# Troubleshooting

```

$ kubectl -n fluxcd logs deployment/flux -f
$ kubectl describe helmreleases.helm.fluxcd.io/ingress-nginx -n ingress-nginx

```



# References
[0] shariqmus/flux-v1-helm-controller: Testing with flux-v1-helm-controller https://github.com/shariqmus/flux-v1-helm-controller
[1] Prerequisites | GitOps Helm Workshop https://helm.workshop.flagger.dev/prerequisites/#flux
[2] fluxcd/flux: Successor: https://github.com/fluxcd/flux2 https://github.com/fluxcd/flux
[3] fluxcd/helm-operator-get-started: Managing Helm releases with Flux Helm Operator https://github.com/fluxcd/helm-operator-get-started/tree/master
