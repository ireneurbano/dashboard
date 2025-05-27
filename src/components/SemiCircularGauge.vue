<template>
  <div ref="chartRef" class="gauge-container"></div>
</template>

<script setup lang="ts">
import { ref, watch, onMounted, onBeforeUnmount } from 'vue';
import * as echarts from 'echarts/core';
import {
  GaugeChart,
} from 'echarts/charts';
import {
  TooltipComponent,
} from 'echarts/components';
import {
  CanvasRenderer,
} from 'echarts/renderers';

echarts.use([GaugeChart, TooltipComponent, CanvasRenderer]);

const props = defineProps({
  value: {
    type: Number,
    required: true,
    default: 0,
  },
  max: {
    type: Number,
    default: 100,
  },
  title: {
    type: String,
    default: '',
  },
  color: {
    type: String,
    default: '#3a8dff',
  },
});

const chartRef = ref<HTMLElement | null>(null);
let chartInstance: echarts.ECharts | null = null;

const renderChart = () => {
  if (!chartInstance || !chartRef.value) return;

  const option = {
    tooltip: {
      formatter: `{a} <br/> {c} / ${props.max}`,
    },
    series: [
      {
        name: props.title,
        type: 'gauge',
        startAngle: 180,
        endAngle: 0,
        radius: '100%',
        min: 0,
        max: props.max,
        splitNumber: 5,
        axisLine: {
          lineStyle: {
            width: 12,
            color: [[props.value / props.max, props.color], [1, '#eee']],
          },
        },
        pointer: {
          length: '70%',
          width: 6,
          itemStyle: {
            color: props.color,
          },
        },
        axisTick: {
          length: 8,
          lineStyle: {
            color: '#999',
            width: 2,
          },
        },
        splitLine: {
          length: 14,
          lineStyle: {
            color: '#999',
            width: 3,
          },
        },
        axisLabel: {
          color: '#666',
          distance: -30,
          fontSize: 12,
        },
        detail: {
          valueAnimation: true,
          formatter: '{value}%',
          color: props.color,
          fontSize: 22,
          offsetCenter: [0, '20%'],
        },
        title: {
          offsetCenter: [0, '45%'],
          fontSize: 14,
          color: '#444',
          fontWeight: 'bold',
          text: props.title,
        },
        data: [{ value: props.value }],
      },
    ],
  };

  chartInstance.setOption(option);
};

onMounted(() => {
  if (chartRef.value) {
    chartInstance = echarts.init(chartRef.value);
    renderChart();
  }
});

watch(() => props.value, () => {
  if (chartInstance) renderChart();
});

onBeforeUnmount(() => {
  if (chartInstance) {
    chartInstance.dispose();
    chartInstance = null;
  }
});
</script>

<style scoped>
.gauge-container {
  width: 100%;
  height: 150px;
}
</style>
