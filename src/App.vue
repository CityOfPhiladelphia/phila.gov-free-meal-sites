<template>
  <div id="vue-app-1">
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
/* #vue-app-1 {
  width: 70rem;
  margin: 0 auto;
} */

thead tr th {
    width: 50%;
  }
</style>
