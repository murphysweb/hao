<template>
  <div class="components-container">
    <aside><strong>结果报告</strong></aside>
    <el-tabs type="border-card">
      <el-tab-pane label="信息" />
      <el-tab-pane label="图表" />
    </el-tabs>
  </div>
</template>

<script>
export default {
  name: 'Zixun',
  components: { },
  data() {
    return {
    }
  },
  methods: {
    resize() {
      console.log('resize')
    }
  }
}
</script>

<style lang="scss" scoped>
  .components-container {
    position: relative;
    margin: 10px 20px 0;
    height: 200vh;
  }
</style>
