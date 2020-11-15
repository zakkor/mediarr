<script context="module">
  import { API_URL } from '../lib/constants.js';

  export async function preload() {
    const response = await this.fetch(API_URL + '/series');
    const series = await response.json();
    return { series };
  }
</script>

<script>
  export let series;
</script>

<style>
  .series-poster {
    width: 204px;
    height: 300px;
    background-image: var(--background);
  }
  .series:hover {
    transform: scale(1.05) translateY(-16px);
  }
</style>

<svelte:head>
  <title>Mediarr</title>
</svelte:head>

<main class="container px-4 md:px-8 py-10">
  {#if series.length > 0}
    <h2 class="text-3xl font-light mb-8">Your series</h2>
    <div class="grid grid-cols-2 md:grid-cols-4 gap-8">
      {#each series as s}
        <a class="series transition-transform ease-in-out duration-200" href="/series/{s.slug}" rel="prefetch">
          <div
            class="series-poster bg-contain bg-no-repeat bg-center rounded-lg"
            style="--background: url('{s.images.poster}');" />
          <h3 class="text-lg ml-1 mt-3">{s.title}</h3>
        </a>
      {/each}
    </div>
  {:else}
    <h2>You're not tracking any series yet, add some by searching for a series</h2>
  {/if}
</main>
