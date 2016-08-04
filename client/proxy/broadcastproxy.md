## Broadcast代理与隔离

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

1.添加\/删除快捷方式，替换Intent为代理Intent，同时替换icon资源

```
if (args[1] instanceof Intent) {   
   Intent intent = (Intent) args[1];  
   handleIntent(intent);
}
```

2.如果广播是指定接受者的ComponentName。需要处理。

（相当于encode，decode操作在 Hook\_RegisterReceiver 的 registerReceiver 方法）。

主要是把接受者替换为ProxyIIntentReceiver，这里面的广播响应方法做处理。

代码：

```
if (args != null && args.length > indexOfIIntentReceiver
        && IIntentReceiver.class.isInstance(args[indexOfIIntentReceiver])) {
    final IIntentReceiver old = (IIntentReceiver) args[indexOfIIntentReceiver];
    //防止重复代理    
    if(!ProxyIIntentReceiver.class.isInstance(old)) {
        IBinder token = old.asBinder();
        IIntentReceiver.Stub proxyIIntentReceiver = mProxyIIntentReceiver.get(token);
        if (proxyIIntentReceiver == null) {
            proxyIIntentReceiver = new ProxyIIntentReceiver(old);
            mProxyIIntentReceiver.put(token, proxyIIntentReceiver);
        }
        try {
            WeakReference WeakReference_mDispatcher = Reflect.on(old).get("mDispatcher");
            Object mDispatcher = WeakReference_mDispatcher.get();            //设置关系
            Reflect.on(mDispatcher).set("mIIntentReceiver", proxyIIntentReceiver);
            args[indexOfIIntentReceiver] = proxyIIntentReceiver;
        } catch (Throwable e) {
            e.printStackTrace();        
        }
    }
}
```

