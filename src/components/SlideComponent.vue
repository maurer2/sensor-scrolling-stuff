<template>
  <section class="slide-container" @pointerdown="pointerDown"  @pointerup="pointerUp" @pointermove="pointerMove">
    <figure class="slide-inner">
      <img src="../assets/cat-small.jpg" alt="cat" class="slide-background slide-background--is-slideable">
      <figcaption class="slide-content">
        Captions
      </figcaption>
    </figure>
  </section>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';

@Component
export default class SlideComponent extends Vue {
 pointerIsDown: boolean = false;
 startPositionX: number = 0;
 imageWidth: number = 960;

 pointerDown(event) {
     this.pointerIsDown = true;
     this.startPositionX = event.pageX;

 }

  pointerUp() {
      this.pointerIsDown = false;
      this.startPositionX = 0;
  }

 pointerMove(event) {
     if (!this.pointerIsDown) {
         return
     }

     const delta = this.startPositionX - event.pageX;
     const percentage = Math.round((this.imageWidth) / delta);

     // const x = 0;
     // const newTransformX = x + event.pageX;

     console.log(percentage);
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
    $width: 200%;
    $overflow-width: $width - 100%;

    width: $width;
    transform: translateX(#{($overflow-width / 4) * -1});
    transform-origin: 50% 50%;
  }
}

.slide-content {
  position: absolute;
  left: 1rem;
  right: 1rem;
  bottom: 1rem;
  text-align: center;
  background: beige;
}
</style>
