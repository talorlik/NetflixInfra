# Install ArgoCD

```console
kubectl create namespace argocd
```

```console
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

```console
kubectl port-forward svc/argocd-server -n argocd 8080:443
```

```console
kubectl get secret argocd-initial-admin-secret -n argocd -o jsonpath="{.data.password}" | base64 --decode
```

In the ArgoCD UI, after logging in, click the + New App button:

Give your app the name netflix-frontend, use the project default, and change the sync policy to Automatic.
Connect your NetflixInfra repo to Argo CD by setting repository url to the github repo url and set the path to k8s/NetflixFrontend/ (the path containing your YAML manifests for the frontend service):
For Destination, set cluster URL to https://kubernetes.default.svc namespace to default (the k8s namespace in which you'll deploy your services)
After filling out the information above, click Create.

Repeat the above process for the NetflixMovieCatalog service.