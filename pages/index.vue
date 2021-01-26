<template>
  <main class="container">
    <AppHeader />
    <AppRestaurantInfo :datasource="foodData" />
  </main>
</template>

<script>
import AppHeader from "@/components/AppHeader.vue";
import AppRestaurantInfo from "@/components/AppRestaurantInfo.vue";
import { mapState } from "vuex";

export default {
  components: {
    AppRestaurantInfo,
    AppHeader
  },

  computed: {
    ...mapState(["foodData"])
  }
};
</script>

<style></style>
