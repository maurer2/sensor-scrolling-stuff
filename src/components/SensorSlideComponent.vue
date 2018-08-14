<template>
  <section class="slide-wrapper">
    <figure class="slide" :style="styleAttribute" ref="slide">
      <img src="../assets/cat2-small.jpg" alt="background-image" class="slide-background slide-background--is-slideable" ref="image">
      <figcaption class="slide-content">
        Caption
      </figcaption>
    </figure>
  </section>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';

@Component
export default class SensorSlideComponent extends Vue {
  private translateX: number = 0;

  private orientationChangEvent(event: DeviceOrientationEvent) {
    const rotationX = event.alpha;

    const container = this.$refs.slide as HTMLElement;
    const image = this.$refs.image as HTMLElement;

    if (typeof container === 'undefined' || typeof image === 'undefined') {
        return;
    }

    const boundingBoxContainer = container.getBoundingClientRect();
    const width = boundingBoxContainer.width;

    const overflowSize = Math.abs(image.offsetLeft);

    const maxTransformX = Math.round(overflowSize * 100 / width);
    const elongationFromCenterAbsolute = (rotationX * 100) / 180;

    //slightly long variable name
    const elongationFromCenterMappedToOverflowSize = (maxTransformX * elongationFromCenterAbsolute) / 100;

    this.translateX = elongationFromCenterMappedToOverflowSize;
  }

  private created () {
    window.addEventListener('deviceorientation', this.orientationChangEvent, true);
  }

  get styleAttribute() {
    return {
      transform: `translateX(${this.translateX}%)`,
    };
  }
}
</script>

<style scoped lang="scss">
.slide-wrapper {
  margin: 2rem 0;
  padding: 0;
  background: aliceblue;
  overflow: hidden;
}

.slide {
  position: relative;
  margin: 0;
  padding: 0;
  transform: translateX(0);
  will-change: transform;
  touch-action: none;
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
