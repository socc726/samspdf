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