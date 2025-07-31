<template>
  <div class="line-chart">
    <Echart :options="option" height="100%" width="100%" />
  </div>
</template>

<script setup>
import { ref } from 'vue'
import { Echart } from '@/components/Echart'
import { computeFontSize } from '@/utils/index'
let data = [
  {
    name: '装备制造',
    value: 84
  },
  {
    name: '现代材料',
    value: 44
  },
  {
    name: '新能源',
    value: 35
  }
]

let titleArr = [],
  seriesArr = []
let colors = [
  ['#0dc779', '#0dc77940'],
  ['#19c9ff', '#19c9ff40'],
  ['#fbbc2d', '#fbbc2d40']
]
data.forEach((item, index) => {
  titleArr.push({
    text: item.name,
    left: `${(index + 0.458) * (100 / data.length)}%`,
    top: '85%',
    textAlign: 'center',
    textStyle: {
      fontWeight: 'normal',
      fontSize: '12',
      color: '#fff',
      textAlign: 'center'
    }
  })
  seriesArr.push({
    name: item.name,
    type: 'pie',
    roundCap: true,
    radius: [computeFontSize(40), computeFontSize(48)],
    itemStyle: {
      normal: {
        color: colors[index][0],
        shadowBlur: 0,
        label: {
          show: false
        },
        labelLine: {
          show: false
        }
      }
    },
    hoverAnimation: false,
    center: [`${(index + 0.5) * (100 / data.length)}%`, '45%'],
    data: [
      {
        value: item.value,
        label: {
          normal: {
            formatter: function (params) {
              return `{value|${params.value}%}\n{label|${params.value > 50 ? '整体上升' : '整体下降'}}`
            },
            position: 'center',
            show: true,
            rich: {
              value: {
                fontSize: computeFontSize(20),
                fontWeight: 'bold',
                color: '#fff',
                padding: [computeFontSize(18), 0, 0, 0]
              },
              label: {
                fontSize: computeFontSize(12),
                color: '#aaa',
                lineHeight: computeFontSize(22)
              }
            }
          }
        }
      },
      {
        value: 100 - item.value,
        name: 'invisible',
        itemStyle: {
          borderRadius: computeFontSize(12),
          normal: {
            color: colors[index][1]
          },
          emphasis: {
            color: colors[index][1]
          }
        }
      }
    ]
  })
})

let option = ref({
  title: titleArr,
  series: seriesArr
})
</script>

<style scoped lang="scss">
.line-chart {
  width: 100%;
  height: 100%;
}
</style>
