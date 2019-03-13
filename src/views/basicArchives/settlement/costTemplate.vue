<template>
  <div id="costTemplate">
    <el-container>
      <el-main>
        <el-row>
          <el-col :span="12">
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
                  :data="oneData"
                  style="width: 100%"
                  border
                  stripe
                  show-header
                  highlight-current-row
                  @row-dblclick="dbEdit"
                  @row-click="rowDiscoloration">
                  <el-table-column prop="id" label="序号" width="100"/>
                  <el-table-column prop="templateName" label="模板名称" sortable width="120"/>
                  <el-table-column prop="reExMark" label="收支标志" width="120"/>
                  <el-table-column prop="creator" label="创建者" width="120"/>
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
          </el-col>

          <el-col :span="12">
            <el-container>
              <el-header style="margin-top:10px">
                <el-row>
                  <el-col :span="24" style="text-align:right">
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
                  :data="filterTwoData"
                  style="width: 100%"
                  border
                  stripe
                  show-header
                  highlight-current-row
                  @row-dblclick="dbEdit2"
                  @row-click="rowDiscoloration2">
                  <el-table-column prop="id" label="序号" width="50"/>
                  <el-table-column prop="costCode" label="费用代码" sortable width="80"/>
                  <el-table-column prop="costName" label="费用名称" width="80"/>
                  <el-table-column prop="unitPrice" label="单价" width="80"/>
                  <el-table-column prop="currency" label="币别" width="80"/>
                  <el-table-column prop="chargeType" label="计费类型" width="80"/>
                  <el-table-column prop="size" label="尺寸" width="80"/>
                  <el-table-column prop="specifications" label="规格" width="80"/>
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
          </el-col>
        </el-row>
      </el-main>
    </el-container>
  </div>
</template>
<script>
export default {
  name: 'CostTemplate',
  data() {
    const costTemplateModel = {
      id: 1,
      templateName: '',
      reExMark: '',
      creator: ''
    }
    return {
      oneData: [{
        id: 1,
        templateName: '常用费用',
        reExMark: '收入',
        creator: '张景清'
      }, {
        id: 2,
        templateName: 'EEEE',
        reExMark: '收入',
        creator: '曾金银'
      }, {
        id: 3,
        templateName: 'THC-APL标准',
        reExMark: '收入',
        creator: '雷天来'
      }, {
        id: 4,
        templateName: '附加费',
        reExMark: '支出',
        creator: '丁硕'
      }, {
        id: 5,
        templateName: '手续费',
        reExMark: '收入',
        creator: '张端洁'
      }],
      twoData: [[{
        id: 1,
        costCode: 'ACC',
        costName: '绿色通道费',
        unitPrice: 10.00,
        currency: 'CNY',
        chargeType: '票',
        size: '',
        specifications: ''
      }], [{
        id: 1,
        costCode: 'YF',
        costName: '运费',
        unitPrice: 17.50,
        currency: 'RMB',
        chargeType: '计费重',
        size: 20,
        specifications: 'RH'
      }], [{
        id: 1,
        costCode: 'COD',
        costName: '转港费',
        unitPrice: 2000.00,
        currency: 'THB',
        chargeType: '票',
        size: 20,
        specifications: 'GP'
      }, {
        id: 2,
        costCode: 'THC',
        costName: '码头操作费',
        unitPrice: 200.00,
        currency: 'CNY',
        chargeType: '票',
        size: 45,
        specifications: 'GP'
      }], [{
        id: 1,
        costCode: 'APD',
        costName: '变更卸货港附加费',
        unitPrice: -1863462912.00,
        currency: 'USD',
        chargeType: '票',
        size: '',
        specifications: ''
      }, {
        id: 2,
        costCode: 'CAF',
        costName: '货币贬值附加费',
        unitPrice: '1.00',
        currency: 'CNY',
        chargeType: '票',
        size: 10,
        specifications: 'type'
      }], [{
        id: 1,
        costCode: 'COD',
        costName: '转港费',
        unitPrice: 5.00,
        currency: 'CNY',
        chargeType: '票',
        size: '',
        specifications: ''
      }, {
        id: 2,
        costCode: 'IMO',
        costName: '危险品附加费',
        unitPrice: 5.00,
        currency: 'FRF',
        chargeType: '票',
        size: '',
        specifications: ''
      }, {
        id: 3,
        costCode: 'THC',
        costName: '码头操作费',
        unitPrice: '8.00',
        currency: 'USD',
        chargeType: '票',
        size: '',
        specifications: ''
      }]],
      listLoading: false,
      /* 当前页数 */
      currentPage: 1,
      // 是否显示弹窗
      dialogVisible: false,
      // 选中的当前行信息
      selectionRow: Object.assign({}, costTemplateModel),
      // 弹窗标题
      dialogTitle: '',
      // 弹窗数据
      dialogForm: Object.assign({}, costTemplateModel),
      // 查询条件
      queryCriteria: ''
    }
  },
  computed: {
    filterTwoData() {
      return this.twoData[this.selectionRow.id - 1]
    }
  },
  methods: {
    doDeleteTableDate() {},
    dbEdit() {},
    rowDiscoloration(row) {
      this.selectionRow = Object.assign({}, row)
    },
    dbEdit2() {},
    rowDiscoloration2(row) {}
  }
}
</script>
<style lang="scss" scoped>

</style>
<style lang="scss">
  .el-table tbody tr:hover>td {
    background-color: rgba(0,0,0,0.3)!important
  }
  .el-table__body tr.current-row>td{
    background-color: rgba(125,125,125,0.8)!important
  }
</style>
