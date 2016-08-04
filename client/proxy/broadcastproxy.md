## Broadcast代理与隔离

### 1.代理

接收权限：

在Hook\_RegisterReceiver.java， am 的registerReceiver方法，给每一个 Receiver 添加权限。

```
if (args != null && args.length > indexOfRequiredPermission 
       && args[indexOfRequiredPermission] instanceof String) {
    args[indexOfRequiredPermission] = VirtualCore.getPermissionBroadcast();
}
```

广播过滤：

在Hook\_BroadcastIntent.java，am 的broadcastIntent方法，给广播添加权限。

```
Class<?> permissionType = method.getParameterTypes()[7];
if (permissionType == String.class) { 
   args[7] = VirtualCore.getPermissionBroadcast();
} else if (permissionType == String[].class) {
   args[7] = new String[]{VirtualCore.getPermissionBroadcast()};
} else {   
   VLog.e(TAG, "replace permission failed.");
}
```

处理广播：

1.添加\/删除快捷方式，替换Intent为代理Intent

```
if (args[1] instanceof Intent) {   
   Intent intent = (Intent) args[1];  
   handleIntent(intent);
}
```



