<template>
  <div class="reward">
    <el-main>

      <div class="filter-container">
        <el-date-picker
          v-model="queryTime"
          value-format="yyyy-MM-dd hh:mm:ss"
          type="daterange"
          range-separator="至"
          start-placeholder="开始日期"
          end-placeholder="结束日期"
        />
        <el-select v-model="typeid" placeholder="归属" clearable @change="changeType()">
          <el-option
            v-for="item in typeList"
            :key="item.deptId"
            :label="item.deptName"
            :value="item.deptId"
          />
        </el-select>
        <el-select v-model="userid" placeholder="推广人员">
          <el-option
            v-for="item in userData"
            :key="item.deptId"
            :label="item.deptName"
            :value="item.deptId"
          />
        </el-select>
        <el-button class="filter-item" type="primary" @click="searchClick(1,'findTeacherRH')">查看日活</el-button>
        <el-button class="filter-item" type="primary" @click="searchClick(2,'findTeacherCZ')">充值状态</el-button>
        <el-button class="filter-item" type="primary" @click="searchClick(3,'findTeacherSC')">查看首充</el-button>
        <el-button class="filter-item" type="primary" @click="searchClick(4,'findTeacherFC')">查看复充</el-button>
        <el-button class="filter-item" type="primary" @click="searchClick(5,'findTeacherYL')">盈利排名</el-button>
        <el-button class="filter-item" type="primary" @click="searchClick(6,'findTeacherKS')">亏损排名</el-button>
      </div>

      <div v-if="showType==1">
        <h3>日活列表：</h3>
        <el-table
          :data="dataList1"
          border
          style="width: 100%"
        >
          <el-table-column
            align="center"
            label="日期"
          >
            <template slot-scope="scope">{{ scope.row.dateNo }}</template>
          </el-table-column>
          <el-table-column
            align="center"
            label="游戏人数"
          >
            <template slot-scope="scope">{{ scope.row.num }}</template>
          </el-table-column>
          <el-table-column
            align="center"
            label="人均/期"
          >
            <template slot-scope="scope">{{ scope.row.avgperiod }}</template>
          </el-table-column>
          <el-table-column
            align="center"
            label="人均消费"
          >
            <template slot-scope="scope">{{ scope.row.avgprice }}</template>
          </el-table-column>
        </el-table>
      </div>
      <div v-if="showType==2">

        <h3>充值动态：</h3>
        <el-table
          :data="dataList2"
          border
          style="width: 100%"
        >
          <el-table-column
            align="center"
            label="日期"
          >
            <template slot-scope="scope">{{ scope.row.dateNo }}</template>
          </el-table-column>
          <el-table-column
            align="center"
            label="充值人数"
          >
            <template slot-scope="scope">{{ scope.row.num }}</template>
          </el-table-column>
          <el-table-column
            align="center"
            label="人均/次"
          >
            <template slot-scope="scope">{{ scope.row.avgperiod }}</template>
          </el-table-column>
          <el-table-column
            align="center"
            label="人均/金额"
          >
            <template slot-scope="scope">{{ scope.row.avgprice }}</template>
          </el-table-column>
        </el-table>
      </div>
      <div v-if="showType==3">
        <h3>首充列表：</h3>
        <el-table
          :data="dataList3"
          border
          style="width: 100%"
        >
          <el-table-column
            align="center"
            label="日期"
          >
            <template slot-scope="scope">{{ scope.row.dateNo }}</template>
          </el-table-column>
          <el-table-column
            align="center"
            label="充值人数"
          >
            <template slot-scope="scope">{{ scope.row.num }}</template>
          </el-table-column>
          <el-table-column
            align="center"
            label="人均/次"
          >
            <template slot-scope="scope">{{ scope.row.avgperiod }}</template>
          </el-table-column>
          <el-table-column
            align="center"
            label="人均/金额"
          >
            <template slot-scope="scope">{{ scope.row.avgprice }}</template>
          </el-table-column>
        </el-table>
      </div>
      <div v-if="showType==4">
        <h3>复充列表：</h3>
        <el-table
          :data="dataList4"
          border
          style="width: 100%"
        >
          <el-table-column
            align="center"
            label="日期"
          >
            <template slot-scope="scope">{{ scope.row.dateNo }}</template>
          </el-table-column>
          <el-table-column
            align="center"
            label="充值人数"
          >
            <template slot-scope="scope">{{ scope.row.num }}</template>
          </el-table-column>
          <el-table-column
            align="center"
            label="人均/次"
          >
            <template slot-scope="scope">{{ scope.row.avgperiod }}</template>
          </el-table-column>
          <el-table-column
            align="center"
            label="人均/金额"
          >
            <template slot-scope="scope">{{ scope.row.avgprice }}</template>
          </el-table-column>
        </el-table>
      </div>
      <div v-if="showType==5">

        <h3>盈利排名：</h3>
        <el-table
          :data="dataList5"
          border
          style="width: 100%"
        >
          <el-table-column
            align="center"
            label="亏损"
          >
            <template slot-scope="scope">{{ scope.row.amount }}</template>
          </el-table-column>
          <el-table-column
            align="center"
            label="手机号"
          >
            <template slot-scope="scope">{{ scope.row.mobile }}</template>
          </el-table-column>
          <el-table-column
            align="center"
            label="名称"
          >
            <template slot-scope="scope">{{ scope.row.userName }}</template>
          </el-table-column>
          <el-table-column
            align="center"
            label="用户id"
          >
            <template slot-scope="scope">{{ scope.row.userId }}</template>
          </el-table-column>
        </el-table>
      </div>
      <div v-if="showType==6">

        <h3>亏损排名：</h3>
        <el-table
          :data="dataList6"
          border
          style="width: 100%"
        >
          <el-table-column
            align="center"
            label="盈利"
          >
            <template slot-scope="scope">{{ scope.row.amount }}</template>
          </el-table-column>
          <el-table-column
            align="center"
            label="手机号"
          >
            <template slot-scope="scope">{{ scope.row.mobile }}</template>
          </el-table-column>
          <el-table-column
            align="center"
            label="名称"
          >
            <template slot-scope="scope">{{ scope.row.userName }}</template>
          </el-table-column>
          <el-table-column
            align="center"
            label="用户id"
          >
            <template slot-scope="scope">{{ scope.row.userId }}</template>
          </el-table-column>
        </el-table>
      </div>

    </el-main>

  </div>
</template>
<script>

export default {
  name: 'Reward',
  data() {
    return {
      userid: null,
      typeid: null,
      queryTime: null,
      userList: [],
      typeList: [],
      userData: [],
      dataList1: [],
      dataList2: [],
      dataList3: [],
      dataList4: [],
      dataList5: [],
      dataList6: [],
      showType: null
    }
  },
  mounted() {
    this.$get('/system/dept/findDeptLists').then((r) => {
      this.typeList = r.data.data
    })
    this.queryTime = [new Date(new Date().setMonth(new Date().getMonth() - 1)), new Date()]
    this.searchClick(1, 'findTeacherRH')
  },
  methods: {
    changeType() {
      this.$get('system/user/findUserByDeptId?deptId=' + this.typeid).then((r) => {
        this.userData = r.data.data
      })
    },
    searchClick(num, url) {
      this.showType = num
      this.$post2('/system/statisticsInfo/' + url, {
        'request': {
          pageSize: 10000,
          pageNum: 1
        },
        'deptId': this.typeid,
        'userId': this.userid,
        'fromdate': this.queryTime ? this.queryTime[0] : null,
        'todate': this.queryTime ? this.queryTime[1] : null
      }).then((r) => {
        if (num == 1) {
          this.dataList1 = r.data.data.rows
        } else if (num == 2) {
          this.dataList2 = r.data.data.rows
        } else if (num == 3) {
          this.dataList3 = r.data.data.rows
        } else if (num == 4) {
          this.dataList4 = r.data.data.rows
        } else if (num == 5) {
          this.dataList5 = r.data.data.rows
        } else if (num == 6) {
          this.dataList6 = r.data.data.rows
        }
      })
    }
  }
}
</script>
<style lang="scss" scoped>
  .filter-container{
    .filter-item{
      margin-bottom: 0!important;
    }
  }
  .table_wrap{
    display: flex;
    align-items: center;
    flex-wrap: wrap;
    > div{
      width: 100%;
      margin: 30px  auto;
      border: 1px solid grey;
      padding: 20px;
      overflow-y: auto;
    }
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
