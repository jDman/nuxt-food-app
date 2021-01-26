<template>
  <main class="container">
    <section
      class="image"
      :style="`background:url(/${currentItem.img}) no-repeat center center`"
    ></section>
    <section class="details">
      <h1>{{ currentItem.item }}</h1>

      <h3>Price: ${{ currentItem.price.toFixed(2) }}</h3>

      <div class="quantity">
        <input type="number" min="1" v-model="numberOfItems" />
        <button @click="addToCart" class="primary">
          Add to Cart - ${{ calculatedPrice }}
        </button>
      </div>
      <fieldset v-if="currentItem.options">
        <legend><h3>Options</h3></legend>

        <div v-for="option in currentItem.options" :key="option">
          <input
            type="radio"
            name="option"
            :id="option"
            :value="option"
            v-model="$v.itemOption.$model"
          />
          <label :for="option">{{ option }}</label>
        </div>
      </fieldset>
      <fieldset v-if="currentItem.addOns">
        <legend><h3>Add Ons</h3></legend>

        <div v-for="addOn in currentItem.addOns" :key="addOn">
          <input
            type="checkbox"
            name="addon"
            :id="addOn"
            :value="addOn"
            v-model="$v.itemAddOns.$model"
          />
          <label :for="addOn">{{ addOn }}</label>
        </div>
      </fieldset>

      <AppToast v-if="cartSubmitted">
        Order submitted <br />

        Check out more <nuxt-link to="/restaurants">restaurants</nuxt-link>!
      </AppToast>
    </section>
    <section class="options">
      <h3>Description</h3>
      <p>{{ currentItem.description }}</p>
    </section>
  </main>
</template>

<script>
import { mapState } from "vuex";

import AppToast from "@/components/AppToast.vue";

import { required } from "vuelidate/lib/validators";

export default {
  data() {
    return {
      id: this.$route.params.id,
      numberOfItems: 1,
      itemAddOns: [],
      itemOption: "",
      cartSubmitted: false,
      errors: false
    };
  },
  validations: {
    itemOption: { required },
    itemAddOns: { required }
  },
  components: {
    AppToast
  },
  computed: {
    ...mapState(["foodData"]),
    currentItem() {
      let result;

      for (let i = 0; i < this.foodData.length; i++) {
        if (result) break;

        for (let j = 0; j < this.foodData[i].menu.length; j++) {
          if (this.foodData[i].menu[j].id === this.id) {
            result = this.foodData[i].menu[j];
            break;
          }
        }
      }

      return result;
    },
    calculatedPrice() {
      return (+this.currentItem.price * +this.numberOfItems).toFixed(2);
    }
  },
  methods: {
    addToCart() {
      const formOutput = {
        item: this.currentItem.item,
        count: this.numberOfItems,
        options: this.itemOption,
        addOns: this.itemAddOns,
        combinedPrice: this.calculatedPrice
      };

      let addOnError = this.$v.itemAddOns.$invalid;
      let optionError = this.currentItem.itemOption
        ? this.$v.itemOption.$invalid
        : false;

      if (addOnError || optionError) {
        this.errors = true;
      } else {
        this.errors = false;
        this.cartSubmitted = true;

        this.$store.commit("addToCart", formOutput);
      }
    }
  }
};
</script>

<style lang="scss" scoped>
.container {
  width: 1000px;
  margin: 100px auto;
  display: grid;
  grid-template-columns: 400px 1fr;
  grid-template-rows: 400px 1fr;
  grid-column-gap: 60px;
  grid-row-gap: 60px;
  line-height: 2;
}

.image {
  grid-area: 1/1/2/2;
}

.details {
  grid-area: 1/2/2/3;
  position: relative;
}

.options {
  grid-area: 2/1/3/2;
}
</style>
