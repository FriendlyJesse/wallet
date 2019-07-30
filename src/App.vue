<template>
  <div id="app">
    <el-menu :default-active="activeIndex" mode="horizontal" router>
      <el-menu-item v-for="(item, key) in routers" :key="key" :index="item.path">{{item.name}}</el-menu-item>
    </el-menu>
    <router-view />
  </div>
</template>

<script>

export default {
  name: 'app',
  data() {
    return {
      activeIndex: '/',
      routers: [
        {
          path: '/',
          name: '首页'
        },
        {
          path: '/transfer',
          name: '转账'
        },
        {
          path: '/contact',
          name: '联系人'
        },
        {
          path: '/setting',
          name: '设置'
        }
      ]
    }
  }
}
</script>

<style>
body {
  margin: 0;
  background: #ececec;
}
.el-menu {
  border-bottom: none !important;
}
</style>
