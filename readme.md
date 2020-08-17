个人谷歌云上的一些API对象

启动脚本退出，容器就退出
pod挂掉就会直接销毁， 里面的数据也就没了, docker有stop   emptydir 临时的  hostPath 非临时的
负载均衡模式 日志打散， 轮询模式负载均衡 + 
先创建service 后创建pod 否则无法环境变量的去服务发现
修改deppodeployment selector  matchlabel 同时修改pod的labels , 原pod会不受控制
