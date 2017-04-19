## UI库的选择和使用

关于Vue的UI库可参考：[Vue相关开源项目库汇总](https://github.com/opendigg/awesome-github-vue)中的UI组件部分

pc端建议使用饿了么前端的 [Element](http://element.eleme.io/#/zh-CN)，后台管理界面可以参考基于Element的 [vue-manage-system](https://github.com/lin-xin/vue-manage-system)

移动端可参考 [vux](https://vux.li/#/) 和 [mint-ui](http://mint-ui.github.io/#!/en)

以Element为例说明一下使用：  
1.安装
```
//npm方式安装
npm i element-ui -S
npm install babel-plugin-component -D

//yarn方式安装
yarn add element-ui 
yarn add babel-plugin-component -D
```
2.配置.babelrc
```
{
  ......
  "plugins": ["transform-runtime",["component", [
    {
      "libraryName": "element-ui",
      "styleLibraryName": "theme-default"
    }
  ]]],
  ......
}
```
3.按需引入
```
import {pagination,card} from 'element-ui'
Vue.use(pagination)
Vue.use(card)
```

移动端如果使用阿里的flexible适配方案，现有的移动端UI库大部分都不适用了，移动端的UI库默认的initial-scale=1，在高清屏上flexible会调整initial-scale=1/dpr，UI的样式被整体缩小了
```
<meta name="viewport" content="width=device-width,initial-scale=1,user-scalable=0">
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