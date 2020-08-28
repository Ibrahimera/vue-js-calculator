<template>
  <div id="app">
    <div class="container">
    <Calculator></Calculator>
    </div>
  </div>
</template>

<script>

import Calculator from './components/calculator.vue';
export default {
  name: 'app',
  components: {
   Calculator
  },

}
</script>

<style>

div.container{
  width: 300px;
  height: 500px;
  margin:0 auto;
}


</style>
