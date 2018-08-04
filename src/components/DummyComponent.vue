<template>
  <section class="dummy-content">
    {{ dummyText }}
  </section>
</template>

<script lang="ts">
// https://github.com/vuejs/vue-class-component
import { Component, Prop, Vue } from 'vue-property-decorator';

@Component
export default class DummyComponent extends Vue {
  @Prop({ default: 0 }) private chars: number;

  private textSample = `Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt
   ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut
   aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu
   fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit
   anim id est laborum.`;

  // computed
  get dummyText () {
      return this.textSample.repeat(this.chars);
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss"></style>
