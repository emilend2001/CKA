Question Ingress

Task
1. Expose the existing deployment with a service called echo-service
using Service Port 8080 type=NodePort
2. Create a new ingress resource named echo in the echo-sound namespace for http://example.org/echo
3. The availability of the Service echo-service can be checked using the following command
curl NODEIP:NODEPORT/echo

In the exam it may give you a command like curl -o /dev/null -s -w "%{http_code}\n" http://example.org/echo
This requires an ingress controller, to get this to work ensure your /etc/hosts file has an entry for your NodeIP
pointing to example.org

Video Link - https://youtu.be/sy9zABvDedQ

<details><summary>Solution</summary>

```bash
Expose deployment as NodePort
kubectl expose deployment echo -n echo-sound --name echo-service --type NodePort --port 8080 --target-port 8080
kubectl get svc -n echo-sound echo-service

Create ingress
cat <<'EOF' > ingress.yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: echo
  namespace: echo-sound
spec:
  rules:
  - host: example.org
    http:
      paths:
      - path: /echo
        pathType: Prefix
        backend:
          service:
            name: echo-service
            port:
              number: 8080
EOF
kubectl apply -f ingress.yaml
kubectl get ingress -n echo-sound

Optional test (NodePort): curl http://<nodeIP>:<nodePort>/echo
```

</details>
