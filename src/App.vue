<template>
  <article class="wrapper">
    <DummyComponent chars="150" class="section" />
    <SlideComponent class="section" />
    <DummyComponent chars="100" class="section" />
    <SensorSlideComponent class="section" />
    <DummyComponent chars="200" class="section" />
    <RubberBandComponent class="section" />
    <DummyComponent chars="1000" class="section" />
  </article>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import DummyComponent from './components/DummyComponent.vue';
import SlideComponent from './components/SlideComponent.vue';
import SensorSlideComponent from './components/SensorSlideComponent.vue';
import RubberBandComponent from './components/RubberBandComponent.vue';

@Component({
  components: {
    DummyComponent, SlideComponent, SensorSlideComponent, RubberBandComponent
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
  margin: auto;
  padding: 1rem 0;
  max-width: 480px;
  background: #fff;
}
</style>
