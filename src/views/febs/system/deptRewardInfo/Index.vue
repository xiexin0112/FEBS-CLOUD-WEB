<template>
  <div class="order">
    <el-main>
      <div class="filter-container">
        <el-select v-model="userId" placeholder="代理商" clearable @change="initOrder()">
          <el-option
            v-for="item in agentLists"
            :key="item.userId"
            :label="item.userName"
            :value="item.userId"
          />
        </el-select>
      </div>
      <el-table
        :data="dataList"
        border
        style="width: 100%"
      >
        <el-table-column
          type="index"
          align="center"
          label="序号"
        />

        <el-table-column
          align="center"
          label="代理名称"
        >
        <template slot-scope="scope">{{ scope.row.agentName }}</template>
        </el-table-column>

        <el-table-column
          align="center"
          label="代理人名称"
        >
        <template slot-scope="scope">{{ scope.row.userName }}</template>
        </el-table-column>

        <el-table-column
          align="center"
          label="代理人电话"
        >
          <template slot-scope="scope">{{ scope.row.mobile }}</template>
        </el-table-column>
        <el-table-column
          align="center"
          label="总佣金"
        >
          <template slot-scope="scope">{{ scope.row.serviceCharge }}</template>
        </el-table-column>
        <el-table-column
          align="center"
          label="总充值金额"
        >
          <template slot-scope="scope">{{ scope.row.rechargeAmount }}</template>
        </el-table-column>

        <el-table-column
          align="center"
          label="总红包金额"
        >
          <template slot-scope="scope">{{ scope.row.redEnvelope }}</template>
        </el-table-column>

        <el-table-column
          align="center"
          label="总提现金额"
        >
          <template slot-scope="scope">{{ scope.row.withdrawAmout }}</template>
        </el-table-column>

        <el-table-column
          align="center"
          label="代理利润"
        >
          <template slot-scope="scope">{{ scope.row.profitSharing }}</template>
        </el-table-column>

        <el-table-column
          align="center"
          label="总平台利润"
        >
          <template slot-scope="scope">{{ scope.row.orderProfit }}</template>
        </el-table-column>

      </el-table>
      <div class="pagination-container">
        <el-pagination
          background
          layout="total, sizes,prev, pager, next,jumper"
          :page-size="pageSize"
          :page-sizes="[5,10,15]"
          :total="total"
          @size-change="handleSizeChange"
          @current-change="handleCurrentChange"
        />
      </div>
    </el-main>

    <el-dialog title="新增" :visible.sync="addDialogVisible">
      <el-form>
        <el-form-item label="用户id" >
          <el-input  v-model="addForm.userId" placeholder="用户id"></el-input>
        </el-form-item>

        <el-form-item label="代理名称" >
          <el-input class="filter-item search-item" v-model="addForm.agentName" placeholder="代理名称"></el-input>
        </el-form-item>
        <el-form-item label="分成比例" >
          <el-input type="number" class="filter-item search-item" v-model="addForm.shareRatio" placeholder="分成比例"></el-input>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="addDialogVisible = false">取 消</el-button>
        <el-button type="primary" @click="add">确 定</el-button>
      </div>
    </el-dialog>
  </div>
</template>
<script>

export default {
  name: 'Order',
  data() {
    return {
      addDialogVisible: false,
      addForm: {
        userId: '',
        agentName: '',
        shareRatio: ''
      },
      userId: '',
      agentLists: [],
      deptId: null,
      total: null,
      pageSize: 10,
      pageIndex: 1,
      dataList: []
    }
  },
  mounted() {
    this.initOrder()
    this.dept()
  },
  methods: {
    /* 修改分页数量 */
    handleSizeChange(val) {
      this.pageSize = val
      this.initOrder()
    },
    /* 修改分页页数 */
    handleCurrentChange(val) {
      this.pageIndex = val
      this.initOrder()
    },
    dept() {
      this.$get('system/deptRewardInfo/findAgentLists').then((r) => {
        this.agentLists = [{ userId: '', userName: '全部' }, ...r.data.data]
      })
    },
    initOrder() {
      this.$post2('system/deptRewardInfo/findAgentDetails', {
        'request': {
          pageSize: this.pageSize,
          pageNum: this.pageIndex
        },
        'userId': this.userId || null
      }).then((r) => {
        this.dataList = r.data.data.rows
        this.total = r.data.data.total
      })
    },
   
  
  }
}
</script>
<style lang="scss" scoped>
  .dept {
    margin: 10px;
    .app-container {
      margin: 0 0 10px 0 !important;
    }
  }

  .el-card.is-always-shadow {
    box-shadow: none;
  }

  .el-card {
    border-radius: 0;
    border: none;
    .el-card__header {
      padding: 10px 20px !important;
      border-bottom: 1px solid #f1f1f1 !important;
    }
  }
</style>
<style lang="scss">
  .vue-treeselect__menu {
    max-height: 165px !important;
  }
</style>
