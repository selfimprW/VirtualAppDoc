# 注入模块

## 1.注入管理器（ PatchManager ）

```
路径：com.lody.virtual.client.core.PatchManager

作用：维护全部的注入对象。

说明：
     在Application对象里面初始化，被VirtualCore.startup\(Context\)所调用。
     每个进程仅初始化一次 PatchManager .injectAll\(\)，添加所有注入对象。
     这种对象的3种类型：PatchObject、AppInstrumentation、HCallbackHook。
```

