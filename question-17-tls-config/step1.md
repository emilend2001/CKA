Question:
There is an existing deployment in the nginx-static namespace. The deployment contains a ConfigMap that supports
TLSv1.2 and TLSv1.3 as well as a Secret for TLS.

There is a service called nginx-service in the nginx-static namespace that is currently exposing the deployment.

Task:
1. Configure the ConfigMap to only support TLSv1.3
2. Add the IP address of the service in /etc/hosts and name it ckaquestion.k8s.local
3. Verify everything is working using the following commands
    curl -vk --tls-max 1.2 https://ckaquestion.k8s.local # should fail
    curl -vk --tlsv1.3 https://ckaquestion.k8s.local # should work

Video Link

<details><summary>Solution</summary>

```bash
Step one
We want to edit the config map to remove all references to tls v1.2
k edit cm -n nginx-static nginx-config # remove TLSv1.2 from SSL protocols (remove from last applied configuration for safety)

Step 2
We need to get the IP of the service
k get svc -n nginx-static
We need to add this IP with the host name to /etc/hosts
sudo echo 'x.x.x.x ckaquestion.k8s.local' >> /etc/hosts
Check the hosts file has been updated the IP and host should be added to the bottom of the file
sudo cat /etc/hosts

Step 3
If we run the check commands now we see v1.2 is still working, this is because we need to restart the deployment to use the new CM config
k rollout restart -n nginx-static deployment nginx-static
Test the commands again and the v1.2 should no longer work```

</details>
