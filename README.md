# flux-v1-helm-controller
Testing with flux-v1-helm-controller


#########kubectl create secret generic flux-git-deploy --from-file=identity=./id_rsa -n flux --dry-run=client -o yaml | kubectl apply -f -

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

# Troubleshooting

```

kubectl -n fluxcd logs deployment/flux -f

k describe helmreleases.helm.fluxcd.io/ingress-nginx -n fluxcd

```

