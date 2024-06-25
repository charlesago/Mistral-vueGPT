<template>
  <div id="app">
    <LlamaInterface />
  </div>
</template>

<script>
import LlamaInterface from './components/LlamaInterface.vue';

export default {
  components: {
    LlamaInterface,
  },
};
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
