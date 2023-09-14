<template>
  <div id="app">
    <HeaderHome></HeaderHome>
    <CalculateTax></CalculateTax>
  </div>
</template>

<script>
import HeaderHome from './components/HeaderHome.vue'
import CalculateTax from './components/CalculateTax.vue'

export default {
  name: 'App',
  components: {
    HeaderHome,
    CalculateTax
  }
}
</script>

<style>

</style>
