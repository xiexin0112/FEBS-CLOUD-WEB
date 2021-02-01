<template>
  <div class="order">
    <el-main>
      <div class="filter-container">
        <el-select v-model="userId" placeholder="代理商" clearable @change="initOrder()">
          <el-option
            label="全部"
            value=""
          />
          <el-option
            v-for="item in agentLists"
            :key="item.userId"
            :label="item.userName"
            :value="item.userId"
          />
        </el-select>
        <el-button class="filter-item" type="primary" @click="addDialogVisible=true">新增</el-button>
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
          label="分成比例"
        >
          <template slot-scope="scope">{{ scope.row.shareRatio }}</template>
        </el-table-column>
        <el-table-column
          align="center"
          label="代理人电话"
        >
          <template slot-scope="scope">{{ scope.row.mobile }}</template>
        </el-table-column>
        <el-table-column
          align="center"
          label="代理人名称"
        >
          <template slot-scope="scope">{{ scope.row.userName }}</template>
        </el-table-column>
        <el-table-column
          align="center"
          label="状态"
        >
          <template slot-scope="scope">{{ scope.row.status ? '有效' : '无效' }}</template>
        </el-table-column>
        <el-table-column
          align="center"
          label="开设时间"
        >
          <template slot-scope="scope">{{ scope.row.createTime }}</template>
        </el-table-column>

        <el-table-column
          align="center"
          label="操作">
          <template slot-scope="scope">
            <el-button size="small" type="primary" @click="remove(scope.row.id)">删除</el-button>
          </template>
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
          <el-select v-model="addForm.userId" placeholder="请选择" clearable>
            <el-option
              v-for="item in agentLists"
              :key="item.userId"
              :label="item.userName"
              :value="item.userId"
            />
          </el-select>
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
        this.agentLists = [ ...r.data.data]
      })
    },
    initOrder() {
      this.$post2('system/deptRewardInfo/deptRewardInfoList', {
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
    async add(){
      if (!this.addForm.userId || !this.addForm.agentName || !this.addForm.shareRatio) {
        return false
      }
      this.$post2('system/deptRewardInfo/addDeptRewardInfo', this.addForm).
      then((r) => {
        this.addDialogVisible = false
        if (r.data.code === '0000') {
          this.$message({
            message: 'success',
            type: 'success'
          })
        } else {
          this.$message({
            message: r.data.message,
            type: 'warning',
          })
        }
      })
    },
    async remove(id){
      await this.$post2('system/deptRewardInfo/deleteDeptRewardInfo', { id })
      this.$message({
        message: 'success',
        type: 'success',
      })
      this.initOrder()
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
