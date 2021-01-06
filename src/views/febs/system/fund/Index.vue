<template>
  <div class="reward">
    <el-main>

      <div class="filter-container">
        <el-input type="text" class="filter-item search-item" v-model="mobile" placeholder="手机号"/>

        <el-select v-model="type" placeholder="类型">
          <el-option
            v-for="item in typeList"
            :key="item.id"
            :label="item.name"
            :value="item.id"
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
        <el-select v-model="deptId" placeholder="归属"  clearable>
          <el-option
            v-for="item in deptList"
            :key="item.deptId"
            :label="item.deptName"
            :value="item.deptId"
          />
        </el-select>

        <el-button class="filter-item" type="primary" @click="getData()">查询</el-button>
      </div>
      <el-table
        :data="userData"
        border
        style="width: 100%">
        <el-table-column
          align="center"
          label="资金增加">
          <template slot-scope="scope">{{scope.row.addition}}</template>
        </el-table-column>
        <el-table-column
          align="center"
          label="资金减少">
          <template slot-scope="scope">{{scope.row.subtract}}</template>
        </el-table-column>
        <el-table-column
          align="center"
          label="类型">
          <template slot-scope="scope">{{scope.row.type=='1' ? '充值' : scope.row.type=='2' ? '游戏': scope.row.type=='3' ?
            '红包':scope.row.type=='4' ? '返佣':scope.row.type=='5' ? '邀请返现':'开户'}}
          </template>
        </el-table-column>
        <el-table-column
          align="center"
          label="余额">
          <template slot-scope="scope">{{scope.row.balance}}</template>
        </el-table-column>
        <el-table-column
          align="center"
          label="创建时间">
          <template slot-scope="scope">{{scope.row.create_time}}</template>
        </el-table-column>
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
        mobile: '',
        type: null,
        userId: null,
        deptId: null,
        deptList: [],
        fromdate: null,
        todate: null,
        total: null,
        pageSize: 10,
        pageIndex: 1,
        userData: [],
        typeList: [
          { name: '充值', id: '1' },
          { name: '游戏', id: '2' },
          { name: '红包', id: '3' },
          { name: '返佣', id: '4' },
          { name: '邀请返现', id: '5' }
        ]
      }
    },
    mounted() {
      this.$get('/system/dept/findDeptLists').then((r) => {
        this.deptList = r.data.data
      })
      this.getData()
    },
    methods: {
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
        this.$post2('system/accountInfo/findAccountList', {
          'request': {
            pageSize: this.pageSize,
            pageNum: this.pageIndex
          },
          'type': this.type,
          'mobile': this.mobile,
          'fromdate': this.fromdate,
          'todate': this.todate,
          deptId: this.deptId
        }).then((r) => {
          this.userData = r.data.data.rows
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
