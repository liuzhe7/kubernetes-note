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


# 服务发现
## 环境变量方式：{SVCNAME}_SERVICE_HOST=host
            {SVCNAME}_SERVICE_PORT=port， 环境变量会自动注入， 坑点 service 必须先创建
## DNS方式

# 负载均衡 clusterip
RoundRobin：轮询模式，这是默认配置。即定义 YAML 文件时不设置 spec.sessionAffinity 或设置 spec.sessionAffinity=None。它表示的是将请求分发到后端各个 Pod 上，没有固定某个 Pod 对请求进行响应。
SessionAffinity：会话保持模式，需要在定义 YAML 文件时设置 spec.sessionAffinity=ClientIP。当某个客户端第一次请求转发到后端的某个 Pod 上，那么之后这个客户端的请求也一直由相同的 Pod 进行响应。

# 负载均衡 headless service
所以 Headless Service 自定义负载均衡的实现逻辑是：通过标签选择器获取到所有符合标签的 Pod 的 IP 地址列表，然后自定义服务响应的方式。(不像clusterip一样自带负载均衡规则)

# 灰度发布 ingress-controller
canary-by-header - > canary-by-cookie - > canary-weight
## 权重模式
nginx.ingress.kubernetes.io/canary-weight：<int>    同一个路由 %x的流量会被打到开启canary-weight的ingress所代理的service

nginx.ingress.kubernetes.io/canary-by-header:   

nginx.ingress.kubernetes.io/canary-by-cookie: