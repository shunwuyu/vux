<template>
  <div style="height:100%" id="app">
    <loading position:"absolute" :show="isLoading"></loading>
    <view-box v-ref:view-box>
      <div class="vux-demo-header-box" slot="header">
          <x-header :left-options="leftOptions" :transition="headerTransition" :title="title" @on-click-title="scrollTop">
          </x-header>
      </div>
      <router-view :transition="'vux-pop-' + (direction === 'forward'?'in':'out')"></router-view>
      <tabbar class="vux-demo-tabbar" icon-class="vux-center" v-show="!isTabbarDemo" slot="bottom">
        <tabbar-item v-link="{path:'/'}" :selected="route.path === '/'">
          <span class="demo-icon-22 vux-demo-tabbar-icon-home" slot="icon">&#xe637;</span>
          <span slot="label">Home</span>
        </tabbar-item>
        <tabbar-item v-link="{path:'/demo'}" :selected="isDemo" badge="9">
          <span class="demo-icon-22" slot="icon">&#xe633;</span>
          <span slot="label"><span v-if="componentName" class="vux-demo-tabbar-component">{{componentName}}</span><span v-else>Demos</span></span>
        </tabbar-item>
        <tabbar-item v-link="{path:'/project/donate'}" :selected="route.path === '/project/donate'" show-dot>
          <span class="demo-icon-22" slot="icon">&#xe630;</span>
          <span slot="label">Donate</span>
        </tabbar-item>
      </tabbar>
    </view-box>
  </div>
</template>

<script>
import store from './vuex/store'
import { Tabbar, TabbarItem, Loading, ViewBox, XHeader } from 'vux/src/components/'
export default {
  components: {
    Tabbar,
    TabbarItem,
    Loading,
    ViewBox,
    XHeader
  },
  store: store,
  vuex: {
    getters: {
      route: (state) => state.route,
      isLoading: (state) => state.isLoading,
      direction: (state) => state.direction
    }
  },
  data () {
    return {
      routerTransition: {
        forward: 'slideRL',
        back: 'slideLR'
      }
    }
  },
  methods: {
    scrollTop () {
      this.$refs.viewBox.$els.viewBoxBody.scrollTop = 0
    }
  },
  computed: {
    leftOptions () {
      return {
        showBack: this.route.path !== '/'
      }
    },
    headerTransition () {
      return this.direction === 'forward' ? 'vux-header-fade-in-right' : 'vux-header-fade-in-left'
    },
    componentName () {
      const parts = this.route.path.split('/')
      if (/component/.test(this.route.path) && parts[2]) return parts[2]
    },
    isDemo () {
      return /component|demo/.test(this.route.path)
    },
    isTabbarDemo () {
      return /tabbar/.test(this.route.path)
    },
    title () {
      if (this.route.path === '/') return 'Home'
      if (this.route.path === '/project/donate') return 'Donate'
      if (this.route.path === '/demo') return 'Demo list'
      return this.componentName ? `Demo/${this.componentName}` : 'Demo/~~'
    }
  }
}
</script>

<style>

</style>
