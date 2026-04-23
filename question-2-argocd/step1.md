Question ArgoCD

Task
Install Argo CD in a kubernetes cluster using helm while ensuring the CRDs are not installed
(as they are pre installed)
1. Add the official Argo CD Helm repository with the name argocd (https://argoproj.github.io/argo-helm)
2. Create a namespace called argocd
3. Generate a Helm template from the Argo CD chart version 7.7.3 for the argocd namespace
4. Ensure that CRDs are not installed by configuring the chart accordingly
5. Save the generated YAML manifest to /root/argo-helm.yaml

Video link - https://youtu.be/e0YGRSjb8CU

<details><summary>Solution</summary>

```bash
Create namespace
kubectl create namespace argocd

Add repo and template manifests (CRDs not installed)
helm repo add argocd https://argoproj.github.io/argo-helm
helm repo update
helm template argocd argo/argo-cd --version 7.7.3 --set crds.install=false --namespace argocd > /root/argo-helm.yaml
cat /root/argo-helm.yaml   # confirm output
```

</details>
