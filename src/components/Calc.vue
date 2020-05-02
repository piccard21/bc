<template>
  <div>
    <!-- quantity -->
    <b-row class="my-4">
      <b-col sm="3">
        <label for="quantity">Anzahl:</label>
      </b-col>
      <b-col sm="9">
        <b-form-input
          id="quantity"
          type="text"
          v-model="quantity"
        ></b-form-input>
      </b-col>
    </b-row>

    <!-- quotation -->
    <b-row class="my-4">
      <b-col sm="3">
        <label for="quotation">Kurs/BC:</label>
      </b-col>
      <b-col sm="9">
        <b-form-input
          id="quotation"
          type="text"
          v-model="quotation"
        ></b-form-input>
      </b-col>
    </b-row>

    <div>
      <b-alert show>Du erhältst: {{ quantityReal }}</b-alert>
    </div>
  </div>
</template>

<script>
export default {
  name: "Calc",
  props: {},
  data() {
    return {
      quantity: undefined,
      quotation: undefined
    };
  },
  computed: {
    quantityReal() {
      return parseFloat(this.quantity) * 0.99;
    }
  }
};
</script>

<style scoped lang="scss"></style>
