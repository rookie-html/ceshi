<template>
  <div id="zManagement">
    <el-row>
      <el-col :span="3">
        <el-input placeholder="输入关键字进行过滤" sizi="mini"/>
        <el-table
          :data="planData"
          highlight-current-row
          size="mini"
          border
          style="width: 100%">
          <el-table-column
            prop="name"
            height="30px"
            label="分区方案"
            size="mini"/>
        </el-table>
      </el-col>
      <el-col :span="21" style="padding-left:20px;">
        <el-row>
          <el-col>
            <el-button-group>
              <el-button type="primary" size="mini">新建</el-button>
              <el-button type="primary" size="mini">保存</el-button>
              <el-button type="danger" size="mini">删除</el-button>
            </el-button-group>
            <el-button type="primary" size="mini" style="padding-left:5px">下载分区管理</el-button>
          </el-col>
        </el-row>
        <el-row>
          <el-form :inline="true" :model="partitionForm" class="demo-form-inline">
            <el-form-item label="分区方案">
              <el-input v-model="partitionForm.programme" size="mini"/>
            </el-form-item>
            <el-form-item label="物流管理类型">
              <el-select v-model="partitionForm.type" size="mini">
                <el-option label="快递" value="kuaidi" checked/>
                <el-option label="专线" value="zhuanxian"/>
                <el-option label="小包" value="xiaobao"/>
              </el-select>
            </el-form-item>
            <el-form-item>
              上传下载
            </el-form-item>
          </el-form>
        </el-row>
        <el-row></el-row>
      </el-col>
    </el-row>
  </div>
</template>
<script>
export default {
  data() {
    return {
      planData: [{
        name: 'amarex-eu'
      }, {
        name: 'DHL'
      }, {
        name: 'FEDEX FFY'
      }, {
        name: 'FEDEX(8-9 0-7)'
      }, {
        name: 'TNT'
      }, {
        name: 'USP'
      }, {
        name: 'UPS-Test'
      }, {
        name: 'UPS大陆'
      }, {
        name: '测试分区'
      }, {
        name: '大陆UPS蓝单'
      }, {
        name: '美国海运卡派分区测试'
      }],
      partitionForm: {
        programme: '',
        type: ''
      }
    }
  }
}
</script>
<style lang="scss" scoped>
.el-table td, .el-table th{
  text-align:center!important;
}
#zManagement{
}
</style>
<style lang="scss">
#zManagement {
  .el-table td, .el-table th{
    text-align:center!important;
  }
  .el-table__body tr.current-row>td{
    background-color: rgba(125,125,125,0.8)!important
  }
}
</style>

