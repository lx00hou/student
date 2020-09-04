<template>
  <div class="student-main">
    <!-- shadow never  hover  -->
    <el-card class="search-bar card" shadow="hover">
      <div slot="header">
        <span>搜索条件</span>
      </div>
      <el-row :gutter="30">
        <el-col :span="6">
          <el-input v-model="search.name" placeholder="输入名字" size="small" />
        </el-col>
        <el-col :span="6">
          <el-date-picker v-model="search.startTime" size="small" type="date" placeholder="选择开始时间" />
        </el-col>
        <el-col :span="6">
          <el-select v-model="search.clazz" size="small" placeholder="选择班级">
            <el-option
              v-for="item in classList"
              :key="item.id"
              :label="item.className"
              :value="item.id"
            />
          </el-select>
        </el-col>
        <el-col :span="6">
          <el-input v-model="search.name" placeholder="输入名字" size="small" />
        </el-col>
      </el-row>
      <el-row style="margin-top:20px;" :gutter="30">
        <el-col :span="6">
          <el-input v-model="search.name" placeholder="输入名字" size="small" />
        </el-col>
        <el-col style="text-align:right;" :span="6" :offset="12">
          <el-button size="small" type="default" @click="doReset">重置</el-button>
          <el-button type="primary" size="small" @click="doSearch">查询</el-button>
        </el-col>
      </el-row>
    </el-card>

    <el-card class="container card" shadow="never">
      <div slot="header">
        <el-button type="primary" size="small" @click="goAdd">新增学员</el-button>

      </div>
      <el-table :data="tableData" border style="width: 100%">
        <el-table-column fixed type="index" label="序号" width="50" />
        <el-table-column prop="date" label="日期" width="150" />
        <el-table-column prop="name" label="姓名" width="120" />
        <el-table-column prop="province" label="省份" width="120" />
        <el-table-column prop="city" label="市区" width="120" />
        <el-table-column prop="address" label="地址" width="300" />
        <el-table-column prop="zip" label="邮编" width="120" />
        <el-table-column fixed="right" label="操作" width="100">
          <!-- 自定义列 使用vue的 作用于插槽 -->
          <!-- scope指的是当前的表格data对象 , 可以通过 scope.row 获取当前行的数据 -->
          <template slot-scope="scope">
            <el-button type="text" size="small" @click="handleClick(scope.row)">查看</el-button>
            <el-button type="text" size="small">编辑</el-button>
          </template>
        </el-table-column>
      </el-table>
      <!-- 分页 -->
      <el-pagination
        style="margin: 30px auto 0; text-align: right;"
        :current-page="1"
        :page-sizes="[10, 20, 30, 40]"
        :page-size="10"
        layout="total, sizes, prev, pager, next, jumper"
        :total="35"
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
      />
    </el-card>

    <!-- 弹窗 -->
    <el-dialog
      title="学员信息"
      :visible.sync="dialog"
      width="60%"
      @close="handleClose"
    >

      <el-form ref="form" :model="form" label-width="80px">
        <el-form-item label="活动名称">
          <el-input v-model="form.name" />
        </el-form-item>
        <el-form-item label="活动区域">
          <el-select v-model="form.region" placeholder="请选择活动区域">
            <el-option label="区域一" value="shanghai" />
            <el-option label="区域二" value="beijing" />
          </el-select>
        </el-form-item>
        <el-form-item label="活动时间">
          <el-col :span="11">
            <el-date-picker v-model="form.date1" type="date" placeholder="选择日期" style="width: 100%;" />
          </el-col>
          <el-col class="line" :span="2">-</el-col>
          <el-col :span="11">
            <el-time-picker v-model="form.date2" type="fixed-time" placeholder="选择时间" style="width: 100%;" />
          </el-col>
        </el-form-item>
        <el-form-item label="即时配送">
          <el-switch v-model="form.delivery" />
        </el-form-item>
        <el-form-item label="活动性质">
          <el-checkbox-group v-model="form.type">
            <el-checkbox label="美食/餐厅线上活动" name="type" />
            <el-checkbox label="地推活动" name="type" />
            <el-checkbox label="线下主题活动" name="type" />
            <el-checkbox label="单纯品牌曝光" name="type" />
          </el-checkbox-group>
        </el-form-item>
        <el-form-item label="特殊资源">
          <el-radio-group v-model="form.resource">
            <el-radio label="线上品牌商赞助" />
            <el-radio label="线下场地免费" />
          </el-radio-group>
        </el-form-item>
        <el-form-item label="活动形式">
          <el-input v-model="form.desc" type="textarea" />
        </el-form-item>
      </el-form>
      <span slot="footer">
        <el-button @click="handleClose">取 消</el-button>
        <el-button type="primary" @click="doSave">确 定</el-button>
      </span>
    </el-dialog>

  </div>
</template>

<script>
export default {
  name: 'XyStudent', // name方便我们用devTool调试代码 建议一定要写 而且保证唯一
  data() {
    return {
      dialog: false,
      search: {},
      classList: [
        {
          id: 1,
          className: '前端'
        },
        {
          id: 2,
          className: 'UI'
        }
      ],
      tableData: [
        {
          date: '2016-05-03',
          name: '王小虎1',
          province: '上海',
          city: '普陀区',
          address: '上海市普陀区金沙江路 1518 弄',
          zip: 200333
        },
        {
          date: '2016-05-02',
          name: '王小虎',
          province: '上海',
          city: '普陀区',
          address: '上海市普陀区金沙江路 1518 弄',
          zip: 200333
        },
        {
          date: '2016-05-04',
          name: '王小虎',
          province: '上海',
          city: '普陀区',
          address: '上海市普陀区金沙江路 1518 弄',
          zip: 200333
        },
        {
          date: '2016-05-01',
          name: '王小虎',
          province: '上海',
          city: '普陀区',
          address: '上海市普陀区金沙江路 1518 弄',
          zip: 200333
        }
      ],
      form: {
        name: '',
        region: '',
        date1: '',
        date2: '',
        delivery: false,
        type: [],
        resource: '',
        desc: ''
      }
    }
  },
  methods: {
    // 条件查询
    doSearch() {},
    doReset() {},
    handleClick(row) {
      console.log(row)
    },
    handleSizeChange(val) {
      console.log(`每页 ${val} 条`)
    },
    handleCurrentChange(val) {
      console.log(`当前页: ${val}`)
    },
    // 打开对话框
    goAdd() {
      this.dialog = true
    },
    // 弹窗关闭的时候 触发
    handleClose() {
      this.dialog = false
    },
    // 保存学生信息
    doSave() {

    }
  }
}
</script>

<style lang="scss" scoped>
.student-main {
  h1 {
    color: red;
  }
  .card {
    margin: 30px;
  }
  .el-select,
  .el-input {
    width: 220px;
  }
}
</style>
