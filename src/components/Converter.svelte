<!-- src/components/Converter.svelte -->
<script lang="ts">
  import { FileText, Download, Copy, Eye, Trash2, Upload, Sparkles } from 'lucide-svelte';
  import { onMount } from 'svelte';
  import { marked } from 'marked';
  
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

### Lists are supported too!

**Ordered List:**
1. First item
2. Second item
3. Third item

**Unordered List:**
- Apple
- Banana  
- Cherry

### Tables

| Feature | Status | Notes |
|---------|--------|-------|
| Headers | ✅ Supported | All heading levels |
| Lists | ✅ Supported | Ordered and unordered |
| Code | ✅ Supported | Inline and blocks |
| Tables | ✅ Supported | Like this one! |

[Links work too!](https://github.com) - Check out the source code.`;

  let isConverting = false;
  let previewHtml = '';
  // No longer need pdfMake
  
  // Configure marked options
  marked.setOptions({
    breaks: true,
    gfm: true,
    headerIds: true,
    mangle: false,
    sanitize: false
  });
  
  onMount(() => {
    updatePreview();
  });
  
  function updatePreview() {
    try {
      previewHtml = marked(markdownInput);
    } catch (error) {
      console.error('Markdown parsing error:', error);
      previewHtml = '<p class="text-red-500">Error parsing markdown</p>';
    }
  }

  
  async function convertToPDF() {
    if (!markdownInput.trim()) return;
    
    isConverting = true;
    
    try {
      // Create a hidden iframe for printing
      const printFrame = document.createElement('iframe');
      printFrame.style.position = 'fixed';
      printFrame.style.top = '0';
      printFrame.style.left = '-10000px';
      printFrame.style.width = '210mm'; // A4 width
      printFrame.style.height = '297mm'; // A4 height
      printFrame.style.border = 'none';
      
      // Add to document first
      document.body.appendChild(printFrame);
      
      // Get the document reference
      const printDocument = printFrame.contentDocument || printFrame.contentWindow?.document;
      if (!printDocument) {
        console.error('Could not access iframe document');
        document.body.removeChild(printFrame);
        isConverting = false;
        return;
      }
      
      // Write the HTML content with print-optimized styles
      printDocument.open();
      printDocument.write(`
        <!DOCTYPE html>
        <html>
          <head>
            <meta charset="UTF-8">
            <title>Markdown PDF</title>
            <style>
              @page {
                size: A4;
                margin: 20mm;
              }
              
              @media print {
                body {
                  margin: 0;
                  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', 'Roboto', sans-serif;
                  line-height: 1.6;
                  color: #1a202c;
                }
              }
              
              body {
                font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', 'Roboto', sans-serif;
                line-height: 1.6;
                color: #1a202c;
                max-width: 100%;
                padding: 20px;
              }
              
              h1 { font-size: 2em; margin-top: 0; margin-bottom: 0.5em; }
              h2 { font-size: 1.5em; margin-top: 1em; margin-bottom: 0.5em; }
              h3 { font-size: 1.25em; margin-top: 1em; margin-bottom: 0.5em; }
              h4 { font-size: 1.1em; margin-top: 1em; margin-bottom: 0.5em; }
              
              p { margin-bottom: 1em; }
              
              blockquote {
                margin: 1em 0;
                padding: 0.5em 1em;
                border-left: 4px solid #3b82f6;
                background-color: #eff6ff;
                color: #1e40af;
                font-style: italic;
              }
              
              code {
                background-color: #f3f4f6;
                padding: 0.2em 0.4em;
                border-radius: 3px;
                font-family: 'Courier New', Courier, monospace;
                font-size: 0.9em;
                color: #1e40af;
              }
              
              pre {
                background-color: #1e293b;
                color: #e2e8f0;
                padding: 1em;
                border-radius: 5px;
                overflow-x: auto;
                margin: 1em 0;
              }
              
              pre code {
                background-color: transparent;
                color: #e2e8f0;
                padding: 0;
              }
              
              ul, ol {
                margin-bottom: 1em;
                padding-left: 2em;
              }
              
              li { margin-bottom: 0.25em; }
              
              table {
                border-collapse: collapse;
                width: 100%;
                margin: 1em 0;
              }
              
              th, td {
                border: 1px solid #e5e7eb;
                padding: 8px 12px;
                text-align: left;
              }
              
              th {
                background-color: #f9fafb;
                font-weight: bold;
              }
              
              a {
                color: #2563eb;
                text-decoration: underline;
              }
              
              hr {
                border: none;
                border-top: 1px solid #e5e7eb;
                margin: 2em 0;
              }
              
              img {
                max-width: 100%;
                height: auto;
              }
            </style>
          </head>
          <body>
            ${previewHtml}
          </body>
        </html>
      `);
      printDocument.close();
      
      // Give it a moment to render
      setTimeout(() => {
        try {
          // Focus the iframe window and print
          printFrame.contentWindow?.focus();
          printFrame.contentWindow?.print();
          
          // Clean up after a delay
          setTimeout(() => {
            document.body.removeChild(printFrame);
            isConverting = false;
          }, 100);
        } catch (err) {
          console.error('Print error:', err);
          document.body.removeChild(printFrame);
          isConverting = false;
        }
      }, 250);
      
    } catch (error) {
      console.error('PDF generation error:', error);
      alert('Error preparing document for print. Please try again.');
      isConverting = false;
    }
  }
  
  function copyToClipboard() {
    navigator.clipboard.writeText(markdownInput).then(() => {
      console.log('Copied to clipboard!');
    });
  }
  
  function clearContent() {
    markdownInput = '';
    updatePreview();
  }
  
  function loadExample() {
    markdownInput = `# Professional Document Example

## Executive Summary
This document demonstrates the **advanced capabilities** of SamsPDF's markdown to PDF conversion, featuring *rich text formatting*, code blocks, tables, and more.

### Key Benefits
1. **Selectable Text** - Unlike image-based PDFs, all text is fully selectable
2. **Smaller File Size** - Text-based PDFs are more efficient
3. **Better Quality** - Vector-based rendering for crisp text at any zoom level
4. **Accessibility** - Screen readers can parse the content

---

## Technical Features

### Code Examples
Here's some inline code: \`const pdf = await generatePDF(markdown)\`

And here's a code block:

\`\`\`javascript
function convertMarkdown(input) {
  const tokens = marked.lexer(input);
  const pdfContent = tokens.map(token => {
    return processToken(token);
  });
  
  return createPDF(pdfContent);
}
\`\`\`

### Data Visualization

| Technology | Performance | Compatibility | Privacy |
|------------|-------------|---------------|---------|
| pdfmake | Excellent | High | 100% |
| html2pdf | Good | Medium | 100% |
| Server-side | Excellent | High | 0% |

### Advanced Formatting

> **Note:** This blockquote demonstrates how pdfmake handles nested formatting with **bold text** and *italic text* within quoted content.

#### Nested Lists Example:
- Frontend Technologies
  - Svelte for reactive UI
  - Tailwind for styling
  - TypeScript for type safety
- PDF Libraries
  - pdfmake for text-based PDFs
  - marked for markdown parsing
- Privacy Features
  - Zero server communication
  - No analytics or tracking
  - Complete offline capability

### Links and References
For more information, visit [SamsPDF GitHub](https://github.com) or check out the [Markdown Guide](https://www.markdownguide.org/).

---

*Generated with ❤️ by SamsPDF - Your privacy-first markdown to PDF converter*`;
    updatePreview();
  }
  
  $: if (markdownInput !== undefined) {
    updatePreview();
  }
</script>

<section id="converter" class="py-20 px-6 bg-gradient-to-br from-blue-50 to-white">
  <div class="container mx-auto max-w-7xl">
    <!-- Section Header -->
    <div class="text-center mb-12">
      <div class="inline-flex items-center space-x-2 bg-blue-100 text-blue-700 px-4 py-2 rounded-full text-sm font-medium mb-4">
        <Sparkles class="w-4 h-4" />
        <span>Try it now - Real PDFs with Selectable Text!</span>
      </div>
      <h2 class="font-bold text-gray-900 mb-4" style="font-size: clamp(2rem, 6vw, 3rem);">
        Convert Your Markdown
      </h2>
      <p class="text-gray-600 max-w-2xl mx-auto" style="font-size: 1.25rem;">
        Generate professional PDFs with real, selectable text - not screenshots. 100% client-side.
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
            class="w-full h-96 p-4 border border-gray-200 rounded-xl focus:ring-2 focus:ring-blue-500 focus:border-transparent resize-none font-mono text-sm text-gray-900"
            placeholder="Type or paste your Markdown here..."
          ></textarea>
          
          <div class="flex flex-wrap items-center gap-3 mt-4">
            <button class="btn-secondary text-sm py-2 px-4 flex items-center" on:click={loadExample}>
              <Upload class="w-4 h-4 mr-2" />
              Load Example
            </button>
            <div class="text-sm text-gray-500">
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
          
          <div class="h-96 overflow-auto border border-gray-200 rounded-xl p-6 bg-white text-gray-900 prose prose-blue max-w-none">
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
          
          <div class="mt-3 space-y-1">
            <p class="text-sm text-gray-500">
              Your content never leaves your browser 🔒
            </p>
            <p class="text-xs text-green-600 font-medium">
              ✨ Tip: Select "Save as PDF" and uncheck "Headers and footers"
            </p>
          </div>
        </div>
      </div>
    </div>
    
    <!-- Quick Stats -->
    <div class="mt-16 grid grid-cols-1 md:grid-cols-3 gap-8">
      <div class="text-center">
        <div class="font-bold text-blue-600 mb-2" style="font-size: 1.875rem;">100%</div>
        <div class="text-gray-600">Text-Based PDFs</div>
      </div>
      <div class="text-center">
        <div class="font-bold text-blue-600 mb-2" style="font-size: 1.875rem;">&lt;100kb</div>
        <div class="text-gray-600">Typical File Size</div>
      </div>
      <div class="text-center">
        <div class="font-bold text-blue-600 mb-2" style="font-size: 1.875rem;">0</div>
        <div class="text-gray-600">Server Requests</div>
      </div>
    </div>
  </div>
</section>

<style>
  @reference "../styles/global.css";
  
  :global(.prose h1) {
    @apply text-3xl font-bold text-gray-900 mb-4;
  }
  
  :global(.prose h2) {
    @apply text-2xl font-semibold text-gray-800 mb-3 mt-6;
  }
  
  :global(.prose h3) {
    @apply text-xl font-medium text-gray-800 mb-2 mt-4;
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
  
  :global(.prose ul) {
    @apply list-disc list-inside mb-3 pl-5;
  }
  
  :global(.prose ol) {
    @apply list-decimal list-inside mb-3 pl-5;
  }
  
  :global(.prose li) {
    @apply text-gray-700 mb-1;
  }
  
  :global(.prose blockquote) {
    @apply border-l-4 border-blue-300 pl-4 py-2 bg-blue-50 text-blue-900 italic my-4;
  }
  
  :global(.prose code) {
    @apply bg-gray-100 px-2 py-1 rounded text-sm font-mono text-blue-600;
  }
  
  :global(.prose pre) {
    @apply bg-gray-900 text-green-400 p-4 rounded-lg overflow-x-auto my-4;
  }
  
  :global(.prose pre code) {
    @apply bg-transparent p-0 text-green-400;
  }
  
  :global(.prose a) {
    @apply text-blue-600 hover:text-blue-800 underline;
  }
  
  :global(.prose hr) {
    @apply border-t border-gray-300 my-8;
  }
  
  :global(.prose table) {
    @apply w-full border-collapse my-4;
  }
  
  :global(.prose th) {
    @apply border border-gray-300 px-4 py-2 bg-gray-100 font-semibold text-left;
  }
  
  :global(.prose td) {
    @apply border border-gray-300 px-4 py-2;
  }
</style>