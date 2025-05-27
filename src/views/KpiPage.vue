<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar>
        <ion-buttons slot="start">
          <ion-menu-button color="primary"></ion-menu-button>
        </ion-buttons>
        <ion-title>🎯 KPIs</ion-title>
      </ion-toolbar>
    </ion-header>

    <ion-content :fullscreen="true" class="ion-padding">
      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">🎯 KPIs</ion-title>
        </ion-toolbar>
      </ion-header>

      <h1 class="ion-padding">🚀 KPIs de Negocio</h1>
      <ion-accordion-group expand="inset" :multiple="true">
        <ion-accordion v-for="item in negocio" :key="item.id" :value="item.id.toString()">
          <ion-item slot="header">
            <ion-label>{{ item.id }}. {{ item.title }}</ion-label>
          </ion-item>
          <div class="ion-padding" slot="content">
            <p>{{ item.description }}</p>
            <ion-list :inset="true">
              <ion-item v-for="(element, index) in item.smart" :key="index">
                <ion-label><b>{{ element.letter }}</b> → {{ element.content }}</ion-label>
              </ion-item>
            </ion-list>
          </div>
        </ion-accordion>
      </ion-accordion-group>

      <h1 class="ion-padding">📈 KPIs Técnicos</h1>
      <ion-accordion-group expand="inset" :multiple="true">
        <ion-accordion v-for="item in tecnicos" :key="item.id" :value="item.id.toString()">
          <ion-item slot="header">
            <ion-label>{{ item.id }}. {{ item.title }}</ion-label>
          </ion-item>
          <div class="ion-padding" slot="content">
            <p>{{ item.description }}</p>
            <ion-list :inset="true">
              <ion-item v-for="(element, index) in item.smart" :key="index">
                <ion-label><b>{{ element.letter }}</b> → {{ element.content }}</ion-label>
              </ion-item>
            </ion-list>
          </div>
        </ion-accordion>
      </ion-accordion-group>
    </ion-content>
  </ion-page>
</template>

<script setup lang="ts">
import { IonButtons, IonContent, IonHeader, IonMenuButton, IonPage, IonTitle, IonToolbar, IonAccordionGroup, IonAccordion, IonItem, IonLabel, IonList } from '@ionic/vue';
import { ref } from 'vue';

interface SmartElement {
  letter: string;
  content: string;
}

interface SmartGoal {
  id: number;
  title: string;
  description: string;
  smart: SmartElement[];
}

// KPIs de Negocio
const negocio = ref<SmartGoal[]>([
  {
    id: 1,
    title: "Incrementar Ventas Mensuales",
    description: "Incrementar las ventas mensuales en un 30% durante los próximos 3 meses mediante campañas de email marketing y promociones en fechas clave.",
    smart: [
      { letter: "S", content: "Incrementar las ventas mensuales" },
      { letter: "M", content: "30% en 3 meses" },
      { letter: "A", content: "con campañas de email marketing y promociones" },
      { letter: "R", content: "para mejorar ingresos y conversión" },
      { letter: "T", content: "durante los próximos 3 meses" }
    ]
  },
  {
    id: 2,
    title: "Aumentar la Retención de Clientes",
    description: "Mejorar la retención de clientes en un 15% en los próximos 6 meses mediante la implementación de un programa de fidelización.",
    smart: [
      { letter: "S", content: "Mejorar la retención de clientes" },
      { letter: "M", content: "15% en 6 meses" },
      { letter: "A", content: "implementando un programa de fidelización" },
      { letter: "R", content: "para reducir la tasa de churn y aumentar ingresos recurrentes" },
      { letter: "T", content: "en los próximos 6 meses" }
    ]
  },
  {
    id: 3,
    title: "Ampliar la Oferta de Productos",
    description: "Lanzar 5 nuevos productos en los próximos 4 meses para diversificar el portafolio.",
    smart: [
      { letter: "S", content: "Lanzar 5 nuevos productos" },
      { letter: "M", content: "5 productos en 4 meses" },
      { letter: "A", content: "a través de un análisis de tendencias y mercado" },
      { letter: "R", content: "para diversificar el portafolio y captar nuevos segmentos de clientes" },
      { letter: "T", content: "en los próximos 4 meses" }
    ]
  },
  {
    id: 4,
    title: "Incrementar la Satisfacción del Cliente",
    description: "Aumentar el puntaje de satisfacción del cliente (CSAT) en un 10% durante el próximo trimestre mediante mejoras en el servicio al cliente.",
    smart: [
      { letter: "S", content: "Aumentar el CSAT" },
      { letter: "M", content: "10% en el próximo trimestre" },
      { letter: "A", content: "mediante mejoras en el servicio al cliente" },
      { letter: "R", content: "para mejorar la fidelidad y retención de clientes" },
      { letter: "T", content: "en los próximos 3 meses" }
    ]
  }
]);

// KPIs Técnicos
const tecnicos = ref<SmartGoal[]>([
  {
    id: 1,
    title: "Reducir Procesamientos Pendientes",
    description: "Reducir la cola de procesamiento en un 25% en las próximas 4 semanas optimizando el rendimiento del backend.",
    smart: [
      { letter: "S", content: "Reducir la cola de procesamiento pendiente" },
      { letter: "M", content: "25% menos en 4 semanas" },
      { letter: "A", content: "optimizando el rendimiento del backend" },
      { letter: "R", content: "para mejorar la eficiencia de procesamiento" },
      { letter: "T", content: "en 4 semanas" }
    ]
  },
  {
    id: 2,
    title: "Optimizar el Uso de la Base de Datos",
    description: "Optimizar las consultas a la base de datos reduciendo el tiempo de respuesta en un 20% durante los próximos 2 meses.",
    smart: [
      { letter: "S", content: "Reducir el tiempo de respuesta en la base de datos" },
      { letter: "M", content: "20% menos en 2 meses" },
      { letter: "A", content: "optimizando las consultas y estructura de índices" },
      { letter: "R", content: "para mejorar la eficiencia en la consulta de datos" },
      { letter: "T", content: "en 2 meses" }
    ]
  },
  {
    id: 3,
    title: "Mejorar el Tiempo de Carga del Sitio Web",
    description: "Reducir el tiempo de carga del sitio web en un 30% en los próximos 3 meses optimizando los recursos y mejorando el rendimiento.",
    smart: [
      { letter: "S", content: "Reducir el tiempo de carga del sitio web" },
      { letter: "M", content: "30% en 3 meses" },
      { letter: "A", content: "optimizando los recursos y mejorando el rendimiento" },
      { letter: "R", content: "para mejorar la experiencia del usuario" },
      { letter: "T", content: "en los próximos 3 meses" }
    ]
  }
]);
</script>

<style scoped>
h1 {
  font-size: 1.5rem;
  font-weight: bold;
  color: #3880ff;
}
</style>
