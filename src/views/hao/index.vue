<template>
  <div class="app-container">
    <!-- 筛选条件 -->
    <el-form
      :inline="true"
      :model="formData"
      size="small"
      class="demo-form-inline"
    >
      <el-form-item>
        <el-button type="primary" @click="addRow">新建</el-button>
      </el-form-item>
      <!-- 搜索工程名称或描述信息 -->
      <el-form-item label="">
        <el-input
          v-model="search"
          placeholder="请输入内容"
        />
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="search">搜索</el-button>
      </el-form-item>
    </el-form>
    <!-- 表格 -->
    <el-table
      ref="tableData"
      :data="tables"
      border
    >
      <el-table-column
        fixed
        prop="name"
        label="名称"
        min-width="100"
      />
      <el-table-column
        prop="desc"
        label="描述"
        min-width="150"
      />
      <el-table-column
        prop="target"
        label="监控目标"
        min-width="200"
      />
      <el-table-column
        prop="date"
        label="创建时间"
        width="100"
      />
      <el-table-column
        fixed="right"
        align="center"
        label="操作"
        width="180"
      >
        <template slot-scope="scope">
          <el-button type="text" size="small" @click="handleClick(scope.row)">查看</el-button>
          <el-button type="text" size="small" @click="handleEdit(scope.$index, scope.row)">编辑</el-button>
          <el-button type="danger" size="small" @click="handleDelete(scope.$index, scope.row)">删除</el-button>
        </template>
      </el-table-column>
    </el-table>
    <!-- 新增窗口 -->
    <el-dialog
      title="新建"
      :visible.sync="addFormVisible"
    >
      <el-form
        ref="addForm"
        :rules="rules"
        :model="addForm"
      >
        <el-form-item
          label="名称"
          prop="name"
          :label-width="formLabelWidth"
        >
          <el-input
            v-model="addForm.name"
            autocomplete="off"
          />
        </el-form-item>
        <el-form-item
          label="日期"
          prop="date"
          :label-width="formLabelWidth"
        >
          <el-date-picker
            v-model="addForm.date"
            type="date"
            placeholder="选择日期"
            format="yyyy年 MM 月 dd 日"
            value-format="yyyy-MM-dd"
          />
        </el-form-item>
        <el-form-item
          label="监控目标"
          prop="target"
          :label-width="formLabelWidth"
        >
          <el-input
            v-model="addForm.target"
            type="textarea"
            autocomplete="off"
          />
        </el-form-item>
        <el-form-item
          label="描述"
          :label-width="formLabelWidth"
        >
          <el-input
            v-model="addForm.desc"
            type="textarea"
            autocomplete="off"
          />
        </el-form-item>

      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="cancel()">取消</el-button>
        <el-button
          type="primary"
          :loading="addLoading"
          @click="sumbitAddRow()"
        >确定</el-button>
      </div>

    </el-dialog>
    <!-- 编辑窗口 -->
    <el-dialog
      title="编辑"
      :visible.sync="editFormVisible"
    >
      >
      <el-form
        ref="editForm"
        :rules="rules"
        :model="editForm"
      >
        <el-form-item
          label="名称"
          prop="name"
          :label-width="formLabelWidth"
        >
          <el-input
            v-model="editForm.name"
            autocomplete="off"
          />
        </el-form-item>
        <el-form-item
          label="日期"
          prop="date"
          :label-width="formLabelWidth"
        >
          <el-date-picker
            v-model="editForm.date"
            type="date"
            placeholder="选择日期"
            format="yyyy 年 MM 月 dd 日"
            value-format="yyyy-MM-dd"
          />
        </el-form-item>
        <el-form-item
          label="监控目标"
          prop="target"
          :label-width="formLabelWidth"
        >
          <el-input
            v-model="editForm.target"
            type="textarea"
            autocomplete="off"
          />
        </el-form-item>
        <el-form-item
          label="描述"
          :label-width="formLabelWidth"
        >
          <el-input
            v-model="editForm.desc"
            type="textarea"
            autocomplete="off"
          />
        </el-form-item>

      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="cancel()">取消</el-button>
        <el-button
          type="primary"
          :loading="addLoading"
          @click="sumbitEditRow()"
        >确定</el-button>
      </div>
    </el-dialog>

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
import axios from 'axios'
var _index// 定义全局变量
export default {
  name: 'DashboardAdmin',
  components: {
  },
  data() {
    return {
      rules: {
        name: [{ required: true, message: 'name is required', trigger: 'change' }],
        // date: [{ type: 'date', required: true, message: 'date is required', trigger: 'change' }],
        target: [{ required: true, message: 'target is required', trigger: 'blur' }]
      },
      formData: {
        name: ''
      },
      pageNumber: 0,
      pageSize: 10,
      formLabelWidth: '120px',
      addFormVisible: false, // 是否显示新增窗口
      name: 'tableList',
      addForm: [],
      addLoading: false,
      editFormVisible: false, // 是否显示编辑窗口
      editForm: [],
      search: '',
      tableData: [{
        date: '2016-05-02',
        name: 'demo',
        desc: '描述',
        target: '目标',
        id: 0
      }]
    }
  },
  computed: {
    // 模糊搜索
    tables() {
      const search = this.search
      if (search) {
        // filter() 方法创建一个新的数组，新数组中的元素是通过检查指定数组中符合条件的所有元素。
        // 注意： filter() 不会对空数组进行检测。
        // 注意： filter() 不会改变原始数组。
        return this.tableData.filter(data => {
          // some() 方法用于检测数组中的元素是否满足指定条件;
          // some() 方法会依次执行数组的每个元素：
          // 如果有一个元素满足条件，则表达式返回true , 剩余的元素不会再执行检测;
          // 如果没有满足条件的元素，则返回false。
          // 注意： some() 不会对空数组进行检测。
          // 注意： some() 不会改变原始数组。
          return Object.keys(data).some(key => {
            // indexOf() 返回某个指定的字符在某个字符串中首次出现的位置，如果没有找到就返回-1；
            // 该方法对大小写敏感！所以之前需要toLowerCase()方法将所有查询到内容变为小写。
            return String(data[key]).toLowerCase().indexOf(search) > -1
          })
        })
      }
      return this.tableData
    }
  },
  mounted() {
    this.getData()
  },
  methods: {
    getData() {
      // const url = process.env.NODE_ENV === 'development' ? '/api/test/allAgents' : '/version/allAgents'
      const url = 'http://172.16.1.25:8086/test/allAgents'
      axios.get(url).then(({ data }) => {
        this.tableData = data.data
      }).catch((error) => {
        console.log(error)
      })
    },
    handleClick(row) {
      // 跳转到相应页
      this.$router.push({
        name: 'hao'
      })
      // 将参数存储到session里边
      sessionStorage.setItem('hao', JSON.stringify(row))
    },
    handleDelete(index) {
      this.$confirm('此操作将永久删除该数据, 是否继续?', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(() => {
        this.tableData.splice(index, 1)
        this.$message({
          type: 'success',
          message: '删除成功!'
        })
      }).catch(err => {
        this.$message({
          type: 'error',
          message: err
        })
      })
    },
    // 新增数据
    addRow() {
      this.addFormVisible = true
      this.addForm = {
        name: '',
        date: '',
        desc: '',
        target: ''
      }
    },
    // 将新增的数据添加到表格中
    sumbitAddRow() {
      this.$refs['addForm'].validate((valid) => {
        if (valid) {
          this.tableData = this.tableData || []
          // let url = process.env.NODE_ENV === 'development' ? '/api/version/newAgent' : '/version/newAgent';
          // axios({
          //   url: url,
          //   method:'POST',
          //   data:{
          //     name: this.addForm.name,
          //     date: this.addForm.date,
          //     desc: this.addForm.desc,
          //     target: this.addForm.target
          //   }
          // }).then(({data}) => {
          this.tableData.push({
            // id: data.data.id,
            name: this.addForm.name,
            date: this.addForm.date,
            desc: this.addForm.desc,
            target: this.addForm.target
          })
          this.addFormVisible = false
          // })
        }
      })
    },
    cancel() {
      this.addFormVisible = false
      this.editFormVisible = false
    },
    // 编辑数据
    handleEdit(index, row) {
      this.editFormVisible = true
      this.editForm = Object.assign({}, row)
      _index = index// 将编辑行数传给全局变量
    },
    // 保存编辑
    sumbitEditRow() {
      this.$refs['editForm'].validate((valid) => {
        console.log(valid)
        if (valid) {
          var editData = _index
          console.log(this.editForm.name)
          this.tableData[editData].name = this.editForm.name
          this.tableData[editData].date = this.editForm.date
          this.tableData[editData].desc = this.editForm.desc
          this.tableData[editData].target = this.editForm.target
          this.editFormVisible = false
        }
      })
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
