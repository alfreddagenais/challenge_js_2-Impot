<template>
  <div id="app">
    <CalculatorImpot/>
  </div>
</template>

<script>
import CalculatorImpot from "./components/CalculatorImpot";

export default {
  name: "App",
  components: {
    CalculatorImpot
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
