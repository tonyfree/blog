> 最近接手了一个派出所案件管理系统的重构，老系统是后台一手操办(java+jsp+ext)，由于时间紧且现在的开发团队没有前后端分离的经验，只好在原来系统限制下使用vue的组件模式

### jsp页面
``` jsp
<%@ page language="java" contentType="text/html; charset=utf-8" pageEncoding="utf-8" %>
<html>
  <head>
    <title>材料管理</title>
    <!-- 样式库和脚本库的加载 -->
    <%@ include file="/police/common/entry.jsp" %>
  </head>
  <body>
    <div id="app">
      <!-- 业务代码省略 -->

      <!-- 使用组件 -->
      <choose-police v-model="choosePoliceVisible" @police="getPolice"/>
    </div>

    <!-- 加载组件模板 -->
    <script type="text/x-template" id="choose-police">
        <%@ include file="/police/common/choosePoliceTemplate.jsp" %>
    </script>
    <!-- 加载组件脚本 -->
    <script src="police/common/choosePoliceComponent.js"></script>

    <!-- 页面逻辑 -->
    <script>
      new Vue({
        el: '#app'
      })
    </script>
```

### 样式库和脚本库的加载: entry.jsp
```jsp
<link rel="stylesheet" href="${pageContext.request.contextPath}/police/css/element-ui.index.css">
<link rel="stylesheet" href="${pageContext.request.contextPath}/police/css/custom.css">

<script src="police/js/vue.js"></script>
<script src="police/js/axios.min.js"></script>
<script src="police/js/element-ui.index.js"></script>
<script src="police/js/xlsx.full.min.js"></script>
<script src="police/js/iconfont.js"></script>
```

### 组件模板: choosePoliceTemplate.jsp
```jsp
<%@ page language="java" contentType="text/html; charset=utf-8" pageEncoding="utf-8" %>
<el-dialog title="选择民警" :visible="value" width="30%" center @close="close">
<!-- 省略业务代码 -->
</el-dialog>
```

### 组件脚本: choosePoliceComponent.js
```js
Vue.component('choose-police', {
  template: '#choose-police',
  props: {},
  data() {},
  created() {},
  methods: {}
}
```