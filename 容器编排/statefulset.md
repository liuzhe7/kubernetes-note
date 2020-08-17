StatefulSet 其实就是一种特殊的 Deployment，而其独特之处在于，它的每个 Pod 都被编号了。
而且，这个编号会体现在 Pod 的名字和 hostname 等标识信息上，这不仅代表了 Pod 的创建顺序，也是 Pod 的重要网络标识（即：在整个集群里唯一的、可被访问的身份）。
有了这个编号后，StatefulSet 就使用 Kubernetes 里的两个标准功能：Headless Service 和 PV/PVC，实现了对 Pod 的拓扑状态和存储状态的维护。



稳定的持久化存储 Pod 在重新调度之后还能继续访问之前的存储数据
稳定的网络标志 Pod 在重新调度后其名字和主机名保持不变
有序部署：构成应用的多个 Pod 启动顺序是固定的，适合 Pod 之间有依赖关系的情况
有序删除：删除应用时 Pod 销毁顺序跟启动顺序正好相反

RolingUpdate：默认策略，更新 StatefulSet 模板后，自动删除旧的 Pod 并创建新的 Pod，并且更新顺序与序号索引相反
OnDelete：更新 StatefulSet 模板后，只有手动删除了旧的 Pod 才会创建新的 Pod