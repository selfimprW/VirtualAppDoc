# 注入模块

## 1.PatchManager（注入管理器）

维护全部的注入对象。

在Application对象里面初始化，被VirtualCore.startup\(Context\)所调用。

每个进程仅初始化一次 PatchManager .injectAll\(\)，添加所有注入对象。

这种对象的3种类型：PatchObject、AppInstrumentation、HCallbackHook。

## 2. PatchObject 

## 3. AppInstrumentation 

## 4. HCallbackHook 



