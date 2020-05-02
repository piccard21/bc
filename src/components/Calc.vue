<template>
  <div>
    <!-- BUY -->
    <b-row class="my-4">
      <b-col>
        <h2><b-badge>BUY</b-badge></h2>
      </b-col>
    </b-row>
    <!-- quantity -->
    <b-row class="my-4">
      <b-col sm="5">
        <label for="quantity">Anzahl:</label>
      </b-col>
      <b-col sm="7">
        <b-form-input
          id="quantity"
          type="text"
          autocomplete="off"
          v-model="quantity"
        ></b-form-input>
      </b-col>
    </b-row>

    <!-- quotation -->
    <b-row class="my-4">
      <b-col sm="5">
        <label for="quotation">Kurs:</label>
      </b-col>
      <b-col sm="7">
        <b-form-input
          id="quotation"
          type="text"
          autocomplete="off"
          v-model="quotation"
        ></b-form-input>
      </b-col>
    </b-row>

    <!-- quantity real -->
    <div v-if="quantityCalculated">
      <b-alert show>Du erhältst: {{ quantityCalculated }} Bitcoins</b-alert>
    </div>

    <!-- SELL -->
    <div>
      <b-row class="my-4">
        <b-col>
          <h2><b-badge>SELL</b-badge></h2>
        </b-col>
      </b-row>

      <!-- quantity -->
      <b-row class="my-4">
        <b-col sm="5">
          <label for="quantityReal">Anzahl:</label>
        </b-col>
        <b-col sm="7">
          <b-form-input
            id="quantityReal"
            type="text"
            autocomplete="off"
            v-model="quantityReal"
          ></b-form-input>
        </b-col>
      </b-row>

      <!-- min. quotation  -->
      <b-row class="my-4">
        <b-col>
          <div v-if="quotationMin">
            <b-alert show>Min. Kurs: {{ quotationMin }}€</b-alert>
          </div>
        </b-col>
      </b-row>
    </div>
  </div>
</template>

<script>
import _ from "lodash";

export default {
  name: "Calc",
  props: {},
  data() {
    return {
      quantity: undefined,
      quantityReal: undefined,
      quotation: undefined
    };
  },
  computed: {
    quantityCalculated() {
      if (_.isEmpty(this.quantity)) return;

      const n = this.toNumber(this.quantity);
      if (!this.isNumber(n)) return;
      return _.round(n * 0.99, 4);
    },
    quotationMin() {
      const n = this.toNumber(this.quotation);
      if (!this.isNumber(n)) return;
      return _.round(n + n * 0.001, 2);
    }
  },
  watch: {
    quantityCalculated(v) {
      this.quantityReal = v ? v : undefined;
    }
  },
  methods: {
    toNumber(v) {
      return _.toNumber(_.replace(v, /,/g, "."));
    },
    isNumber(v) {
      return !_.isNaN(v) && _.isNumber(v);
    }
  }
};
</script>

<style scoped lang="scss"></style>
