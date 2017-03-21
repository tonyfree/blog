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

2.在build文件夹下创建rap-host.js文件
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

3.分别在build/dev-server.js和build/build.js文件中调用hostConfig，在开发或者打包的时候就自动在src/modules/js/下生成或覆盖host-config.js(注意：要确保modules/js文件夹的存在)  
build/dev-server.js
```
require('./check-versions')()
require('./host').hostConfig(true)
```
build/build.js
```
require('./check-versions')()
require('./host').hostConfig(false)
```

4.在modules文件夹下创建js文件夹，用于存放js模块  
创建fetch.js，用来封装获取异步数据的通用方法和RAP接口的url完整拼接，fetch方法可以根据后台数据结构的设计做相应调整
```
let host = require('./host-config.js').host

Vue.use(Resource)

function rap(urlList) {
  let obj = {}
  Object.keys(urlList).forEach(key => {
    obj[key] = host + urlList[key]
  })
  return obj
}

function fetch(url, data = undefined) {
  return new Promise((resolve, reject) => {
    Vue.http({
      url,
      method: 'post',
      params: data
    }).then((res) => {
        let result = res.data
        if (typeof(result) === "string") {
          result = JSON.parse(result)
        }
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

5.在页面或者组件中使用：
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

+ <a href="multi-page-app-01.md" target="_blank">基础结构的搭建</a>
+ <a href="multi-page-app-02.md" target="_blank">postcss插件和css预编译配置</a>
+ <a href="multi-page-app-03.md" target="_blank">路径别名和模块自动加载配置</a>
+ <a href="multi-page-app-05.md" target="_blank">自动化部署</a>
+ <a href="" target="_blank">移动端适配方案</a>
+ <a href="" target="_blank">UI库的选择和使用</a>