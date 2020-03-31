<template>
  <div id="vue-app">
    <FreeMeals />
  </div>
</template>

<script>
import FreeMeals from './components/FreeMeals.vue'


export default {
  name: 'App',
  components: {
    FreeMeals,
  }
}
</script>

<style>
thead tr td {
    width: 50%;
  }
</style>
