<template>
  <div class="chartjs-box">
    <div class="info">
      <h4>{{ title }}</h4>
      <p>{{ description }}</p>
    </div>
    <canvas ref="chartRef"></canvas>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, onBeforeUnmount, watch } from 'vue';
import { Chart, registerables, TooltipItem } from 'chart.js';
import ChartDataLabels, { Context as DataLabelsContext } from 'chartjs-plugin-datalabels';

Chart.register(...registerables, ChartDataLabels);

interface Props {
  labels: string[];
  values: number[];
  title: string;
  description: string;
}
const props = defineProps<Props>();

const chartRef = ref<HTMLCanvasElement | null>(null);
let chartInstance: Chart<'doughnut', number[], string> | null = null;

function initChart() {
  if (!chartRef.value) return;

  chartInstance?.destroy();

  chartInstance = new Chart(chartRef.value, {
    type: 'doughnut',
    data: {
      labels: props.labels,
      datasets: [{
        label: props.title,
        data: props.values,
        backgroundColor: [
          '#0396FFaa',
          '#6be084aa',
          '#e78f30aa',
          '#d65db1aa',
          '#EE9AE5aa',
          '#d65656'
        ],
        borderWidth: 2,
        borderColor: '#1E1E1E'
      }]
    },
    options: {
      responsive: true,
      maintainAspectRatio: false,
      plugins: {
        legend: {
          position: 'right',
          labels: {
            color: '#ccc',
            boxWidth: 12,
            padding: 16
          }
        },
        tooltip: {
          callbacks: {
            label: (ctx: TooltipItem<'doughnut'>) => {
              const data = ctx.dataset.data as number[];
              const total = data?.reduce((sum, val) => sum + val, 0) || 0;
              const value = ctx.parsed;
              const percentage = total > 0 ? ((value / total) * 100).toFixed(1) : '0';
              return `${ctx.label}: ${value} (${percentage}%)`;
            }
          }
        },
        datalabels: {
          color: '#fff',
          font: { weight: 'bold' },
          formatter: (value: number, context: DataLabelsContext) => {
            const dataset = context.chart.data.datasets[0];
            const data = dataset?.data as number[] | undefined;
            const total = data?.reduce((sum, val) => sum + val, 0) || 0;
            const percentage = total > 0 ? ((value / total) * 100).toFixed(1) : '0';
            return `${percentage}%`;
          }
        }
      }
    }
  });
}

onMounted(initChart);
watch([() => props.labels, () => props.values], initChart);
onBeforeUnmount(() => chartInstance?.destroy());
</script>

<style scoped>
.chartjs-box {
  width: 100%;
  height: 100%;
  background: #1E1E1E;
  border-radius: 5px;
  padding: 2.5rem;
  display: flex;
  flex-direction: column;
  justify-content: center;
}
.info {
  text-align: center;
  margin-bottom: 1rem;
}
.info h4 {
  margin: 0; font-size: 1.1rem; color: #fff;
}
.info p {
  margin: 0; font-size: 0.85rem; color: #aaa;
}
</style>
