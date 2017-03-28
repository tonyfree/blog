## 移动调试和异常监控

#### 移动调试

移动端调试开发环境可以用chrome模拟设备，如果是微信公众号的开发可以使用[微信web开发者工具](https://mp.weixin.qq.com/debug/wxadoc/dev/devtools/download.html)

相关的调试方法可参考下面这两篇文章：[移动端前端开发调试](http://yujiangshui.com/multidevice-frontend-debug/)、[各种真机远程调试方法汇总](https://github.com/jieyou/remote_inspect_web_on_real_device)

大家常用的真机调试方式，以安卓机为例：数据线+chrome://inspect 或者 Weinre 

这里向推荐：WechatFE出品的 [vConsole](https://github.com/WechatFE/vConsole)，同类型的产品还有 [eruda](https://github.com/liriliri/eruda)

vConsole的 [演示地址](http://wechatfe.github.io/vconsole/demo.html)

手机预览  
![](imgs/qrcode.png)


#### 异常监控
采用 [sentry](https://sentry.io) 系统平台，需要创建Team和Project
查看官方文档，找到 [集成vue](https://docs.sentry.io/clients/javascript/integrations/vue/) 的文档，登录以后就可以选择相应的Team/Project，会自动生成一个配置地址和对应的代码，复制到项目下即可
![](imgs/sentry.png)

如果要收集额外的异常，可如下处理：
```
import Raven from 'raven-js'

Raven.captureException(new Error('some message'))
```

异常列表：  
![](imgs/sentrylist.png)

异常详情：  
![](imgs/sentrydetail.png)

更多使用请参考 [官方文档](https://docs.sentry.io/) 