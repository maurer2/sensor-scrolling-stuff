<script lang="ts">
  import { faker } from '@faker-js/faker';
  import { onMount } from 'svelte';

  let pointerIsDown;
  let translateX = 0;
  let container;
  let image;

  function activatePointer(): void {
    pointerIsDown = true;
  }

  function deactivatePointer(): void {
    pointerIsDown = false;
  }

  function pointerMove(event): void {
    if (!pointerIsDown) {
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

<section>
  <h2 class="text-xl pb-4">Slide component</h2>
  <p class="pb-4">{faker.lorem.lines()}</p>
  <div class="m-0 p-0 overflow-hidden"
    on:pointerdown={activatePointer}
    on:pointerup={deactivatePointer}
    on:pointermove={pointerMove}
    on:pointerleave={deactivatePointer}
  >
    <figure bind:this={container} class="relative select-none touch-none" style="translate: {translateX}% 0;">
      <img src={faker.image.cats(960, 640, false)} bind:this={image} class="w-[150%] ml-[-25%] mr-[-25%] max-w-none pointer-events-none" alt="Cat">
      <figcaption class="absolute inset-x-4 bottom-4 text-center bg-white">
        Meow!
      </figcaption>
    </figure>
  </div>

</section>
