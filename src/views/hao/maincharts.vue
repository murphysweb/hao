<template>
  <div class="app-container tree-box">
    <div id="tree" />
    <div class="components-container board">
      <!-- 看板 -->
      <div class="board-column">
        <div class="board-column-header">
          推荐关系
        </div>
        <draggable
          :list="list"
          v-bind="$attrs"
          class="board-column-content"
          :set-data="setData"
        >
          <div v-for="element in list" :key="element.id" class="board-item">
            {{ element.name }} {{ element.id }}
            <el-button-group style="margin-left: 14%;">
              <el-button type="primary" @click="accept(element)">接受</el-button>
              <el-button type="primary" @click="ignore(element)">忽略</el-button>
            </el-button-group>
          </div>
        </draggable>
      </div>
    </div>
  </div>
</template>

<script>
// 全部引入
import draggable from 'vuedraggable'
var echarts = require('echarts')
export default {
  name: 'DashboardAdmin',
  components: { draggable },
  data() {
    return {
      list: [
        { name: '推荐', id: 1 },
        { name: '推荐', id: 2 },
        { name: '推荐', id: 3 },
        { name: '推荐', id: 4 },
        { name: '推荐', id: 5 },
        { name: '推荐', id: 6 }
      ],
      rowData: null
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
    // 初始化图形插件
    initCharts() {
      this.chart = echarts.init(document.getElementById('tree'))
      this.setOptions()
    },
    setOptions() {
      this.chart.setOption({
        tooltip: { // 提示框组件
          // trigger: 'item', // 触发类型，默认：item（数据项图形触发，主要在散点图，饼图等无类目轴的图表中使用）。可选：'axis'：坐标轴触发，主要在柱状图，折线图等会使用类目轴的图表中使用。'none':什么都不触发。
          // triggerOn: 'mousemove' // 提示框触发的条件，默认mousemove|click（鼠标点击和移动时触发）。可选mousemove：鼠标移动时，click：鼠标点击时，none：
        },
        animationDurationUpdate: 1500,
        animationEasingUpdate: 'quinticInOut',
        series: [{
          type: 'graph',
          layout: 'none',
          symbolSize: 50,
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
            source: 0,
            target: 1,
            symbolSize: [5, 20]
          }, {
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
    },
    // 接受
    accept(e) {
      // 操作数据
      alert('接受推荐：' + e.id)
    },
    // 忽略
    ignore(e) {
      // 操作数据
      alert('忽略推荐：' + e.id)
    }
  }
}
</script>

<style lang="scss" scoped>
#tree {
  width: 900px;
  height: 400px;
  position: absolute;
}
.tree-box{
  border:1px solid #d3d3d3;
  padding: 0;
  margin:20px;
}
.board {
  width: 1000px;
  margin-left: 34%;
  display: flex;
  justify-content: space-around;
  flex-direction: row;
  align-items: flex-start;
}
.board-column {
  min-width: 300px;
  min-height: 100px;
  //max-height: 360px;
  height: auto;
  overflow: hidden;
  background: #f0f0f0;
  border-radius: 3px;

  .board-column-header {
    height: 50px;
    line-height: 50px;
    overflow: hidden;
    padding: 0 20px;
    text-align: center;
    background: #333;
    color: #fff;
    border-radius: 3px 3px 0 0;
  }

  .board-column-content {
    // height: auto;
    // overflow: hidden;
    max-height: 310px;
    overflow: auto;
    border: 10px solid transparent;
    min-height: 60px;
    display: flex;
    justify-content: flex-start;
    flex-direction: column;
    align-items: center;

    .board-item {
      cursor: pointer;
      width: 100%;
      height: 64px;
      margin: 5px 0;
      background-color: #fff;
      text-align: left;
      line-height: 54px;
      padding: 5px 10px;
      box-sizing: border-box;
      box-shadow: 0px 1px 3px 0 rgba(0, 0, 0, 0.2);
    }
  }
}
</style>
