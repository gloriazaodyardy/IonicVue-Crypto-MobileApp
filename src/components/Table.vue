<template>
  <ion-grid>
    <ion-row v-for="item in dataOnDisplay" :key="item.id">
      <ion-col>
        <strong>Rank</strong>
        <strong>{{ item.rank }}</strong>
      </ion-col>
      <ion-col>
        <strong>{{ item.name }}</strong>
        <p>{{ item.symbol }}</p>
      </ion-col>
      <ion-col>
        <strong>Price (USD)</strong>
        <p>${{ parseFloat(item.priceUSD).toFixed(2) }}</p>
      </ion-col>
    </ion-row>
  </ion-grid>
  <ion-infinite-scroll @ionInfinite="loadMore" threshold="100px">
    <ion-infinite-scroll-content></ion-infinite-scroll-content>
  </ion-infinite-scroll>
</template>

<script setup lang="ts">
import { ref, onMounted } from "vue";
import {
  IonCol,
  IonGrid,
  IonRow,
  IonInfiniteScroll,
  IonInfiniteScrollContent,
  InfiniteScrollCustomEvent,
} from "@ionic/vue";

interface cryptoItem {
  //interface for crypto data item
  id: string;
  rank: number;
  name: string;
  symbol: string;
  priceUSD: string;
}

const cryptoData = ref<cryptoItem[]>([]); //main storage for crypto data
const APIURL: string = "https://api.coinlore.net/api/tickers/"; //api url
const dataOnDisplay = ref<cryptoItem[]>([]); //data displayed on the screen
const pageSize: number = 10; //number of data to load per scroll

///// Fetch Crypto Data /////

onMounted(async () => {
  //fetch crypto data
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

    // initialize first chunk to display
    dataOnDisplay.value = cryptoData.value.slice(0, pageSize);
  } catch (error) {
    //handle fetch error
    console.error("Error fetching crypto data:", error);
  }
});

///// Infinite Scroll Handler /////

// function to load more data on scroll
const loadMore = (event: InfiniteScrollCustomEvent) => {
  const currentLength = dataOnDisplay.value.length;
  const nextEndIndex = currentLength + pageSize;
  const nextChunk = cryptoData.value.slice(currentLength, nextEndIndex);

  dataOnDisplay.value = [...dataOnDisplay.value, ...nextChunk]; // append new items to display

  event.target.complete();

  // Disable infinite scroll if all data is loaded
  if (nextEndIndex >= cryptoData.value.length) {
    event.target.disabled = true;
  }
};
</script>

<style scoped>
ion-col {
  display: flex;
  flex-direction: column;
  text-align: center;
  align-items: center;
  border: solid 1px var(--ion-color-secondary);

  &:first-child > strong:nth-child(2) {
    margin-top: 1rem;
  }
  > strong:first-child {
    font-weight: bold;
    margin-block: 1rem;
    color: var(--ion-color-primary);
  }
}
</style>
