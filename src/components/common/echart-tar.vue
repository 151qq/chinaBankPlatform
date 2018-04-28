<template>
  <div class="echar-box" :id="idName"></div>
</template>
<script>
import echarts from 'echarts'

export default {
  props: ['idName', 'echartsDate'],
  data () {
    return {
      isNoFirst: false,
      option: {
        title: {
          text: ''
        },
        tooltip: {
          trigger: 'axis',
          axisPointer: {
            type: 'cross'
          }
        },
        grid: {
          left: '3%',
          right: '4%',
          bottom: '3%',
          containLabel: true
        },
        legend: {
          right: 0,
          data: []
        },
        xAxis: {
          data: []
        },
        yAxis: {
          type: 'value'
        },
        series: []
      },
      pageInfo: {}
    }
  },
  methods: {
    // 获取echarts数据
    setEcharts () {
      let datas = this.echartsDate

      // 设置标题
      this.option.title.text = datas.title ? datas.title : ''
      // 设置legend
      this.option.legend.data = datas.legend
      // 设置横轴数据
      if (datas.xAxisData) {
        this.option.xAxis.data = datas.xAxisData
      } else {
        this.option.xAxis = {
          type: 'value'
        }

        var dataObj = {
          type: 'category',
          data: datas.yAxisData
        }
        this.option.yAxis = dataObj
      }
      
      // 设置纵轴数据
      this.option.series = this.setStyle(datas.series)

      this.drawEchart()
    },
    setStyle (arrs) {
      if (!arrs) {
        return []
      }
      var arrList = arrs.map((item, index) => {
        item.label = {
            normal: {
                show: true,
                position: 'insideRight'
            }
        }
        item.stack = '总量'
        return item
      })

      return arrList
    },
    drawEchart () {
      // 使用刚指定的配置项和数据显示图表。
      if (!this.isInit) {
        // 基于准备好的dom，初始化echarts实例
        var dom = document.getElementById(this.idName)
        this.myChart = echarts.init(dom)
      }
      console.log(this.option, 'o')
      this.myChart.setOption(this.option)
      this.isInit = true
    }
  }
}
</script>
<style lang="scss">
  .echar-box {
    width: 100%;
    height: 560px;
    box-sizing: border-box;
  }
</style>
