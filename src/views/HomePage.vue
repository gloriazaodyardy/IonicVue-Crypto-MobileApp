<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar>
        <ion-title>Cryptocurrency</ion-title>
      </ion-toolbar>
    </ion-header>

    <ion-content :fullscreen="true">
      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">Cryptocurrency</ion-title>
        </ion-toolbar>
      </ion-header>
      <div id="container">
        <h1>CRYPTOCURRENCY APPLICATION</h1>
        <p>Explore the best cryptocurrencies to invest in.</p>
        <div class="container-table">
          <Table />
        </div>
      </div>
    </ion-content>
  </ion-page>
</template>

<script setup lang="ts">
import {
  IonContent,
  IonHeader,
  IonPage,
  IonTitle,
  IonToolbar,
} from "@ionic/vue";
import Table from "@/components/Table.vue";
</script>

<style scoped>
#container {
  margin-top: 28dvh;
  text-align: center;
  padding-inline: clamp(1rem, 4.286vw + 0.143rem, 4rem);

  div {
    margin-top: 4rem;
  }

  h1 {
    font-weight: 900;
    font-size: xx-large;
    color: var(--ion-color-primary);
  }
}
</style>
