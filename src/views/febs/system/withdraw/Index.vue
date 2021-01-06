<template>
  <div class="reward">
    <el-main>

      <div class="filter-container">
        <el-input type="text" class="filter-item search-item" v-model="mobile" placeholder="手机号"/>

        <el-select v-model="status" placeholder="状态">
          <el-option
            v-for="item in statusList"
            :key="item.id"
            :label="item.name"
            :value="item.id"
          />
        </el-select>
        <el-select v-model="deptId" placeholder="归属"  clearable>
          <el-option
            v-for="item in deptList"
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
        style="width: 100%">
        <el-table-column
          align="center"
          label="卡号">
          <template slot-scope="scope">{{scope.row.bankcard}}</template>
        </el-table-column>
        <el-table-column
          align="center"
          label="状态">
          <template slot-scope="scope">{{scope.row.status=='0' ? '申请中' : scope.row.status=='1' ? '已通过': scope.row.status=='2' ?
            '已拒绝':'--'}}
          </template>
        </el-table-column>
        <el-table-column
          align="center"
          label="类型">
          <template slot-scope="scope">{{scope.row.type=='0' ? '余额提现' : scope.row.type=='1' ? '佣金': scope.row.type=='2' ?
            '红包':'--'}}
          </template>
        </el-table-column>
        <el-table-column
          align="center"
          label="手机号">
          <template slot-scope="scope">{{scope.row.mobile}}</template>
        </el-table-column>
        <el-table-column
          align="center"
          label="创建时间">
          <template slot-scope="scope">{{scope.row.createTime}}</template>
        </el-table-column>
        <el-table-column
          align="center"
          label="提款金额">
          <template slot-scope="scope">{{scope.row.withdrawalAmount}}</template>
        </el-table-column>
        <el-table-column
          align="center"
          label="操作">
          <template slot-scope="scope">
            <el-button v-if="scope.row.status=='0'" size="small" type="primary" @click="editItem(scope.row.id, 1)">通过</el-button>
            <el-button v-if="scope.row.status=='0'" size="small" type="primary" @click="editItem(scope.row.id, 2)">拒绝</el-button>
          </template>
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
        status: null,
        userId: null,
        deptId: null,
        deptList: [],
        fromdate: null,
        todate: null,
        total: null,
        pageSize: 10,
        pageIndex: 1,
        userData: [],
        statusList: [
          { name: '申请中', id: '0' },
          { name: '已通过', id: '1' },
          { name: '已拒绝', id: '2' }
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
      editItem (id, state) {
        this.$confirm('确定执行?', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          width: 100,
          type: 'warning'
        }).then(() => {
          this.$post2('system/withdrawInfo/auditWithdrawInfo', {
            id: id,
            audit: state
          }).then((r) => {
            if (r.data.code === '0000') {
              this.getData()
            } else {
              this.$message({
                message: r.data.message,
                type: 'warning',
              })
            }
          })
        }).catch(() => {
          this.$message({
            type: 'info',
            message: '已取消'
          })
        })
      },
      getData() {
        this.$post2('system/withdrawInfo/withdrawInfoList', {
          'request': {
            pageSize: this.pageSize,
            pageNum: this.pageIndex
          },
          'mobile': this.mobile,
          'status': this.status,
          'fromdate': null,
          'todate': null,
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
