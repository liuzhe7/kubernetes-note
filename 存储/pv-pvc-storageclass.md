用户提交请求创建pod，Kubernetes发现这个pod声明使用了PVC，那就靠PersistentVolumeController帮它找一个PV配对。

没有现成的PV，就去找对应的StorageClass，帮它新创建一个PV，然后和PVC完成绑定。

新创建的PV，还只是一个API 对象，需要经过“两阶段处理”变成宿主机上的“持久化 Volume”才真正有用：
第一阶段由运行在master上的AttachDetachController负责，为这个PV完成 Attach 操作，为宿主机挂载远程磁盘；
第二阶段是运行在每个节点上kubelet组件的内部，把第一步attach的远程磁盘 mount 到宿主机目录。这个控制循环叫VolumeManagerReconciler，运行在独立的Goroutine，不会阻塞kubelet主循环。

完成这两步，PV对应的“持久化 Volume”就准备好了，POD可以正常启动，将“持久化 Volume”挂载在容器内指定的路径。


其实 volume 只是一个目录，至于这个 volume 是如何创建的，它的内容是什么等等，实际上是取决于所使用的 volume 类型。常见的类型包括 emptyDir、hostPath、nfs、Ceph 等，完整列表可以参考官网说明：

emptydir 临时的  hostPath 非临时的