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