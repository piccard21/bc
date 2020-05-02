<template>
  <div id="app">
    <b-container class="bv-example-row">
      <b-row>
        <b-col> <img alt="BC logo" src="./assets/bc_logo.svg"/></b-col>
      </b-row>
      <b-row>
        <calc msg="Welcome to Your Vue.js App" />
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Calc from "./components/Calc.vue";

export default {
  name: "App",
  components: {
    Calc
  }
};
</script>
