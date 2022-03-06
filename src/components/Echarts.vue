<template>
  <div class="Echart" ref="echart"></div>
</template>
<script setup>
import { onMounted, reactive, ref } from 'vue'
import * as echarts from 'echarts';
onMounted(() => {
  Echart(echartdata.xData5H, echartdata.yData5H)
})
defineExpose({ drawEcharts })
function drawEcharts(time, data) {
  let xData = []
  let yData = []
  switch (time) {
    case '5h':
      xData = echartdata.xData5H
      break;
    case '1h':
      xData = echartdata.xData1H
      break;
    case '15m':
      xData = echartdata.xData15M
      break;
    default:
      xData = echartdata.xData5H
  }

  switch (data) {
    case '5h':
      yData = echartdata.yData5H
      break;
    case '1h':
      yData = echartdata.yData1H
      break;
    case '15m':
      yData = echartdata.yData15M
      break;
    default:
      yData = echartdata.yData5H
  }

  console.log("test-successful");
  Echart(xData, yData)
}

const echart = ref(null)
const Echart = function (xData, yData) {
  const myChart = echarts.init(echart.value);
  myChart.clear()
  let yDataShadow = []

  yData.forEach(item => {
    if (item > 0) {
      yDataShadow.push(Number(item - 1))
    } else {
      yDataShadow.push(0)
    }
  })

  myChart.setOption({
    tooltip: {
      show: true,
      trigger: "axis",
      formatter: function (params, ticket, callback) {
        var name = params[0].name
        var value = params[0].value
        var marker = '<span style="display:inline-block;margin-right:5px;border-radius:10px;width:10px;height:10px;background-color:#00ECA8"></span>'
        return `Time: ${name}
                <br/ >
                ${marker}Price: ${value}
        `
      }
    },
    grid: {
      top: "5%",
      right: "10px",
      left: "35px",
      bottom: "8%",
    },
    xAxis: {
      data: xData,
      axisLine: {
        show: false
      },
      axisTick: {
        show: false
      },
      splitLine: {
        show: true,
        lineStyle: {
          color: "#E5E5E5"
        }
      }
    },
    yAxis: {
      axisLine: {
        show: false
      },
      splitLine: {
        show: true,
        lineStyle: {
        }
      }
    },
    series: [
      {
        name: 'average',
        type: 'line',
        data: yData,
        smooth: true,
        symbol: 'none',
        symbolSize: 12,
        lineStyle: {
          color: "#00ECA8",
          cap: "round",
          width: 12,
          shadowColor: 'rgba(0, 0, 0, 0.5)',
          shadowBlur: 22,
          shadowOffsetY: 14
        },
      },
    ]
  });
}

</script>
<style scoped lang="scss">
.flex {
  display: flex;
}
.Echart {
  margin-top: 30px;
  width: 100%;
  height: 250px;
}
@media only screen and (max-width: 767px) {
  .Echart {
    margin-top: 20px;
  }
}
</style>