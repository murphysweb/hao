<template>
  <div class="app-container tree-box">
    <div id="tree" />
    <div class="components-container board">
      <Kanban :list="list" header-text="推荐关系" />
    </div>
  </div>
</template>

<script>
// 全部引入
import Kanban from '@/components/Kanban'
var echarts = require('echarts')
export default {
  name: 'DashboardAdmin',
  components: { Kanban },
  data() {
    return {
      list: [
        { name: '推荐', id: 1 },
        { name: '推荐', id: 2 },
        { name: '推荐', id: 3 },
        { name: '推荐', id: 4 },
        { name: '推荐', id: 5 },
        { name: '推荐', id: 6 }
      ]
    }
  },
  created() {
    // 从内存中取出来首页列表参数
    this.rowData = JSON.parse(sessionStorage.getItem('hao'))
  },
  mounted() {
    this.initCharts()
  },
  methods: {
    initCharts() {
      this.chart = echarts.init(document.getElementById('tree'))
      this.setOptions()
    },
    accept() {
      console.log('111111')
    },
    mousemove() {
      console.log('222222')
    },
    setOptions() {
      this.chart.setOption({
        tooltip: { // 提示框组件
          trigger: 'item', // 触发类型，默认：item（数据项图形触发，主要在散点图，饼图等无类目轴的图表中使用）。可选：'axis'：坐标轴触发，主要在柱状图，折线图等会使用类目轴的图表中使用。'none':什么都不触发。
          triggerOn: 'mousemove' // 提示框触发的条件，默认mousemove|click（鼠标点击和移动时触发）。可选mousemove：鼠标移动时，click：鼠标点击时，none：
        },
        animationDurationUpdate: 1500,
        animationEasingUpdate: 'quinticInOut',
        series: [{
          type: 'graph',
          layout: 'none',
          symbolSize: 80,
          roam: true,
          label: {
            show: true
          },
          edgeSymbol: ['circle', 'arrow'],
          edgeSymbolSize: [4, 10],
          edgeLabel: {
            fontSize: 20
          },
          data: [{
            name: '节点1',
            x: 500,
            y: 300
          }, {
            name: '节点2',
            x: 800,
            y: 300
          }, {
            name: '节点3',
            x: 550,
            y: 100
          }, {
            name: '节点4',
            x: 550,
            y: 500
          }],
          // links: [],
          links: [{
            source: '节点1',
            target: '节点2'
          }, {
            source: '节点1',
            target: '节点3'
          }, {
            source: '节点1',
            target: '节点4'
          }],
          lineStyle: {
            opacity: 0.9,
            width: 2,
            curveness: 0
          }
        }]
      })
    }
  }
}
</script>

<style lang="scss" scoped>
#tree {
  width: 900px;
  height: 420px;
  position: absolute;
}
.tree-box{
  border:1px solid #d3d3d3;
  padding: 0;
  margin:20px;
}
.board {
  width: 200px;
  margin-left: 78%;
  display: flex;
  justify-content: space-around;
  flex-direction: row;
  align-items: flex-start;
}
</style>
