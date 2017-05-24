## rap自动切换配置

### 前后端分离管理平台RAP
前后端分离的一个重要问题就是接口，可视化的接口管理工具可以极大的提高前后端的协助效率，目前开源的接口管理平台有阿里的[RAP](http://rap.taobao.org)和网易的[NEI](https://nei.netease.com/),本篇已RAP为例说明。

关于RAP的相关资料：

+ [学习视频](http://thx.github.io/RAP/study.html)
+ [用户手册](https://github.com/thx/RAP/wiki/user_manual_cn)
+ [部署手册](https://github.com/thx/RAP/wiki/deploy_manual_cn)

RAP接口只在开发环境使用，打包上线或者测试阶段使用真实接口。

RAP接口的完整url格式为：
> http://域名/mockjsdata/项目id/接口url

如果直接使用阿里的，则域名为：
> rap.taobao.org

如果公司自己部署，使用对应的域名即可

### 配置RAP
1.在项目根目录下创建项目配置文件:project.config.js
```
var rapHost = "'http://rap.taobao.org/mockjsdata/15659'"

module.exports = {
  rapHost: rapHost
}
```

2.在build文件夹下创建
host.js:
```
var rapHost = require('../project.config.js').rapHost

exports.hostConfig = function(isDev) {
  var host = isDev ? rapHost : "''"
  var tempHost = isDev ? "''" : rapHost
  var fileTxt = `module.exports = {
        host: ${host}
        // host: ${tempHost}
      }`

  require('fs').writeFile(require('path').join(__dirname, '../src/modules/js/host-config.js'), fileTxt)
}

```

api-rap.js
```
let fileTxt = `
module.exports = {
  api: true
}`

require('fs').writeFile(require('path').join(__dirname, './api-conf.js'), fileTxt)
```

api-test.js
```
let fileTxt = `
module.exports = {
  api: false
}`

require('fs').writeFile(require('path').join(__dirname, './api-conf.js'), fileTxt)
```

3.分别在build/dev-server.js和build/build.js文件中调用hostConfig，在开发或者打包的时候就自动在src/modules/js/下生成或覆盖host-config.js(注意：要确保modules/js文件夹的存在)  
build/dev-server.js
```
require('./check-versions')()
require('./host').hostConfig(require('./api-conf.js').api)
```
build/build.js
```
require('./check-versions')()
require('./host').hostConfig(false)
```

4.package.json配置
```
"scripts": {
    //开发阶段使用rap接口
    "dev": "node build/api-rap.js && node build/dev-server.js",
    //联调阶段
    "api": "node build/api-test.js && node build/dev-server.js",
    //打包
    "build": "node build/build.js",
  },
```

5.在modules文件夹下创建js文件夹，用于存放js模块  
创建fetch.js，用来封装获取异步数据的通用方法和RAP接口的url完整拼接，fetch方法可以根据后台数据结构的设计做相应调整
```
let host = require('./host-config.js').host

import axios from 'axios'

function rap(urlList) {
  let obj = {}
  Object.keys(urlList).forEach(key => {
    obj[key] = host + urlList[key]
  })
  return obj
}

function fetch(url, data = undefined) {
  return new Promise((resolve, reject) => {
    axios.post(url,data).then((res) => {
        let result = res.data
        if (result.status === 200) {
          resolve(result)
        } else if (result.status === 300) {
          // 未登录的处理
        } else {
          reject(result)
        }
      },
      (res) => {
        reject({
          status: -1,
          message: '系统错误，请稍后重试'
        })
      })
  })
}

export {
  rap,
  fetch
}

```

6.fetch和rap方法在页面或者组件中使用：
```
import { rap, fetch } from 'js/fetch.js'

let url = {
  test: '/test.do'
}
url = rap(url)

new Vue({
  el: '#app',
  created() {
    fetch(url.test).then(res => {
      // to something       
    })
  }
})

```

> 本系列文章：

1. <a href="https://github.com/tonyfree/blog/issues/1" target="_blank">基础结构的搭建</a>
2. <a href="https://github.com/tonyfree/blog/issues/2" target="_blank">postcss插件和css预编译配置</a>
3. <a href="https://github.com/tonyfree/blog/issues/3" target="_blank">路径别名和模块自动加载配置</a>
4. <a href="https://github.com/tonyfree/blog/issues/4" target="_blank">rap自动切换配置</a>
5. <a href="https://github.com/tonyfree/blog/issues/5" target="_blank">自动化部署</a>
6. <a href="https://github.com/tonyfree/blog/issues/6" target="_blank">移动端适配方案</a>
7. <a href="https://github.com/tonyfree/blog/issues/7" target="_blank">UI库的选择和使用</a>
8. <a href="https://github.com/tonyfree/blog/issues/8" target="_blank">移动调试和异常监控</a>