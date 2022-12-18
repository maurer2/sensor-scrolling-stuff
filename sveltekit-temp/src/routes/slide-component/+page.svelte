<script lang="ts">
  import { faker } from '@faker-js/faker';
  import { onMount } from 'svelte';


  let pointerIsDown = false;
  let translateX = 0;
  let transformValue = '';
  let container;
  let image;

  function activatePointer(): void {
    pointerIsDown = true;
  }

  function deactivatePointer(): void {
    pointerIsDown = false;
  }

  function pointerMove(event): void {
    if (pointerIsDown) {
      return
    }

    const boundingBoxContainer = container.getBoundingClientRect();
    const width = boundingBoxContainer.width;
    const offsetLeft = container.offsetLeft;

    const overflowSize = Math.abs(image.offsetLeft);

    const pointerX = event.x;

    const currentPositionInPercent = Math.round((pointerX - offsetLeft) * 100 / width);
    const maxTransformX = Math.round(overflowSize * 100 / width);
    const elongationFromCenterAbsolute = (currentPositionInPercent - 50) * 2;

    const elongationFromCenterMappedToOverflowSize = (maxTransformX * elongationFromCenterAbsolute) / 100;
    translateX = elongationFromCenterMappedToOverflowSize;
  }

</script>

<section
  on:pointerdown={activatePointer}
  on:pointerup={deactivatePointer}
  on:pointermove={pointerMove}
  on:pointerleave={deactivatePointer}

>
  <h2 class="text-xl pb-4">Slide component</h2>

  <p class="pb-4">{faker.lorem.lines()}</p>

  <figure bind:this={container} style:--translateX="{translateX}">
    <figcaption class="absolute bottom-0 bg-white">
      Meow! {translateX}%
    </figcaption>
    <img src={faker.image.cats(640, 480, false)} alt="Cat" bind:this={image}>
  </figure>

</section>

