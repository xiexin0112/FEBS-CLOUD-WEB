<template>
  <div class="order">
    <el-main>
      <el-table
        :data="dataList"
        border
        style="width: 100%">
        <el-table-column
          type="index"
          align="center"
          label="序号">
        </el-table-column>
        <el-table-column
          align="center"
          label="用户id">
          <template slot-scope="scope">{{scope.row.userId}}</template>
        </el-table-column>
        <el-table-column
          align="center"
          label="邀请人数">
          <template slot-scope="scope">{{scope.row.inverstNum}}</template>
        </el-table-column>
        <el-table-column
          align="center"
          label="红包">
          <template slot-scope="scope">{{scope.row.reward }}</template>
        </el-table-column>
        <el-table-column
          align="center"
          label="提成比例">
          <template slot-scope="scope">{{scope.row.rewardRate }}</template>
        </el-table-column>
        <!--<el-table-column-->
          <!--align="center"-->
          <!--label="rewardWithdraw">-->
          <!--<template slot-scope="scope">{{scope.row.rewardWithdraw }}</template>-->
        <!--</el-table-column>-->
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
    name: 'order',
    data() {
      return {
        total: null,
        pageSize: 10,
        pageIndex: 1,
        dataList: []
      }
    },
    mounted() {
      this.initOrder()
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
      initOrder() {
        let user = JSON.parse(localStorage.getItem('USER'))
        this.$post2('system/rewardInfo/rewardInfoList', {
          'request': {
            pageSize: this.pageSize,
            pageNum: this.pageIndex
          },
          // 'userId': user.userId
        }).then((r) => {
          this.dataList = r.data.data.rows
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
