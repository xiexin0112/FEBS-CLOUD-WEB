<template>
  <div class="statiscs">
    <el-container>
      <el-main>
        <div  class="box">
          <el-row :gutter="20">
            <el-col :span="6">
              <div class="grid-content bg-purple">
                <h3>{{list[0].name}}：</h3>
                <p>{{list[0].value}}</p>
              </div>
            </el-col>
            <el-col :span="6">
              <div class="grid-content bg-purple">
                <h3>{{list[2].name}}：</h3>
                <p>{{list[2].value}}</p>
              </div>
            </el-col>
            <el-col :span="6">
              <div class="grid-content bg-purple">
                <h3>{{list[3].name}}：</h3>
                <p>{{list[3].value}}</p>
              </div>
            </el-col>
            <el-col :span="6">
              <div class="grid-content bg-purple">
                <h3>{{list[5].name}}：</h3>
                <p>{{list[5].value}}</p>
              </div>
            </el-col>
          </el-row>
        </div>

        <div  class="box">
          <el-row :gutter="20">
            <el-col :span="6">
              <div class="grid-content bg-purple">
                <h3>{{list[6].name}}：</h3>
                <p>{{list[6].value}}</p>
              </div>
            </el-col>
            <el-col :span="6">
              <div class="grid-content bg-purple">
                <h3>{{list[8].name}}：</h3>
                <p>{{list[8].value}}</p>
              </div>
            </el-col>

            <el-col :span="6">
              <div class="grid-content bg-purple">
                <h3>{{list[13].name}}：</h3>
                <p>{{list[13].value}}</p>
              </div>
            </el-col>
            <el-col :span="6">
              <div class="grid-content bg-purple">
                <h3>{{list[15].name}}：</h3>
                <p>{{list[15].value}}</p>
              </div>
            </el-col>



          </el-row>
        </div>
        <div  class="box">
          <el-row :gutter="20">
            <el-col :span="6">
              <div class="grid-content bg-purple">
                <h3>{{list[11].name}}：</h3>
                <p>{{list[11].value}}</p>
              </div>
            </el-col>
            <el-col :span="6">
              <div class="grid-content bg-purple">
                <h3>{{list[12].name}}：</h3>
                <p>{{list[12].value}}</p>
              </div>
            </el-col>
            <el-col :span="6">
              <div class="grid-content bg-purple">
                <h3>{{list[9].name}}：</h3>
                <p>{{list[9].value}}</p>
              </div>
            </el-col>
            <el-col :span="6">
              <div class="grid-content bg-purple">
                <h3>{{list[10].name}}：</h3>
                <p>{{list[10].value}}</p>
              </div>
            </el-col>



          </el-row>
        </div>


      </el-main>

    </el-container>
    <div class="echarts_wrap">
      <div  ref="chart1" style="width: 50%;height:400px;"></div>
      <div  ref="chart2" style="width: 50%;height:400px;"></div>
      <div  ref="chart3" style="width: 50%;height:400px;"></div>
      <div  ref="chart4" style="width: 50%;height:400px;"></div>
    </div>

  </div>
</template>
<script>
  import echarts from 'echarts'


export default {
  name: 'statiscs',
  data() {
    return {
      list: ''
    }
  },
  mounted() {
    this.getData()
    this.getfindControlNumber()
  },
  methods: {
    drawChart1(item) {
      // 基于准备好的dom，初始化echarts实例
      let myChart = echarts.init(this.$refs.chart1);
      var option={
        //标题
        title:{
          text:'每日新增用户走势图'
        },
        //x轴
        xAxis:{
          data:item.list1
        },
        //y轴没有显式设置，根据值自动生成y轴
        yAxis:{},
        //数据-data是最终要显示的数据
        series:[{
          type:'line',
          data:item.list2
        }]
      };
      //使用刚刚指定的配置项和数据项显示图表
      myChart.setOption(option);
    },
    drawChart2(item) {
      // 基于准备好的dom，初始化echarts实例
      let myChart = echarts.init(this.$refs.chart2);
      var option={
        //标题
        title:{
          text:'每日平台盈利走势图'
        },
        //x轴
        xAxis:{
          data:item.list1
        },
        //y轴没有显式设置，根据值自动生成y轴
        yAxis:{},
        //数据-data是最终要显示的数据
        series:[{
          type:'line',
          data:item.list2
        }]
      };
      //使用刚刚指定的配置项和数据项显示图表
      myChart.setOption(option);
    },
    drawChart3(item) {
      // 基于准备好的dom，初始化echarts实例
      let myChart = echarts.init(this.$refs.chart3);
      var option={
        //标题
        title:{
          text:'每日充值走势'
        },
        //x轴
        xAxis:{
          data:item.list1
        },
        //y轴没有显式设置，根据值自动生成y轴
        yAxis:{},
        //数据-data是最终要显示的数据
        series:[{
          type:'line',
          data:item.list2
        }]
      };
      //使用刚刚指定的配置项和数据项显示图表
      myChart.setOption(option);
    },
    drawChart4(item) {
      // 基于准备好的dom，初始化echarts实例
      let myChart = echarts.init(this.$refs.chart4);
      var option={
        //标题
        title:{
          text:'每日提现走势'
        },
        //x轴
        xAxis:{
          data:item.list1
        },
        //y轴没有显式设置，根据值自动生成y轴
        yAxis:{},
        //数据-data是最终要显示的数据
        series:[{
          type:'line',
          data:item.list2
        }]
      };
      //使用刚刚指定的配置项和数据项显示图表
      myChart.setOption(option);
    },
    getData() {
      this.$post2('/system/statisticsInfo/newStatisticsList', {
      }).then((r) => {
        let that = this
        this.list = r.data.data
        let echartItem1 = {
          list1: [],
          list2: []
        }
        let echartItem2 = {
          list1: [],
          list2: []
        }
        let echartItem3 = {
          list1: [],
          list2: []
        }
        let echartItem4 = {
          list1: [],
          list2: []
        }
        console.log(this.list[5])
        this.list[1].value.map((item) => {
          echartItem1.list1.push(item.dateNo)
          echartItem1.list2.push(item.num)
        })
        this.list[4].value.map((item) => {
          echartItem2.list1.push(item.dateNo)
          echartItem2.list2.push(item.datebenefit)
        })
        this.list[7].value.map((item) => {
          echartItem3.list1.push(item.dateNo)
          echartItem3.list2.push(item.amount)
        })
        this.list[14].value.map((item) => {
          echartItem4.list1.push(item.dateNo)
          echartItem4.list2.push(item.amount)
        })


        setTimeout(function(){
          that.drawChart1(echartItem1)
          that.drawChart2(echartItem2)
          that.drawChart3(echartItem3)
          that.drawChart4(echartItem4)
        },200)
      })
    },
  }
}
</script>
<style lang="scss" scoped>
  .bg-purple-dark {
    background: #99a9bf;
  }

  .bg-purple {
    background: #d3dce6;
  }

  .bg-purple-light {
    background: #e5e9f2;
  }

  .grid-content {
    border-radius: 4px;
    min-height: 36px;
    margin-top: 20px;
    padding: 20px;
    color: #ffffff;
  }

  .grid-content h3 {
    font-size: 18px;
    margin-bottom: 14px;
    color: #ffffff;
  }
  .demo-form-inline{
    margin-top: 20px;
    margin-left: 20px;
  }
  .box:nth-of-type(1) .grid-content{
    background: #1989fa;
  }
  .box:nth-of-type(2) .grid-content{
    background: #5cb87a;
  }
  .box:nth-of-type(3) .grid-content{
    background: #e6a23c;
  }
  .box:nth-of-type(4) .grid-content{
    background: #f56c6c;
  }
  .box:nth-of-type(5) .grid-content{
    background: #795548;
  }
  .el-row-title{
    padding-top: 24px;
    padding-left: 20px;
  }
  .echarts_wrap{
    justify-content: space-between;
    margin: 20px;
    display: flex;
    flex-wrap: wrap;
  }
  .echarts_wrap > div{
    margin-top: 30px;
  }
</style>
<style lang="scss">
  .vue-treeselect__menu {
    max-height: 165px !important;
  }
</style>
