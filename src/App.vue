<template>
  <article class="wrapper">
    <DummyComponent chars="666" class="section" />
    <SlideComponent class="section" />
    <DummyComponent chars="999" class="section" />
    <SensorSlideComponent class="section" />
    <DummyComponent chars="5000" class="section" />
  </article>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import DummyComponent from './components/DummyComponent.vue';
import SlideComponent from './components/SlideComponent.vue';
import SensorSlideComponent from './components/SensorSlideComponent.vue';

@Component({
  components: {
    DummyComponent, SlideComponent, SensorSlideComponent,
  },
})
export default class App extends Vue {}
</script>

<style lang="scss">
html {
  margin: 0;
  background: black;
}

body {
  margin: 0;
  padding: 0;
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}

.wrapper {
  max-width: 480px;
  margin: auto;
  background: #fff;
}
</style>
