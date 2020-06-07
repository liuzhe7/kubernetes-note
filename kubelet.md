# kubelet 工作原理
kubelet 的工作核心，就是一个控制循环。kubelet 启动的时候，要做的第一件事情，就是设置 Listers，也就是注册它所关心的各种事件的 Informer。 kubelet 同时还维护着很多小循环，大循环监听到的事件交给小循环去做。
比如大循环检测到apiserver 发来了创建pod 的请求就会调用小循环HANDLEPODs，HANDLEPODS 通过cri间接的控制docker, 这个组件叫做cri shime， 他的作用就是把kubelet 发过来的请求，翻译成对docker api的请求