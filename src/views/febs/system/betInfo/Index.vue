<template>
  <div class="order">
    <el-main>
      <p>&nbsp</p>
      <el-table
        :data="dataList"
        border
        style="width: 100%">
        <el-table-column
          align="center"
          label="开奖依据">
          <template slot-scope="scope">{{scope.row.beforeResult}}</template>
        </el-table-column>
        <el-table-column
          align="center"
          label="期号">
          <template slot-scope="scope">{{scope.row.betNo}}</template>
        </el-table-column>
        <el-table-column
          align="center"
          label="开奖数字">
          <template slot-scope="scope">{{scope.row.result}}</template>
        </el-table-column>
        <el-table-column
          align="center"
          label="投注金额">
          <template slot-scope="scope">{{scope.row.price}}</template>
        </el-table-column>
        <el-table-column
          align="center"
          label="投注人数">
          <template slot-scope="scope">{{scope.row.allnum}}</template>
        </el-table-column>
        <el-table-column
          align="center"
          label="中奖金额">
          <template slot-scope="scope">{{scope.row.amount}}</template>
        </el-table-column>
        <el-table-column
          align="center"
          label="中奖人数">
          <template slot-scope="scope">{{scope.row.num}}</template>
        </el-table-column>


        <el-table-column
          align="center"
          label="平台盈亏">
          <template slot-scope="scope">{{scope.row.benefit}}</template>
        </el-table-column>

        <el-table-column
          align="center"
          label="结果值">
          <template slot-scope="scope">
            <!--{{scope.row.color}}-->
            <!--绿色-->
            <div class="point-box__anodd"
                 v-if="scope.row.result=='1' || scope.row.result=='3'||scope.row.result=='5'||scope.row.result=='7'||scope.row.result=='9'"></div>
            <!--红色-->
            <div class="point-box__aneven"
                 v-if="scope.row.result=='0'||scope.row.result=='2'||scope.row.result=='4' || scope.row.result=='6' ||scope.row.result=='8'"></div>
            <!--5 red和violet 0是green和violet-->
            <div class="point-box__aliquot" v-if="scope.row.result=='0'||scope.row.result=='5'"></div>
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
          :page-sizes="[5,10,20]"
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
        typeList: [
          {name: 'loni', id: '1'},
          {name: 'parity', id: '2'},
          {name: 'sapre', id: '3'},
          {name: 'bcone', id: '4'}
        ],
        type: '1',
        total: null,
        pageSize: 20,
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
        this.$post2('system/betInfo/findplantformBetInfoByPage', {
          'request': {
            pageSize: this.pageSize,
            pageNum: this.pageIndex
          },
          'type': 1
        }).then((r) => {
          this.dataList = r.data.data.rows
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
