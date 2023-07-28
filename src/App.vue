<template>
    <h1 class="text-3xl font-bold underline">
        <SliderComponent></SliderComponent>
    </h1>
</template>

<script>
import SliderComponent from "@/components/SliderComponent.vue";
export default {
  name: 'App',
  components: {
      SliderComponent
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
