<template>
  <div id="app">
    <MainContent />
  </div>
</template>

<script>
import MainContent from "./components/MainContent.vue";
export default {
  name: "App",
  components: {
    MainContent,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
}
</style>
