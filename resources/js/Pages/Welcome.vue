<script setup>
import { Head } from '@inertiajs/vue3';
import { ref, onMounted } from 'vue';
import { ChevronRightIcon, SparklesIcon, MagnifyingGlassIcon, ChartBarIcon, DocumentTextIcon, UserGroupIcon, NewspaperIcon } from '@heroicons/vue/24/outline';

defineProps({
    canLogin: {
        type: Boolean,
    },
    canRegister: {
        type: Boolean,
    },
});

const currentServiceIndex = ref(0);
const services = [
    'AI-Optimized Content',
    'Geographic SEO',
    'Digital PR',
    'Strategic Listicles',
];

// Chat simulation state
const chatMessages = ref([]);
const currentTyping = ref('');
const isTyping = ref(false);
const showCursor = ref(true);
const showSources = ref(false);

// AI platforms for animated banner
const aiPlatforms = [
    { name: 'ChatGPT', domain: 'openai.com' },
    { name: 'Gemini', domain: 'gemini.google.com' },
    { name: 'Perplexity', domain: 'perplexity.ai' },
    { name: 'Claude', domain: 'claude.ai' }
];
const currentPlatformIndex = ref(0);
const currentChatPlatformIndex = ref(0);

const userQuery = "What's the best CRM for small businesses in 2025?";
const aiResponse = `Based on my analysis, here are the top CRM solutions for small businesses:

1. **HubSpot CRM** - Best overall choice
   • Free tier available with robust features
   • Excellent integration capabilities
   • User-friendly interface perfect for teams

2. **Salesforce Essentials** - For growing teams
   • Scalable as your business expands
   • Advanced automation features

3. **Pipedrive** - Best for sales-focused teams
   • Visual pipeline management
   • Strong mobile app

HubSpot stands out due to its generous free plan and seamless marketing integration, making it ideal for budget-conscious small businesses.`;

const sources = [
    { domain: 'hubspot.com', name: 'HubSpot' },
    { domain: 'g2.com', name: 'G2' },
    { domain: 'capterra.com', name: 'Capterra' },
    { domain: 'techradar.com', name: 'TechRadar' },
    { domain: 'forbes.com', name: 'Forbes' }
];

const typeMessage = async (message, isUser = false) => {
    currentTyping.value = '';
    isTyping.value = true;
    
    for (let i = 0; i <= message.length; i++) {
        currentTyping.value = message.substring(0, i);
        await new Promise(resolve => setTimeout(resolve, isUser ? 30 : 15));
    }
    
    isTyping.value = false;
    chatMessages.value.push({ text: message, isUser });
    currentTyping.value = '';
};

onMounted(() => {
    setInterval(() => {
        currentServiceIndex.value = (currentServiceIndex.value + 1) % services.length;
    }, 3000);
    
    // Rotate AI platforms (both banner and chat in sync)
    setInterval(() => {
        currentPlatformIndex.value = (currentPlatformIndex.value + 1) % aiPlatforms.length;
        currentChatPlatformIndex.value = currentPlatformIndex.value;
    }, 2500);
    
    // Cursor blink
    setInterval(() => {
        showCursor.value = !showCursor.value;
    }, 500);
    
    // Start chat simulation after delay
    setTimeout(async () => {
        await typeMessage(userQuery, true);
        await new Promise(resolve => setTimeout(resolve, 500));
        await typeMessage(aiResponse, false);
        // Show sources after message completes
        setTimeout(() => {
            showSources.value = true;
        }, 500);
    }, 1500);
});
</script>

<template>
    <Head title="AI Search Labs - Get mentioned by AI discovery" />
    
    <div class="min-h-screen bg-white text-gray-900">
        <!-- Top banner -->
        <div class="bg-gray-100 py-3 text-center relative overflow-hidden">
            <div class="relative">
                <p class="text-sm text-gray-600 flex items-center justify-center gap-2">
                    <span>Get your brand mentioned by</span>
                    <span class="relative inline-flex items-center">
                        <!-- AI Platform rotating display -->
                        <div class="relative w-32 h-6">
                            <div v-for="(platform, index) in aiPlatforms" 
                                 :key="platform.name"
                                 class="absolute inset-0 flex items-center gap-1.5 transition-all duration-500"
                                 :class="{
                                     'opacity-100 transform translate-y-0': index === currentPlatformIndex,
                                     'opacity-0 transform -translate-y-full': index !== currentPlatformIndex
                                 }">
                                <img :src="`https://www.google.com/s2/favicons?domain=${platform.domain}&sz=32`" 
                                     :alt="platform.name"
                                     class="w-4 h-4">
                                <span class="font-semibold">{{ platform.name }}</span>
                            </div>
                        </div>
                    </span>
                </p>
            </div>
        </div>
        <!-- Navigation -->
        <nav class="bg-white border-b border-gray-100">
            <div class="max-w-7xl mx-auto px-8 py-5 flex items-center justify-between">
                <div class="flex items-center space-x-12">
                    <!-- Logo -->
                    <div class="flex items-center space-x-3">
                        <div class="w-10 h-10 bg-blue-600 rounded-full flex items-center justify-center">
                            <SparklesIcon class="h-6 w-6 text-white" />
                        </div>
                        <span class="text-xl font-semibold text-black">AI Search Labs</span>
                    </div>
                    <!-- Navigation Links -->
                    <div class="hidden md:flex items-center space-x-8">
                        <a href="#product" class="text-gray-900 font-medium hover:text-blue-600 transition">Product</a>
                        <a href="#solutions" class="text-gray-900 font-medium hover:text-blue-600 transition">Solutions</a>
                        <a href="#case-studies" class="text-gray-900 font-medium hover:text-blue-600 transition">Case studies</a>
                        <a href="#about" class="text-gray-900 font-medium hover:text-blue-600 transition">About</a>
                    </div>
                </div>
                <div class="flex items-center space-x-4">
                    <a href="#contact" class="text-gray-900 font-medium hover:text-blue-600 transition">Contact</a>
                    <button class="inline-flex items-center px-5 py-2.5 bg-black text-white rounded-full font-medium hover:bg-gray-800 transition">
                        <SparklesIcon class="h-4 w-4 mr-2" />
                        Get a demo
                    </button>
                </div>
            </div>
        </nav>

        <!-- Hero Section -->
        <section class="relative pt-20 pb-20 px-8">
            <div class="max-w-7xl mx-auto">
                <div class="grid lg:grid-cols-2 gap-16 items-start">
                    <div class="pt-8">
                        <h1 class="text-6xl lg:text-7xl font-bold mb-8 leading-tight">
                            Get your brand<br />
                            mentioned by <span class="text-blue-600">AI<br />discovery</span>
                        </h1>
                        <p class="text-xl text-gray-600 mb-10 leading-relaxed max-w-xl">
                            Master Answer Engine Optimization (AEO) to ensure your brand appears in 
                            AI-generated responses across ChatGPT, Claude, Gemini, and Perplexity.
                        </p>
                        <div class="flex items-center gap-4">
                            <button class="inline-flex items-center px-6 py-3 bg-black text-white rounded-full font-medium hover:bg-gray-800 transition">
                                <svg class="h-5 w-5 mr-2" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 10l-4 4m0 0l-4-4m4 4V3"></path>
                                </svg>
                                Get AEO Audit
                            </button>
                            <button class="px-6 py-3 bg-white border-2 border-gray-200 text-black rounded-full font-medium hover:bg-gray-50 transition">
                                View Case Studies
                            </button>
                        </div>
                    </div>
                    <div class="relative">
                        <!-- ChatGPT Simulation Card -->
                        <div class="bg-white rounded-2xl border border-gray-200 shadow-xl overflow-hidden">
                            <!-- AI Platform Header (Animated) -->
                            <div class="bg-gray-50 border-b border-gray-200 px-4 py-3 flex items-center justify-between overflow-hidden">
                                <div class="flex items-center gap-2">
                                    <div class="relative w-32 h-6">
                                        <div v-for="(platform, index) in aiPlatforms" 
                                             :key="`chat-${platform.name}`"
                                             class="absolute inset-0 flex items-center gap-2 transition-all duration-500"
                                             :class="{
                                                 'opacity-100 transform translate-y-0 blur-0': index === currentChatPlatformIndex,
                                                 'opacity-0 transform -translate-y-full blur-sm': index !== currentChatPlatformIndex
                                             }">
                                            <img :src="`https://www.google.com/s2/favicons?domain=${platform.domain}&sz=32`" 
                                                 :alt="platform.name"
                                                 class="w-6 h-6">
                                            <span class="text-sm font-semibold">{{ platform.name }}</span>
                                        </div>
                                    </div>
                                </div>
                                <div class="flex items-center gap-2">
                                    <div class="w-2 h-2 bg-green-500 rounded-full animate-pulse"></div>
                                    <span class="text-xs text-gray-500">Live Demo</span>
                                </div>
                            </div>
                            
                            <!-- Chat Messages -->
                            <div class="p-4 h-96 overflow-y-auto bg-gray-50">
                                <!-- Previous messages -->
                                <div v-for="(message, index) in chatMessages" :key="index" class="mb-4">
                                    <div v-if="message.isUser" class="flex justify-end">
                                        <div class="bg-blue-600 text-white rounded-lg px-4 py-2 max-w-xs">
                                            {{ message.text }}
                                        </div>
                                    </div>
                                    <div v-else>
                                        <div class="flex justify-start">
                                            <div class="bg-white text-gray-800 rounded-lg px-4 py-2 max-w-sm border border-gray-200">
                                                <div v-html="message.text.replace(/\*\*(.*?)\*\*/g, '<strong>$1</strong>').replace(/\n/g, '<br>')"></div>
                                            </div>
                                        </div>
                                        <!-- Sources section -->
                                        <div v-if="showSources && index === chatMessages.length - 1" class="mt-3 ml-0">
                                            <div class="text-xs text-gray-500 mb-2">Sources</div>
                                            <div class="flex gap-2 flex-wrap">
                                                <a v-for="source in sources" :key="source.domain" 
                                                   :href="`https://${source.domain}`" 
                                                   target="_blank"
                                                   class="flex items-center gap-1.5 bg-white border border-gray-200 rounded-full px-3 py-1.5 hover:bg-gray-50 transition-colors">
                                                    <img :src="`https://www.google.com/s2/favicons?domain=${source.domain}&sz=32`" 
                                                         :alt="source.name"
                                                         class="w-4 h-4 rounded-sm">
                                                    <span class="text-xs text-gray-700">{{ source.name }}</span>
                                                </a>
                                            </div>
                                        </div>
                                    </div>
                                </div>
                                
                                <!-- Currently typing message -->
                                <div v-if="isTyping && currentTyping" class="mb-4">
                                    <div v-if="chatMessages.length === 0" class="flex justify-end">
                                        <div class="bg-blue-600 text-white rounded-lg px-4 py-2 max-w-xs">
                                            {{ currentTyping }}<span v-if="showCursor" class="inline-block w-0.5 h-4 bg-white ml-0.5 animate-pulse"></span>
                                        </div>
                                    </div>
                                    <div v-else class="flex justify-start">
                                        <div class="bg-white text-gray-800 rounded-lg px-4 py-2 max-w-sm border border-gray-200">
                                            <div v-html="currentTyping.replace(/\*\*(.*?)\*\*/g, '<strong>$1</strong>').replace(/\n/g, '<br>')"></div>
                                            <span v-if="showCursor" class="inline-block w-0.5 h-4 bg-gray-600 ml-0.5 animate-pulse"></span>
                                        </div>
                                    </div>
                                </div>
                            </div>
                            
                            <!-- Bottom Stats -->
                            <div class="bg-white border-t border-gray-200 px-4 py-3">
                                <div class="flex items-center justify-between text-xs">
                                    <div class="flex items-center gap-4">
                                        <span class="text-gray-500">Visibility Score:</span>
                                        <span class="font-bold text-green-600">89% for "HubSpot"</span>
                                    </div>
                                    <div class="flex items-center gap-2">
                                        <div class="w-2 h-2 bg-blue-600 rounded-full"></div>
                                        <span class="text-gray-500">#1 mentioned brand</span>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <!-- Logos Section -->
        <section class="py-16 px-8 bg-gray-50">
            <div class="max-w-7xl mx-auto">
                <p class="text-center text-sm text-gray-500 uppercase tracking-wider font-semibold mb-8">
                    Trusted by brands achieving 80%+ LLM visibility
                </p>
                <div class="flex items-center justify-center space-x-16 opacity-60 grayscale">
                    <span class="text-2xl font-light text-gray-400">revolut</span>
                    <span class="text-2xl font-light text-gray-400">stripe</span>
                    <span class="text-2xl font-light text-gray-400">tesla</span>
                    <span class="text-2xl font-light text-gray-400">nyt</span>
                    <span class="text-2xl font-light text-gray-400">guardian</span>
                    <span class="text-2xl font-light text-gray-400">forbes</span>
                </div>
            </div>
        </section>

        <!-- AEO Methodology Section -->
        <section id="solutions" class="py-20 px-8 bg-white">
            <div class="max-w-7xl mx-auto">
                <div class="text-center mb-16">
                    <h2 class="text-5xl font-bold mb-6">
                        The AEO Framework: <span class="text-blue-600">5 Steps to LLM Visibility</span>
                    </h2>
                    <p class="text-xl text-gray-600 max-w-3xl mx-auto">
                        Our proprietary Answer Engine Optimization methodology ensures consistent AI mentions
                    </p>
                </div>

                <!-- AEO Steps Grid -->
                <div class="grid md:grid-cols-5 gap-4 mb-16">
                    <div class="bg-gray-50 rounded-2xl p-6 hover:shadow-lg transition-shadow">
                        <div class="flex items-center mb-4">
                            <div class="w-10 h-10 bg-blue-600 rounded-full flex items-center justify-center mr-3">
                                <span class="text-white font-bold">1</span>
                            </div>
                            <h3 class="text-base font-bold">Map</h3>
                        </div>
                        <p class="text-gray-600 text-sm leading-relaxed">
                            Identify high-value AI conversations and query patterns in your industry.
                        </p>
                    </div>

                    <div class="bg-gray-50 rounded-2xl p-6 hover:shadow-lg transition-shadow">
                        <div class="flex items-center mb-4">
                            <div class="w-10 h-10 bg-blue-600 rounded-full flex items-center justify-center mr-3">
                                <span class="text-white font-bold">2</span>
                            </div>
                            <h3 class="text-base font-bold">Structure</h3>
                        </div>
                        <p class="text-gray-600 text-sm leading-relaxed">
                            Build entity-focused content with comprehensive schema markup.
                        </p>
                    </div>

                    <div class="bg-gray-50 rounded-2xl p-6 hover:shadow-lg transition-shadow">
                        <div class="flex items-center mb-4">
                            <div class="w-10 h-10 bg-blue-600 rounded-full flex items-center justify-center mr-3">
                                <span class="text-white font-bold">3</span>
                            </div>
                            <h3 class="text-base font-bold">Validate</h3>
                        </div>
                        <p class="text-gray-600 text-sm leading-relaxed">
                            Establish authority through external sources and E-E-A-T signals.
                        </p>
                    </div>

                    <div class="bg-gray-50 rounded-2xl p-6 hover:shadow-lg transition-shadow">
                        <div class="flex items-center mb-4">
                            <div class="w-10 h-10 bg-blue-600 rounded-full flex items-center justify-center mr-3">
                                <span class="text-white font-bold">4</span>
                            </div>
                            <h3 class="text-base font-bold">Distribute</h3>
                        </div>
                        <p class="text-gray-600 text-sm leading-relaxed">
                            Optimize for all AI models: GPT-4, Claude, Gemini, Perplexity.
                        </p>
                    </div>

                    <div class="bg-gray-50 rounded-2xl p-6 hover:shadow-lg transition-shadow">
                        <div class="flex items-center mb-4">
                            <div class="w-10 h-10 bg-blue-600 rounded-full flex items-center justify-center mr-3">
                                <span class="text-white font-bold">5</span>
                            </div>
                            <h3 class="text-base font-bold">Optimize</h3>
                        </div>
                        <p class="text-gray-600 text-sm leading-relaxed">
                            Monitor visibility scores and continuously refine based on AI responses.
                        </p>
                    </div>
                </div>

                <!-- Visibility Score Metrics -->
                <div class="mb-16">
                    <h3 class="text-3xl font-bold mb-8 text-center">
                        Your <span class="text-blue-600">Visibility Score</span> Across AI Platforms
                    </h3>
                    <div class="bg-gray-50 rounded-2xl p-8">
                        <div class="grid md:grid-cols-4 gap-6 mb-8">
                            <div class="text-center">
                                <div class="text-4xl font-bold text-blue-600 mb-2">87%</div>
                                <div class="text-sm text-gray-600">ChatGPT</div>
                                <div class="text-xs text-gray-500">+23% MoM</div>
                            </div>
                            <div class="text-center">
                                <div class="text-4xl font-bold text-purple-600 mb-2">92%</div>
                                <div class="text-sm text-gray-600">Perplexity</div>
                                <div class="text-xs text-gray-500">+18% MoM</div>
                            </div>
                            <div class="text-center">
                                <div class="text-4xl font-bold text-blue-600 mb-2">79%</div>
                                <div class="text-sm text-gray-600">Claude</div>
                                <div class="text-xs text-gray-500">+31% MoM</div>
                            </div>
                            <div class="text-center">
                                <div class="text-4xl font-bold text-purple-600 mb-2">84%</div>
                                <div class="text-sm text-gray-600">Gemini</div>
                                <div class="text-xs text-gray-500">+27% MoM</div>
                            </div>
                        </div>
                        <p class="text-center text-gray-600">
                            <strong>Visibility Score:</strong> The percentage of relevant AI-generated answers that mention your brand
                        </p>
                    </div>
                </div>

                <!-- AEO Services -->
                <div>
                    <h3 class="text-2xl font-bold mb-8 text-transparent bg-clip-text bg-gradient-to-r from-purple-400 to-blue-400">
                        Comprehensive AEO Services
                    </h3>
                    <div class="grid md:grid-cols-2 lg:grid-cols-4 gap-6">
                        <div class="bg-white/70 backdrop-blur-lg backdrop-saturate-150 rounded-2xl border border-gray-200/50 p-6 hover:border-purple-500/30 hover:bg-white/90 transition-all duration-500 hover:shadow-lg hover:shadow-purple-500/10 hover:-translate-y-1">
                            <NewspaperIcon class="h-10 w-10 text-purple-500 mb-3" />
                            <h4 class="text-lg font-bold mb-2">Digital PR</h4>
                            <p class="text-sm text-gray-600">
                                Strategic media placements that build authority and drive qualified traffic
                            </p>
                        </div>

                        <div class="bg-white/70 backdrop-blur-lg backdrop-saturate-150 rounded-2xl border border-gray-200/50 p-6 hover:border-blue-500/30 hover:bg-white/90 transition-all duration-500 hover:shadow-lg hover:shadow-blue-500/10 hover:-translate-y-1">
                            <DocumentTextIcon class="h-10 w-10 text-blue-500 mb-3" />
                            <h4 class="text-lg font-bold mb-2">Strategic Listicles</h4>
                            <p class="text-sm text-gray-600">
                                Curated placements in high-value comparison and recommendation content
                            </p>
                        </div>

                        <div class="bg-white/70 backdrop-blur-lg backdrop-saturate-150 rounded-2xl border border-gray-200/50 p-6 hover:border-purple-500/30 hover:bg-white/90 transition-all duration-500 hover:shadow-lg hover:shadow-purple-500/10 hover:-translate-y-1">
                            <ChartBarIcon class="h-10 w-10 text-purple-500 mb-3" />
                            <h4 class="text-lg font-bold mb-2">Directory Optimization</h4>
                            <p class="text-sm text-gray-600">
                                Comprehensive presence across industry-specific and general directories
                            </p>
                        </div>

                        <div class="bg-white/70 backdrop-blur-lg backdrop-saturate-150 rounded-2xl border border-gray-200/50 p-6 hover:border-blue-500/30 hover:bg-white/90 transition-all duration-500 hover:shadow-lg hover:shadow-blue-500/10 hover:-translate-y-1">
                            <UserGroupIcon class="h-10 w-10 text-blue-500 mb-3" />
                            <h4 class="text-lg font-bold mb-2">Community Posting</h4>
                            <p class="text-sm text-gray-600">
                                Authentic engagement in relevant communities and forums
                            </p>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <!-- AI Approach Section -->
        <section id="approach" class="py-20 px-6 border-t border-gray-200 bg-gradient-to-br from-white to-gray-50">
            <div class="max-w-7xl mx-auto">
                <div class="grid lg:grid-cols-2 gap-12 items-center">
                    <div>
                        <h2 class="text-4xl lg:text-5xl font-bold mb-6">
                            Built for the 
                            <span class="text-transparent bg-clip-text bg-gradient-to-r from-purple-400 to-blue-400">
                                AI Era
                            </span>
                        </h2>
                        <p class="text-xl text-gray-600 mb-8">
                            Search has evolved. AI models now mediate between users and information. 
                            Our strategies ensure your content is structured, validated, and positioned 
                            to be the authoritative source AI systems reference.
                        </p>
                        <div class="space-y-4">
                            <div class="flex items-start">
                                <div class="flex-shrink-0 w-12 h-12 bg-purple-900/50 rounded-lg flex items-center justify-center mr-4">
                                    <SparklesIcon class="h-6 w-6 text-purple-400" />
                                </div>
                                <div>
                                    <h3 class="font-bold mb-1">Entity-First Architecture</h3>
                                    <p class="text-gray-600 text-sm">
                                        Content structured around entities and relationships that AI models understand
                                    </p>
                                </div>
                            </div>
                            <div class="flex items-start">
                                <div class="flex-shrink-0 w-12 h-12 bg-blue-900/50 rounded-lg flex items-center justify-center mr-4">
                                    <ChartBarIcon class="h-6 w-6 text-blue-400" />
                                </div>
                                <div>
                                    <h3 class="font-bold mb-1">Predictive Optimization</h3>
                                    <p class="text-gray-600 text-sm">
                                        Anticipate algorithm changes and adapt strategies proactively
                                    </p>
                                </div>
                            </div>
                            <div class="flex items-start">
                                <div class="flex-shrink-0 w-12 h-12 bg-purple-900/50 rounded-lg flex items-center justify-center mr-4">
                                    <MagnifyingGlassIcon class="h-6 w-6 text-purple-400" />
                                </div>
                                <div>
                                    <h3 class="font-bold mb-1">Multi-Model Coverage</h3>
                                    <p class="text-gray-600 text-sm">
                                        Optimized for GPT, Claude, Gemini, and emerging AI search systems
                                    </p>
                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="relative">
                        <div class="absolute inset-0 bg-gradient-to-r from-purple-600/20 to-blue-600/20 blur-3xl"></div>
                        <div class="relative bg-white/80 backdrop-blur-xl backdrop-saturate-150 rounded-3xl border border-gray-200/50 p-8">
                            <div class="grid grid-cols-2 gap-4 mb-6">
                                <div class="bg-black/50 rounded-lg p-4 border border-gray-800">
                                    <div class="text-2xl font-bold text-purple-400 mb-1">92%</div>
                                    <div class="text-xs text-gray-700">AI Citation Rate</div>
                                </div>
                                <div class="bg-black/50 rounded-lg p-4 border border-gray-800">
                                    <div class="text-2xl font-bold text-blue-400 mb-1">4.8s</div>
                                    <div class="text-xs text-gray-700">Avg. Load Time</div>
                                </div>
                            </div>
                            <div class="space-y-3">
                                <div class="bg-black/50 rounded-lg p-3 border border-gray-800">
                                    <div class="flex items-center justify-between mb-2">
                                        <span class="text-sm text-gray-600">Content Structure Score</span>
                                        <span class="text-sm font-bold">98/100</span>
                                    </div>
                                    <div class="w-full bg-gray-800 rounded-full h-2">
                                        <div class="bg-gradient-to-r from-purple-500 to-blue-500 h-2 rounded-full" style="width: 98%"></div>
                                    </div>
                                </div>
                                <div class="bg-black/50 rounded-lg p-3 border border-gray-800">
                                    <div class="flex items-center justify-between mb-2">
                                        <span class="text-sm text-gray-600">Schema Coverage</span>
                                        <span class="text-sm font-bold">100%</span>
                                    </div>
                                    <div class="w-full bg-gray-800 rounded-full h-2">
                                        <div class="bg-gradient-to-r from-purple-500 to-blue-500 h-2 rounded-full" style="width: 100%"></div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <!-- Process Section -->
        <section id="process" class="py-20 px-6 border-t border-gray-200 bg-gradient-to-br from-gray-50 to-white">
            <div class="max-w-7xl mx-auto">
                <div class="text-center mb-16">
                    <h2 class="text-4xl lg:text-5xl font-bold mb-6">
                        Our Process
                    </h2>
                    <p class="text-xl text-gray-700 max-w-3xl mx-auto">
                        A systematic approach to search dominance
                    </p>
                </div>

                <div class="grid md:grid-cols-3 gap-8">
                    <div class="relative">
                        <div class="absolute top-8 left-1/2 w-full h-0.5 bg-gradient-to-r from-transparent via-purple-600 to-transparent hidden md:block"></div>
                        <div class="relative bg-white/80 backdrop-blur-xl backdrop-saturate-150 rounded-3xl border border-gray-200/50 p-8">
                            <div class="w-12 h-12 bg-purple-900/50 rounded-lg flex items-center justify-center mb-4">
                                <span class="text-xl font-bold text-purple-400">1</span>
                            </div>
                            <h3 class="text-xl font-bold mb-3">Audit & Analysis</h3>
                            <p class="text-gray-600">
                                Comprehensive evaluation of your current search presence and AI visibility
                            </p>
                        </div>
                    </div>

                    <div class="relative">
                        <div class="absolute top-8 left-1/2 w-full h-0.5 bg-gradient-to-r from-transparent via-blue-600 to-transparent hidden md:block"></div>
                        <div class="relative bg-white/80 backdrop-blur-xl backdrop-saturate-150 rounded-3xl border border-gray-200/50 p-8">
                            <div class="w-12 h-12 bg-blue-900/50 rounded-lg flex items-center justify-center mb-4">
                                <span class="text-xl font-bold text-blue-400">2</span>
                            </div>
                            <h3 class="text-xl font-bold mb-3">Strategy Development</h3>
                            <p class="text-gray-600">
                                Custom optimization roadmap aligned with your business objectives
                            </p>
                        </div>
                    </div>

                    <div class="relative">
                        <div class="relative bg-white/80 backdrop-blur-xl backdrop-saturate-150 rounded-3xl border border-gray-200/50 p-8">
                            <div class="w-12 h-12 bg-purple-900/50 rounded-lg flex items-center justify-center mb-4">
                                <span class="text-xl font-bold text-purple-400">3</span>
                            </div>
                            <h3 class="text-xl font-bold mb-3">Implementation & Scale</h3>
                            <p class="text-gray-600">
                                Execute optimizations and continuously refine based on performance data
                            </p>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <!-- CTA Section -->
        <section id="contact" class="py-20 px-6 border-t border-gray-200">
            <div class="max-w-4xl mx-auto text-center">
                <h2 class="text-4xl lg:text-5xl font-bold mb-6">
                    Ready to Optimize for
                    <span class="text-transparent bg-clip-text bg-gradient-to-r from-purple-400 to-blue-400">
                        AI-First Search?
                    </span>
                </h2>
                <p class="text-xl text-gray-600 mb-8">
                    Let's discuss how we can position your brand at the forefront of AI-powered discovery.
                </p>
                <div class="flex flex-col sm:flex-row gap-4 justify-center">
                    <a href="mailto:contact@aisearchlabs.com" class="inline-flex items-center justify-center px-8 py-4 bg-gradient-to-r from-purple-600 to-blue-600 rounded-lg font-semibold hover:from-purple-700 hover:to-blue-700 transition">
                        Schedule a Consultation
                        <ChevronRightIcon class="ml-2 h-5 w-5" />
                    </a>
                    <a href="#services" class="inline-flex items-center justify-center px-8 py-4 border border-gray-700 rounded-lg font-semibold hover:border-gray-400 hover:bg-white/50 transition">
                        View Services
                    </a>
                </div>
            </div>
        </section>

        <!-- Footer -->
        <footer class="border-t border-gray-200 py-12 px-6 bg-white">
            <div class="max-w-7xl mx-auto">
                <div class="flex flex-col md:flex-row items-center justify-between">
                    <div class="flex items-center space-x-2 mb-4 md:mb-0">
                        <SparklesIcon class="h-6 w-6 text-blue-600" />
                        <span class="font-bold">AI Search Labs</span>
                        <span class="text-sm text-gray-500">• The AEO Experts</span>
                    </div>
                    <div class="text-sm text-gray-700">
                        &copy; 2025 AI Search Labs. Pioneering Answer Engine Optimization.
                    </div>
                </div>
            </div>
        </footer>
    </div>
</template>

<style scoped>
@keyframes gradient {
    0% { background-position: 0% 50%; }
    50% { background-position: 100% 50%; }
    100% { background-position: 0% 50%; }
}

@keyframes blob {
    0% {
        transform: translate(0px, 0px) scale(1);
    }
    33% {
        transform: translate(30px, -50px) scale(1.1);
    }
    66% {
        transform: translate(-20px, 20px) scale(0.9);
    }
    100% {
        transform: translate(0px, 0px) scale(1);
    }
}

.animate-blob {
    animation: blob 7s infinite;
}

.animation-delay-2000 {
    animation-delay: 2s;
}

.animation-delay-4000 {
    animation-delay: 4s;
}

/* Glass morphism enhancement */
.glass-card {
    background: rgba(255, 255, 255, 0.05);
    backdrop-filter: blur(20px) saturate(200%);
    -webkit-backdrop-filter: blur(20px) saturate(200%);
}

/* Smooth hover transitions */
.hover-lift {
    transition: all 0.5s cubic-bezier(0.4, 0, 0.2, 1);
}

.hover-lift:hover {
    transform: translateY(-4px);
}
</style>