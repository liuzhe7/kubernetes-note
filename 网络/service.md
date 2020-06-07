# Service 是由 kube-proxy 组件，加上 iptables 来共同实现的。
kube-proxy会感知Service对象的变化，进而在宿主机上创建iptables规则，配置凡事访问service vip的流量，KUBE-SVC-NWV5X2332I4OT4T3 的 iptables 链进行处理。（service的VIP只是一个规则,ping他不会有任何反应）
KUBE-SVC-NWV5X2332I4OT4T3是一组规则集合，配置service代理的pod的访问规则实现负载均衡，
kube-proxy也会监听pod的变化更新iptables规则
# Service 是由 kube-proxy 组件，加上 ipvs 来共同实现的。
kube-proxy在宿主机创建一个虚拟网卡，IP地址是service的vip，为这个网卡设置ipvs虚拟机代理pod,这些规则的处理放到了内核态，提升了性能

# service与DNS
、ClusterIP 模式的 Service 为你提供的，就是一个 Pod 的稳定的 IP 地址，即 VIP。并且，这里 Pod 和 Service 的关系是可以通过 Label 确定的。
Headless Service 为你提供的，则是一个 Pod 的稳定的 DNS 名字，并且，这个名字是可以通过 Pod 名字和 Service 名字拼接出来的。

# 暴露到外界
所谓 Service 的访问入口，其实就是每台宿主机上由 kube-proxy 生成的 iptables 规则，以及 kube-dns 生成的 DNS 记录。而一旦离开了这个集群，这些信息对用户来说，也就自然没有作用了

## spec.type: NodePort