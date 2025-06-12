# Completely-remove-and-clean-Kubernetes
```bash
kubeadm reset
sudo apt-get purge kubeadm kubectl kubelet kubernetes-cni kube*   
sudo apt-get autoremove  
sudo rm -rf ~/.kube
```
```bash
# 1. Obriši Calico CNI fajlove
sudo rm -f /etc/cni/net.d/10-calico.conflist \
             /etc/cni/net.d/calico-kubeconfig

# Ako više nemaš drugih CNI plug-ina, možeš obrisati i sve u tom direktorijumu:
# sudo rm -rf /etc/cni/net.d/*

# 2. Obriši Calico direktorijume sa lokalnog diska
sudo rm -rf /var/lib/calico /var/lib/cni/calico /var/run/calico

# 3. (Opcionalno) Ukloni Calico mrežne interfejse ‘cali*’
for iface in $(ip -o link show | awk -F': ' '/cali/ {print $2}'); do
  sudo ip link delete $iface
done

# 4. (Opcionalno) Očisti iptables pravila koje je Calico dodao
sudo iptables -F
sudo iptables -t nat -F
sudo iptables -t mangle -F
sudo iptables -X

# 5. Restartuj kubelet i CRI (docker/cri‐o/containerd)
sudo systemctl restart kubelet
sudo systemctl restart containerd    # ili docker/cri-o, zavisno šta koristiš

```
