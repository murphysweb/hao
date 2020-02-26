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
    setOptions() {
      this.chart.setOption({
        tooltip: { // 提示框组件
          trigger: 'item', // 触发类型，默认：item（数据项图形触发，主要在散点图，饼图等无类目轴的图表中使用）。可选：'axis'：坐标轴触发，主要在柱状图，折线图等会使用类目轴的图表中使用。'none':什么都不触发。
          triggerOn: 'mousemove' // 提示框触发的条件，默认mousemove|click（鼠标点击和移动时触发）。可选mousemove：鼠标移动时，click：鼠标点击时，none：
        },
        series: [ // 系列列表
          {
            type: 'tree', // 树形结构
            data: [{
              'children': [
                {
                  'children': [
                    {
                      'children': [
                        {
                          'children': [],
                          'name': 'F'
                        }
                      ],
                      'name': 'B'
                    }
                  ],
                  'name': 'C'
                },
                {
                  'children': [
                    {
                      'children': [],
                      'name': 'E'
                    }
                  ],
                  'name': 'D'
                }
              ],
              'name': 'A'
            }],
            top: '1%', // 距离上
            left: '7%', // 左
            bottom: '1%', // 下
            right: '20%', // 右的距离

            symbolSize: 30, // 标记的大小，就是那个小圆圈，默认7

            label: { // 每个节点所对应的标签的样式
              normal: {
                position: 'left', // 标签的位置
                verticalAlign: 'middle', // 文字垂直对齐方式，默认自动。可选：top，middle，bottom
                align: 'right', // 文字水平对齐方式，默认自动。可选：top，center，bottom
                fontSize: 9 // 标签文字大小
              }
            },

            leaves: { // 叶子节点的特殊配置，如上面的树图示例中，叶子节点和非叶子节点的标签位置不同
              label: {
                normal: {
                  position: 'right',
                  verticalAlign: 'middle',
                  align: 'left'
                }
              }
            },

            expandAndCollapse: true, // 子树折叠和展开的交互，默认打开
            animationDuration: 550, // 初始动画的时长，支持回调函数,默认1000
            animationDurationUpdate: 750// 数据更新动画的时长，默认300
          }
        ]
      })
    }
  }
}
</script>

<style lang="scss" scoped>
#tree {
  height: 200px;
  width: 400px;
  position: absolute;
}
.tree-box{
  border:1px solid #d3d3d3;
  padding: 0;
  margin:20px;
}
.board {
  width: 1000px;
  margin-left: 42%;
  display: flex;
  justify-content: space-around;
  flex-direction: row;
  align-items: flex-start;
}
</style>
