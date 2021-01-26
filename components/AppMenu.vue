<template>
  <nav>
    <ul>
      <li>
        <nuxt-link to="/" exact><AppLogo /></nuxt-link>
      </li>
      <li><nuxt-link to="/restaurants" exact>Restaurants</nuxt-link></li>
    </ul>
    <div v-if="totalCount > 0" class="smallnum">{{ totalCount }}</div>
    <nuxt-link to="/cart">Cart </nuxt-link>
  </nav>
</template>

<script>
import AppLogo from "@/components/AppLogo.vue";

import { mapGetters } from "vuex";

export default {
  components: {
    AppLogo
  },
  computed: {
    ...mapGetters(["totalCount"])
  }
};
</script>

<style lang="scss" scoped></style>
