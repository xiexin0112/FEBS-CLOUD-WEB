<template>
  <div class="reward">
    <el-main>
      <div class="filter-container">
        <el-input type="text" class="filter-item search-item" v-model="userId" placeholder="手机号"/>
        <el-select v-model="deptId" placeholder="归属" clearable>
          <el-option
            v-for="item in typeList"
            :key="item.deptId"
            :label="item.deptName"
            :value="item.deptId"
          />
        </el-select>

        <el-date-picker
          v-model="fromdate"
          value-format="yyyy-MM-dd hh:mm:ss"
          type="date"
          placeholder="开始日期">
        </el-date-picker>
        <el-date-picker
          v-model="todate"
          value-format="yyyy-MM-dd hh:mm:ss"
          type="date"
          placeholder="结束日期">
        </el-date-picker>

        <el-button class="filter-item" type="primary" @click="getData()">查询</el-button>
      </div>

      <el-table
        :data="userData"
        border
        show-summary
        :summary-method="getSummaries"
        style="width: 100%">
        <el-table-column
          align="center"
          label="手机号">
          <template slot-scope="scope">{{scope.row.mobile}}</template>
        </el-table-column>
        <el-table-column
          align="center"
          label="用户名">
          <template slot-scope="scope">{{scope.row.username}}</template>
        </el-table-column>
        <el-table-column
          align="center"
          label="充值金额">
          <template slot-scope="scope">{{scope.row.rechargeAmount}}</template>
        </el-table-column>
        <el-table-column
          align="center"
          label="提现金额">
          <template slot-scope="scope">{{scope.row.withdrawalAmount}}</template>
        </el-table-column>
        <el-table-column
          align="center"
          label="平台盈利">
          <template slot-scope="scope">{{scope.row.yk}}</template>
        </el-table-column>
        <el-table-column
          align="center"
          label="用户余额">
          <template slot-scope="scope">{{scope.row.balance}}</template>
        </el-table-column>
        <el-table-column
          align="center"
          label="红包">
          <template slot-scope="scope">{{scope.row.redEnvelope}}</template>
        </el-table-column>
        <el-table-column
          align="center"
          label="佣金">
          <template slot-scope="scope">{{scope.row.memberRewardAmount}}</template>
        </el-table-column>
        <el-table-column
          align="center"
          label="邀请总人数">
          <template slot-scope="scope">{{scope.row.userNum}}</template>
        </el-table-column>
        <el-table-column
          align="center"
          label="新邀请人数">
          <template slot-scope="scope">{{scope.row.newUserNum}}</template>
        </el-table-column>
        <el-table-column
          align="center"
          label="邀请人">
          <template slot-scope="scope">{{scope.row.nviteesName}}</template>
        </el-table-column>
        <el-table-column
          align="center"
          label="所属部门">
          <template slot-scope="scope">{{scope.row.deptName}}</template>
        </el-table-column>

      </el-table>

      <div class="pagination-container">
        <el-pagination
          background
          @size-change="handleSizeChange"
          @current-change="handleCurrentChange"
          layout="total, sizes,prev, pager, next,jumper"
          :page-size="pageSize"
          :page-sizes="[5,10,15]"
          :total="total">
        </el-pagination>
      </div>
    </el-main>
  </div>
</template>
<script>

  export default {
    name: 'reward',
    data() {
      return {
        rechargeAmountTotal: null,
        withdrawalAmountTotal: null,
        rewardAmountTotal: null,
        memberRewardAmountTotal: null,
        ykTotal: null,
        userId: null,
        deptId: null,
        fromdate: null,
        todate: null,
        total: null,
        pageSize: 10,
        pageIndex: 1,
        userData: [],
        typeList: []
      }
    },
    mounted() {
      this.$get('/system/dept/findDeptLists').then((r) => {
        this.typeList = r.data.data
      })
      this.getData()
    },
    methods: {
      getSummaries(param) {
        const { columns, data } = param;
        let sums = ['统计', '', 0,0,0,0,0,0,0,0,''];
        //  <span>充值金额统计：{{rechargeAmountTotal}}</span> &nbsp&nbsp&nbsp
        // <span>提现金额统计：{{withdrawalAmountTotal}}</span> &nbsp&nbsp&nbsp
        // <span>盈亏金额统计：{{ykTotal}}</span> &nbsp&nbsp&nbsp
        // <span>邀请奖励统计：{{rewardAmountTotal}}</span> &nbsp&nbsp&nbsp
        // <span>佣金统计：{{memberRewardAmountTotal}}</span> &nbsp&nbsp&nbsp
        // <span>邀请总人数统计：{{userNumTotal}}</span> &nbsp&nbsp&nbsp
        // <span>新邀请人数统计：{{newUserNumTotal}}</span> &nbsp&nbsp&nbsp
        data.forEach(item=>{
          sums[2]+= item.rechargeAmount
          sums[3]+= item.withdrawalAmount
          sums[4]+= item.yk
          sums[5]+= item.balance
          sums[6]+= (item.redEnvelope || 0)
          sums[7]+= item.memberRewardAmount
          sums[8]+= item.userNum
          sums[9]+= item.newUserNum
        })

        sums = sums.map(item=>{
          if(!isNaN(Number(item)) && item !=='') {
            item = item.toFixed(2)
          }
          return item
        })
        return sums
      },
      /* 修改分页数量 */
      handleSizeChange(val) {
        this.pageSize = val
        this.getData()
      },
      /* 修改分页页数 */
      handleCurrentChange(val) {
        this.pageIndex = val
        this.getData()
      },
      getData() {
        this.$post2('system/user/findDataDetail', {
          deptId: this.deptId,
          userId: this.userId,
          'request': {
            pageSize: this.pageSize,
            pageNum: this.pageIndex
          },
        }).then((r) => {
          this.userData = r.data.data.rows
          this.rechargeAmountTotal = 0;
          this.withdrawalAmountTotal = 0;
          this.rewardAmountTotal =0;
          this.memberRewardAmountTotal=0;
          this.userNumTotal=0;
          this.newUserNumTotal=0;
          for (var i = 0; i < this.userData.length; i++) {
            this.rechargeAmountTotal += this.userData[i].rechargeAmount;
            this.withdrawalAmountTotal += this.userData[i].withdrawalAmount;
            this.rewardAmountTotal += this.userData[i].rewardAmount;
            this.memberRewardAmountTotal += this.userData[i].memberRewardAmount;
            this.userNumTotal += this.userData[i].userNum;
            this.newUserNumTotal += this.userData[i].newUserNum;
            this.ykTotal += this.userData[i].yk;
          }
          this.total = r.data.data.total
        })
      }
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
