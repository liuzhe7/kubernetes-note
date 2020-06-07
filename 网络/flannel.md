# flannel网络插件原理
flannel 会划分子网，保证每个宿主机上的docker0的ip网段不会重复，从而形成OVERLAY的基础
<br>
Flannel 项目是 CoreOS 公司主推的容器网络方案。事实上，Flannel 项目本身只是一个框架，真正为我们提供容器网络功能的，是 Flannel 的后端实现。目前，Flannel 支持三种后端实现，分别是：VXLAN, HOST-GW, UDP
## VXLAN实现原理
1.每一台宿主机都有flannel.1网络设备，和flanneld守护进程,并且flannel.1的ip地址充当他所在宿主机的docker0的网关（kubernetes里用cni0代替了docker0）
2.docker0网桥发现目的地址并不在自己的网段，会将网络包转发给flannel.1网络设备， 路由规则有flanneld配置.
3.flannel.1收到网络包后会根据目容器的ip地址计算出目的flannel.1的ip地址,然后通过arp找到目的flannel.1的mac地址，arp由flanneld维护。
4.封装一个内部数据帧，从外到内依次是VNI，目的flannel.1的mac地址，目的容器的ip地址,其中VNI是VXLAN的标志，目的flannel.1必须要收到这个数据才能接受数据包
5.内核将这个内部数据帧封装成UPD数据包准备发送，通过内核FDB数据库查询到目的flannel.1的mac地址与目的宿主机ip地址的对应关系，拿到目的宿主机的ip, 然后就是一个普通的二层网络之间的数据包传输了
## host-gw,即把宿主机当网关了
1.flanneld配置路由信息由c1 发送到c2的数据包应由本机的eth0发送并且他的下一跳是目标pod所在的宿主机的eth0，因此这种模式要求宿主机必须在同一个二层网络

# Calico与 host-gw类似,是一个三层网络方案

## 普通模式
除了没有使用网桥，和引入BGP协议而不是flanneld管理路由信息，其他和host-gw一样, 仅支持宿主机在同一个二层网络

## IPIP模式
通过ip层进行一层封包，把容器到node2的网络包改为node1到node2的网络包， 从而就可以利用原有的路由协议一步一步跳到node2,然后再被calico拆包解析出要发到哪个容器,所以支持宿主机在不同的二层网络