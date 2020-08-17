# 常用方法

创建一个serviceAccount对象作为用户， 创建一个role作为角色， 创建一个rolebinding 将角色与用户绑定， 创建pod时指定使用这个serviceAccount 代替默认的default-serviceAccount, 这个pod就拥有了role中定义的权限

在 Kubernetes 中鉴权表示验证用来访问接口的用户名密码或者 Token 是否合法，而授权则是在鉴权完成之后，检查这些进行鉴权的用户是否拥有操作 Kubernetes 系统中的资源的权限。这个操作的权限是细分的，主要分为 create、update、delete、patch、get、list、watch。前面四个对应了写对象的权限，后面三个对应了读对象的权限。


# 鉴权
1. 创建一个serviceAccount ()
2. kubectl config set-credentials <mysa> --token <TokenOfSecret>,  token保存在这个sa 绑定的secret里,设置一个账号鉴权信息,    token 认证
3. kubectl config set-cluster k8s-learning --server https://10.192.0.2:6443 --certificate-authority /home/shiyanlou/ca.crt --embed-certs=true    ca认证

Cluster "k8s-learning" set.设置集群的访问信息
4. 创建一个 Context，把集群信息和鉴权信息绑定在一起
5. 然后切换到这个context 就是使用这个sa 访问集群了(context,可以理解为kubectl获得了用户的上下文)

# 为用户授予权限
1.  role  和role binding