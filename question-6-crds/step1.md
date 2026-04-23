Question CRDs

Task
1. Create a list of all cert-manager [CRDs] and save it to /root/resources.yaml
2. Using kubectl extract the documentation for the subject specification field of the Certifciate
Custom Resource and save it to /root/subject.yaml
You may use any output format that kubectl supports

Video Link - https://youtu.be/SA1DzLQaDJs

<details><summary>Solution</summary>

```bash
List cert-manager CRDs and save
kubectl get crd | grep cert-manager | tee /root/resources.yaml

Save spec subject explain output
kubectl explain certificate.spec.subject | tee /root/subject.yaml
```

</details>
