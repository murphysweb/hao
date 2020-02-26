<template>
  <div class="app-container">
    股价管理
  </div>
</template>

<script>
export default {
  name: 'Gujia',
  components: {
  },
  data() {
    return {
      rowData: null
    }
  },
  created() {
    this.rowData = JSON.parse(sessionStorage.getItem('hao'))
  },
  methods: {

  }
}
</script>

<style lang="scss" scoped>

</style>
