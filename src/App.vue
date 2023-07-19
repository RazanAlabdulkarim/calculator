<template>
  <CalculatorApp msg="Calculator" /> <!-- Updated component name to "CalculatorApp" -->
</template>

<script>
import CalculatorApp from './components/Calculator.vue' // Updated import name to "CalculatorApp"

export default {
 name: 'App',
 components: {
   CalculatorApp  // Updated component name to "CalculatorApp"
 }
}
</script>

<style>
#app {
 font-family: Avenir, Helvetica, Arial, sans-serif;
 -webkit-font-smoothing: antialiased;
 -moz-osx-font-smoothing: grayscale;
 text-align: center;
 color: #2c3e50;
 margin-top: 60px;
}
</style>
