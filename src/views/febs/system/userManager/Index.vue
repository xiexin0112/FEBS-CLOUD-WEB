<template>
  <div class="reward">
    <el-main>
      <div class="filter-container">
        <el-input type="text" class="filter-item search-item" v-model="mobile" placeholder="手机号"/>

        <el-date-picker
          v-model="fromdate"
          value-format="yyyy-MM-dd hh:mm:ss"
          type="date"
          placeholder="注册开始日期">
        </el-date-picker>
        <el-date-picker
          v-model="todate"
          value-format="yyyy-MM-dd hh:mm:ss"
          type="date"
          placeholder="注册结束日期">
        </el-date-picker>

        <el-date-picker
          v-model="fromdatelast"
          value-format="yyyy-MM-dd hh:mm:ss"
          type="date"
          placeholder="最后登录开始日期">
        </el-date-picker>
        <el-date-picker
          v-model="todatelast"
          value-format="yyyy-MM-dd hh:mm:ss"
          type="date"
          placeholder="最后登录结束日期">
        </el-date-picker>

        <!-- <el-select v-model="belongUserId" placeholder="推广人员">
          <el-option
            v-for="item in findUserList"
            :key="item.USER_ID"
            :label="item.USERNAME"
            :value="item.USER_ID"
          />
        </el-select> -->
        <el-select v-model="deptId" placeholder="归属"  clearable>
          <el-option
            v-for="item in typeList"
            :key="item.deptId"
            :label="item.deptName"
            :value="item.deptId"
          />
        </el-select>

        <el-button class="filter-item" type="primary" @click="getUser()">查询</el-button>
      </div>
      <el-table
        :data="userData"
        border
        style="width: 100%">
        <el-table-column
          align="center"
          label="手机号">
          <template slot-scope="scope">{{scope.row.mobile}}</template>
        </el-table-column>
        
        <el-table-column
          align="center"
          label="红包">
          <template slot-scope="scope">{{scope.row.redEnvelope}}</template>
        </el-table-column>
        <el-table-column
          align="center"
          label="充值金额">
          <template slot-scope="scope">{{scope.row.rechargeAmount}}</template>
        </el-table-column>
        <el-table-column
          align="center"
          label="提现金额">
          <template slot-scope="scope">{{scope.row.withdrawalAmount}}</template>
        </el-table-column>
        <el-table-column
          align="center"
          label="佣金">
          <template slot-scope="scope">{{scope.row.rewardAmount}}</template>
        </el-table-column>
        <el-table-column
          align="center"
          label="可用余额">
          <template slot-scope="scope">{{scope.row.balance}}</template>
        </el-table-column>
        <el-table-column
          align="center"
          label="所属部门">
          <template slot-scope="scope">{{scope.row.deptName}}</template>
        </el-table-column>
        <el-table-column
          align="center"
          label="注册时间">
          <template slot-scope="scope">{{scope.row.createTime}}</template>
        </el-table-column>
        <el-table-column
          align="center"
          label="最后登录时间">
          <template slot-scope="scope">{{scope.row.lastLoginTime}}</template>
        </el-table-column>
        
        <el-table-column
          align="center"
          label="状态">
          <template slot-scope="scope">{{scope.row.userStatus}}</template>
        </el-table-column>
        <el-table-column
          align="center"
          label="用户名">
          <template slot-scope="scope">{{scope.row.username}}</template>
        </el-table-column>

        <el-table-column
          align="center"
          label="分佣状态">
          <template slot-scope="scope">
            
            {{scope.row.rebateStatus ? scope.row.rewardRate || scope.row.rebateStatus : getRebateText(scope.row.rebateStatus)}}
            <i v-if="!rebateStatus" class="el-icon-edit" @click="rebateDialogVisible=true;setRebateForm.userId=scope.row.userId"></i>
          </template>
        </el-table-column>

        <el-table-column
          align="center"
          label="操作">
          <template slot-scope="scope">
            <el-button v-if="scope.row.userStatus=='正常'" size="small" type="primary" @click="editItem(scope.row.userId, 0)">禁用</el-button>
            <el-button v-else size="small" type="primary" @click="editItem(scope.row.userId, 1)">启用</el-button>
          </template>
        </el-table-column>
        <el-table-column
          align="center"
          label="红包"
          width="110px">
          <template slot-scope="scope">
            <el-button  size="small" type="primary" @click="redItem(scope.row)">发放红包</el-button>
          </template>
        </el-table-column>

        <el-table-column
          align="center"
          label="佣金"
          width="110px">
          <template slot-scope="scope">
            <el-button  size="small" type="primary" @click="rebateItem(scope.row)">发放佣金</el-button>
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
      title="红包"
      :visible.sync="redToogle"
     
    >
      <el-tabs v-model="activeName">
        <el-tab-pane label="发放红包" name="first">

          <div></div>
          <p>金额:</p>
          <el-input type="number" class="filter-item search-item" v-model="redEnvelope" />
          <p>密码:</p>
          <el-input type="password" class="filter-item search-item" v-model="redEnvelopePassword" />
          <br><br><br>

          <div class="dialog-footer">
            <el-button @click="redToogle = false">取 消</el-button>
            <el-button type="primary" @click="redItemConfim()">确 定</el-button>
          </div>

        </el-tab-pane>
        <el-tab-pane label="发放记录"  name="second">
          <el-table
            :data="redList"
            style="width: 100%">
            <el-table-column
              prop="createTime"
              label="发放时间">
            </el-table-column>
            <el-table-column
              prop="addition"
              label="金额">

              <template slot-scope="scope">
                {{ scope.row.addition ? `+${ scope.row.addition }` : `-${scope.row.subtract}` }}
              </template>
            </el-table-column>

          </el-table>
        </el-tab-pane>
      </el-tabs>
    </el-dialog>

    <el-dialog
      title="佣金"
      :visible.sync="rebateToogle"
    >
      <el-tabs v-model="activeName">
        <el-tab-pane label="发放佣金" name="first">
           <el-alert
            :closable="false"
            :title="`应发佣金:${this.rebateInfo.serviceCharge || 0 } \t 比例:${ this.rebateInfo.rewardRate || 0 } \t 已发:${ this.rebateList.length && this.rebateList[0].reward || 0 }`"
            type="info">
          </el-alert>
          <div > </div>
          <!-- <div > </div> -->
          <p>金额:</p>
          <el-input type="number" class="filter-item search-item" v-model="rebateEnvelope" />

          <p>密码:</p>
          <el-input type="password" class="filter-item search-item" v-model="rebateEnvelopePassword" />
          <br><br><br>

          <div class="dialog-footer">
            <el-button @click="rebateToogle = false">取 消</el-button>
            <el-button type="primary" @click="rebateItemConfim()">确 定</el-button>
          </div>

        </el-tab-pane>
        <el-tab-pane label="发放记录"  name="second">
          <el-table
            :data="rebateList"
            style="width: 100%">
            <el-table-column
              prop="createTime"
              label="发放时间">
            </el-table-column>
            <el-table-column
              prop="addition"
              label="金额">

              <template slot-scope="scope">
                {{ scope.row.addition ? `+${ scope.row.addition }` : `-${scope.row.subtract}` }}
              </template>
            </el-table-column>
          </el-table>
        </el-tab-pane>
      </el-tabs>
    </el-dialog>


    <el-dialog title="设置分成" :visible.sync="rebateDialogVisible">
      <el-form>
        <el-form-item label="分成比例">
          <div class="progress-content">
            <el-progress type="dashboard" :percentage="percentage" :color="colors"></el-progress>
            <div class="buttons">
              <el-button-group>
                <el-button icon="el-icon-minus" @click="decrease"></el-button>
                <el-button icon="el-icon-plus" @click="increase"></el-button>
              </el-button-group>
            </div>
          </div>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="rebateDialogVisible = false">取 消</el-button>
        <el-button type="primary" @click="setRebate">确 定</el-button>
      </div>
    </el-dialog>
  </div>
</template>
<script>

  export default {
    name: 'reward',
    data() {
      return {
        activeName: 'first',
        rebateDialogVisible: false,
        redEnvelope: null,
        rebateEnvelope: null,
        redEnvelopePassword: null,
        rebateEnvelopePassword: null,
        mobile: null,
        fromdate: null,
        todate: null,
        fromdatelast: null,
        todatelast: null,
        redToogle: false,
        rebateToogle: false,
        typeList: [],
        total: null,
        pageSize: 10,
        pageIndex: 1,
        userData: [],
        findUserList: [],
        belongUserId: null,
        redUser: null,
        deptId: null,
        setRebateForm: {
          userId: '',
          rewardRate: 0,
          status: true
        },
        percentage: 10,
        colors: [
          {color: '#f56c6c', percentage: 20},
          {color: '#e6a23c', percentage: 40},
          {color: '#5cb87a', percentage: 60},
          {color: '#1989fa', percentage: 80},
          {color: '#6f7ad3', percentage: 100}
        ],
        redList: [],
        rebateList: [],
        rebateInfo: {}
      }
    },
    mounted() {
      this.$get('/system/dept/findDeptLists').then((r) => {
        this.typeList = [ { deptId: '', deptName: '全部' }, ...r.data.data ]
      })
      // this.findUser()
      this.getUser()
    },
    methods: {
      getRebateText(status){
        return ['申请中', '已开通'][status] || '未开通'
      },
      async getRebateRecord(userId){
        const data = await this.$post2('system/rewardInfo/rewardInfoList', {
          userId,
           "request": {
            "pageSize": 40,
            "pageNum": 1
          },
        })
        this.rebateList = data.data.data.pageData.rows
        this.rebateInfo = data.data.data.rebate
      },
      rebateItem (item) {
        this.redUser = item
        this.rebateToogle = true
        this.rebateList = []
        this.activeName = 'first'
        this.getRebateRecord(item.userId)
      },
      rebateItemConfim ( ) {
        if(!this.rebateEnvelope || !this.rebateEnvelopePassword) {
          return false
        }
        this.$post2('system/rewardInfo/addRewardInfo', {
          addition:this.rebateEnvelope,
          userId:this.redUser.userId,
          password: this.rebateEnvelopePassword
        }).then((r) => {
          if (r.data.code === '0000') {
            this.$message({
              message: '已发放',
              type: 'success',
            })
            this.rebateToogle = false
          } else {
            this.$message({
              message: r.data.message,
              type: 'warning',
            })
          }
        })
      },
      async getRedRecord(userId){
        const data = await this.$post2('system/envelopeInfo/getEnvelopeInfosByUserId', {
          userId
        })
        this.redList = data.data.data
      },
      redItem (item) {
        this.redUser = item
        this.redToogle = true
        this.redList = []
        this.activeName = 'first'
        this.getRedRecord(item.userId)
      },
      redItemConfim ( ) {
        if(!this.redEnvelope || !this.redEnvelopePassword) {
          return false
        }
        this.$post2('system/envelopeInfo/createEnvelopeInfo', {
          redEnvelope:this.redEnvelope,
          userId:this.redUser.userId,
          deptId:this.redUser.deptId,
          redEnvelopePassword: this.redEnvelopePassword
        }).then((r) => {
          if (r.data.code === '0000') {
            this.$message({
              message: '已发放',
              type: 'success',
            })
            this.redToogle = false
          } else {
            this.$message({
              message: r.data.message,
              type: 'warning',
            })
          }
        })
      },
      editItem (id, state) {
        this.$confirm('确定要' + (state==0?'禁用':'启用') + '该用户吗?', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          width: 100,
          type: 'warning'
        }).then(() => {
          this.$post2('system/user/updateUserStatus', {
            userId: id,
            status: state
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
        }).catch(() => {
          this.$message({
            type: 'info',
            message: '已取消'
          })
        })
      },
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
      searchClick () {},
      getUser() {
        this.$post2('system/user/findUserListByPage', {
          'request': {
            pageSize: this.pageSize,
            pageNum: this.pageIndex
          },
          mobile: this.mobile,
          belongUserId: this.belongUserId,
          fromdate: this.fromdate,
          todate: this.todate,
          fromdatelast: this.fromdatelast,
          todatelast: this.todatelast,
          deptId: this.deptId
        }).then((r) => {
          this.userData = r.data.data.rows
          this.total = r.data.data.total
        })
      },
      // findUser() {
      //   this.$post2('system/user/findUserByDeptId', {
      //   }).then((r) => {
      //     this.findUserList = [ { USER_ID: '', USERNAME: "全部" }, ...r.data.data ]
      //   })
      // },

      async setRebate(){
      
        this.$post2('system/standRewardInfo/addStandRewardInfo', this.setRebateForm).
        then((r) => {
          this.rebateDialogVisible = false
          if (r.data.code === '0000') {
            this.getUser()
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
      increase() {
        this.percentage += 10;
        if (this.percentage > 100) {
          this.percentage = 100;
        }
        this.setRebateForm.rewardRate = this.percentage / 100
      },
      decrease() {
        this.percentage -= 10;
        if (this.percentage < 0) {
          this.percentage = 0;
        }
        this.setRebateForm.rewardRate = this.percentage / 100
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
  .progress-content {
    padding-left: 20px;
    display: flex;
    align-items: center;
    .buttons {
      margin-left: 20px;
    }
  }
</style>
