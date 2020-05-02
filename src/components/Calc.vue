<template>
  <b-row class="my-4">
    <b-col sm="6">
      <!-- ===== BUY ====== -->
      <b-row>
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
          <b-input-group append="€">
            <b-form-input
              id="quotation"
              type="text"
              autocomplete="off"
              v-model="quotation"
            ></b-form-input>
          </b-input-group>
        </b-col>
      </b-row>

      <!-- quantity cost -->
      <div v-if="quantityCost">
        <b-alert show>
          <div>Anzahl Bitcoins: {{ quantityCost }}</div>
        </b-alert>
        <b-alert show v-if="priceCost">
          <div>EK-Preis: {{ priceCost }}€</div>
        </b-alert>
      </div>
    </b-col>

    <!-- ===== SELL ===== -->
    <b-col sm="6">
      <div>
        <b-row>
          <b-col>
            <h2><b-badge>SELL</b-badge></h2>
          </b-col>
        </b-row>

        <!-- quantity -->
        <b-row class="my-4">
          <b-col sm="5">
            <label for="quantityCost">Anzahl:</label>
          </b-col>
          <b-col sm="7">
            <b-form-input
              id="quantityCost"
              type="text"
              autocomplete="off"
              v-model="quantityCost"
            ></b-form-input>
          </b-col>
        </b-row>

        <!-- quotation -->
        <b-row class="my-4">
          <b-col sm="5">
            <label for="quotationSell">Kurs:</label>
          </b-col>
          <b-col sm="7">
            <b-input-group append="€">
              <b-form-input
                id="quotationSell"
                type="text"
                autocomplete="off"
                v-model="quotationSell"
              ></b-form-input>
            </b-input-group>
          </b-col>
        </b-row>

        <!-- quotatioSelln  -->
        <b-row class="my-4">
          <b-col>
            <div v-if="quotationMin">
              <b-alert show>Min. Kurs: {{ quotationMin }}€</b-alert>
            </div>
            <div v-if="profit">
              <b-alert show>Gewinn: {{ profit }}€</b-alert>
            </div>
          </b-col>
        </b-row>
      </div>
    </b-col>
  </b-row>
</template>

<script>
import _ from "lodash";

export default {
  name: "Calc",
  props: {},
  data() {
    return {
      quantity: undefined,
      quotation: undefined,
      quotationSell: undefined
    };
  },
  computed: {
    quantityCost: {
      get() {
        if (!this.isNumber(this.quantity)) return;
        return _.round(this.quantity * 0.99, 4);
      },
      set(v) {
        v = this.toNumber(v);
        if (!this.isNumber(v)) return;
        this.quantity = _.round(v / 0.99, 4);
      }
    },
    priceCost() {
      if (!this.isNumber(this.quotation)) return;
      const total = this.quotation * this.quantity;

      return _.round(total - total * 0.005, 2);
    },
    quotationMin() {
      if (!this.isNumber(this.quotation)) return;
      return _.round(this.quotation + this.quotation * 0.001, 2);
    },
    profit() {
      const n = this.quantityCost * this.quotationSell;
      return _.round(n - n * 0.005, 2);
    }
  },
  watch: {
    quantity(v) {
      this.quantity = this.toNumber(v);
    },
    quotation(v) {
      this.quotation = this.toNumber(v);
    }
  },
  methods: {
    toNumber(v) {
      v = v.length === 1 && v.charAt(0) === "," ? "0,0" : v;
      return _.toNumber(_.replace(v, /,/g, "."));
    },
    isNumber(v) {
      return !_.isNaN(v) && _.isNumber(v);
    }
  }
};
</script>

<style scoped lang="scss"></style>
