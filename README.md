# Install ArgoCD

```console
kubectl create namespace argocd
```

```console
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

```console
kubectl port-forward svc/argocd-server -n argocd 8082:443
```

```console
kubectl get secret argocd-initial-admin-secret -n argocd -o jsonpath="{.data.password}" | base64 --decode
```

In the ArgoCD UI, after logging in, click the + New App button:

1. Give your app the name netflix-frontend, use the project default, and change the sync policy to Automatic.
2. Connect your NetflixInfra repo to Argo CD by setting repository url to the github repo url and set the path to k8s/NetflixFrontend/ (the path containing your YAML manifests for the frontend service):
3. For Destination, set cluster URL to https://kubernetes.default.svc namespace to default (the k8s namespace in which you'll deploy your services)
4. After filling out the information above, click Create.

Repeat the above process for the NetflixMovieCatalog service.