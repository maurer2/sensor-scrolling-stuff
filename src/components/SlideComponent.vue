<template>
  <figure class="figure">
    Sliding figure
  </figure>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';

@Component
export default class SlideComponent extends Vue {
  @Prop() private msg!: string;
}
</script>

<style scoped lang="scss">
.figure {
  margin: 0;
  padding: 2rem;
  background: aliceblue;
  text-align: center;
}
</style>
