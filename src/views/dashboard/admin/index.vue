<template>
  <div class="app-container">
    <!--  -->
    <el-table
      :data="tableData"
      border
    >
      <el-table-column
        fixed
        prop="date"
        label="名称"
        min-width="150"
      />
      <el-table-column
        prop="name"
        label="描述"
        min-width="300"
      />
      <el-table-column
        prop="province"
        label="创建时间"
        width="200"
      />
      <el-table-column
        fixed="right"
        label="操作"
      >
        <template slot-scope="scope">
          <el-button type="text" size="small" @click="handleClick(scope.row)">查看</el-button>
          <el-button type="text" size="small">编辑</el-button>
        </template>
      </el-table-column>
    </el-table>
    <!-- 分页 -->
    <el-pagination
      :current-page="pageNumber"
      :page-sizes="[10, 20, 30, 50]"
      :page-size="pageSize"
      layout="total, sizes, prev, pager, next, jumper"
      class="main-pagination"
      :total="tableData.length"
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
    />
  </div>
</template>

<script>
export default {
  name: 'DashboardAdmin',
  components: {
  },
  data() {
    return {
      pageNumber: 0,
      pageSize: 10,
      tableData: [{
        date: '2016-05-02',
        name: '王小虎',
        desc: '描述描述描述',
        id: 1
      }, {
        date: '2016-05-02',
        name: '王小虎',
        desc: '描述描述描述',
        id: 2
      }]
    }
  },
  methods: {
    handleClick() {
      //
    },
    // 列表更改pageSize
    handleSizeChange(val) {
      this.pageNumber = 1
      this.pageSize = val
      // 获取数据
      // this.geList()
    },
    // 列表更改页
    handleCurrentChange(val) {
      this.pageNumber = val
      // 获取数据
      // this.geList()
    }
  }
}
</script>

<style lang="scss" scoped>
.main-pagination {
  text-align: right;
  margin: 20px;
}
</style>
