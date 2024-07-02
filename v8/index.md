### V8引擎源码学习

##### 前言
```bash
# 项目地址
https://github.com/v8/v8

# 重点内容位置
${PROJECT_ROOT}/src/
```

##### 重点内容导读索引

- [api/](api/) - V8 的公开 API
- [builtins/](builtins/) - 内置函数实现
- [codegen/](codegen/) - 代码生成，包括低级代码生成和汇编器
- [compiler/](compiler/) - 编译器，包括字节码生成和编译器优化
- [debug/](debug/) - 调试器相关代码
- [execution/](execution/) - 执行相关代码，包括堆栈和上下文管理
- [heap/](heap/) - 垃圾回收和堆管理
- [init/](init/) - 初始化相关代码
- [objects/](objects/) - JavaScript 对象和内置类型实现
- [runtime/](runtime/) - 运行时库函数
- [snapshot/](snapshot/) - 快照生成和恢复
- [wasm/](wasm/) - WebAssembly 支持代码