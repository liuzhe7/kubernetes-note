# kubelet 工作原理
kubelet 的工作核心，就是一个控制循环。kubelet 启动的时候，要做的第一件事情，就是设置 Listers，也就是注册它所关心的各种事件的 Informer。 kubelet 同时还维护着很多小循环，大循环监听到的事件交给小循环去做。
比如大循环检测到apiserver 发来了创建pod 的请求就会调用小循环HANDLEPODs，HANDLEPODS 通过cri间接的控制docker, 这个组件叫做cri shime， 他的作用就是把kubelet 发过来的请求，翻译成对docker api的请求

pod 管理：kubelet 定期从 Api Server 接口获取 Node 上的 pod 或是 container 的期望状态（比如使用什么镜像创建容器、运行的 pod 副本数量、如何配置网络以及存储），并调用容器平台接口达到这个状态。
容器健康检查：kubelet 创建容器后需要检查容器是否正常运行，如果没有正常运行会根据 pod 重启策略进行处理。
容器监控：通过 cAdvisor 监控所在 Node 的资源使用情况并定时向 Master 汇报，便于 Master 了解 Node 的整体情况。