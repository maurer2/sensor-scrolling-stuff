<script lang="ts">
  import { faker } from '@faker-js/faker';

  let containerElement;
  let imageElement;

  let translateX = 0; // todo make reactive

  function orientationChang(event): void {
    const rotationX = event.alpha; // y-axis

    const { width } = containerElement.getBoundingClientRect();

    const overflowSize = Math.abs(imageElement.offsetLeft);

    const maxTransformX = Math.round(overflowSize * 100 / width);
    const elongationFromCenterAbsolute = (rotationX * 100) / 180;

    const elongationFromCenterMappedToOverflowSize = (maxTransformX * elongationFromCenterAbsolute) / 100;

    translateX = elongationFromCenterMappedToOverflowSize;
  }

</script>

<svelte:window on:deviceorientation={orientationChang}/>

<section>
  <h2 class="text-xl pb-4">Sensor slide component</h2>
  <p class="pb-8">Rotation around the y-axis</p>
  <div class="-m-4 p-0 overflow-hidden"
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
        alt="Cat"
      >
      <figcaption class="absolute inset-x-4 bottom-4 text-center bg-white">
        Meow!
      </figcaption>
    </figure>
  </div>
</section>
