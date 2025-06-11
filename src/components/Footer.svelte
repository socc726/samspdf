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