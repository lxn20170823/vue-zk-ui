<template>
  <el-container class="app-layout app-layout-default" :style="{ height: `${windowHeight}px` }">
    <el-aside class="aside" :width="showCollapse ? '64px' : '200px'">
      <el-menu
        class="aside-menu"
        :collapse="showCollapse"
        :default-active="$route.path"
        :default-openeds="nav.map(navItem => navItem.index)"
        router>
        <template v-for="navItem in nav">
          <el-submenu v-if="navItem.childers && navItem.childers.length" :index="navItem.index" :key="navItem.index" class="submenu">
            <template slot="title">
              <i :class="['aside-menu-icon', navItem.icon]" />
              <span>{{ navItem.label }}</span>
            </template>
            <el-menu-item v-for="childItem in navItem.childers" :index="childItem.index" :key="childItem.index">
              <template slot="title">
                <span>{{ childItem.label }}</span>
              </template>
            </el-menu-item>
          </el-submenu>
          <el-menu-item v-else :index="navItem.index" :key="navItem.index">
            <i :class="['aside-menu-icon', navItem.icon]" />
            <span>{{ navItem.label }}</span>
          </el-menu-item>
        </template>
      </el-menu>
    </el-aside>
    <el-container>
      <el-main class="main">
        <router-view class="main-view" />
      </el-main>
    </el-container>
  </el-container>
</template>

<script>
import { createNamespacedHelpers } from 'vuex'
const { mapState: appState, mapMutations: appMutations } = createNamespacedHelpers('app')

export default {
  name: 'LayoutPanel',
  computed: {
    ...appState(['title', 'windowWidth', 'windowHeight', 'nav', 'collapse']),
    showCollapse () {
      return this.windowWidth <= 1024 || !this.collapse
    }
  },
  provide: function () {
    return {
      $App: this
    }
  },
  watch: {
    title: {
      immediate: true,
      handler (title) {
        window.document.title = title
      }
    }
  },
  methods: {
    ...appMutations(['setCollapse', 'setWindowWidth', 'setWindowHeight']),
    updateWindowHeight () {
      this.setWindowWidth(document.documentElement.clientWidth)
      this.setWindowHeight(document.documentElement.clientHeight)
    }
  },
  created () {
    this.updateWindowHeight()
    window.onresize = this.updateWindowHeight
  }
}
</script>

<style lang="scss">
::-webkit-scrollbar {
  width: 6px;
  height: 6px;
  &-track-piece {
    background: #eee;
  }
  &-thumb {
    background-color: #A5A5A5;
  }
  &-thumb:hover {
    background-color: #bbb;
  }
}
</style>

<style lang="scss">
body {
  margin: 0 !important;
  overflow: auto !important;
}
body {
  -webkit-tap-highlight-color: rgba(255, 255, 255, 0);
  -webkit-tap-highlight-color: transparent;
  -webkit-appearance: none;
}
</style>

<style lang="scss" scoped>
.header {
  padding: 0;
  overflow: hidden;
}
.header-menu {
  border: none;
  .menu-block {
    float: left;
    height: 60px;
    line-height: 60px;
    margin: 0;
    border-bottom: 2px solid transparent;
    color: #fff;
    border-bottom-color: transparent;
    font-size: 14px;
    padding: 0 20px;
    cursor: pointer;
    background-color: rgb(60, 68, 77);
    outline: none;
    &.menu-block-a {
      padding: 0;
      a {
        display: inline-block;
        height: 100%;
        padding: 0 20px;
        color: #fff;
        text-decoration: none;
        &:hover {
          background-color: #303440;
        }
      }
    }
  }
  .menu-block-right {
    float: right;
  }
  .login-link {
    color: #ffd04b;
    text-decoration: none;
    &:hover {
      color: #ffb64c;
    }
  }
}
.aside {
  background-color: #f5f7f8;
  transition: width .4s;
}
.aside-menu {
  background-color: #f5f7f8;
  transition: width .4s;
  border: none;
  .aside-menu-icon {
    margin-right: 4px;
  }
}
.main {
  padding: 0;
  position: relative;
  height: 100%;
  .main-view {
    padding: 20px;
    box-sizing: border-box;
  }
}
</style>
