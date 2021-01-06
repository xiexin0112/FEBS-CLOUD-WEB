<template>
  <div class="order">
    <el-main>
      <el-form :inline="true" class="demo-form-inline">
        <el-form-item label="当前控杀比">
          <el-input v-model="controlNumber" type="number" placeholder="当前控杀比" />
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="setontrolNumber()">设置</el-button>
        </el-form-item>
        <!--<el-form-item>-->
        <!--<el-select v-model="type" placeholder="类型" @change="initOrder()" clearable>-->
        <!--<el-option-->
        <!--v-for="item in typeList"-->
        <!--:key="item.id"-->
        <!--:label="item.name"-->
        <!--:value="item.id"/>-->
        <!--</el-select>-->
        <!--</el-form-item>-->
      </el-form>
      <div class="filter-container" style="display: flex;align-items: center;justify-content: space-between">
        <h2>
          <span>当前期号：{{ data.betNo }}</span> &nbsp <span>投注人数：{{ data.orderCount }}</span>&nbsp <span>投注金额：{{ data.orderPrice }}</span>&nbsp
        </h2>
        <h2 style="color: #0088ff">{{ countDown ? countDown: '' }} &nbsp {{ numTime >= 30 ?'投注中' : (numTime < 30 && numTime
          >0)?'计算结果':'已结束' }} &nbsp 系统时间 {{ time | parseTime('{y}-{m}-{d} {h}:{i}:{s}') }} </h2>

      </div>
      <div style="margin-top: 15px;">
        <el-input v-model="beforeResult" placeholder="请输入预约号(0~9)" type="number">
          <el-button slot="append" :disabled="numTime<=30" @click="beforeClick()">更新预约号</el-button>
        </el-input>
      </div>
      <div class="list">
        <div class="li">
          <div class="item">
            <h4>下注情况：</h4>
          </div>
          <div class="item">
            <h4>金额：</h4>
          </div>
        </div>
        <div class="li">
          <div class="item">
            <span class="box violet">0</span> &nbsp {{ data.num0OrderCount }} / 次
          </div>
          <div class="item">{{ data.num0OrderPrice }}</div>
        </div>
        <div class="li">
          <div class="item">
            <span class="box green">1</span> &nbsp {{ data.num1OrderCount }} / 次
          </div>
          <div class="item">{{ data.num1OrderPrice }}</div>
        </div>
        <div class="li">
          <div class="item">
            <span class="box red">2</span> &nbsp {{ data.num2OrderCount }} / 次
          </div>
          <div class="item">{{ data.num2OrderPrice }}</div>
        </div>
        <div class="li">
          <div class="item">
            <span class="box green">3</span> &nbsp {{ data.num3OrderCount }} / 次
          </div>
          <div class="item">{{ data.num3OrderPrice }}</div>
        </div>
        <div class="li">
          <div class="item">
            <span class="box red">4</span> &nbsp {{ data.num4OrderCount }} / 次
          </div>
          <div class="item">{{ data.num4OrderPrice }}</div>
        </div>
        <div class="li">
          <div class="item">
            <span class="box violet">5</span> &nbsp {{ data.num5OrderCount }} / 次
          </div>
          <div class="item">{{ data.num5OrderPrice }}</div>
        </div>
        <div class="li">
          <div class="item">
            <span class="box red">6</span> &nbsp {{ data.num6OrderCount }} / 次
          </div>
          <div class="item">{{ data.num6OrderPrice }}</div>
        </div>
        <div class="li">
          <div class="item">
            <span class="box green">7</span> &nbsp {{ data.num7OrderCount }} / 次
          </div>
          <div class="item">{{ data.num7OrderPrice }}</div>
        </div>
        <div class="li">
          <div class="item">
            <span class="box red">8</span> &nbsp {{ data.num8OrderCount }} / 次
          </div>
          <div class="item">{{ data.num8OrderPrice }}</div>
        </div>
        <div class="li">
          <div class="item">
            <span class="box green">9</span> &nbsp {{ data.num9OrderCount }} / 次
          </div>
          <div class="item">{{ data.num9OrderPrice }}</div>
        </div>
        <div class="li">
          <div class="item">
            <span class="box red">R</span> &nbsp {{ data.redOrderCount }} / 次
          </div>
          <div class="item">{{ data.redOrderPrice }}</div>
        </div>
        <div class="li">
          <div class="item">
            <span class="box green">G</span> &nbsp {{ data.greenOrderCount }} / 次
          </div>
          <div class="item">{{ data.greenOrderPrice }}</div>
        </div>
        <div class="li">
          <div class="item">
            <span class="box violet">V</span> &nbsp {{ data.violetOrderCount }} / 次
          </div>
          <div class="item">{{ data.violetOrderPrice }}</div>
        </div>
      </div>
      <!--<el-table-->
      <!--style="width: 100%">-->
      <!--<el-table-column-->
      <!--type="index"-->
      <!--align="center"-->
      <!--label="index">-->
      <!--</el-table-column>-->
      <!--<el-table-column-->
      <!--align="center"-->
      <!--label="benefit">-->
      <!--<template slot-scope="scope">11</template>-->
      <!--</el-table-column>-->
      <!--<el-table-column-->
      <!--align="center"-->
      <!--label="withRate">-->
      <!--<template slot-scope="scope">22</template>-->
      <!--</el-table-column>-->
      <!--<el-table-column-->
      <!--align="center"-->
      <!--label="addition">-->
      <!--<template slot-scope="scope">33</template>-->
      <!--</el-table-column>-->

      <!--</el-table>-->

    </el-main>
  </div>
</template>
<script>
import { parseTime } from '@/utils'
export default {
  name: 'Order',
  data() {
    return {
      controlNumber: '',
      beforeResult: null,
      numTime: 0,
      countDown: null,
      typeList: [
        { name: 'loni', id: '1' },
        { name: 'parity', id: '2' },
        { name: 'sapre', id: '3' },
        { name: 'bcone', id: '4' }
      ],
      type: '1',
      data: '',
      time: '',
      timer: null
    }
  },
  mounted() {
    this.initOrder()
    this.getfindControlNumber()
  },
  created() {
    this.timeFn()
  },
  beforeDestroy() {
    clearInterval(this.timeFn())
  },
  destroyed() {
    this.timer && clearInterval(this.timer)
  },

  methods: {
    timeFn() {
      const that = this

      // setInterval(function () {
      that.$post2('/system/betInfo/getSystemDate', {
      }).then((r) => {
        that.time = new Date(r.data.data + ' GMT+0730').getTime()
        that.countDowns()
      })
      // },60000)
    },
    countDowns() {
      this.timer = setInterval(() => {
        this.time -= 1000
      }, 1000)
    },
    getfindControlNumber() {
      this.$post2('/system/controlInfo/findControlNumber', {
        controlNumber: this.controlNumber
      }).then((r) => {
        this.controlNumber = r.data.data
      })
    },
    setontrolNumber() {
      this.$post2('/system/controlInfo/updateControlNumber', {
        controlNumber: this.controlNumber
      }).then((r) => {
        if (r.data.code === '0000') {
          this.getfindControlNumber()
        }
      })
    },
    beforeClick() {
      if (this.beforeResult >= 0 && this.beforeResult <= 9) {
        this.$post2('system/betInfo/upsetLastBetInfo', {
          id: this.data.betId,
          beforeResult: this.beforeResult
        }).then((r) => {
          if (r.data.code === '0000') {
            this.initOrder()
            this.$message({
              message: '设置成功',
              type: 'success'
            })
          } else {
            this.$message({
              message: r.data.message,
              type: 'warning'
            })
          }
        })
      } else {
        this.$message({
          message: '0~9',
          type: 'warning'
        })
      }
    },
    secondToDate: function(result) {
      var h = Math.floor(result / 3600) < 10 ? '0' + Math.floor(result / 3600) : Math.floor(result / 3600)
      var m = Math.floor((result / 60 % 60)) < 10 ? '0' + Math.floor((result / 60 % 60)) : Math.floor((result / 60 % 60))
      var s = Math.floor((result % 60)) < 10 ? '0' + Math.floor((result % 60)) : Math.floor((result % 60))
      return m + ':' + s
    },
    initOrder() {
      this.$post2('system/betInfo/findLastBetInfoByTeacher', {
        type: this.type
      }).then((r) => {
        this.data = r.data.data[0]
        const that = this
        if (that.data.countdown) {
          var settime = setInterval(function() {
            that.data.countdown--
            if (that.data.countdown <= 0) {
              that.countDown = 0
              that.initOrder()
              clearInterval(settime)
            } else {
              that.countDown = that.secondToDate(that.data.countdown)
            }
            that.numTime = that.data.countdown
          }, 1000)
        }
      })
    }
  }
}
</script>
<style lang="scss" scoped>
  .list {
    box-sizing: border-box;
    width: 100%;
  }

  .green {
    background: #1eb83f;
  }

  .red {
    background: #f52926;
  }

  .violet {
    background: blueviolet;
  }

  .list .li {
    width: 100%;
    list-style: none;
    border-bottom: 1px solid #dfe6ec;
    display: flex;
    align-items: center;
    .item {
      width: 50%;
      text-align: center;
      padding: 4px 0;
      .box {
        display: inline-block;
        width: 30px;
        height: 30px;
        line-height: 30px;
        font-weight: 400;
        border-radius: 50%;
        text-align: center;
        font-size: 13px;
        color: #ffffff;
      }
    }
  }
</style>
<style lang="scss">
  .vue-treeselect__menu {
    max-height: 165px !important;
  }
</style>
