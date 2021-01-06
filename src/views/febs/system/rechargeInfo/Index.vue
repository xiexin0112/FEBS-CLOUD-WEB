<template>
  <div class="reward">
    <el-main>
      <div class="filter-container">
        <el-input class="filter-item search-item" v-model="userId" placeholder="用户名"/>
        <el-input type="number" class="filter-item search-item" v-model="addition" placeholder="金额"/>
        <el-button class="filter-item" type="primary" @click="add()">确定</el-button>
      </div>
    </el-main>

  </div>
</template>
<script>

  export default {
    name: 'reward',
    data() {
      return {
        userId: null, //	必填项	number	userId
        addition: null,	//必填项	number	充值金额数字
        subtract: 0 //	必填项	number	减少金额数字默认给我传0回来
      }
    },
    mounted() {
    },
    methods: {
      add() {
        if (!this.userId || !this.addition) {
          return false
        }
        this.$post2('system/accountInfo/rechargeTeacherAccount', {
          userId: this.userId,
          addition: this.addition,
          subtract: this.subtract
        }).
        then((r) => {
          if (r.data.code === '0000') {
            this.$message({
              message: 'success',
              type: 'success',
            })
          } else {
            this.$message({
              message: r.data.message,
              type: 'warning',
            })
          }
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
