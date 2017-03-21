# read source code(rsc) of dcos-cli

1. 在函数里面定义函数，有什么优缺点和使用场景？
2. 有些类里面有一个函数实现非常一般的功能，然后有很多“代理”函数通过调用“一般函数”和参数控制来获取个性化结果。
    - dcos/mesos.py  `_framework_dicts`
    - dcos/mesos.py `tasks`
3. docstring用法