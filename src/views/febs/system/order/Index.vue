<template>
  <div class="order">
      
    <el-main>
      <div class="filter-container">
        <el-input type="text" class="filter-item search-item" v-model="mobile" placeholder="手机号"/>
        <el-input type="text" class="filter-item search-item" v-model="price" placeholder="金额"/>
        <el-input type="text" class="filter-item search-item" v-model="betNo" placeholder="期号"/>
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
        <el-button class="filter-item" type="primary" @click="getUser()">查询</el-button>
      </div>
      <el-table
        :data="orderData"
        border
        style="width: 100%">
        <el-table-column
          align="center"
          label="id">
          <template slot-scope="scope">{{scope.row.id}}</template>
        </el-table-column>
        <el-table-column
          align="center"
          label="类型">
          <template slot-scope="scope">{{scope.row.title}}</template>
        </el-table-column>
        <el-table-column
          align="center"
          label="金额">
          <template slot-scope="scope">{{scope.row.orderPrice}}</template>
        </el-table-column>
        <el-table-column
          align="center"
          label="订单金额">
          <template slot-scope="scope">{{scope.row.delivery}}</template>
        </el-table-column>
        <el-table-column
          align="center"
          label="费用">
          <template slot-scope="scope">{{scope.row.fee}}</template>
        </el-table-column>
        <el-table-column
          align="center"
          label="盈利">
          <template slot-scope="scope">{{scope.row.amount}}</template>
        </el-table-column>

        <el-table-column
          align="center"
          label="时间">
          <template slot-scope="scope">{{scope.row.createTime}}</template>
        </el-table-column>
        <el-table-column
          align="center"
          label="用户id">
          <template slot-scope="scope">{{scope.row.userId}}</template>
        </el-table-column>
        <el-table-column
          align="center"
          label="开奖数字">
          <template slot-scope="scope">{{scope.row.resultNumber}}</template>
        </el-table-column>
        <el-table-column
          align="center"
          label="开奖结果">
          <template slot-scope="scope">
            <div class="point-box__anodd"
                 v-if="scope.row.resultNumber=='1' || scope.row.resultNumber=='3'||scope.row.resultNumber=='5'||scope.row.resultNumber=='7'||scope.row.resultNumber=='9'"></div>
            <!--红色-->
            <div class="point-box__aneven"
                 v-if="scope.row.resultNumber=='0'||scope.row.resultNumber=='2'||scope.row.resultNumber=='4' || scope.row.resultNumber=='6' ||scope.row.resultNumber=='8'"></div>
            <!--5 red和violet 0是green和violet-->
            <div class="point-box__aliquot" v-if="scope.row.resultNumber=='0'||scope.row.resultNumber=='5'"></div>
            <!--{{scope.row.resultColor}}-->

          </template>
        </el-table-column>

        <el-table-column
          align="center"
          label="状态">
          <template slot-scope="scope">{{scope.row.statu}}</template>
        </el-table-column>

        <el-table-column
          align="center"
          label="期号">
          <template slot-scope="scope">{{scope.row.betId}}</template>
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
    name: 'order',
    data() {
      return {
        betNo: null,
        mobile: null,
        price: null,
        fromdate: null,
        todate: null,
        total: null,
        pageSize: 10,
        pageIndex: 1,
        orderData: []
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
        this.$post2('system/orderInfo/findOrderList', {
          mobile: this.mobile,
          betNo: this.betNo,
          price: this.price,
          fromdate: this.fromdate,
          todate: this.todate,
          'request': {
            pageSize: this.pageSize,
            pageNum: this.pageIndex
          }
        }).then((r) => {
          this.orderData = r.data.data.rows
          this.total = r.data.data.total
        })
      }
    }
  }
</script>
<style lang="scss" scoped>
  .point-box__anodd {
    width: 15px;
    height: 15px;
    border-radius: 50%;
    background-color: #1eb83f;
    display: inline-block;
  }

  .point-box__aneven {
    width: 15px;
    height: 15px;
    border-radius: 50%;
    background-color: #f52926;
    display: inline-block;
  }

  .point-box__aliquot {
    width: 15px;
    height: 15px;
    border-radius: 50%;
    background-color: blueviolet;
    margin-left: 5px;
    display: inline-block;
  }

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
