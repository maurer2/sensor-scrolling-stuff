<script lang="ts">
  import "../app.postcss";
  import { page } from '$app/stores';

  // import { page } from '$app/stores';
  const routesAndFiles = import.meta.glob('./**/*.svelte');

  // extract file ending, +page and ./ from file path
  const routes = Object.keys(routesAndFiles).map(routePath => routePath
    .replace('+page', '')
    .replace('.svelte', '')
    .replace('./', '/')
  );

</script>

<div class="h-screen bg-black">
  <nav class="bg-gray-300 flex flex-wrap justify-center sticky top-0">
    {#each routes as route}
      <a class="p-4 flex-none capitalize" href="{route}" class:text-blue-500="{$page.url.pathname === route}">
        {route === '/' ? 'Home' : route.slice(1, -1)}
      </a>
    {/each}
  </nav>
  <main class="w-[480px] ml-auto mr-auto p-4 bg-white">
    <slot></slot>
  </main>
</div>
