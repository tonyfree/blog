###  vue2生命周期图  
<img src="./imgs/lifecycle.png" alt="" height="60%" width="60%">

### [生命周期钩子](http://cn.vuejs.org/v2/api/#选项-生命周期钩子)
+ [beforeCreate](http://cn.vuejs.org/v2/api/#beforeCreate)：在实例初始化之后
+ [created](http://cn.vuejs.org/v2/api/#created)：实例已经创建完成之后被调用，实例已完成以下的配置：数据观测(data observer)，属性和方法的运算， watch/event 事件回调
+ [beforeMount](http://cn.vuejs.org/v2/api/#beforeMount)：在挂载开始之前被调用：相关的 render 函数首次被调用
+ [mounted](http://cn.vuejs.org/v2/api/#mounted)：el 被新创建的 vm.$el 替换，并挂载到实例上去之后调用该钩子
+ [beforeUpdate](http://cn.vuejs.org/v2/api/#beforeUpdate)：数据更新时调用，发生在虚拟 DOM 重新渲染和打补丁之前
+ [updated](http://cn.vuejs.org/v2/api/#updated)：由于数据更改导致的虚拟 DOM 重新渲染和打补丁，在这之后会调用该钩子。组件 DOM 已经更新，所以你现在可以执行依赖于 DOM 的操作
+ [activated](http://cn.vuejs.org/v2/api/#activated):keep-alive 组件激活时调用
+ [deactivated](http://cn.vuejs.org/v2/api/#deactivated):keep-alive 组件停用时调用
+ [beforeDestroy](http://cn.vuejs.org/v2/api/#beforeDestroy):实例销毁之前调用。在这一步，实例仍然完全可用
+ [destroyed](http://cn.vuejs.org/v2/api/#destroyed):Vue 实例销毁后调用。调用后，Vue 实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁

### 源码：2.2.4 vue.common.js
+ L9245 模块化
```
module.exports = Vue$3;
```
+ L3788 构造函数
```
function Vue$3 (options) {
  this._init(options);
}
```

+ L3663 初始化
```
Vue.prototype._init = function (options) {
    initLifecycle(vm);
    initEvents(vm);
    initRender(vm);
    callHook(vm, 'beforeCreate');
    initInjections(vm); // resolve injections before data/props
    initState(vm);
    initProvide(vm); // resolve provide after data/props
    callHook(vm, 'created');
}
```

+ L2253 生命周期钩子
```
function callHook (vm, hook) {}
```

+ L337 生命周期钩子清单
```
var config = {
  _lifecycleHooks: [
    'beforeCreate',
    'created',
    'beforeMount',
    'mounted',
    'beforeUpdate',
    'updated',
    'beforeDestroy',
    'destroyed',
    'activated',
    'deactivated'
  ],
}  
```

### 业务逻辑的处理时机
+ created：异步数据的获取、初始化
+ mounted：挂载元素内dom节点的获取
+ nextTick：更新数据后立即操作dom
+ updated：任何数据的更新，如果要做统一的业务逻辑处理
+ watch：监听具体数据变化，并做相应的处理