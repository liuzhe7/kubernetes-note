# 调度器的核心就是两个控制循环
1.第一个控制循环，我们可以称之为 Informer Path。它的主要目的，是启动一系列 Informer，用来监听（Watch apiserver对api对象的操作,间接的监听etcd中的变化）Etcd 中 Pod、Node、Service 等与调度相关的 API 对象的变化。比如，当一个待调度 Pod（即：它的 nodeName 字段是空的）被创建出来之后，调度器就会通过 Pod Informer 的 Handler，将这个待调度 Pod 添加进调度队列
2.第二个控制循环，是调度器负责 Pod 调度的主循环，我们可以称之为 Scheduling Path。从队列中取出一个pod 然后计算出所有可以部署这个pod的node， 然后再给每个node打分，然后调度到得分最高的node上


串行调度， 保证资源的正确计算