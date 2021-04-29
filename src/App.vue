<template>
  <div id="app">
    <Places />
  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'
import Places from "./components/Places.vue";

export default {
  name: "App",
  components: {
    Places,
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
