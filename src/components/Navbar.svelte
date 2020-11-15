<script>
  import { goto } from '@sapper/app';
  import { search } from '../lib/stores.js';
</script>

<style>
</style>

<nav class="bg-indigo-100 border-b py-3">
  <div class="container flex items-center mx-auto">
    <a href="/" class="text-3xl font-light tracking-wide">Mediarr</a>
    <input
      type="text"
      class="w-3/5 text-lg rounded-md border focus:outline-none focus:border-indigo-400 transition-colors ease-in-out duration-200 py-4 px-6 ml-auto"
      bind:value={$search}
      on:keydown={event => {
        if (event.key === 'Enter') {
          goto(`/search?term=${encodeURIComponent($search)}`);
        }
      }}
      placeholder="Search for films or TV series..." />
  </div>
</nav>
