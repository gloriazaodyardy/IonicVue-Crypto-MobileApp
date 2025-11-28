<template>
  <ion-grid>
    <ion-row v-for="item in cryptoData" :key="item.id">
      <ion-col>
        <strong>Rank</strong>
        <p>{{ item.rank }}</p>
      </ion-col>
      <ion-col>
        <strong>{{ item.name }}</strong>
        <p class="cryptoSymbol">{{ item.symbol }}</p>
      </ion-col>
      <ion-col>
        <strong>Price (USD)</strong>
        <p>${{ parseFloat(item.priceUSD).toFixed(2) }}</p>
      </ion-col>
    </ion-row>
  </ion-grid>
</template>

<script setup lang="ts">
import { onMounted, ref } from "vue";
import { IonCol, IonGrid, IonRow } from "@ionic/vue";

interface cryptoItem {
  //interface for crypto data item
  id: string;
  rank: number;
  name: string;
  symbol: string;
  priceUSD: string;
}

const cryptoData = ref<cryptoItem[]>([]); //main storage for crypto data
const APIURL = "https://api.coinlore.net/api/tickers/"; //api url to fetch crypto data

onMounted(async () => {
  //fetch crypto data on component mount
  try {
    const response = await fetch(APIURL);
    const data = await response.json();
    cryptoData.value = data.data.map((item: any) => ({
      //map fetched data to cryptoItem interface
      id: item.id,
      rank: item.rank,
      name: item.name,
      symbol: item.symbol,
      priceUSD: item.price_usd,
    }));
  } catch (error) {
    //handle fetch error
    console.error("Error fetching crypto data:", error);
  }
});
</script>

<style scoped>
ion-col {
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
  justify-content: center;
  border: solid 1px var(--ion-color-secondary);
}
strong {
  margin: 0.5rem 0;
  font-weight: bold;
  color: var(--ion-color-primary);
}

.cryptoSymbol {
  font-weight: 900;
  font-size: 1.2rem;
}
</style>
