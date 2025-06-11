# Project Structure

```
.astro/
  collections/
  content-assets.mjs
  content-modules.mjs
  content.d.ts
  data-store.json
  settings.json
  types.d.ts
public/
  favicon.svg
src/
  components/
    Converter.svelte
    Features.svelte
    Footer.svelte
    Header.svelte
    Hero.svelte
    Welcome.svelte
  layouts/
    Layout.astro
  pages/
    index.astro
  styles/
    global.css
  App.svelte
.gitignore
astro.config.mjs
package-lock.json
package.json
README.md
svelte.config.js
tailwind.config.js
tsconfig.json
```



# Selected Files Content

## src/components/Converter.svelte

```svelte
<!-- src/components/Converter.svelte -->
<script lang="ts">
  import { FileText, Download, Copy, Eye, Trash2, Upload, Sparkles } from 'lucide-svelte';
  import { onMount } from 'svelte';
  
  let markdownInput = `# Welcome to SamsPDF!

## What is this?
This is a **completely client-side** Markdown to PDF converter. Your content never leaves your browser!

### Features
- ✨ **Instant conversion** - No waiting, no uploads
- 🔒 **100% Private** - Everything happens in your browser
- 🎨 **Beautiful output** - Professional PDF formatting
- 📱 **Mobile friendly** - Works on all devices

### How to use
1. Type or paste your Markdown content
2. See the live preview update instantly
3. Click "Download PDF" when ready

---

**Try editing this text** and watch the preview update in real-time!

> This is a blockquote. Perfect for highlighting important information.

\`\`\`javascript
// Even code blocks work perfectly!
function convertToPDF() {
  return "magic happens here ✨";
}
\`\`\`

[Links work too!](https://github.com)`;

  let isConverting = false;
  let previewHtml = '';
  
  // Simulate markdown to HTML conversion (you'll need a real library like marked.js)
  function convertMarkdownToHtml(markdown: string): string {
    // This is a very basic simulation - use a proper markdown library
    return markdown
      .replace(/^# (.*$)/gim, '<h1>$1</h1>')
      .replace(/^## (.*$)/gim, '<h2>$1</h2>')
      .replace(/^### (.*$)/gim, '<h3>$1</h3>')
      .replace(/\*\*(.*)\*\*/gim, '<strong>$1</strong>')
      .replace(/\*(.*)\*/gim, '<em>$1</em>')
      .replace(/^\- (.*$)/gim, '<li>$1</li>')
      .replace(/^> (.*$)/gim, '<blockquote>$1</blockquote>')
      .replace(/\[([^\]]+)\]\(([^)]+)\)/gim, '<a href="$2">$1</a>')
      .replace(/```(\w+)?\n([\s\S]*?)```/gim, '<pre><code>$2</code></pre>')
      .replace(/`([^`]+)`/gim, '<code>$1</code>')
      .replace(/\n/gim, '<br>');
  }
  
  function updatePreview() {
    previewHtml = convertMarkdownToHtml(markdownInput);
  }
  
  async function convertToPDF() {
    isConverting = true;
    
    // Simulate conversion time
    await new Promise(resolve => setTimeout(resolve, 1500));
    
    // Here you would implement actual PDF generation
    // using libraries like jsPDF, Puppeteer, or html2pdf
    console.log('Converting to PDF...');
    
    isConverting = false;
    
    // For now, just show success
    alert('PDF conversion would happen here! 🎉');
  }
  
  function copyToClipboard() {
    navigator.clipboard.writeText(markdownInput);
    // You could add a toast notification here
  }
  
  function clearContent() {
    markdownInput = '';
    updatePreview();
  }
  
  function loadExample() {
    markdownInput = `# Sample Document

## Introduction
This is a sample document to demonstrate the **Markdown to PDF** conversion.

### Key Features
- Fast conversion
- Beautiful formatting  
- No server required

> "Simplicity is the ultimate sophistication." - Leonardo da Vinci

\`\`\`
Code blocks are supported too!
\`\`\``;
    updatePreview();
  }
  
  onMount(() => {
    updatePreview();
  });
  
  $: if (markdownInput) {
    updatePreview();
  }
</script>

<section id="converter" class="py-20 px-6 bg-gradient-to-br from-blue-50 to-white">
  <div class="container mx-auto max-w-7xl">
    <!-- Section Header -->
    <div class="text-center mb-12">
      <div class="inline-flex items-center space-x-2 bg-blue-100 text-blue-700 px-4 py-2 rounded-full text-sm font-medium mb-4">
        <Sparkles class="w-4 h-4" />
        <span>Try it now</span>
      </div>
      <h2 class="font-bold text-gray-900 mb-4" style="font-size: clamp(2rem, 6vw, 3rem);">
        Convert Your Markdown
      </h2>
      <p class="text-gray-600 max-w-2xl mx-auto" style="font-size: 1.25rem;">
        Paste your Markdown content below and watch it transform into a beautiful PDF instantly.
      </p>
    </div>
    
    <div class="grid lg:grid-cols-2 gap-8">
      <!-- Input Panel -->
      <div class="space-y-6">
        <div class="card p-6">
          <div class="flex items-center justify-between mb-4">
            <div class="flex items-center space-x-2">
              <FileText class="w-5 h-5 text-blue-600" />
              <h3 class="font-semibold text-gray-900" style="font-size: 1.125rem;">Markdown Input</h3>
            </div>
            <div class="flex items-center space-x-2">
              <button 
                class="p-2 text-gray-500 hover:text-blue-600 hover:bg-blue-50 rounded-lg transition-all duration-200"
                on:click={copyToClipboard}
                title="Copy to clipboard"
              >
                <Copy class="w-4 h-4" />
              </button>
              <button 
                class="p-2 text-gray-500 hover:text-red-600 hover:bg-red-50 rounded-lg transition-all duration-200"
                on:click={clearContent}
                title="Clear content"
              >
                <Trash2 class="w-4 h-4" />
              </button>
            </div>
          </div>
          
          <textarea 
            bind:value={markdownInput}
            class="w-full h-96 p-4 border border-gray-200 rounded-xl focus:ring-2 focus:ring-blue-500 focus:border-transparent resize-none font-mono text-sm"
            placeholder="Type or paste your Markdown here..."
          ></textarea>
          
          <div class="flex flex-wrap gap-3 mt-4">
            <button class="btn-secondary text-sm py-2 px-4" on:click={loadExample}>
              <Upload class="w-4 h-4 mr-2" />
              Load Example
            </button>
            <div class="text-sm text-gray-500 flex items-center">
              {markdownInput.length} characters
            </div>
          </div>
        </div>
      </div>
      
      <!-- Preview Panel -->
      <div class="space-y-6">
        <div class="card p-6">
          <div class="flex items-center justify-between mb-4">
            <div class="flex items-center space-x-2">
              <Eye class="w-5 h-5 text-blue-600" />
              <h3 class="font-semibold text-gray-900" style="font-size: 1.125rem;">Live Preview</h3>
            </div>
          </div>
          
          <div class="h-96 overflow-auto border border-gray-200 rounded-xl p-6 bg-white prose prose-blue max-w-none">
            {#if previewHtml}
              {@html previewHtml}
            {:else}
              <div class="text-gray-400 text-center py-12">
                <FileText class="w-12 h-12 mx-auto mb-4 text-gray-300" />
                <p>Your preview will appear here...</p>
              </div>
            {/if}
          </div>
        </div>
        
        <!-- Convert Button -->
        <div class="card p-6 text-center">
          <button 
            class="btn-primary w-full text-lg py-4 flex items-center justify-center space-x-3 disabled:opacity-50 disabled:cursor-not-allowed"
            on:click={convertToPDF}
            disabled={isConverting || !markdownInput.trim()}
          >
            {#if isConverting}
              <div class="w-5 h-5 border-2 border-white border-t-transparent rounded-full animate-spin"></div>
              <span>Converting...</span>
            {:else}
              <Download class="w-5 h-5" />
              <span>Download PDF</span>
            {/if}
          </button>
          
          <p class="text-sm text-gray-500 mt-3">
            Your content never leaves your browser 🔒
          </p>
        </div>
      </div>
    </div>
    
    <!-- Quick Stats -->
    <div class="mt-16 grid grid-cols-1 md:grid-cols-3 gap-8">
      <div class="text-center">
        <div class="font-bold text-blue-600 mb-2" style="font-size: 1.875rem;">100%</div>
        <div class="text-gray-600">Client-Side Processing</div>
      </div>
      <div class="text-center">
        <div class="font-bold text-blue-600 mb-2" style="font-size: 1.875rem;">&lt;1s</div>
        <div class="text-gray-600">Average Conversion Time</div>
      </div>
      <div class="text-center">
        <div class="font-bold text-blue-600 mb-2" style="font-size: 1.875rem;">0</div>
        <div class="text-gray-600">Files Uploaded to Servers</div>
      </div>
    </div>
  </div>
</section>

<style>
  :global(.prose h1) {
    @apply text-2xl font-bold text-gray-900 mb-4;
  }
  
  :global(.prose h2) {
    @apply text-xl font-semibold text-gray-800 mb-3 mt-6;
  }
  
  :global(.prose h3) {
    @apply text-lg font-medium text-gray-800 mb-2 mt-4;
  }
  
  :global(.prose p) {
    @apply text-gray-700 mb-3 leading-relaxed;
  }
  
  :global(.prose strong) {
    @apply font-semibold text-gray-900;
  }
  
  :global(.prose em) {
    @apply italic;
  }
  
  :global(.prose li) {
    @apply text-gray-700 mb-1;
  }
  
  :global(.prose blockquote) {
    @apply border-l-4 border-blue-300 pl-4 py-2 bg-blue-50 text-blue-900 italic;
  }
  
  :global(.prose code) {
    @apply bg-gray-100 px-2 py-1 rounded text-sm font-mono text-blue-600;
  }
  
  :global(.prose pre) {
    @apply bg-gray-900 text-green-400 p-4 rounded-lg overflow-x-auto;
  }
  
  :global(.prose a) {
    @apply text-blue-600 hover:text-blue-800 underline;
  }
</style>
```

## src/components/Features.svelte

```svelte
<!-- src/components/Features.svelte -->
<script lang="ts">
  import { 
    Shield, 
    Zap, 
    Lock, 
    Smartphone, 
    Download, 
    Palette,
    Code,
    Globe,
    Heart
  } from 'lucide-svelte';
  
  const features = [
    {
      icon: Shield,
      title: "100% Private & Secure",
      description: "Your documents never leave your browser. All processing happens locally on your device, ensuring complete privacy and security.",
      color: "from-blue-500 to-blue-600"
    },
    {
      icon: Zap,
      title: "Lightning Fast",
      description: "Convert Markdown to PDF in seconds. No waiting for server processing or file uploads. Instant results every time.",
      color: "from-yellow-500 to-orange-500"
    },
    {
      icon: Lock,
      title: "No File Uploads",
      description: "Keep your sensitive documents safe. Everything processes in your browser - no data transmission, no storage concerns.",
      color: "from-green-500 to-emerald-600"
    },
    {
      icon: Smartphone,
      title: "Works Everywhere",
      description: "Mobile, tablet, or desktop - SamsPDF works perfectly on any device with a modern web browser.",
      color: "from-purple-500 to-pink-500"
    },
    {
      icon: Download,
      title: "Instant Downloads",
      description: "Get your converted PDF immediately. No account required, no email verification - just instant, high-quality results.",
      color: "from-blue-600 to-cyan-500"
    },
    {
      icon: Palette,
      title: "Beautiful Formatting",
      description: "Professional PDF output with clean typography, proper spacing, and support for all Markdown elements.",
      color: "from-rose-500 to-pink-600"
    },
    {
      icon: Code,
      title: "Developer Friendly",
      description: "Full support for code blocks, syntax highlighting, tables, and all the Markdown features developers love.",
      color: "from-indigo-500 to-blue-600"
    },
    {
      icon: Globe,
      title: "No Internet Required",
      description: "Once loaded, SamsPDF works completely offline. Perfect for sensitive environments or unreliable connections.",
      color: "from-teal-500 to-green-500"
    },
    {
      icon: Heart,
      title: "Free Forever",
      description: "No subscriptions, no limits, no hidden costs. SamsPDF is completely free to use with unlimited conversions.",
      color: "from-red-500 to-pink-500"
    }
  ];
</script>

<section id="features" class="py-20 px-6 bg-white">
  <div class="container mx-auto max-w-7xl">
    <!-- Section Header -->
    <div class="text-center mb-16">
      <div class="inline-flex items-center space-x-2 bg-blue-100 text-blue-700 px-4 py-2 rounded-full text-sm font-medium mb-4">
        <Zap class="w-4 h-4" />
        <span>Why choose SamsPDF?</span>
      </div>
      <h2 class="text-4xl md:text-5xl font-bold text-gray-900 mb-4">
        Why Choose SamsPDF?
      </h2>
      <p class="text-xl text-gray-600 max-w-3xl mx-auto">
        Experience the future of document conversion with our client-side technology. 
        No compromises on privacy, speed, or quality.
      </p>
    </div>
    
    <!-- Features Grid -->
    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
      {#each features as feature, index}
        <div class="card p-8 group hover:scale-105 transition-all duration-300">
          <div class="mb-6">
            <div class={`inline-flex p-3 rounded-2xl bg-gradient-to-r ${feature.color} shadow-lg`}>
              <svelte:component this={feature.icon} class="w-6 h-6 text-white" />
            </div>
          </div>
          
          <h3 class="text-xl font-bold text-gray-900 mb-3 group-hover:text-blue-600 transition-colors">
            {feature.title}
          </h3>
          
          <p class="text-gray-600 leading-relaxed">
            {feature.description}
          </p>
        </div>
      {/each}
    </div>
    
    <!-- Trust Section -->
    <div class="mt-20 text-center">
      <div class="card p-12 bg-gradient-to-br from-blue-50 to-indigo-50">
        <div class="max-w-3xl mx-auto">
          <div class="mb-8">
            <div class="inline-flex p-4 bg-blue-100 rounded-full mb-4">
              <Shield class="w-8 h-8 text-blue-600" />
            </div>
            <h3 class="text-3xl font-bold text-gray-900 mb-4">
              Your Privacy is Our Priority
            </h3>
            <p class="text-lg text-gray-600 mb-8">
              In a world where data privacy matters more than ever, SamsPDF ensures your documents 
              stay exactly where they belong - with you. No servers, no storage, no tracking.
            </p>
          </div>
          
          <div class="grid grid-cols-1 md:grid-cols-3 gap-8 text-center">
            <div>
              <div class="text-3xl font-bold text-blue-600 mb-2">0</div>
              <div class="text-gray-600">Servers Touched</div>
            </div>
            <div>
              <div class="text-3xl font-bold text-blue-600 mb-2">0</div>
              <div class="text-gray-600">Data Collected</div>
            </div>
            <div>
              <div class="text-3xl font-bold text-blue-600 mb-2">100%</div>
              <div class="text-gray-600">Your Control</div>
            </div>
          </div>
        </div>
      </div>
    </div>
    
    <!-- Performance Stats -->
    <div class="mt-16">
      <div class="grid grid-cols-2 md:grid-cols-4 gap-8 text-center">
        <div class="space-y-2">
          <div class="text-2xl md:text-3xl font-bold text-blue-600">&lt;500ms</div>
          <div class="text-sm text-gray-600">Average Load Time</div>
        </div>
        <div class="space-y-2">
          <div class="text-2xl md:text-3xl font-bold text-blue-600">99.9%</div>
          <div class="text-sm text-gray-600">Uptime Guarantee</div>
        </div>
        <div class="space-y-2">
          <div class="text-2xl md:text-3xl font-bold text-blue-600">∞</div>
          <div class="text-sm text-gray-600">Conversion Limit</div>
        </div>
        <div class="space-y-2">
          <div class="text-2xl md:text-3xl font-bold text-blue-600">0kb</div>
          <div class="text-sm text-gray-600">Data Usage</div>
        </div>
      </div>
    </div>
  </div>
</section>
```

## src/components/Footer.svelte

```svelte
<!-- src/components/Footer.svelte -->
<script lang="ts">
  import { 
    FileText, 
    Github, 
    Twitter, 
    Heart, 
    Coffee, 
    Mail,
    ExternalLink,
    Star
  } from 'lucide-svelte';
  
  const currentYear = new Date().getFullYear();
  
  const quickLinks = [
    { name: 'Converter', href: '#converter' },
    { name: 'Features', href: '#features' },
    { name: 'About', href: '#about' },
    { name: 'Privacy', href: '#privacy' }
  ];
  
  const resources = [
    { name: 'Markdown Guide', href: 'https://www.markdownguide.org/', external: true },
    { name: 'PDF Specs', href: 'https://www.adobe.com/devnet/pdf/', external: true },
    { name: 'GitHub Repo', href: 'https://github.com/', external: true },
    { name: 'Report Issue', href: 'https://github.com/', external: true }
  ];
  
  const socialLinks = [
    { 
      name: 'GitHub', 
      icon: Github, 
      href: 'https://github.com/',
      color: 'hover:text-gray-900'
    },
    { 
      name: 'Twitter', 
      icon: Twitter, 
      href: 'https://twitter.com/',
      color: 'hover:text-blue-500'
    },
    { 
      name: 'Email', 
      icon: Mail, 
      href: 'mailto:hello@samspdf.com',
      color: 'hover:text-green-500'
    }
  ];
</script>

<footer class="bg-gradient-to-br from-gray-50 to-blue-50 border-t border-blue-100">
  <div class="container mx-auto px-6 py-16">
    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-8">
      <!-- Brand Section -->
      <div class="lg:col-span-1">
        <div class="flex items-center space-x-3 mb-6">
          <div class="p-2 bg-gradient-to-br from-blue-500 to-blue-600 rounded-xl">
            <FileText class="w-6 h-6 text-white" />
          </div>
          <div>
            <span class="text-2xl font-bold bg-gradient-to-r from-blue-600 to-blue-800 bg-clip-text text-transparent">
              SamsPDF
            </span>
            <div class="text-xs text-blue-600 font-medium">Client-Side Conversion</div>
          </div>
        </div>
        
        <p class="text-gray-600 mb-6 leading-relaxed">
          The fastest, most private way to convert Markdown to PDF. 
          Built with privacy and simplicity in mind.
        </p>
        
        <div class="flex items-center space-x-4">
          {#each socialLinks as social}
            <a 
              href={social.href}
              target="_blank"
              rel="noopener noreferrer"
              class={`p-2 text-gray-500 ${social.color} bg-white rounded-lg shadow-sm hover:shadow-md transition-all duration-200`}
              aria-label={social.name}
            >
              <svelte:component this={social.icon} class="w-5 h-5" />
            </a>
          {/each}
        </div>
      </div>
      
      <!-- Quick Links -->
      <div>
        <h3 class="text-lg font-semibold text-gray-900 mb-4">Quick Links</h3>
        <ul class="space-y-3">
          {#each quickLinks as link}
            <li>
              <a 
                href={link.href}
                class="text-gray-600 hover:text-blue-600 transition-colors flex items-center"
              >
                <span>{link.name}</span>
              </a>
            </li>
          {/each}
        </ul>
      </div>
      
      <!-- Resources -->
      <div>
        <h3 class="text-lg font-semibold text-gray-900 mb-4">Resources</h3>
        <ul class="space-y-3">
          {#each resources as resource}
            <li>
              <a 
                href={resource.href}
                target={resource.external ? '_blank' : '_self'}
                rel={resource.external ? 'noopener noreferrer' : ''}
                class="text-gray-600 hover:text-blue-600 transition-colors flex items-center"
              >
                <span>{resource.name}</span>
                {#if resource.external}
                  <ExternalLink class="w-3 h-3 ml-1 opacity-50" />
                {/if}
              </a>
            </li>
          {/each}
        </ul>
      </div>
      
      <!-- Support -->
      <div>
        <h3 class="text-lg font-semibold text-gray-900 mb-4">Support SamsPDF</h3>
        <p class="text-gray-600 mb-4 text-sm">
          Help us keep SamsPDF free and ad-free forever!
        </p>
        
        <div class="space-y-3">
          <button class="w-full btn-primary text-sm py-2 px-4 flex items-center justify-center space-x-2">
            <Heart class="w-4 h-4" />
            <span>Sponsor</span>
          </button>
          
          <button class="w-full btn-secondary text-sm py-2 px-4 flex items-center justify-center space-x-2">
            <Coffee class="w-4 h-4" />
            <span>Buy me a coffee</span>
          </button>
          
          <a 
            href="https://github.com/"
            target="_blank"
            rel="noopener noreferrer"
            class="w-full bg-gray-100 hover:bg-gray-200 text-gray-700 font-medium py-2 px-4 rounded-lg flex items-center justify-center space-x-2 transition-all duration-200"
          >
            <Star class="w-4 h-4" />
            <span>Star on GitHub</span>
          </a>
        </div>
      </div>
    </div>
    
    <!-- Bottom Section -->
    <div class="mt-16 pt-8 border-t border-blue-200">
      <div class="flex flex-col md:flex-row justify-between items-center space-y-4 md:space-y-0">
        <div class="text-sm text-gray-600">
          © {currentYear} SamsPDF. Made with 
          <Heart class="w-4 h-4 inline text-red-500" /> 
          for privacy-conscious users.
        </div>
        
        <div class="flex items-center space-x-6 text-sm">
          <a href="#privacy" class="text-gray-600 hover:text-blue-600 transition-colors">
            Privacy Policy
          </a>
          <a href="#terms" class="text-gray-600 hover:text-blue-600 transition-colors">
            Terms of Use
          </a>
          <div class="flex items-center space-x-1 text-gray-500">
            <div class="w-2 h-2 bg-green-500 rounded-full animate-pulse"></div>
            <span>All systems operational</span>
          </div>
        </div>
      </div>
      
      <!-- Fun Stats -->
      <div class="mt-8 p-6 bg-white rounded-xl border border-blue-100">
        <div class="text-center">
          <div class="text-sm text-gray-600 mb-3">🎉 Since launch, SamsPDF has helped convert</div>
          <div class="text-2xl font-bold text-blue-600 mb-2">1,234,567+</div>
          <div class="text-sm text-gray-600">documents while keeping them 100% private!</div>
        </div>
      </div>
    </div>
  </div>
</footer>
```

## src/components/Header.svelte

```svelte
<!-- src/components/Header.svelte -->
<script lang="ts">
  import { FileText, Menu, X, Github, Heart } from 'lucide-svelte';
  
  let isMenuOpen = false;
  let isScrolled = false;
  
  const toggleMenu = () => {
    isMenuOpen = !isMenuOpen;
  };
  
  const handleScroll = () => {
    isScrolled = window.scrollY > 20;
  };
  
  import { onMount } from 'svelte';
  onMount(() => {
    window.addEventListener('scroll', handleScroll);
    return () => window.removeEventListener('scroll', handleScroll);
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
        on:click={toggleMenu}
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
```

## src/components/Hero.svelte

```svelte
<!-- src/components/Hero.svelte -->
<script lang="ts">
  import { ArrowDown, Shield, Zap, Lock } from 'lucide-svelte';
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
      
      <!-- Demo preview -->
      <div class="max-w-4xl mx-auto">
        <div class="card p-1">
          <div class="bg-gradient-to-br from-gray-50 to-blue-50 rounded-xl p-8">
            <div class="grid md:grid-cols-2 gap-8 items-center">
              <!-- Markdown side -->
              <div class="space-y-4">
                <div class="text-left">
                  <div class="text-sm font-medium text-gray-500 mb-2">Markdown Input</div>
                  <div class="bg-gray-900 rounded-lg p-4 text-left font-mono text-sm text-green-400">
                    <div class="mb-2"># My Document</div>
                    <div class="mb-2 text-blue-300">## Introduction</div>
                    <div class="mb-2 text-gray-300">This is **bold** text and</div>
                    <div class="mb-2 text-gray-300">this is *italic* text.</div>
                    <div class="text-yellow-300">- List item 1</div>
                    <div class="text-yellow-300">- List item 2</div>
                  </div>
                </div>
              </div>
              
              <!-- Arrow -->
              <div class="flex justify-center">
                <div class="p-3 bg-blue-100 rounded-full">
                  <ArrowDown class="w-6 h-6 text-blue-600 transform rotate-90 md:rotate-0" />
                </div>
              </div>
              
              <!-- PDF side -->
              <div class="space-y-4">
                <div class="text-left">
                  <div class="text-sm font-medium text-gray-500 mb-2">PDF Output</div>
                  <div class="bg-white border-2 border-gray-200 rounded-lg p-6 shadow-inner">
                    <div class="text-2xl font-bold text-gray-900 mb-3">My Document</div>
                    <div class="text-lg font-semibold text-gray-800 mb-2">Introduction</div>
                    <div class="text-gray-700 mb-3">
                      This is <strong>bold</strong> text and this is <em>italic</em> text.
                    </div>
                    <ul class="text-gray-700 list-disc list-inside space-y-1">
                      <li>List item 1</li>
                      <li>List item 2</li>
                    </ul>
                  </div>
                </div>
              </div>
            </div>
          </div>
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
```

## src/components/Welcome.svelte

```svelte
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
```

## src/layouts/Layout.astro

```astro
---
import "../styles/global.css";

interface Props {
  title: string;
  description?: string;
}

const { title, description = "A modern web application built with Astro, Svelte, and Tailwind CSS" } = Astro.props;
---

<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <link rel="icon" type="image/svg+xml" href="/favicon.ico" />
    <meta name="generator" content={Astro.generator} />
    <meta name="description" content={description} />
    <title>{title}</title>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
  </head>
  <body class="min-h-screen bg-gradient-to-b from-gray-900 to-gray-950 text-white font-['Inter']">
    <slot />
  </body>
</html>

<style is:global>
  :root {
    --accent-blue: #4B93D6;
    --accent-purple: #8E449B;
    --accent-red: #F13E56;
  }
  
  html {
    scroll-behavior: smooth;
  }
  
  body {
    margin: 0;
    padding: 0;
    overflow-x: hidden;
  }
  
  /* Custom scrollbar */
  ::-webkit-scrollbar {
    width: 8px;
  }
  
  ::-webkit-scrollbar-track {
    background: rgba(255, 255, 255, 0.1);
  }
  
  ::-webkit-scrollbar-thumb {
    background: var(--accent-blue);
    border-radius: 10px;
  }
  
  ::-webkit-scrollbar-thumb:hover {
    background: var(--accent-purple);
  }
</style>
```

## src/pages/index.astro

```astro
---
import Layout from '../layouts/Layout.astro';
import App from '../App.svelte';
---

<Layout 
  title="SamsPDF - Convert Markdown to PDF | 100% Client-Side & Private"
  description="Convert Markdown to beautiful PDFs instantly in your browser. No uploads, no servers, completely private. Fast, free, and secure Markdown to PDF conversion."
>
  <App client:load />
</Layout>
```

## src/styles/global.css

```css
@import "tailwindcss";

/* Custom global styles for SamsPDF */
:root {
  --accent-blue: #3b82f6;
  --accent-purple: #8E449B;
  --accent-red: #F13E56;
}

/* Global button styles */
.btn-primary {
  background: linear-gradient(to right, rgb(37, 99, 235), rgb(29, 78, 216));
  color: white;
  font-weight: 600;
  padding: 0.75rem 1.5rem;
  border-radius: 0.75rem;
  box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1), 0 4px 6px -2px rgba(0, 0, 0, 0.05);
  transition: all 0.2s ease-in-out;
}

.btn-primary:hover {
  background: linear-gradient(to right, rgb(29, 78, 216), rgb(30, 64, 175));
  box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.1), 0 10px 10px -5px rgba(0, 0, 0, 0.04);
  transform: translateY(-2px);
}

.btn-primary:disabled {
  opacity: 0.5;
  cursor: not-allowed;
  transform: none;
}

.btn-secondary {
  background: white;
  color: rgb(37, 99, 235);
  border: 2px solid rgb(191, 219, 254);
  font-weight: 600;
  padding: 0.75rem 1.5rem;
  border-radius: 0.75rem;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
  transition: all 0.2s ease-in-out;
}

.btn-secondary:hover {
  border-color: rgb(147, 197, 253);
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1);
  transform: translateY(-2px);
}

.card {
  background: white;
  border-radius: 1rem;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
  border: 1px solid rgb(219, 234, 254);
  transition: all 0.3s ease-in-out;
}

.card:hover {
  box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1), 0 4px 6px -2px rgba(0, 0, 0, 0.05);
}

.glass {
  background: rgba(255, 255, 255, 0.8);
  backdrop-filter: blur(12px);
  border: 1px solid rgba(255, 255, 255, 0.2);
}

/* Animation utilities */
@keyframes float {
  0%, 100% {
    transform: translateY(0px);
  }
  50% {
    transform: translateY(-10px);
  }
}

.animate-float {
  animation: float 6s ease-in-out infinite;
}

/* Prose styles for markdown preview */
.prose h1 {
  font-size: 1.5rem;
  font-weight: 700;
  color: rgb(17, 24, 39);
  margin-bottom: 1rem;
}

.prose h2 {
  font-size: 1.25rem;
  font-weight: 600;
  color: rgb(31, 41, 55);
  margin-bottom: 0.75rem;
  margin-top: 1.5rem;
}

.prose h3 {
  font-size: 1.125rem;
  font-weight: 500;
  color: rgb(31, 41, 55);
  margin-bottom: 0.5rem;
  margin-top: 1rem;
}

.prose p {
  color: rgb(55, 65, 81);
  margin-bottom: 0.75rem;
  line-height: 1.625;
}

.prose strong {
  font-weight: 600;
  color: rgb(17, 24, 39);
}

.prose em {
  font-style: italic;
}

.prose li {
  color: rgb(55, 65, 81);
  margin-bottom: 0.25rem;
}

.prose blockquote {
  border-left: 4px solid rgb(147, 197, 253);
  padding-left: 1rem;
  padding-top: 0.5rem;
  padding-bottom: 0.5rem;
  background: rgb(239, 246, 255);
  color: rgb(30, 58, 138);
  font-style: italic;
}

.prose code {
  background: rgb(243, 244, 246);
  padding: 0.25rem 0.5rem;
  border-radius: 0.25rem;
  font-size: 0.875rem;
  font-family: ui-monospace, SFMono-Regular, monospace;
  color: rgb(37, 99, 235);
}

.prose pre {
  background: rgb(17, 24, 39);
  color: rgb(74, 222, 128);
  padding: 1rem;
  border-radius: 0.5rem;
  overflow-x: auto;
}

.prose a {
  color: rgb(37, 99, 235);
  text-decoration: underline;
}

.prose a:hover {
  color: rgb(29, 78, 216);
}
```

## src/App.svelte

```svelte
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
  
  :global(.btn-primary) {
    @apply bg-gradient-to-r from-blue-600 to-blue-700 hover:from-blue-700 hover:to-blue-800 text-white font-semibold py-3 px-6 rounded-xl shadow-lg hover:shadow-xl transform hover:-translate-y-0.5 transition-all duration-200;
  }
  
  :global(.btn-secondary) {
    @apply bg-white text-blue-600 border-2 border-blue-200 hover:border-blue-300 font-semibold py-3 px-6 rounded-xl shadow-sm hover:shadow-md transform hover:-translate-y-0.5 transition-all duration-200;
  }
  
  :global(.card) {
    @apply bg-white rounded-2xl shadow-sm border border-blue-100 hover:shadow-lg transition-all duration-300;
  }
  
  :global(.glass) {
    @apply bg-white/80 backdrop-blur-md border border-white/20;
  }
</style>
```

## astro.config.mjs

```mjs
// @ts-check
import { defineConfig } from 'astro/config';

import svelte from '@astrojs/svelte';

import sitemap from '@astrojs/sitemap';

// https://astro.build/config
export default defineConfig({
  site: 'https://samspdf.com',
  integrations: [svelte(), sitemap()],
  output: 'static',

  build: {
    assets: 'assets',
  }
});
```

## package.json

```json
{
  "name": "samspdf",
  "type": "module",
  "version": "0.0.1",
  "scripts": {
    "dev": "astro dev",
    "build": "astro build",
    "preview": "astro preview",
    "astro": "astro"
  },
  "dependencies": {
    "@astrojs/sitemap": "^3.4.1",
    "@astrojs/svelte": "^7.1.0",
    "@tailwindcss/vite": "^4.1.8",
    "astro": "^5.9.2",
    "flowbite-svelte": "^0.47.4",
    "highlight.js": "^11.11.1",
    "html2pdf.js": "^0.10.3",
    "jspdf": "^3.0.1",
    "lucide-svelte": "^0.469.0",
    "marked": "^15.0.12",
    "resend": "^4.5.2",
    "sharp": "^0.33.5",
    "svelte": "^5.33.18",
    "tailwindcss": "^4.1.8",
    "typescript": "^5.8.3"
  }
}
```

## svelte.config.js

```js
import { vitePreprocess } from '@astrojs/svelte';

export default {
	preprocess: vitePreprocess(),
}
```

## tailwind.config.js

```js
/** @type {import('tailwindcss').Config} */
export default {
  content: ['./src/**/*.{astro,html,js,jsx,md,mdx,svelte,ts,tsx,vue}'],
  theme: {
    extend: {
      colors: {
        primary: {
          50: '#eff6ff',
          100: '#dbeafe',
          200: '#bfdbfe',
          300: '#93c5fd',
          400: '#60a5fa',
          500: '#3b82f6',
          600: '#2563eb',
          700: '#1d4ed8',
          800: '#1e40af',
          900: '#1e3a8a',
        },
        accent: {
          400: '#8E449B',
          500: '#F13E56',
        },
      },
      fontFamily: {
        sans: ['Inter', 'system-ui', 'sans-serif'],
      },
      animation: {
        'float': 'float 6s ease-in-out infinite',
        'pulse-slow': 'pulse 4s ease-in-out infinite',
      },
      keyframes: {
        float: {
          '0%, 100%': { transform: 'translateY(0px)' },
          '50%': { transform: 'translateY(-10px)' },
        }
      },
      backdropBlur: {
        xs: '2px',
      }
    },
  },
}
```

## tsconfig.json

```json
{
  "extends": "astro/tsconfigs/strict",
  "include": [".astro/types.d.ts", "**/*"],
  "exclude": ["dist"]
}
```

