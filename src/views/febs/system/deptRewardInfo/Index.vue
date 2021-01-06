<template>
  <div class="order">
    <el-main>
      <el-select v-model="deptId" placeholder="代理商" clearable @change="initOrder()">
        <el-option
          v-for="item in deptList"
          :key="item.deptId"
          :label="item.deptName"
          :value="item.deptId"
        />
      </el-select>
      <p>&nbsp</p>
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
          label="返佣"
        >
          <template slot-scope="scope">{{ scope.row.benefit }}</template>
        </el-table-column>
        <el-table-column
          align="center"
          label="提成比例"
        >
          <template slot-scope="scope">{{ scope.row.withRate }}</template>
        </el-table-column>
        <el-table-column
          align="center"
          label="资金增加"
        >
          <template slot-scope="scope">{{ scope.row.addition }}</template>
        </el-table-column>
        <el-table-column
          align="center"
          label="资金减少"
        >
          <template slot-scope="scope">{{ scope.row.subtract }}</template>
        </el-table-column>
        <el-table-column
          align="center"
          label="时间"
        >
          <template slot-scope="scope">{{ scope.row.createTime }}</template>
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
  </div>
</template>
<script>

export default {
  name: 'Order',
  data() {
    return {
      deptList: [],
      deptId: null,
      total: null,
      pageSize: 5,
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
      this.$get('system/dept/findDeptLists').then((r) => {
        this.deptList = r.data.data
      })
    },
    initOrder() {
      this.$post2('system/deptRewardInfo/deptRewardInfoList', {
        'request': {
          pageSize: this.pageSize,
          pageNum: this.pageIndex
        },
        'deptId': this.deptId ? this.deptId : null
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
