<template>
  <div class="kpi-container" v-if="props.kpis.length">
    <div v-for="(kpi, index) in props.kpis" :key="index" class="kpi">
      <span class="kpi-label">{{ kpi.label + ":" }}</span>
      <span class="kpi-value">{{ kpi.value }}</span>
    </div>
  </div>
  <div class="box-mixedChart">
    <VueApexCharts type="line" height="100%" :options="options" :series="series" />
  </div>
</template>

<script setup lang="ts">
import VueApexCharts from 'vue3-apexcharts';
import { withDefaults, defineProps } from 'vue';

const props = withDefaults(defineProps<{
  series: ApexAxisChartSeries;
  options?: ApexCharts.ApexOptions;
  kpis?: { label: string; value: string | number }[];
}>(), {
  kpis: () => [],
  options: () => ({
    chart: {
      type: 'line',
      stacked: false,
      background: 'transparent',
    },
    theme: {
      mode: 'dark', // Tema general oscuro (puedes cambiarlo si lo deseas)
      palette: 'palette1',
    },
    stroke: {
      width: [0, 2, 4],
      curve: 'smooth'
    },
    plotOptions: {
      bar: {
        columnWidth: '50%'
      }
    },
    fill: {
      opacity: [0.85, 0.25, 1],
      gradient: {
        inverseColors: false,
        shade: 'light',
        type: 'vertical',
        opacityFrom: 0.85,
        opacityTo: 0.55,
        stops: [0, 100, 100, 100],
      }
    },
    markers: { size: 0 },
    xaxis: {
      categories: ['Ene', 'Feb', 'Mar', 'Abr', 'May', 'Jun', 'Jul', 'Ago', 'Sep', 'Oct', 'Nov', 'Dic'],
      labels: { style: { colors: '#8C8C8C' } }
    },
    yaxis: {
      labels: { style: { colors: '#8C8C8C' } },
      title: {
        style: { color: '#8C8C8C', fontWeight: 'normal' }
      },
      tooltip: { enabled: true }
    },
    grid: {
      show: true,
      borderColor: '#555',
    },
    legend: {
      position: 'top'
    },
    tooltip: {
      shared: true,
      intersect: false,
      theme: 'light', // CAMBIO CLAVE: tooltip claro
      y: {
        formatter: (y: number | null) => y !== null ? `${y.toFixed(0)} points` : ''
      }
    }
  })
});
</script>

<style scoped>
.box-mixedChart {
  display: flex;
  flex-direction: column;
  height: 100%;
  min-height: 290px;
  width: 100%;
  padding: 16px;
}

.kpi-container {
  display: flex;
  justify-content: space-around;
  gap: 20px;
  margin-bottom: 1rem;
  background-color: white; /* fondo claro para KPIs */
  padding: 1rem;
  border-radius: 8px;
}

.kpi {
  text-align: center;
}

.kpi-value {
  font-size: 1.5rem;
  font-weight: bold;
}

.kpi-label {
  font-size: 0.9rem;
  color: #666;
  margin-right: 10px;
}
</style>
