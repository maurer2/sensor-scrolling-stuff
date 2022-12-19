<script lang="ts">
  import { faker } from '@faker-js/faker';
  import { onMount } from 'svelte';

  let containerElement;
  let imageElement;

  let pointerIsDown;
  let translateX = 0; // todo make reactive

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

    const { width } = containerElement.getBoundingClientRect();
    const { offsetLeft } = containerElement;

    const overflowSize = Math.abs(imageElement.offsetLeft);
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
    style="--translateX: {translateX}%"
  >
    <figure
      bind:this={containerElement}
      class="relative select-none touch-none transform-gpu translate-x-[var(--translateX)]"
    >
      <img
        bind:this={imageElement}
        class="w-[150%] ml-[-25%] mr-[-25%] max-w-none pointer-events-none"
        src={faker.image.cats(800, 600, false)}
        alt="Cat">
      <figcaption class="absolute inset-x-4 bottom-4 text-center bg-white">
        Meow!
      </figcaption>
    </figure>
  </div>
</section>
