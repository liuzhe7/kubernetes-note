# APIServer 的第一个功能，就是过滤这个请求，并完成一些前置性的工作，比如授权、超时处理、审计等。
# 请求会进入 MUX 和 Routes 流程,找到处理这个对象的handler
# 把用户提交的 YAML 文件，转换成一个叫作 Super Version 的对象，它正是该 API 资源类型所有版本的字段全集
# APIServer 会先后进行 Admission() 和 Validation() 操作
Admissio也就是进行merge操作，拿用户提交的yaml里的定义和这个super version做合并(还有可能有Initializer，则做三方合并  )
Validation 就是校验是否合法