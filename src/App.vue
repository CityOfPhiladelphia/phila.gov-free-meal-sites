<template>
  <div id="vue-app">
    <FreeMeals />
    <StudentMeals />
  </div>
</template>

<script>
import FreeMeals from './components/FreeMeals.vue'
import StudentMeals from './components/StudentMeals.vue'

export default {
  name: 'App',
  components: {
    FreeMeals,
    StudentMeals
  }
}
</script>

<style>
#vue-app {
  max-width: 70rem;
  margin: 0 auto;
}
</style>
