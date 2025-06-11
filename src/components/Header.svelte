<!-- src/components/Header.svelte -->
<script lang="ts">
  import { FileText, Menu, X, Github, Heart } from 'lucide-svelte';
  import { onMount, onDestroy } from 'svelte';
  
  let isMenuOpen = $state(false);
  let isScrolled = $state(false);
  
  const toggleMenu = () => {
    isMenuOpen = !isMenuOpen;
  };
  
  const handleScroll = () => {
    isScrolled = window.scrollY > 20;
  };
  
  onMount(() => {
    window.addEventListener('scroll', handleScroll);
  });
  
  onDestroy(() => {
    window.removeEventListener('scroll', handleScroll);
  });
</script>

<header class={`fixed w-full top-0 z-50 transition-all duration-300 ${isScrolled ? 'glass shadow-lg' : 'bg-transparent'}`}>
  <div class="container mx-auto px-6 py-4">
    <nav class="flex justify-between items-center">
      <!-- Logo -->
      <a href="#" class="flex items-center space-x-3 group">
        <div class="p-2 bg-gradient-to-br from-blue-500 to-blue-600 rounded-xl group-hover:scale-105 transition-transform duration-200">
          <FileText class="w-6 h-6 text-white" />
        </div>
        <div>
          <span class="text-2xl font-bold bg-gradient-to-r from-blue-600 to-blue-800 bg-clip-text text-transparent">
            SamsPDF
          </span>
          <div class="text-xs text-blue-600 font-medium">Client-Side Conversion</div>
        </div>
      </a>
      
      <!-- Desktop Navigation -->
      <div class="hidden md:flex items-center space-x-8">
        <a href="#converter" class="text-gray-700 hover:text-blue-600 font-medium transition-colors">
          Convert
        </a>
        <a href="#features" class="text-gray-700 hover:text-blue-600 font-medium transition-colors">
          Features
        </a>
        <a href="#about" class="text-gray-700 hover:text-blue-600 font-medium transition-colors">
          About
        </a>
        <div class="flex items-center space-x-3">
          <a 
            href="https://github.com" 
            target="_blank" 
            rel="noopener noreferrer"
            class="p-2 text-gray-600 hover:text-blue-600 hover:bg-blue-50 rounded-lg transition-all duration-200"
            aria-label="GitHub"
          >
            <Github class="w-5 h-5" />
          </a>
          <button class="btn-primary flex items-center space-x-2">
            <Heart class="w-4 h-4" />
            <span>Support</span>
          </button>
        </div>
      </div>
      
      <!-- Mobile menu button -->
      <button 
        type="button" 
        class="md:hidden p-2 text-gray-700 hover:text-blue-600 hover:bg-blue-50 rounded-lg transition-all duration-200"
        onclick={toggleMenu}
        aria-label="Toggle menu"
      >
        {#if isMenuOpen}
          <X class="w-6 h-6" />
        {:else}
          <Menu class="w-6 h-6" />
        {/if}
      </button>
    </nav>
    
    <!-- Mobile Navigation -->
    {#if isMenuOpen}
      <div class="md:hidden mt-4 p-4 glass rounded-2xl">
        <div class="flex flex-col space-y-4">
          <a href="#converter" class="text-gray-700 hover:text-blue-600 font-medium py-2 transition-colors">
            Convert
          </a>
          <a href="#features" class="text-gray-700 hover:text-blue-600 font-medium py-2 transition-colors">
            Features
          </a>
          <a href="#about" class="text-gray-700 hover:text-blue-600 font-medium py-2 transition-colors">
            About
          </a>
          <div class="flex items-center justify-between pt-4 border-t border-blue-100">
            <a 
              href="https://github.com" 
              target="_blank"
              class="p-2 text-gray-600 hover:text-blue-600 hover:bg-blue-50 rounded-lg transition-all duration-200"
            >
              <Github class="w-5 h-5" />
            </a>
            <button class="btn-primary flex items-center space-x-2">
              <Heart class="w-4 h-4" />
              <span>Support</span>
            </button>
          </div>
        </div>
      </div>
    {/if}
  </div>
</header>