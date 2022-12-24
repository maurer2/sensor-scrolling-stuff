<script lang="ts">
  import { faker } from '@faker-js/faker';

  let containerElement: HTMLElement;
  let imageElement: HTMLElement;

  let pointerIsDown: boolean;
  let translateX = 0; // todo make reactive
  let transitionDuration = 0;

  function activatePointer(): void {
    pointerIsDown = true;
  }

  function deactivatePointer(): void {
    pointerIsDown = false;
    translateX = 0;
    transitionDuration = 500;
  }

  function pointerMove(event: PointerEvent): void {
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
    transitionDuration = 0;
  }
</script>

<section>
  <h2 class="text-xl pb-4">Rubber band component</h2>
  <p class="pb-8">Drag left and right with return to the centre</p>
  <div class="-m-4 p-0 overflow-hidden"
    on:pointerdown={activatePointer}
    on:pointerup={deactivatePointer}
    on:pointermove={pointerMove}
    on:pointerleave={deactivatePointer}
    style="--translateX: {translateX}%; --transitionDuration: {transitionDuration}ms"
  >
    <figure
      bind:this={containerElement}
      class="relative select-none touch-none transform-gpu translate-x-[var(--translateX)] transition-transform duration-[var(--transitionDuration)] ease-linear"
    >
      <img
        bind:this={imageElement}
        class="w-[200%] ml-[-50%] mr-[-50%] max-w-none pointer-events-none"
        src={faker.image.cats(800, 400, false)}
        alt="Cat">
      <figcaption class="absolute inset-x-4 bottom-4 text-center bg-white">
        Meow!
      </figcaption>
    </figure>
  </div>
</section>
