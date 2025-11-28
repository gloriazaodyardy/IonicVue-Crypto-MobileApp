<template>
  <ion-fab horizontal="center" vertical="bottom" slot="fixed">
    <ion-fab-button title="Options" aria-label="Options">
      <ion-icon :icon="ellipsisHorizontal"></ion-icon>
    </ion-fab-button>
    <!-- Refresh button -->
    <ion-fab-list side="start">
      <ion-fab-button color="secondary" @click="reloadPage" title="Refresh" aria-label="Refresh">
        <ion-icon :icon="refresh"></ion-icon>
      </ion-fab-button>
    </ion-fab-list>
    <!-- Scroll to top button -->
    <ion-fab-list side="end">
      <ion-fab-button color="secondary" @click="scrollToTop" title="Scroll up" aria-label="Scroll up">
        <ion-icon :icon="caretUp"></ion-icon>
      </ion-fab-button>
    </ion-fab-list>
  </ion-fab>
</template>
<script setup lang="ts">
import { ellipsisHorizontal, caretUp, refresh } from "ionicons/icons";
import { IonFab, IonFabButton, IonFabList, IonIcon } from "@ionic/vue";

const reloadPage = () => {
    // Reload the current page
    if (typeof globalThis !== "undefined" && globalThis.location?.reload) { //check if globalThis and location.reload are available
        globalThis.location.reload();
    }
};
const scrollToTop = () => {
  // Scroll to top of the page
 const ionContent = document.querySelector("ion-content"); //select ion-content
  if (ionContent && typeof (ionContent as any).scrollToTop === "function") { //if ion-content found
    (ionContent as any).scrollToTop(300); // smooth scroll to top in 300ms
    return;
  }
  // fallback to window scroll if ion-content not found
  if (typeof globalThis !== "undefined" && globalThis.window?.scrollTo) {
    globalThis.window.scrollTo({ top: 0, behavior: "smooth" });
  }
};
</script>
