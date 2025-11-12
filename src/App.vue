<template>
  <div class="wrap">
    <Header />
    <main>
      <RouterView></RouterView>
      <Quick />
    </main>
    <Footer v-if="!route.meta.hideFooter" />
  </div>
</template>
<script setup>
import { useRoute } from "vue-router";
import Footer from "./components/Footer.vue";
import Header from "./components/Header.vue";
import Quick from "./components/Quick.vue";

const route = useRoute()
</script>

<style lang="scss" scoped></style>
