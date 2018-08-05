<template>
  <section class="slide-container" @pointerdown="activatePointer" @pointerup="deactivatePointer"
           @pointermove="pointerMove" @pointerleave="deactivatePointer" >
    <figure class="slide-inner" ref="image">
      <img src="../assets/cat-small.jpg" alt="background-image" class="slide-background slide-background--is-slideable"
        :style="styleAttribute">
      <figcaption class="slide-content">
        Caption
      </figcaption>
    </figure>
  </section>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';

@Component
export default class SlideComponent extends Vue {
  private pointerIsDown: boolean = false;
  private currentPositionInPercent: number = 0;

  private activatePointer() {
    this.pointerIsDown = true;
  }

  private deactivatePointer() {
    this.pointerIsDown = true;
    this.currentPositionInPercent = 0;
  }

  private pointerMove(event: PointerEvent) {
    if (!this.pointerIsDown) {
      return;
    }

    const element = this.$refs.image as HTMLElement;
    const boundingBox = element.getBoundingClientRect();

    const width = boundingBox.width;
    const offsetX = boundingBox.left;
    const pointerX = event.x;

    this.currentPositionInPercent = Math.round((pointerX - offsetX) * 100 / width);
  }

  get styleAttribute() {
    return {
      transform: `translateX(${this.currentPositionInPercent}%)`,
    };
  }
}
</script>

<style scoped lang="scss">
.slide-container {
  margin: 2rem 0;
  padding: 0;
  background: aliceblue;
  overflow: hidden;
}

.slide-inner {
  position: relative;
  margin: 0;
  padding: 0;
}

.slide-background {
  display: block;
  width: 100%;
  height: auto;
  pointer-events: none;

  &--is-slideable {
    $width: 150%;
    $overflow-width: $width - 100%;

    width: $width;
    margin-left: ($overflow-width / 2) * -1;
    margin-right: ($overflow-width / 2) * -1;
    transform: translateX(0%);
    will-change: transform;
  }
}

.slide-content {
  position: absolute;
  left: 1rem;
  right: 1rem;
  bottom: 1rem;
  text-align: center;
  background: #fff;
}
</style>
