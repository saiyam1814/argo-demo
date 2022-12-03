# This is the demo to show GitOps in action using ArgoCD on Civo Kubernetes

In order to learn more about ArgoCD quickly wach my CNCFMinutes video [here](https://youtu.be/2B3qcyCcBXs).


Steps to do it on your own 

- Fork the project 
- create the project secrets 
![image](https://user-images.githubusercontent.com/8190114/152713162-6faf5632-4355-4bff-b03a-4afd102b89ef.png)
- Create Civo Kubernetes cluster 
- Install ArgoCd
- Install Crossplane
- Setup the git repository to watch for using the cli or UI 
- Make changes to the html `templates/name.html`


=====
Crossplane installation 

```
kubectl create namespace crossplane-system

helm repo add crossplane-stable https://charts.crossplane.io/stable
helm repo update

helm install crossplane --namespace crossplane-system crossplane-stable/crossplane
```

Civo Provider

```
kubectl crossplane install provider crossplane/provider-civo:main

```

```
apiVersion: v1
kind: Secret
metadata:
  namespace: crossplane-system
  name: civo-provider-secret
type: Opaque
data:
  credentials: igfkhawgflkhjasgflkhaslfjhslfgjasfgsdfghasfgafsg==
---
apiVersion: civo.crossplane.io/v1alpha1
kind: ProviderConfig
metadata:
  name: civo-provider
spec:
  region: fra1
  credentials:
    source: Secret
    secretRef:
      namespace: crossplane-system
      name: civo-provider-secret
      key: credentials

```
