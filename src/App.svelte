<!-- src/App.svelte -->
<script lang="ts">
  import { onMount } from 'svelte';
  import Header from './components/Header.svelte';
  import Hero from './components/Hero.svelte';
  import Features from './components/Features.svelte';
  import Converter from './components/Converter.svelte';
  import Footer from './components/Footer.svelte';
  
  // Load any initial data or settings if needed
  onMount(() => {
    console.log('SamsPDF App mounted');
  });
</script>

<main class="min-h-screen bg-gradient-to-br from-blue-50 via-white to-blue-50">
  <Header />
  <Hero />
  <Converter />
  <Features />
  <Footer />
</main>

<style>
  /* Global styles */
  :global(html) {
    scroll-behavior: smooth;
  }
  
  :global(body) {
    margin: 0;
    padding: 0;
    font-family: 'Inter', sans-serif;
  }
  
</style>