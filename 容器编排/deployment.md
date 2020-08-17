kubectl scale deployment <deployment name> --replicas=4  扩缩容pod

kubectl create -f <nginx-deployment.yaml> --record    记录对deployment的操作生成版本号，可用于回滚操作

kubectl rollout status deployment/<deployment name>  查看更新过程

kubectl rollout undo deployment/<deployment name> 回滚到上一版本
kubectl rollout undo deployment/<deployment name> --to-revision=2 回滚到2版本

kubectl rollout history deployment/<deployment name>  查看历史版本

kubectl rollout history deployment/<deployment name> --revision=2  查看版本号为2的 deployment的详细信息

kubectl rollout undo deployment/<deployment name> --to-revision=2

kubectl rollout pause deployment/<deployment name> 暂停deployment, 不会滚动也不会创建新的rs
kubectl rollout resume deploy/<deployment name> 从暂停中恢复，暂停期间的所有操作会触发一滚动更新


状态:AVAILABLE才是最终的期望状态
    DESIRED：用户期望的 Pod 副本个数（spec.replicas 的值）；
    CURRENT：当前处于 Running 状态的 Pod 的个数；
    UP-TO-DATE：当前处于最新版本的 Pod 的个数，所谓最新版本指的是 Pod 的 Spec 部分与 Deployment 里 Pod 模板里定义的完全一致；
    AVAILABLE：当前已经可用的 Pod 的个数，即：既是 Running 状态，又是最新版本，并且已经处于 Ready（健康检查正确）状态的 Pod 的个数。

滚动更新，多重更新，目标版本还没创建完时 修改deployment 会直接去创建新的版本，而不是等原来的版本创建完再去创建新的版本


添加标签选择器后，旧的pod无法匹配到导致失去管理



# 简易灰度发布
kubectl rollout pause deployments deployment-demo 暂停更新
kubectl rollout resume deployments deployment-demo 继续更新