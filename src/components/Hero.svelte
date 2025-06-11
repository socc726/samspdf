<!-- src/components/Hero.svelte -->
<script lang="ts">
  import { ArrowDown, ArrowRight, Shield, Zap, Lock, FileText, FileDown } from 'lucide-svelte';
  import { onMount } from 'svelte';
  
  let mounted = false;
  
  onMount(() => {
    mounted = true;
  });
  
  const scrollToConverter = () => {
    document.getElementById('converter')?.scrollIntoView({ behavior: 'smooth' });
  };
</script>

<section class="pt-32 pb-20 px-6 relative overflow-hidden">
  <!-- Background decoration -->
  <div class="absolute inset-0 -z-10">
    <div class="absolute top-0 left-1/4 w-72 h-72 bg-blue-200 rounded-full mix-blend-multiply filter blur-xl opacity-30 animate-pulse"></div>
    <div class="absolute top-0 right-1/4 w-72 h-72 bg-blue-300 rounded-full mix-blend-multiply filter blur-xl opacity-20 animate-pulse" style="animation-delay: 1s;"></div>
    <div class="absolute -bottom-8 left-1/3 w-72 h-72 bg-blue-100 rounded-full mix-blend-multiply filter blur-xl opacity-40 animate-pulse" style="animation-delay: 2s;"></div>
  </div>
  
  <div class="container mx-auto max-w-6xl">
    <div class="text-center">
      <!-- Main headline -->
      {#if mounted}
        <h1 class="text-5xl md:text-7xl font-bold text-gray-900 mb-6 leading-tight">
          Convert 
          <span class="bg-gradient-to-r from-blue-600 via-blue-700 to-blue-800 bg-clip-text text-transparent">
            Markdown
          </span>
          <br>
          to PDF
          <span class="inline-block ml-4">
            <div class="w-3 h-3 bg-blue-500 rounded-full animate-ping"></div>
          </span>
        </h1>
      {/if}
      
      <!-- Subtitle -->
      <p class="text-xl md:text-2xl text-gray-600 mb-8 max-w-3xl mx-auto leading-relaxed">
        Beautiful, fast, and completely private. Convert your Markdown files to professional PDFs 
        <span class="font-semibold text-blue-700">directly in your browser</span> - no uploads required.
      </p>
      
      <!-- Trust indicators -->
      <div class="flex flex-wrap justify-center items-center gap-6 mb-12 text-sm text-gray-500">
        <div class="flex items-center space-x-2">
          <Lock class="w-4 h-4 text-blue-500" />
          <span>100% Private</span>
        </div>
        <div class="flex items-center space-x-2">
          <Zap class="w-4 h-4 text-blue-500" />
          <span>Instant Conversion</span>
        </div>
        <div class="flex items-center space-x-2">
          <Shield class="w-4 h-4 text-blue-500" />
          <span>No File Uploads</span>
        </div>
      </div>
      
      <!-- CTA Buttons -->
      <div class="flex flex-col sm:flex-row gap-4 justify-center items-center mb-16">
        <button 
          class="btn-primary text-lg px-8 py-4 flex items-center space-x-3"
          on:click={scrollToConverter}
        >
          <span>Try it Now</span>
          <ArrowDown class="w-5 h-5" />
        </button>
        
        <button class="btn-secondary text-lg px-8 py-4">
          Learn More
        </button>
      </div>
      
      <!-- Demo preview - Improved Design -->
      <div class="max-w-5xl mx-auto">
        <div class="relative">
          <!-- Connection line (hidden on mobile) -->
          <div class="hidden md:block absolute top-1/2 left-1/2 transform -translate-x-1/2 -translate-y-1/2 w-32 h-0.5 bg-gradient-to-r from-blue-200 via-blue-400 to-blue-200"></div>
          
          <div class="grid md:grid-cols-2 gap-8 md:gap-16">
            <!-- Markdown Input Card -->
            <div class="relative">
              <div class="absolute -top-4 -left-4 w-12 h-12 bg-blue-100 rounded-full flex items-center justify-center">
                <FileText class="w-6 h-6 text-blue-600" />
              </div>
              <div class="card hover:shadow-xl transition-all duration-300">
                <div class="p-6">
                  <div class="text-sm font-semibold text-blue-600 mb-3 uppercase tracking-wide">Input</div>
                  <div class="bg-gray-900 rounded-lg p-4 text-left font-mono text-sm">
                    <div class="text-green-400 mb-2"># My Document</div>
                    <div class="text-blue-300 mb-2">## Introduction</div>
                    <div class="text-gray-300 mb-2">This is **bold** text and</div>
                    <div class="text-gray-300 mb-2">this is *italic* text.</div>
                    <div class="text-yellow-300">- List item 1</div>
                    <div class="text-yellow-300">- List item 2</div>
                  </div>
                </div>
              </div>
            </div>
            
            <!-- Conversion indicator (centered between cards) -->
            <div class="md:hidden flex justify-center my-4">
              <div class="p-3 bg-gradient-to-r from-blue-500 to-blue-600 rounded-full shadow-lg">
                <ArrowDown class="w-6 h-6 text-white" />
              </div>
            </div>
            
            <!-- PDF Output Card -->
            <div class="relative">
              <div class="absolute -top-4 -right-4 w-12 h-12 bg-green-100 rounded-full flex items-center justify-center">
                <FileDown class="w-6 h-6 text-green-600" />
              </div>
              <div class="card hover:shadow-xl transition-all duration-300">
                <div class="p-6">
                  <div class="text-sm font-semibold text-green-600 mb-3 uppercase tracking-wide">Output</div>
                  <div class="bg-white border-2 border-gray-100 rounded-lg p-4 shadow-inner">
                    <h1 class="text-2xl font-bold text-gray-900 mb-3">My Document</h1>
                    <h2 class="text-lg font-semibold text-gray-800 mb-2">Introduction</h2>
                    <p class="text-gray-700 mb-3">
                      This is <strong>bold</strong> text and this is <em>italic</em> text.
                    </p>
                    <ul class="text-gray-700 list-disc list-inside space-y-1">
                      <li>List item 1</li>
                      <li>List item 2</li>
                    </ul>
                  </div>
                </div>
              </div>
            </div>
          </div>
          
          <!-- Floating arrow indicator (hidden on mobile) -->
          <div class="hidden md:flex absolute top-1/2 left-1/2 transform -translate-x-1/2 -translate-y-1/2">
            <div class="bg-white rounded-full p-3 shadow-lg border-2 border-blue-200">
              <ArrowRight class="w-6 h-6 text-blue-600" />
            </div>
          </div>
        </div>
        
        <!-- Feature callout -->
        <div class="mt-12 text-center">
          <p class="text-sm text-gray-500 flex items-center justify-center space-x-2">
            <span class="inline-block w-2 h-2 bg-green-500 rounded-full animate-pulse"></span>
            <span>Live preview • Instant conversion • Professional output</span>
          </p>
        </div>
      </div>
    </div>
  </div>
</section>

<style>
  @keyframes float {
    0%, 100% {
      transform: translateY(0px);
    }
    50% {
      transform: translateY(-10px);
    }
  }
  
  .animate-float {
    animation: float 3s ease-in-out infinite;
  }
</style>