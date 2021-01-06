<template>
  <div class="member">
    <el-main>
      <el-table
        :data="userData"
        border
        style="width: 100%">
        <el-table-column
          type="index"
          prop="categoryName"
          align="center"
          label="index">
        </el-table-column>
        <el-table-column
          align="center"
          label="userId">
          <template slot-scope="scope">{{scope.row.userId}}</template>
        </el-table-column>
        <el-table-column
          align="center"
          label="createTime">
          <template slot-scope="scope">{{scope.row.createTime}}</template>
        </el-table-column>
        <el-table-column
          align="center"
          label="username">
          <template slot-scope="scope">{{scope.row.username}}</template>
        </el-table-column>
        <el-table-column
          align="center"
          label="password">
          <template slot-scope="scope">{{scope.row.password}}</template>
        </el-table-column>
        <el-table-column
          align="center"
          label="mobile">
          <template slot-scope="scope">{{scope.row.mobile}}</template>
        </el-table-column>
        <el-table-column
          align="center"
          label="deptName">
          <template slot-scope="scope">{{scope.row.deptName}}</template>
        </el-table-column>
        <el-table-column
          align="center"
          label="qrcode">
          <template slot-scope="scope">{{scope.row.qrcode}}</template>
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
  name: 'member',
  data() {
    return {
      total: null,
      pageSize: 10,
      pageIndex: 1,
      userData: []
    }
  },
  mounted() {
    this.getUser()
  },
  methods: {
    /* 修改分页数量 */
    handleSizeChange (val) {
      this.pageSize = val
      this.getUser()
    },
    /* 修改分页页数 */
    handleCurrentChange (val) {
      this.pageIndex = val
      this.getUser()
    },
    getUser () {
      this.$post2('system/orderInfo/findOrderList', {
        'request': {
          pageSize: this.pageSize,
          pageNum: this.pageIndex
        }
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
