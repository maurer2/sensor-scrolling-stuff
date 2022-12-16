<script lang="ts">
  // import { page } from '$app/stores';
  const routesAndFiles = import.meta.glob('./**/*.svelte');

  // extract file ending, +page and ./ from file path
  const routes = Object.keys(routesAndFiles).map(routePath => routePath
    .replace('+page', '')
    .replace('.svelte', '')
    .replace('./', '/')
  );

  console.log(routes)
</script>

<nav>
  {#each routes as route}
    <a href="{route}">
      {route === '/' ? '/Home' : route}
    </a>
  {/each}

</nav>

<slot></slot>
