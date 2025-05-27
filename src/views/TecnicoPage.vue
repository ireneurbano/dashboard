<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar>
        <ion-buttons slot="start">
          <ion-menu-button color="dark"></ion-menu-button>
        </ion-buttons>
        <ion-title>🖥️ Dashboard Técnico</ion-title>
      </ion-toolbar>
    </ion-header>

    <ion-content :fullscreen="true" class="ion-padding">
      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">🖥️ Dashboard Técnico</ion-title>
        </ion-toolbar>
      </ion-header>

      <ion-grid class="dashboard-grid">
        <!-- FILA 1: 4 KPI como sparklines -->
        <ion-row class="ion-row-1">
          <ion-col size="6" size-lg="3">
            <div class="box gradient-blue">
              <div class="kpi-title">CPU Uso</div>
              <div class="kpi-value">{{ cpuUsage }}%</div>
              <apexchart
                type="radialBar"
                :options="cpuChartOptions"
                :series="[cpuUsage]"
                height="100"
              />
            </div>
          </ion-col>
          <ion-col size="6" size-lg="3">
            <div class="box gradient-green">
              <div class="kpi-title">Memoria</div>
              <div class="kpi-value">{{ memoryUsed }} GB</div>
              <apexchart
                type="radialBar"
                :options="memoryChartOptions"
                :series="[memoryUsed]"
                height="100"
              />
            </div>
          </ion-col>
          <ion-col size="6" size-lg="3">
            <div class="box gradient-orange">
              <div class="kpi-title">Red Entrada</div>
              <div class="kpi-value">{{ networkInSeries[0].data.slice(-1)[0] }} Mbps</div>
              <apexchart
                type="line"
                :options="networkInChartOptions"
                :series="networkInSeries"
                height="100"
              />
            </div>
          </ion-col>
          <ion-col size="6" size-lg="3">
            <div class="box gradient-pink">
              <div class="kpi-title">Red Salida</div>
              <div class="kpi-value">{{ networkOutSeries[0].data.slice(-1)[0] }} Mbps</div>
              <apexchart
                type="line"
                :options="networkOutChartOptions"
                :series="networkOutSeries"
                height="100"
              />
            </div>
          </ion-col>
        </ion-row>

        <!-- FILA 2: Dos gráficos grandes -->
        <ion-row class="ion-row-2">
          <ion-col size="12" size-lg="9">
            <div class="box">
              <div class="box-title">Errores por Categoría (últimos 30 días)</div>
              <apexchart
                type="bar"
                :options="errorChartOptions"
                :series="errorSeries"
                height="300"
              />
            </div>
          </ion-col>
          <ion-col size="12" size-lg="3">
            <div class="box">
              <div class="box-title">Estado General</div>
              <apexchart
                type="radialBar"
                :options="systemHealthOptions"
                :series="[systemHealth]"
                height="300"
              />
              <div class="health-label">{{ systemHealthLabel }}</div>
            </div>
          </ion-col>
        </ion-row>

        <!-- FILA 3: Dos gráficos nuevos -->
        <ion-row class="ion-row-3">
          <ion-col size="12" size-lg="6">
            <div class="box">
              <div class="box-title">Distribución de Recursos</div>
              <apexchart
                type="polarArea"
                :options="polarAreaOptions"
                :series="polarAreaSeries"
                height="350"
              />
            </div>
          </ion-col>
          <ion-col size="12" size-lg="6">
            <div class="box">
              <div class="box-title">Comparación de Métricas</div>
              <apexchart
                type="radar"
                :options="radarOptions"
                :series="radarSeries"
                height="350"
              />
            </div>
          </ion-col>
        </ion-row>
      </ion-grid>
    </ion-content>
  </ion-page>
</template>

<script setup lang="ts">
import {
  IonButtons,
  IonContent,
  IonHeader,
  IonMenuButton,
  IonPage,
  IonTitle,
  IonToolbar,
  IonGrid,
  IonRow,
  IonCol,
} from '@ionic/vue';
import ApexCharts from 'apexcharts';
import ApexChart from 'vue3-apexcharts';
import { ref, onMounted, onUnmounted } from 'vue';

const cpuUsage = ref(56);
const memoryUsed = ref(12.4);

const networkInSeries = ref([
  {
    name: 'Entrada',
    data: [25, 35, 45, 30, 40, 50, 60, 70, 65, 80],
  },
]);

const networkOutSeries = ref([
  {
    name: 'Salida',
    data: [15, 25, 30, 20, 25, 35, 40, 50, 55, 60],
  },
]);

const errorSeries = ref([
  {
    name: 'Errores Críticos',
    data: [5, 6, 7, 8, 4, 7, 6, 9, 10, 8, 7, 6, 8, 9, 7, 6, 8, 9, 10, 9, 8, 7, 6, 5, 7, 8, 9, 10, 8, 7],
  },
  {
    name: 'Errores Warning',
    data: [10, 9, 8, 7, 8, 9, 10, 8, 7, 8, 9, 10, 9, 8, 7, 8, 9, 10, 11, 10, 9, 8, 7, 9, 10, 9, 8, 7, 6, 7],
  },
]);

const systemHealth = ref(75);
const systemHealthLabel = ref('Operativo');

const cpuChartOptions = {
  chart: {
    type: 'radialBar',
    sparkline: { enabled: true },
    dropShadow: { enabled: true, top: 1, left: 1, blur: 2, opacity: 0.5 }
  },
  colors: ['#fff'],
  plotOptions: {
    radialBar: {
      hollow: { size: '65%' },
      dataLabels: {
        name: { show: false },
        value: { 
          fontSize: '22px', 
          color: '#fff',
          offsetY: 5,
          formatter: function(val: any) {
            return val + "%"
          }
        },
      },
    },
  },
  labels: ['CPU'],
};

const memoryChartOptions = {
  chart: {
    type: 'radialBar',
    sparkline: { enabled: true },
    dropShadow: { enabled: true, top: 1, left: 1, blur: 2, opacity: 0.5 }
  },
  colors: ['#fff'],
  plotOptions: {
    radialBar: {
      hollow: { size: '65%' },
      dataLabels: {
        name: { show: false },
        value: { 
          fontSize: '22px', 
          color: '#fff',
          offsetY: 5,
          formatter: function(val: any) {
            return val + "GB"
          }
        },
      },
    },
  },
  labels: ['Memoria'],
};

const networkInChartOptions = {
  chart: {
    toolbar: { show: false },
    zoom: { enabled: false },
    sparkline: { enabled: true },
    dropShadow: { enabled: true, top: 1, left: 1, blur: 2, opacity: 0.5 }
  },
  stroke: {
    curve: 'smooth',
    width: 3,
  },
  colors: ['#fff'],
  tooltip: {
    enabled: false
  },
  xaxis: {
    categories: Array.from({ length: 10 }, (_, i) => `T-${10 - i}`),
    labels: { show: false }
  },
  yaxis: { show: false }
};

const networkOutChartOptions = {
  chart: {
    toolbar: { show: false },
    zoom: { enabled: false },
    sparkline: { enabled: true },
    dropShadow: { enabled: true, top: 1, left: 1, blur: 2, opacity: 0.5 }
  },
  stroke: {
    curve: 'smooth',
    width: 3,
  },
  colors: ['#fff'],
  tooltip: {
    enabled: false
  },
  xaxis: {
    categories: Array.from({ length: 10 }, (_, i) => `T-${10 - i}`),
    labels: { show: false }
  },
  yaxis: { show: false }
};

const errorChartOptions = {
  chart: {
    stacked: true,
    stackType: 'normal',
    toolbar: { show: true },
    type: 'bar',
  },
  colors: ['#DC143C', '#FFA500'],
  plotOptions: {
    bar: {
      horizontal: false,
      columnWidth: '60%',
    },
  },
  xaxis: {
    categories: Array.from({ length: 30 }, (_, i) => `D-${30 - i}`),
  },
  legend: {
    position: 'top',
    horizontalAlign: 'right',
  },
  fill: {
    opacity: 0.85,
  },
  dataLabels: { enabled: false },
};

const systemHealthOptions = {
  chart: {
    type: 'radialBar',
    sparkline: { enabled: true },
  },
  colors: ['#32CD32'],
  plotOptions: {
    radialBar: {
      hollow: { size: '70%' },
      dataLabels: {
        name: { show: false },
        value: { fontSize: '28px', fontWeight: 'bold', color: '#000' },
      },
    },
  },
  labels: ['Salud del sistema'],
};

function updateSystemHealthLabel(value: number) {
  if (value >= 80) return 'Óptimo';
  if (value >= 50) return 'Operativo';
  if (value >= 30) return 'Problemas';
  return 'Crítico';
}

// --- NUEVAS VARIABLES Y OPCIONES PARA LOS NUEVOS GRÁFICOS ---

// Polar Area
const polarAreaSeries = ref([14, 23, 21, 17, 15, 10]);
const polarAreaOptions = {
  chart: {
    type: 'polarArea',
  },
  stroke: {
    colors: ['#fff']
  },
  fill: {
    opacity: 0.8
  },
  colors: ['#3A8DFF', '#28C76F', '#FF914D', '#FF5CA3', '#7367F0', '#EA5455'],
  labels: ['CPU', 'Memoria', 'Almacenamiento', 'Red', 'GPU', 'Cache'],
  yaxis: {
    show: false
  },
  legend: {
    position: 'bottom'
  },
  plotOptions: {
    polarArea: {
      rings: {
        strokeWidth: 1,
        strokeColor: '#e8e8e8'
      },
      spokes: {
        strokeWidth: 1,
        connectorColors: '#e8e8e8'
      }
    }
  },
  responsive: [{
    breakpoint: 480,
    options: {
      chart: {
        width: 200
      },
      legend: {
        position: 'bottom'
      }
    }
  }]
};

// Radar
const radarSeries = ref([
  {
    name: 'Métricas Actuales',
    data: [80, 50, 30, 40, 60, 50]
  },
  {
    name: 'Métricas Objetivo',
    data: [70, 60, 50, 60, 70, 60]
  }
]);

const radarOptions = {
  chart: {
    type: 'radar',
    dropShadow: {
      enabled: true,
      blur: 1,
      left: 1,
      top: 1
    }
  },
  colors: ['#3A8DFF', '#FF5CA3'],
  stroke: {
    width: 2
  },
  fill: {
    opacity: 0.1
  },
  markers: {
    size: 0
  },
  labels: ['Disponibilidad', 'Rendimiento', 'Escalabilidad', 'Seguridad', 'Latencia', 'Throughput'],
  yaxis: {
    show: false
  },
  legend: {
    position: 'bottom'
  },
  plotOptions: {
    radar: {
      size: 140,
      polygons: {
        strokeColors: '#e8e8e8',
        fill: {
          colors: ['#f8f8f8', '#fff']
        }
      }
    }
  }
};

// ---- INTERVALOS PARA ACTUALIZAR TODOS LOS GRÁFICOS ---

let updateInterval: number | undefined;

onMounted(() => {
  systemHealthLabel.value = updateSystemHealthLabel(systemHealth.value);

  updateInterval = window.setInterval(() => {
    // Actualización de fila 1 y 2 (existente)
    cpuUsage.value = Math.floor(Math.random() * 50) + 40;
    memoryUsed.value = Number((Math.random() * 16).toFixed(1));
    systemHealth.value = Math.floor(Math.random() * 50) + 30;
    systemHealthLabel.value = updateSystemHealthLabel(systemHealth.value);

    const newNetIn = networkInSeries.value[0].data.slice(1);
    newNetIn.push(Math.floor(Math.random() * 100));
    networkInSeries.value[0].data = newNetIn;

    const newNetOut = networkOutSeries.value[0].data.slice(1);
    newNetOut.push(Math.floor(Math.random() * 80));
    networkOutSeries.value[0].data = newNetOut;

    // Actualización de fila 3 (nuevos gráficos)
    polarAreaSeries.value = polarAreaSeries.value.map(() => Math.floor(Math.random() * 25) + 5);

    radarSeries.value[0].data = radarSeries.value[0].data.map(() => Math.floor(Math.random() * 60) + 20);
  }, 3000);
});

onUnmounted(() => {
  if (updateInterval) clearInterval(updateInterval);
});
</script>

<style scoped>
.dashboard-grid {
  gap: 16px;
}

.box {
  background: var(--ion-color-light);
  padding: 16px;
  border-radius: 12px;
  box-shadow: 0 2px 12px rgba(0, 0, 0, 0.1);
  height: 100%;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

.gradient-blue {
  background: linear-gradient(135deg, #3a8dff, #1e4aff);
  color: white;
}

.gradient-green {
  background: linear-gradient(135deg, #28c76f, #20a458);
  color: white;
}

.gradient-orange {
  background: linear-gradient(135deg, #ff914d, #ff6e1d);
  color: white;
}

.gradient-pink {
  background: linear-gradient(135deg, #ff5ca3, #ff367f);
  color: white;
}

.kpi-title {
  font-weight: 600;
  font-size: 1rem;
  margin-bottom: 8px;
  text-align: center;
}

.kpi-value {
  font-weight: 700;
  font-size: 1.5rem;
  text-align: center;
  margin: 8px 0;
}

.box-title {
  font-weight: 700;
  font-size: 1.1rem;
  margin-bottom: 12px;
  color: #222;
  text-align: center;
}

.health-label {
  text-align: center;
  margin-top: 12px;
  font-weight: 600;
  font-size: 1.2rem;
  color: #222;
}

.ion-row-1 ion-col,
.ion-row-3 ion-col {
  min-height: 160px;
}
.ion-row-2 ion-col {
  min-height: 350px;
}
</style>
