<template>
  <Calulator />
</template>

<script>
import Calulator from "./components/Calulator.vue";

export default {
  name: "App",
  components: {
    Calulator,
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Karla&display=swap");
* {
  padding: 0;
  margin: 0;
}
#app {
  font-family: "Karla", sans-serif !important;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /* text-align: center; */
  /* color: #2c3e50; */
  background-color: #f2f2f2;
  height: 100vh;
}
</style>
