<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar>
        <ion-buttons slot="start">
          <ion-menu-button color="primary"></ion-menu-button>
        </ion-buttons>
        <ion-title>🚀 Negocio</ion-title>
      </ion-toolbar>
    </ion-header>

    <ion-content :fullscreen="true" class="ion-padding">
      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">🚀 Negocio</ion-title>
        </ion-toolbar>
      </ion-header>

      <ion-grid class="dashboard-grid">
        <!-- 🟢 Fila 1: 4 Sparklines -->
        <ion-row class="ion-row-1">
          <ion-col size="6" size-lg="3">
            <div class="box">
              <div class="sparkline-title">{{ sparkData1.title }}</div>
              <spark-line v-bind="sparkData1" />
              <div class="sparkline-value">{{ sparkData1.value }}</div>
            </div>
          </ion-col>
          <ion-col size="6" size-lg="3">
            <div class="box">
              <div class="sparkline-title">{{ sparkData2.title }}</div>
              <spark-line v-bind="sparkData2" />
              <div class="sparkline-value">{{ sparkData2.value }}</div>
            </div>
          </ion-col>
          <ion-col size="6" size-lg="3">
            <div class="box">
              <div class="sparkline-title">{{ sparkData3.title }}</div>
              <spark-line v-bind="sparkData3" />
              <div class="sparkline-value">{{ sparkData3.value }}</div>
            </div>
          </ion-col>
          <ion-col size="6" size-lg="3">
            <div class="box">
              <div class="sparkline-title">{{ sparkData4.title }}</div>
              <spark-line v-bind="sparkData4" />
              <div class="sparkline-value">{{ sparkData4.value }}</div>
            </div>
          </ion-col>
        </ion-row>

        <!-- 🔵 Fila 2: Gráfico principal + medidor -->
        <ion-row class="ion-row-2">
          <ion-col size="12" size-lg="9">
            <div class="box">
              <ApexMixedChart
                :series="dataMixedChartSeries"
                :kpis="[
                  { label: 'Juegos vendidos', value: '1,230'},
                  { label: 'Carritos completados', value: '842' },
                ]"
              />
            </div>
          </ion-col>
          <ion-col size="12" size-lg="3">
            <div class="box">
              <EchartsGauge :value="dataGauge" title="OBJETIVO VENTAS" />
            </div>
          </ion-col>
        </ion-row>

        <!-- 🟠 Fila 3: Mapa + gráfico de donut -->
        <ion-row class="ion-row-3">
          <ion-col size="12" size-lg="6">
            <div class="box">
              <EchartsMap
                :data="dataDownloadsWorld"
                title="Mapa de Descargas"
              />
            </div>
          </ion-col>
          <ion-col size="12" size-lg="6">
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
  IonCol 
} from '@ionic/vue';

import { ref, computed, onMounted, onUnmounted } from 'vue';

import SparkLine from '@/components/SparkLine.vue';
import ApexMixedChart from '@/components/ApexMixedChart.vue';
import EchartsGauge from '@/components/EchartsGauge.vue';
import EchartsMap from '@/components/EchartsMap.vue';
import ChartJsDoughnut from '@/components/ChartJsDoughnut.vue';

import { handRightOutline } from 'ionicons/icons';

// Datos completos de países
const dataDownloadsWorld = ref([
  { name: 'United States of America', value: 4780 },
  { name: 'China', value: 4520 },
  { name: 'India', value: 3890 },
  { name: 'Brazil', value: 3210 },
  { name: 'Russia', value: 2750 },
  { name: 'Germany', value: 4980 },
  { name: 'France', value: 3600 },
  { name: 'United Kingdom', value: 3410 },
  { name: 'Japan', value: 4100 },
  { name: 'Canada', value: 2950 },
  { name: 'Italy', value: 3150 },
  { name: 'South Korea', value: 2890 },
  { name: 'Mexico', value: 2500 },
  { name: 'Indonesia', value: 1970 },
  { name: 'Spain', value: 2630 },
  { name: 'Australia', value: 3100 },
  { name: 'Turkey', value: 1800 },
  { name: 'Saudi Arabia', value: 2260 },
  { name: 'South Africa', value: 2050 },
  { name: 'Argentina', value: 1750 },
  { name: 'Poland', value: 2480 },
  { name: 'Netherlands', value: 2140 },
  { name: 'Sweden', value: 1640 },
  { name: 'Switzerland', value: 1910 },
  { name: 'Portugal', value: 1470 },
  { name: 'Vietnam', value: 1220 },
  { name: 'Nigeria', value: 1130 },
  { name: 'Egypt', value: 980 },
  { name: 'Pakistan', value: 870 },
  { name: 'Ukraine', value: 660 },
  { name: 'Uruguay', value: 410 },
]);

const dataGauge = ref(80);

const dataMixedChartSeries = ref([
  {
    name: 'Juegos vendidos',
    type: 'column',
    data: [23, 11, 22, 27, 13, 22, 37, 21, 44, 22, 30, 25],
  },
  {
    name: 'Carritos completados',
    type: 'area',
    data: [44, 55, 41, 67, 22, 43, 21, 41, 56, 27, 43, 50],
  },
]);

// Cálculos para el gráfico de donut
const sorted = computed(() =>
  [...dataDownloadsWorld.value].sort((a, b) => b.value - a.value)
);
const top5 = computed(() => sorted.value.slice(0, 5));
const others = computed(() =>
  sorted.value.slice(5).reduce((sum, c) => sum + c.value, 0)
);
const doughnutLabels = computed(() =>
  top5.value.map((c) => c.name).concat('Otros')
);
const doughnutValues = computed(() =>
  top5.value.map((c) => c.value).concat(others.value)
);

// Configuración de los sparklines
const sparkData1 = ref({
  title: 'USUARIOS ACTIVOS',
  value: '10,000',
  bgColor: 'gradient-blue',
  textColor: 'white',
  iconName: 'person-outline',
  chartOptions: {
    chart: { 
      type: 'area', 
      sparkline: { enabled: true }, 
      dropShadow: { enabled: true, top: 1, left: 1, blur: 2, opacity: 0.5 } 
    },
    stroke: { curve: 'smooth', width: 3 },
    colors: ['#fff'],
    tooltip: { 
      theme: 'dark', 
      x: { show: false }, 
      y: { title: { formatter: () => '' } } 
    },
  },
  chartSeries: [{ data: [5400, 6000, 7200, 7800, 8500, 9000, 9500, 9700, 9900] }],
});

const sparkData2 = ref({
  title: 'VENTAS',
  value: '15,000 €',
  bgColor: 'gradient-green',
  textColor: 'white',
  iconName: 'cash-outline',
  chartOptions: {
    chart: { 
      type: 'bar', 
      sparkline: { enabled: true }, 
      dropShadow: { enabled: true, top: 1, left: 1, blur: 2, opacity: 0.5 } 
    },
    stroke: { curve: 'smooth', width: 3 },
    colors: ['#fff'],
    tooltip: { 
      theme: 'dark', 
      x: { show: false }, 
      y: { title: { formatter: () => '' } } 
    },
  },
  chartSeries: [{ data: [13000, 15000, 12000, 11000, 12500, 14000, 9500, 10000, 11500, 10500] }],
});

const sparkData3 = ref({
  title: 'NUEVOS USUARIOS',
  value: '400',
  bgColor: 'gradient-orange',
  textColor: 'white',
  iconName: 'person-add-outline',
  chartOptions: {
    chart: { 
      type: 'line', 
      sparkline: { enabled: true }, 
      dropShadow: { enabled: true, top: 1, left: 1, blur: 2, opacity: 0.5 } 
    },
    stroke: { curve: 'smooth', width: 3 },
    colors: ['#fff'],
    tooltip: { 
      theme: 'dark', 
      x: { show: false }, 
      y: { title: { formatter: () => '' } } 
    },
  },
  chartSeries: [{ data: [100, 200, 300, 150, 220, 180, 250, 230, 270] }],
});

const sparkData4 = ref({
  title: 'INTERACCIONES',
  value: '6,500',
  bgColor: 'gradient-pink',
  textColor: 'white',
  iconName: handRightOutline,
  chartOptions: {
    chart: { 
      type: 'area', 
      sparkline: { enabled: true }, 
      dropShadow: { enabled: true, top: 1, left: 1, blur: 2, opacity: 0.5 } 
    },
    stroke: { curve: 'smooth', width: 3 },
    colors: ['#fff'],
    tooltip: { 
      theme: 'dark', 
      x: { show: false }, 
      y: { title: { formatter: () => '' } } 
    },
  },
  chartSeries: [{ data: [4000, 4500, 5000, 5200, 5500, 6000, 6300, 6500] }],
});

// Intervalos para animación de datos
let intervalId: number | undefined;
let intervalId2: number | undefined;
let intervalId3: number | undefined;
let intervalId4: number | undefined;

onMounted(() => {
  // Usuarios activos
  intervalId = window.setInterval(() => {
    const base = 5000;
    const randomNewValue = base + Math.floor(Math.random() * 5000);
    const newData = Array.from({ length: 9 }, () =>
      base + Math.floor(Math.random() * 5000)
    );
    sparkData1.value.chartSeries = [{ data: newData }];
    sparkData1.value.value = randomNewValue.toLocaleString();
  }, 2000);

  // Ventas
  intervalId2 = window.setInterval(() => {
    const max = 10000;
    const randomNewValue = Math.floor(Math.random() * max);
    const newData = Array.from({ length: 10 }, () =>
      Math.floor(Math.random() * max)
    );
    sparkData2.value.chartSeries = [{ data: newData }];
    sparkData2.value.value = `${randomNewValue.toLocaleString()} €`;
  }, 2000);

  // Nuevos usuarios
  intervalId3 = window.setInterval(() => {
    const max = 400;
    const newData = Array.from({ length: 9 }, () =>
      Math.floor(Math.random() * max)
    );
    sparkData3.value.chartSeries = [{ data: newData }];
    const latest = newData[newData.length - 1];
    sparkData3.value.value = latest.toLocaleString();
  }, 2000);

  // Interacciones
  intervalId4 = window.setInterval(() => {
    const base = 4000;
    const randomNewValue = base + Math.floor(Math.random() * 3000);
    const newData = Array.from({ length: 8 }, () =>
      base + Math.floor(Math.random() * 3000)
    );
    sparkData4.value.chartSeries = [{ data: newData }];
    sparkData4.value.value = randomNewValue.toLocaleString();
  }, 2000);
});

onUnmounted(() => {
  if (intervalId) clearInterval(intervalId);
  if (intervalId2) clearInterval(intervalId2);
  if (intervalId3) clearInterval(intervalId3);
  if (intervalId4) clearInterval(intervalId4);
});
</script>

<style scoped>
.dashboard-grid {
  gap: 16px;
}

ion-row {
  overflow: hidden;
}

ion-col {
  max-height: 100%;
  --ion-grid-column-padding: 10px;
}

.box {
  background: var(--ion-color-light);
  height: 100%;
  max-height: 100%;
  overflow: hidden;
  border-radius: 12px;
  box-shadow: 0 2px 12px rgba(0, 0, 0, 0.1);
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: center;
  padding: 10px;
}

.sparkline-title {
  font-size: 14px;
  color: var(--ion-color-dark);
  margin-bottom: 8px;
  font-weight: bold;
  text-align: center;
}

.sparkline-value {
  font-size: 16px;
  color: var(--ion-color-dark);
  margin-top: 8px;
  font-weight: bold;
  text-align: center;
}

/* Aplicar altura total y por filas, solo en pantallas ≥ md */
@media (min-width: 992px) {  
  ion-grid { height: 100%; }
  .ion-row-1 { height: 20%; max-height: 20%; }
  .ion-row-2 { height: 40%; max-height: 40%; }
  .ion-row-3 { height: 40%; max-height: 40%; }
}

/* Gradientes para los sparklines */
.gradient-blue {
  background: linear-gradient(135deg, #3a8dff, #1e4aff);
}
.gradient-green {
  background: linear-gradient(135deg, #28c76f, #20a458);
}
.gradient-orange {
  background: linear-gradient(135deg, #ff914d, #ff6e1d);
}
.gradient-pink {
  background: linear-gradient(135deg, #ff5ca3, #ff367f);
}
</style>
