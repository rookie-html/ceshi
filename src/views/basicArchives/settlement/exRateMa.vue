<template>
  <div id="exRateMa">
    <el-container>
      <el-header style="margin-top:10px">
        <el-row>
          <el-col :span="4" style="text-align:right">
            <el-input v-model="queryCriteria" placeholder="请输入查询内容" size="mini" style="width:200px">
              <el-button slot="append" icon="el-icon-search"/>
            </el-input>
          </el-col>
          <el-col :span="20" style="text-align:right">
            <el-button-group>
              <el-button type="primary" size="mini" @click="showDialog('add')">添加</el-button>
              <el-button type="primary" size="mini" @click="showDialog('edit')">编辑</el-button>
              <el-button type="danger" size="mini" @click="doDeleteTableDate">删除</el-button>
            </el-button-group>
          </el-col>
        </el-row>
      </el-header>
      <el-main>
        <el-table
          :data="filterData"
          style="width: 100%"
          border
          stripe
          show-header
          highlight-current-row
          @row-dblclick="dbEdit"
          @row-click="rowDiscoloration">
          <el-table-column prop="id" label="序号" sortable width="100"/>
          <el-table-column prop="currencyClass" label="货币别" sortable width="120"/>
          <el-table-column prop="cOfCC" label="兑换币别" sortable width="120"/>
          <el-table-column prop="coCharacter" label="折算符" sortable width="120"/>
          <el-table-column prop="exRate" label="汇率(繁体)" sortable width="120"/>
          <el-table-column prop="acRate" label="做账汇率" sortable width="120"/>
          <el-table-column prop="inRate" label="发票汇率" sortable width="120"/>
          <el-table-column prop="efDate" label="生效日期" sortable width="120"/>
          <el-table-column prop="clDate" label="截止日期" sortable width="120"/>
        </el-table>
      </el-main>
      <el-footer>
        <el-pagination
          :current-page="currentPage"
          :page-sizes="[1, 2, 3, 4]"
          :page-size="1"
          :total="2"
          layout="total, sizes, prev, pager, next, jumper"/>
      </el-footer>
    </el-container>
    <el-dialog :title="dialogTitle" :visible.sync="dialogVisible" width="50%" center show-close @closed="resetForm">
      <el-form ref="dialog" :model="dialogForm" label-width="120px">
        <el-row>
          <el-col :span="12">
            <el-form-item label="货币别" prop="currencyClass">
              <el-input v-model="dialogForm.currencyClass" autocomplete="off"/>
            </el-form-item>
          </el-col>
          <el-col :span="12">
            <el-form-item label="兑换币别" prop="cOfCC">
              <el-input v-model="dialogForm.cOfCC" autocomplete="off"/>
            </el-form-item>
          </el-col>
        </el-row>
        <el-row>
          <el-col :span="12">
            <el-form-item label="折算符" prop="coCharacter">
              <el-input v-model="dialogForm.coCharacter" autocomplete="off"/>
            </el-form-item>
          </el-col>
          <el-col :span="12">
            <el-form-item label="汇率" prop="exRate">
              <el-input v-model="dialogForm.exRate" autocomplete="off"/>
            </el-form-item>
          </el-col>
        </el-row>
        <el-row>
          <el-col :span="12">
            <el-form-item label="做账汇率" prop="acRate">
              <el-input v-model="dialogForm.acRate" autocomplete="off"/>
            </el-form-item>
          </el-col>
          <el-col :span="12">
            <el-form-item label="发票汇率" prop="inRate">
              <el-input v-model="dialogForm.inRate" autocomplete="off"/>
            </el-form-item>
          </el-col>
        </el-row>
        <el-row>
          <el-col :span="12">
            <el-form-item label="生效日期" prop="efDate">
              <el-input v-model="dialogForm.efDate" autocomplete="off"/>
            </el-form-item>
          </el-col>
          <el-col :span="12">
            <el-form-item label="截止日期" prop="clDate">
              <el-input v-model="dialogForm.clDate" autocomplete="off"/>
            </el-form-item>
          </el-col>
        </el-row>
      </el-form>
      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogCancel">取 消</el-button>
        <el-button type="primary" @click="dialogSubmit">确 定</el-button>
      </span>
    </el-dialog>
  </div>
</template>
<script>

export default {
  name: 'ExRateMa',
  // filters: {
  //   query(value) {
  //     return value.filter((valueOb) => {
  //       var findWhether = false
  //       console.log(this.queryCriteria)
  //       Object.keys(valueOb).forEach(valueKey => {
  //         if (new RegExp('1').test(valueOb[valueKey])) {
  //           findWhether = true
  //         }
  //       })
  //       return findWhether
  //     })
  //   }
  // },
  data() {
    const exRateModel = {
      id: '',
      currencyClass: '',
      cOfCC: '',
      coCharacter: '',
      exRate: '',
      acRate: '',
      inRate: '',
      efDate: '',
      clDate: ''
    }
    return {
      tableData: [{
        id: '1',
        currencyClass: 'NZD',
        cOfCC: 'NZD',
        coCharacter: '/',
        exRate: '12',
        acRate: '做账汇率',
        inRate: '发票汇率',
        efDate: '2019-01-14',
        clDate: '截止日期'
      },
      {
        id: '2',
        currencyClass: 'NZD',
        cOfCC: 'NZD',
        coCharacter: '/',
        exRate: '1',
        acRate: '做账汇率',
        inRate: '发票汇率',
        efDate: '2019-01-14',
        clDate: '截止日期'
      },
      {
        id: '11',
        currencyClass: 'RMB',
        cOfCC: 'USD',
        coCharacter: '/',
        exRate: '6.45',
        acRate: '6.45',
        inRate: '6.45',
        efDate: '2018-08-01',
        clDate: '截止日期'
      },
      {
        id: '12',
        currencyClass: 'USD',
        cOfCC: 'RMB',
        coCharacter: '*',
        exRate: '6.45',
        acRate: '6.45',
        inRate: '6.45',
        efDate: '2018-08-01',
        clDate: '截止日期'
      }],
      listLoading: false,
      /* 当前页数 */
      currentPage: 1,
      // 是否显示弹窗
      dialogVisible: false,
      // 选中的当前行信息
      selectionRow: Object.assign({}, exRateModel),
      // 弹窗标题
      dialogTitle: '',
      // 弹窗数据
      dialogForm: Object.assign({}, exRateModel),
      // 查询条件
      queryCriteria: ''
    }
  },
  computed: {
    filterData() {
      return this.tableData.filter((valueOb) => {
        var findWhether = false
        Object.keys(valueOb).forEach(valueKey => {
          if (new RegExp(this.queryCriteria).test(valueOb[valueKey])) {
            findWhether = true
          }
        })
        return findWhether
      })
    }
  },
  methods: {
    rowDiscoloration(row, column, event) {
      // this.selectionRow.id = row.id
      // this.selectionRow.currencyClass = row.currencyClass
      // this.selectionRow.cOfCC = row.cOfCC
      // this.selectionRow.coCharacter = row.coCharacter
      // this.selectionRow.exRate = row.exRate
      // this.selectionRow.acRate = row.acRate
      // this.selectionRow.inRate = row.inRate
      // this.selectionRow.efDate = row.efDate
      // this.selectionRow.clDate = row.clDate
      this.selectionRow = Object.assign({}, row)
    },
    dbEdit(row) {
      this.dialogForm = Object.assign({}, row)
      this.showDialog('edit')
    },
    doDeleteTableDate() {
      this.$confirm('你确定要删除此记录吗？', '删除', {
        distinguishCancelAndClose: true,
        confirmButtonText: '确定',
        cancelButtonText: '取消'
      }).then(() => {
        this.tableData = this.tableData.filter((value, index, arr) => value.id !== this.selectionRow.id)
        this.$notify({
          type: 'success',
          message: '删除成功'
        })
      })
    },
    // 弹出弹窗 弹窗取消 弹窗确定
    showDialog(type) {
      if (type === 'add') {
        this.dialogTitle = '新增 - 汇率信息'
      } else {
        this.dialogTitle = '编辑 - 汇率信息'
        this.dialogForm = Object.assign({}, this.selectionRow)
      }
      // this.dialogTitle = type === 'add' ? '新增 - 汇率信息' : '编辑 - 汇率信息'
      this.dialogVisible = true
    },
    dialogCancel() {
      this.dialogVisible = false
    },
    dialogSubmit() {
      this.dialogVisible = false
      if (this.dialogTitle === '新增 - 汇率信息') {
        this.addExRate()
      } else {
        this.updateExRate()
      }
    },
    // 添加汇率
    addExRate() {
      this.dialogForm.id = 0
      this.tableData.forEach(value => {
        // console.log('this.dialogForm.id=' + this.dialogForm.id)
        // console.log('value.id=' + value.id)
        // console.log('this.dialogForm.id < value.id' + this.dialogForm.id < value.id)
        if (this.dialogForm.id - value.id < 0) {
          // console.log(value.id)
          this.dialogForm.id = value.id
        }
      })
      this.dialogForm.id = Number(this.dialogForm.id) + 1
      this.tableData.push(Object.assign({}, this.dialogForm))
      this.$notify({
        type: 'success',
        message: '添加成功'
      })
    },
    // 修改汇率
    updateExRate() {
      var newTableData = []
      this.tableData.forEach((value) => {
        if (value.id !== this.selectionRow.id) {
          newTableData.push(value)
        } else {
          this.dialogForm.id = this.selectionRow.id
          newTableData.push(Object.assign({}, this.dialogForm))
        }
        this.tableData = newTableData
      })
      this.$notify({
        type: 'success',
        message: '修改成功'
      })
    },
    // 重置表单
    resetForm() {
      this.$refs['dialog'].resetFields()
    }
    // query() {
    //   var test = '王'
    //   var mycars = new Array(3)
    //   mycars[0] = '晨王晨'
    //   mycars[1] = '问问'
    //   mycars[2] = '嗯嗯'
    //   var queryCriteria = new RegExp(test)
    //   for (let i = 0; i < 3; i++) {
    //     console.log(queryCriteria.test(mycars[i]))
    //   }
    // }
  }
}
</script>
<style lang="scss" scoped>
#exRateMa{
  .el-table{
    height:680px;
  }
  tbody .current-row>td {
    background: rgba(185, 221, 249, .75)!important;
  }
}
</style>
