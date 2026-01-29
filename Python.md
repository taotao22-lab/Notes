# Python 笔记

## 1. <font color="red">exec</font>函数

- **描述**：执行字符串形式的python代码

- **语法**：```exec(object, globals, locals)```

- **参数**：

    - object：必选，Python代码，字符串

    - globals：可选，存放全局变量，如果提供，必须是字典对象

    - locals：可选，存放局部变量。如果该参数被忽略，它会取与globals相同的值

- **返回值**：永远为**None**。

- **实例**：

    ```
    code = 'result = 2 + 2'
    global_vars = {}
    exec(code, global_vars)
    result = global_vars['result']
    print(result)
    ```

    在这个例子中，**exec**执行字符串‘**result=2+2**’，并将结果存储在**global_vars**字典中的'result'键中。

