<script lang="ts">
  import { onMount } from 'svelte';
  import { ChevronRight } from 'lucide-svelte';
  
  let mounted = false;
  
  onMount(() => {
    mounted = true;
  });
</script>

{#if mounted}
  <div class="bg-gray-800 rounded-lg p-8 border border-gray-700">
    <h2 class="text-2xl font-bold mb-4 flex items-center">
      <span>Svelte Component</span>
      <ChevronRight class="ml-2 w-6 h-6 text-primary-400" />
    </h2>
    <p class="text-gray-300 mb-4">
      This is a Svelte component rendered within an Astro page!
    </p>
    <button 
      class="bg-accent-400 hover:bg-accent-500 px-4 py-2 rounded font-medium transition-colors"
      on:click={() => alert('Hello from Svelte!')}
    >
      Click me!
    </button>
  </div>
{/if}