## Notification

插件框架，最头疼就是自定义样式的通知栏。因为资源以包名和id的形式传给com.android.systemui。

自定义通知栏：通过id构建个View，然后RemoteViews.set\*方法去设置内容，然后seOnClickPendIntent（点击事件）。

通过id构建个View，这个通过Remove





如果去替换Notification？

hook通知栏服务的3个添加通知栏方法：

enqueueNotification，enqueueNotificationWithTag，enqueueNotificationWithTagPriority。

1.hook惯例，替换插件包名为宿主包名。

2.处理Notification：主要方法replaceNotification，先克隆一个通知栏NotificaitionUtils.clone（通知栏有一个Notification.cloneInto克隆方法，但是里面的extras则是处理过后的，导致部分rom的通知栏icon显示不正确）。

处理通知栏的2个RemoteViews（bigContentViews，contentViews），先通过RemoteViewsCompat去查找这2个view。

绘制过程在RemoteViewsUtils._getInstance_\(\).createViews，我们的代理通知栏采用自定义样式的通知栏（准备了2个，一个是子view的点击事件，另一个是仅有默认点击事件，把差距的通知栏绘制成bitmap，然后设置到我们的代理通知栏的ImageView上面。

子view的点击事件处理：pendIntentCompat.setPendIntent\(\)方法。通过在通知栏上面添加一堆NxM的TextView，然后根据原始的子View所占区域，去给TextView添加点击事件。

















## AppWidget

