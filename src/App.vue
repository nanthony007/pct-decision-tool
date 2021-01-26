<template>
  <div id="app" class="p-4">
    <decision-tool />
  </div>
</template>

<script>
import DecisionTool from "./components/DecisionTool.vue";

export default {
  name: "App",
  components: {
    DecisionTool,
  },
};
</script>

<style>
#app {
  background-image: linear-gradient(white 50%, blue);
}
</style>
