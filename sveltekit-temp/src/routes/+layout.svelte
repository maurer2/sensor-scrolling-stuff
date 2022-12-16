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

<div class="container h-screen ml-auto mr-auto bg-gray-100">
  <nav class="bg-gray-300 grid grid-flow-col auto-cols-max">
    {#each routes as route}
      <a class="p-4" href="{route}" class:text-blue-500="{$page.url.pathname === route}">
        {route === '/' ? '/Home' : route}
      </a>
    {/each}
  </nav>
  <main class="p-4">
    <slot></slot>
  </main>
</div>
