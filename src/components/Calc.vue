<template>
  <b-row class="my-4">
    <b-col sm="6" class="pr-4">
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
        <b-alert show variant="primary">
          <div>
            <span class="font-weight-bold">Anzahl Bitcoins:</span>
            {{ quantityCost | precision(4) }}
          </div>
        </b-alert>
        <b-alert show variant="primary" v-if="priceCost">
          <div>
            <span class="font-weight-bold">Ausgaben:</span>
            {{ priceCost | precision(2) }}€
          </div>
        </b-alert>
      </div>
    </b-col>

    <!-- ===== SELL ===== -->
    <b-col sm="6" class="pr-4">
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

        <!-- quotationSell -->
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

        <!-- profit -->
        <b-row class="my-4">
          <b-col>
            <div v-if="quotationMin">
              <b-alert show variant="warning">
                <span class="font-weight-bold">Min. Kurs:</span>
                {{ quotationMin | precision(2) }}€</b-alert
              >
            </div>
            <div v-if="priceSell">
              <b-alert show variant="info">
                <span class="font-weight-bold">Auszahlung:</span>
                {{ priceSell | precision(2) }}€</b-alert
              >
            </div>
            <div v-if="isNumber(profit)">
              <b-alert
                show
                :variant="
                  profit > 0 ? 'success' : profit === 0 ? 'info' : 'danger'
                "
              >
                <span class="font-weight-bold">Gewinn:</span>
                {{ profit | precision(2) }}€</b-alert
              >
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
  filters: {
    precision(v, precision) {
      if (!_.isFinite(v)) return "";
      return v.toFixed(_.round(precision));
    }
  },
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
        return this.quantity * 0.99;
      },
      set(v) {
        v = this.toNumber(v);
        if (!this.isNumber(v)) return;
        this.quantity = _.round(v / 0.99, 6);
      }
    },
    priceCost() {
      if (!this.isNumber(this.quotation)) return;
      const total = this.quotation * this.quantity;

      return this.toNumber(_.round(total - total * 0.005, 2));
    },
    priceSell() {
      if (!this.isNumber(this.profit) || !this.isNumber(this.priceCost)) return;
      return this.priceCost + this.profit;
    },
    profit() {
      if (!this.isNumber(this.quotationSell)) return;
      const p = this.quotationSell * this.quantityCost * 0.995 - this.priceCost;
      return this.toNumber(_.round(p, 2));
    },
    quotationMin() {
      if (!this.isNumber(this.quotation)) return;
      return this.priceCost / (this.quantityCost * 0.995);
    }
  },
  watch: {
    quantity(v) {
      this.quantity = this.toNumber(v);
    },
    quotation(v) {
      this.quotation = this.toNumber(v);
    },
    quotationSell(v) {
      this.quotationSell = this.toNumber(v);
    }
  },
  methods: {
    toNumber(v) {
      v = v.length === 1 && v.charAt(0) === "," ? "0,0" : v;
      return _.toNumber(_.replace(v, /,/g, "."));
    },
    isNumber(v) {
      return _.isFinite(v);
    }
  }
};
</script>

<style scoped lang="scss"></style>
