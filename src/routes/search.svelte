<script>
  import { stores } from '@sapper/app';
  import { search } from '../lib/stores.js';
  import { API_URL } from '../lib/constants.js';

  const { page } = stores();

  $search = $page.query.term;

  let fetchResults = [];
  async function doSearch(q) {
    const r = await fetch(`${API_URL}/series/lookup?term=${encodeURIComponent(q)}`);
    fetchResults = r.json();
    console.log(fetchResults);
  }
  $: process.browser && doSearch($page.query.term);
</script>

<style>
  .result {
    height: 300px;
  }
  .result-poster,
  .result-poster-placeholder {
    width: 204px;
    height: 300px;
  }
</style>

{#await fetchResults}
  <div class="flex mt-16">
    <div class="loader-heart mx-auto">
      <div />
    </div>
  </div>
{:then results}
  <div class="container mx-auto py-10">
    <h2 class="text-3xl font-light mb-8">Search results</h2>
    <div class="flex flex-col space-y-8">
      {#each results as r}
        <div class="result flex border rounded-lg border-indigo-100">
          {#if r.images.poster}
            <img
              class="result-poster flex-shrink-0 rounded-l-lg"
              src={r.images.poster}
              width="204"
              height="300"
              alt={r.title} />
          {:else}
            <div class="result-poster-placeholder flex flex-shrink-0 bg-gray-200 rounded-l-lg">
              <span class="text-sm tracking-wide italic text-gray-500 uppercase m-auto">No image</span>
            </div>
          {/if}
          <div class="flex flex-col w-full px-8 py-6">
            <div class="flex w-full items-center mb-4">
              <h2 class="text-2xl">{r.title}</h2>
              <h4 class="text-xl ml-8 text-gray-600">{new Date(r.first_aired).getFullYear()}</h4>
            </div>
            {#if r.overview && r.overview.length > 500}
              <p class="leading-relaxed">{r.overview.slice(0, 500)}...</p>
            {:else if r.overview}
              <p class="leading-relaxed">{r.overview}</p>
            {:else}
              <p class="text-gray-600 italic">This series doesn't have a description.</p>
            {/if}
            <div class="mt-auto">
              <div class="flex items-center justify-end">
                {#if r.seasons.some(s => s.monitored)}
                  <p class="text-sm tracking-wide italic text-gray-500 uppercase">Monitored</p>
                {:else}
                  <p class="text-sm tracking-wide text-gray-600 uppercase leading-none mr-6 mt-1">Not monitored</p>
                  <button class="focus:outline-none bg-blue-200 hover:bg-blue-300 text-gray-800 text-sm border border-indigo-200 uppercase leading-none tracking-wide rounded-lg transition-colors ease-in-out duration-200 px-10 pt-4 pb-3">
                    Add
                  </button>
                {/if}
              </div>
            </div>
          </div>
        </div>
      {/each}
    </div>
  </div>
{:catch error}
  <h2>Couldn't search series: {error}</h2>
{/await}
