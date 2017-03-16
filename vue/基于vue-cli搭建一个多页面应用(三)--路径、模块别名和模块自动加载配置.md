#### 路径、模块别名的设置
在build/webpack.base.conf.js中可以看到如下的配置：
```
  ......
  resolve: {
    extensions: ['.js', '.vue', '.json'],
    alias: {
      'vue$': 'vue/dist/vue.esm.js',
      '@': resolve('src'),
    }
  }
  ......
```
这涉及到webpack的resolve的配置，可查看官方[相关API](https://webpack.js.org/configuration/resolve),其中extensions是设置自动解析扩展程序，alias是设置路径或模块的别名

在pages/index/router/index.js中有相应的使用：
```
import Hello from '@/components/Hello'
```

同理我们可进一步设置一些路径的别名：
```
    alias: {
      'vue$': 'vue/dist/vue.esm.js',
      '@': resolve('src'),
      'components': resolve('src/components'),
      'pages': resolve('src/pages'),
      'js': resolve('src/modules/js'),
      'css': resolve('src/modules/css'),
      'sass': resolve('src/modules/sass'),
      'imgs': resolve('src/modules/imgs')
    }
```

在pages/index/router/index.js中对Hello.vue的引用就可以简写为：
```
import Hello from 'components/Hello'
```

#### 模块自动化配置
在pages/index/router/index.js中，对vue和vue-router的使用都需要先加载
```
import Vue from 'vue'
import Router from 'vue-router'
Vue.use(Router)
```
可不可以自动化加载，不需要每次都手动引用呢？webpack自带的[ProvidePlugin](https://webpack.js.org/plugins/provide-plugin/)插件配置解决了这个问题。
在build/webpack.base.conf.js中添加如下配置：
```
var webpack = require('webpack')

module.exports = {
  entry: utils.entries(),
  output: {...},
  plugins: [
    new webpack.ProvidePlugin({
      Vue: 'vue',
      Resource: 'vue-resource',
      Router: 'vue-router',
      Vuex: 'vuex'
    })
  ],
  resolve: {...}
  ......
}
```
这样模块在使用时会自动加载，在pages/index/router/index.js中，可以把对vue和vue-router的引入省略掉：
```
//import Vue from 'vue'
//import Router from 'vue-router'
Vue.use(Router)
```
但是要注意：在上面设置模块别名的时候，已经设置了vue的别名，为什么模板自动设置了别名呢？还记得我们在<a href="基于vue-cli搭建一个多页面应用(一).md" target="_blank">基础结构的搭建</a>中Vue build这个步的选择Runtime + Compiler吗？vue的NPM包默认导出的是运行时构建，所以在设置了这个别名，可见vue官方文档对[独立构建vs运行时构建](https://cn.vuejs.org/v2/guide/installation.html#独立构建-vs-运行时构建)的说明
![](07.png)
默认vue模块的配置,使用了ES2015的模块化脚本，但是webpack本质上是基于node的一个工具而已，目前node还不支持ES2015的原生模块化
```
'vue$': 'vue/dist/vue.esm.js'
```
我们来对比一下vue.common.js和vue.esm.js,会发现两处差异：一个是严格模式，另一个就是模块化：
![](08.png)  
所以，如果我们不做模块自动化加载，vue模块的别名可以使用vue.esm.js，在使用中ES2015会被babel转译，但是如果使用webpack.ProvidePlugin做模块自动化加载，需要把别名修改为：
```
 resolve: {
    extensions: ['.js', '.vue', '.json'],
    alias: {
      'vue$': 'vue/dist/vue.common.js',
      '@': resolve('src'),
    }
  }
```

最后，关于node对ES2015的支持情况，可查看[node.green](http://node.green/#ES2015),其中不包含对import/export模块的支持

最后的最后，曾尝试用ES2015的语法写webpack配置文件，但是并没有找好较好的实践方案，如果你有好的实践方案请推荐一下，谢谢！