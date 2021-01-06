<template>
  <div class="reward">
    <el-main>
      <div class="filter-container">
        <el-input type="number" class="filter-item search-item" v-model="item.levelNum" placeholder="levelNum"/>
        <el-input type="number" class="filter-item search-item" v-model="item.inviteNum" placeholder="inviteNum"/>
        <el-input type="number" class="filter-item search-item" v-model="item.rewardRate" placeholder="rewardRate"/>
        <el-input type="number" class="filter-item search-item" v-model="item.rewardAccount" placeholder="rewardAccount"/>
        <el-button class="filter-item" type="primary" @click="add()">新增</el-button>
      </div>
      <el-table
        :data="userData"
        border
        style="width: 100%">
        <el-table-column
          type="index"
          prop="categoryName"
          align="center"
          label="序号">
        </el-table-column>
        <el-table-column
          align="center"
          label="时间">
          <template slot-scope="scope">{{scope.row.createTime}}</template>
        </el-table-column>
        <el-table-column
          align="center"
          label="邀请人数">
          <template slot-scope="scope">{{scope.row.inviteNum}}</template>
        </el-table-column>
        <el-table-column
          align="center"
          label="级别">
          <template slot-scope="scope">{{scope.row.levelNum}}</template>
        </el-table-column>
        <el-table-column
          align="center"
          label="奖励金额">
          <template slot-scope="scope">{{scope.row.rewardAccount}}</template>
        </el-table-column>
        <el-table-column
          align="center"
          label="奖励比例">
          <template slot-scope="scope">{{scope.row.rewardRate}}</template>
        </el-table-column>
        <el-table-column
          align="center"
          label="编辑">
          <template slot-scope="scope">
            <el-button size="small" type="primary" @click="editItem(scope.row)">编辑</el-button>
            <el-button size="small" type="danger" @click="deleteItem(scope.row)">删除</el-button>

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
    <el-dialog
      title="编辑"
      :visible.sync="editToogle"
      width="30%"
      >
      <p>levelNum:</p>
      <el-input type="number" class="filter-item search-item" v-model="edit.levelNum" placeholder="levelNum"/>
      <p>inviteNum:</p>
      <el-input type="number" class="filter-item search-item" v-model="edit.inviteNum" placeholder="inviteNum"/>
      <p>rewardRate:</p>
      <el-input type="number" class="filter-item search-item" v-model="edit.rewardRate" placeholder="rewardRate"/>
      <p>rewardAccount:</p>
      <el-input type="number" class="filter-item search-item" v-model="edit.rewardAccount" placeholder="rewardAccount"/>
      <span slot="footer" class="dialog-footer">
    <el-button @click="editToogle = false">取 消</el-button>
    <el-button type="primary" @click="editConfim()">确 定</el-button>
  </span>
    </el-dialog>
  </div>
</template>
<script>

  export default {
    name: 'reward',
    data() {
      return {
        edit: {},
        editToogle: false,
        item: {
          levelNum: null,
          inviteNum: null,
          rewardRate: null,
          rewardAccount: null
        },
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
      handleSizeChange(val) {
        this.pageSize = val
        this.getUser()
      },
      /* 修改分页页数 */
      handleCurrentChange(val) {
        this.pageIndex = val
        this.getUser()
      },
      editItem(item) {
        this.edit = item
        this.editToogle = true
      },
      deleteItem (i) {
        this.$post2('system/standRewardInfo/deleteStandRewardInfo', {
         id: i.id
        }).then((r) => {
          if (r.data.code === '0000') {
            this.getUser()
          } else {
            this.$message({
              message: r.data.message,
              type: 'warning',
            })
          }
        })
      },
      editConfim () {
        this.$post2('system/standRewardInfo/updateStandRewardInfo', this.edit).
        then((r) => {
          if (r.data.code === '0000') {
            this.editToogle = false
            this.getUser()
          } else {
            this.$message({
              message: r.data.message,
              type: 'warning',
            })
          }
        })
      },
    add() {
        this.$post2('system/standRewardInfo/addStandRewardInfo', {
          levelNum: this.item.levelNum,
          inviteNum: this.item.inviteNum,
          rewardRate: this.item.rewardRate,
          rewardAccount: this.item.rewardAccount
      }).
        then((r) => {
          if (r.data.code === '0000') {
            this.item = {
              levelNum: null,
              inviteNum: null,
              rewardRate: null,
              rewardAccount: null
            }
            this.getUser()
          } else {
            this.$message({
              message: r.data.message,
              type: 'warning',
            })
          }
        })
      },
      getUser() {
        this.$post2('system/standRewardInfo/findStandRewardList', {
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
