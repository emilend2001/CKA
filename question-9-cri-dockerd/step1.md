Question: Cri-Dockerd

Task:
Set up cri-dockerd
Install the debian package ~/cri-dockerd.deb using dpkg
Enable and start the cri-docker service
Configure these parameters:
1. Set net.bridge.bridge-nf-call-iptables to 1
2. Set net.ipv6.conf.all.forwarding to 1
3. Set net.ipv4.ip_forward to 1
4. Set net.netfilter.nf_conntrack_max to 131072

Video Link - https://youtu.be/ybzo1vXiqjU

<details><summary>Solution</summary>

```bash
Install and run cri-dockerd
sudo dpkg -i cri-dockerd.deb
sudo systemctl enable --now cri-docker.service
sudo systemctl status cri-docker.service

Set sysctl (make persistent)
sudo tee /etc/sysctl.d/kube.conf >/dev/null <<'EOF'
net.bridge.bridge-nf-call-iptables=1
net.ipv6.conf.all.forwarding=1
net.ipv4.ip_forward=1
net.netfilter.nf_conntrack_max=131072
EOF
sudo sysctl --system
```

</details>
