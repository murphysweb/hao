<template>
  <el-breadcrumb class="app-breadcrumb" separator="/">
    <!-- logo -->
    <div v-if="noSidebar" style="margin-left:15px;height:50px;"><img height="90%" src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAASwAAABeCAMAAACuPtk5AAAAAXNSR0IB2cksfwAAAAlwSFlzAAALEwAACxMBAJqcGAAAAwBQTFRFAAAAa2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xra2xr4nGKcAAAAQB0Uk5TABhGc5i41Of5//vw5MiskGQ3C2Cj3v7PjEwMGvetWAFKsvzykysE1Tlb3MA2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" alt="" srcset=""></div>
    <!-- <transition-group name="breadcrumb">
      <el-breadcrumb-item v-for="(item,index) in levelList" :key="item.path">
        <span v-if="item.redirect==='noRedirect'||index==levelList.length-1" class="no-redirect">{{ item.meta.title }}</span>
        <a v-else @click.prevent="handleLink(item)">{{ item.meta.title }}</a>
      </el-breadcrumb-item>
    </transition-group> -->
  </el-breadcrumb>
</template>

<script>
import pathToRegexp from 'path-to-regexp'
import { mapState } from 'vuex'
export default {
  data() {
    return {
      levelList: null,
      hideSidebar: false
    }
  },
  computed: {
    ...mapState({
      noSidebar: state => state.app.noSidebar
    })
  },
  watch: {
    $route(route) {
      // if you go to the redirect page, do not update the breadcrumbs
      if (route.path.startsWith('/redirect/')) {
        return
      }
      this.getBreadcrumb()
    }
  },
  created() {
    this.getBreadcrumb()
    if (this.$route.name === 'Dashboard') { this.hideSidebar = true }
  },
  methods: {
    getBreadcrumb() {
      // only show routes with meta.title
      let matched = this.$route.matched.filter(item => item.meta && item.meta.title)
      const first = matched[0]

      if (!this.isDashboard(first)) {
        matched = [{ path: '/dashboard', meta: { title: 'Dashboard' }}].concat(matched)
      }

      this.levelList = matched.filter(item => item.meta && item.meta.title && item.meta.breadcrumb !== false)
    },
    isDashboard(route) {
      const name = route && route.name
      if (!name) {
        return false
      }
      return name.trim().toLocaleLowerCase() === 'Dashboard'.toLocaleLowerCase()
    },
    pathCompile(path) {
      // To solve this problem https://github.com/PanJiaChen/vue-element-admin/issues/561
      const { params } = this.$route
      var toPath = pathToRegexp.compile(path)
      return toPath(params)
    },
    handleLink(item) {
      const { redirect, path } = item
      if (redirect) {
        this.$router.push(redirect)
        return
      }
      this.$router.push(this.pathCompile(path))
    }
  }
}
</script>

<style lang="scss" scoped>
.app-breadcrumb.el-breadcrumb {
  display: inline-block;
  font-size: 14px;
  line-height: 50px;
  margin-left: 8px;

  .no-redirect {
    color: #97a8be;
    cursor: text;
  }
}
</style>
