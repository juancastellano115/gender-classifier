<template>
  <div id="app">
    <Title msg="Clasificación de Género con Vue y Tensorflow" />
    <ia />
  </div>
</template>

<script>
import Title from "./components/Title.vue";
import ia from "./components/ia";
export default {
  name: "App",
  components: {
    Title,
    ia
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
.footerVue{
  margin-top: 1em;
  width: 100%;
  font-weight: bold;
}
</style>
