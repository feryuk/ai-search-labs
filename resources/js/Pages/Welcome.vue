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

onMounted(() => {
    setInterval(() => {
        currentServiceIndex.value = (currentServiceIndex.value + 1) % services.length;
    }, 3000);
});
</script>

<template>
    <Head title="AI Search Labs - Next-Gen Search Optimization" />
    
    <div class="min-h-screen bg-black text-white relative overflow-hidden">
        <!-- Animated gradient orbs for liquid effect -->
        <div class="fixed inset-0 overflow-hidden pointer-events-none">
            <div class="absolute -top-40 -right-40 w-80 h-80 bg-purple-500 rounded-full mix-blend-screen filter blur-3xl opacity-20 animate-blob"></div>
            <div class="absolute -bottom-40 -left-40 w-80 h-80 bg-blue-500 rounded-full mix-blend-screen filter blur-3xl opacity-20 animate-blob animation-delay-2000"></div>
            <div class="absolute top-1/2 left-1/2 transform -translate-x-1/2 -translate-y-1/2 w-80 h-80 bg-indigo-500 rounded-full mix-blend-screen filter blur-3xl opacity-20 animate-blob animation-delay-4000"></div>
        </div>
        <!-- Navigation with Glass Effect -->
        <nav class="fixed top-0 w-full bg-black/20 backdrop-blur-xl backdrop-saturate-150 border-b border-white/10 z-50">
            <div class="max-w-7xl mx-auto px-6 py-4 flex items-center justify-between">
                <div class="flex items-center space-x-2">
                    <SparklesIcon class="h-8 w-8 text-purple-500" />
                    <span class="text-xl font-bold">AI Search Labs</span>
                </div>
                <div class="hidden md:flex items-center space-x-8">
                    <a href="#services" class="text-gray-300/80 hover:text-white transition-all duration-300 relative group">
                        Services
                        <span class="absolute -bottom-1 left-0 w-0 h-0.5 bg-gradient-to-r from-purple-400 to-blue-400 group-hover:w-full transition-all duration-300"></span>
                    </a>
                    <a href="#approach" class="text-gray-300/80 hover:text-white transition-all duration-300 relative group">
                        Approach
                        <span class="absolute -bottom-1 left-0 w-0 h-0.5 bg-gradient-to-r from-purple-400 to-blue-400 group-hover:w-full transition-all duration-300"></span>
                    </a>
                    <a href="#process" class="text-gray-300/80 hover:text-white transition-all duration-300 relative group">
                        Process
                        <span class="absolute -bottom-1 left-0 w-0 h-0.5 bg-gradient-to-r from-purple-400 to-blue-400 group-hover:w-full transition-all duration-300"></span>
                    </a>
                    <a href="#contact" class="text-gray-300/80 hover:text-white transition-all duration-300 relative group">
                        Contact
                        <span class="absolute -bottom-1 left-0 w-0 h-0.5 bg-gradient-to-r from-purple-400 to-blue-400 group-hover:w-full transition-all duration-300"></span>
                    </a>
                </div>
            </div>
        </nav>

        <!-- Hero Section -->
        <section class="relative pt-32 pb-20 px-6 overflow-hidden">
            <div class="absolute inset-0 bg-gradient-to-br from-purple-900/20 via-black to-blue-900/20"></div>
            <div class="relative max-w-7xl mx-auto">
                <div class="grid lg:grid-cols-2 gap-12 items-center">
                    <div>
                        <h1 class="text-5xl lg:text-7xl font-bold mb-6 leading-tight">
                            Search Optimization
                            <span class="block text-transparent bg-clip-text bg-gradient-to-r from-purple-400 to-blue-400">
                                Engineered for AI
                            </span>
                        </h1>
                        <p class="text-xl text-gray-400 mb-8 leading-relaxed">
                            We optimize your digital presence for the next generation of search. 
                            From AI-driven content strategies to geographic optimization, 
                            we position your brand where it matters most.
                        </p>
                        <div class="flex flex-col sm:flex-row gap-4">
                            <a href="#contact" class="group inline-flex items-center px-8 py-4 bg-gradient-to-r from-purple-600 to-blue-600 rounded-2xl font-semibold hover:from-purple-500 hover:to-blue-500 transition-all duration-500 shadow-lg shadow-purple-500/25 hover:shadow-xl hover:shadow-purple-500/40 hover:-translate-y-0.5">
                                Start Your Optimization
                                <ChevronRightIcon class="ml-2 h-5 w-5 group-hover:translate-x-1 transition-transform" />
                            </a>
                            <a href="#services" class="inline-flex items-center px-8 py-4 border border-white/20 bg-white/5 backdrop-blur-xl rounded-2xl font-semibold hover:border-white/30 hover:bg-white/10 transition-all duration-500 hover:shadow-lg hover:shadow-white/10 hover:-translate-y-0.5">
                                Explore Services
                            </a>
                        </div>
                    </div>
                    <div class="relative">
                        <div class="absolute inset-0 bg-gradient-to-r from-purple-600/20 to-blue-600/20 blur-3xl"></div>
                        <div class="relative bg-white/5 backdrop-blur-2xl backdrop-saturate-200 rounded-3xl border border-white/10 p-8">
                            <div class="text-sm text-gray-500 mb-2">Currently Optimizing</div>
                            <div class="text-2xl font-bold mb-6 h-8 transition-all duration-500">
                                {{ services[currentServiceIndex] }}
                            </div>
                            <div class="space-y-4">
                                <div class="flex items-center justify-between">
                                    <span class="text-gray-400">Search Visibility</span>
                                    <span class="text-green-400">+287%</span>
                                </div>
                                <div class="flex items-center justify-between">
                                    <span class="text-gray-400">AI Answer Coverage</span>
                                    <span class="text-green-400">94%</span>
                                </div>
                                <div class="flex items-center justify-between">
                                    <span class="text-gray-400">Geographic Reach</span>
                                    <span class="text-green-400">127 Cities</span>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <!-- Stats Section -->
        <section class="py-20 px-6 border-t border-gray-900">
            <div class="max-w-7xl mx-auto">
                <div class="grid md:grid-cols-4 gap-8">
                    <div class="text-center">
                        <div class="text-4xl font-bold text-transparent bg-clip-text bg-gradient-to-r from-purple-400 to-blue-400 mb-2">
                            89%
                        </div>
                        <div class="text-gray-400">AI Answer Rate</div>
                    </div>
                    <div class="text-center">
                        <div class="text-4xl font-bold text-transparent bg-clip-text bg-gradient-to-r from-purple-400 to-blue-400 mb-2">
                            3.2x
                        </div>
                        <div class="text-gray-400">Organic Growth</div>
                    </div>
                    <div class="text-center">
                        <div class="text-4xl font-bold text-transparent bg-clip-text bg-gradient-to-r from-purple-400 to-blue-400 mb-2">
                            150+
                        </div>
                        <div class="text-gray-400">Optimized Brands</div>
                    </div>
                    <div class="text-center">
                        <div class="text-4xl font-bold text-transparent bg-clip-text bg-gradient-to-r from-purple-400 to-blue-400 mb-2">
                            24/7
                        </div>
                        <div class="text-gray-400">AI Monitoring</div>
                    </div>
                </div>
            </div>
        </section>

        <!-- Services Section -->
        <section id="services" class="py-20 px-6">
            <div class="max-w-7xl mx-auto">
                <div class="text-center mb-16">
                    <h2 class="text-4xl lg:text-5xl font-bold mb-6">
                        Comprehensive Optimization Suite
                    </h2>
                    <p class="text-xl text-gray-400 max-w-3xl mx-auto">
                        Strategic solutions designed for modern search algorithms and AI-powered discovery engines
                    </p>
                </div>

                <!-- On-Site Optimization -->
                <div class="mb-16">
                    <h3 class="text-2xl font-bold mb-8 text-transparent bg-clip-text bg-gradient-to-r from-purple-400 to-blue-400">
                        On-Site Optimization
                    </h3>
                    <div class="grid md:grid-cols-2 gap-8">
                        <div class="bg-white/5 backdrop-blur-2xl backdrop-saturate-200 rounded-3xl border border-white/10 p-8 hover:border-purple-500/30 hover:bg-white/10 transition-all duration-500 hover:shadow-lg hover:shadow-purple-500/10 hover:-translate-y-1">
                            <SparklesIcon class="h-12 w-12 text-purple-500 mb-4" />
                            <h4 class="text-xl font-bold mb-3">AIO (AI Optimization)</h4>
                            <p class="text-gray-400 mb-4">
                                Structure your content to be the preferred source for AI language models. 
                                We optimize for featured snippets, knowledge graphs, and AI-generated answers.
                            </p>
                            <ul class="space-y-2 text-sm text-gray-500">
                                <li class="flex items-center">
                                    <ChevronRightIcon class="h-4 w-4 mr-2 text-purple-500" />
                                    Schema markup optimization
                                </li>
                                <li class="flex items-center">
                                    <ChevronRightIcon class="h-4 w-4 mr-2 text-purple-500" />
                                    Entity-based content structuring
                                </li>
                                <li class="flex items-center">
                                    <ChevronRightIcon class="h-4 w-4 mr-2 text-purple-500" />
                                    AI-readable data formatting
                                </li>
                            </ul>
                        </div>

                        <div class="bg-white/5 backdrop-blur-2xl backdrop-saturate-200 rounded-3xl border border-white/10 p-8 hover:border-blue-500/30 hover:bg-white/10 transition-all duration-500 hover:shadow-lg hover:shadow-blue-500/10 hover:-translate-y-1">
                            <MagnifyingGlassIcon class="h-12 w-12 text-blue-500 mb-4" />
                            <h4 class="text-xl font-bold mb-3">GEO (Geographic Optimization)</h4>
                            <p class="text-gray-400 mb-4">
                                Dominate local search results across multiple locations. 
                                Our geographic optimization ensures visibility in every market you serve.
                            </p>
                            <ul class="space-y-2 text-sm text-gray-500">
                                <li class="flex items-center">
                                    <ChevronRightIcon class="h-4 w-4 mr-2 text-blue-500" />
                                    Multi-location page optimization
                                </li>
                                <li class="flex items-center">
                                    <ChevronRightIcon class="h-4 w-4 mr-2 text-blue-500" />
                                    Local business schema
                                </li>
                                <li class="flex items-center">
                                    <ChevronRightIcon class="h-4 w-4 mr-2 text-blue-500" />
                                    Regional content strategies
                                </li>
                            </ul>
                        </div>
                    </div>
                </div>

                <!-- Off-Site Optimization -->
                <div>
                    <h3 class="text-2xl font-bold mb-8 text-transparent bg-clip-text bg-gradient-to-r from-purple-400 to-blue-400">
                        Off-Site Optimization
                    </h3>
                    <div class="grid md:grid-cols-2 lg:grid-cols-4 gap-6">
                        <div class="bg-white/5 backdrop-blur-xl backdrop-saturate-150 rounded-2xl border border-white/10 p-6 hover:border-purple-500/30 hover:bg-white/10 transition-all duration-500 hover:shadow-lg hover:shadow-purple-500/10 hover:-translate-y-1">
                            <NewspaperIcon class="h-10 w-10 text-purple-500 mb-3" />
                            <h4 class="text-lg font-bold mb-2">Digital PR</h4>
                            <p class="text-sm text-gray-400">
                                Strategic media placements that build authority and drive qualified traffic
                            </p>
                        </div>

                        <div class="bg-white/5 backdrop-blur-xl backdrop-saturate-150 rounded-2xl border border-white/10 p-6 hover:border-blue-500/30 hover:bg-white/10 transition-all duration-500 hover:shadow-lg hover:shadow-blue-500/10 hover:-translate-y-1">
                            <DocumentTextIcon class="h-10 w-10 text-blue-500 mb-3" />
                            <h4 class="text-lg font-bold mb-2">Strategic Listicles</h4>
                            <p class="text-sm text-gray-400">
                                Curated placements in high-value comparison and recommendation content
                            </p>
                        </div>

                        <div class="bg-white/5 backdrop-blur-xl backdrop-saturate-150 rounded-2xl border border-white/10 p-6 hover:border-purple-500/30 hover:bg-white/10 transition-all duration-500 hover:shadow-lg hover:shadow-purple-500/10 hover:-translate-y-1">
                            <ChartBarIcon class="h-10 w-10 text-purple-500 mb-3" />
                            <h4 class="text-lg font-bold mb-2">Directory Optimization</h4>
                            <p class="text-sm text-gray-400">
                                Comprehensive presence across industry-specific and general directories
                            </p>
                        </div>

                        <div class="bg-white/5 backdrop-blur-xl backdrop-saturate-150 rounded-2xl border border-white/10 p-6 hover:border-blue-500/30 hover:bg-white/10 transition-all duration-500 hover:shadow-lg hover:shadow-blue-500/10 hover:-translate-y-1">
                            <UserGroupIcon class="h-10 w-10 text-blue-500 mb-3" />
                            <h4 class="text-lg font-bold mb-2">Community Posting</h4>
                            <p class="text-sm text-gray-400">
                                Authentic engagement in relevant communities and forums
                            </p>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <!-- AI Approach Section -->
        <section id="approach" class="py-20 px-6 border-t border-gray-900">
            <div class="max-w-7xl mx-auto">
                <div class="grid lg:grid-cols-2 gap-12 items-center">
                    <div>
                        <h2 class="text-4xl lg:text-5xl font-bold mb-6">
                            Built for the 
                            <span class="text-transparent bg-clip-text bg-gradient-to-r from-purple-400 to-blue-400">
                                AI Era
                            </span>
                        </h2>
                        <p class="text-xl text-gray-400 mb-8">
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
                                    <p class="text-gray-400 text-sm">
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
                                    <p class="text-gray-400 text-sm">
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
                                    <p class="text-gray-400 text-sm">
                                        Optimized for GPT, Claude, Gemini, and emerging AI search systems
                                    </p>
                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="relative">
                        <div class="absolute inset-0 bg-gradient-to-r from-purple-600/20 to-blue-600/20 blur-3xl"></div>
                        <div class="relative bg-white/5 backdrop-blur-2xl backdrop-saturate-200 rounded-3xl border border-white/10 p-8">
                            <div class="grid grid-cols-2 gap-4 mb-6">
                                <div class="bg-black/50 rounded-lg p-4 border border-gray-800">
                                    <div class="text-2xl font-bold text-purple-400 mb-1">92%</div>
                                    <div class="text-xs text-gray-500">AI Citation Rate</div>
                                </div>
                                <div class="bg-black/50 rounded-lg p-4 border border-gray-800">
                                    <div class="text-2xl font-bold text-blue-400 mb-1">4.8s</div>
                                    <div class="text-xs text-gray-500">Avg. Load Time</div>
                                </div>
                            </div>
                            <div class="space-y-3">
                                <div class="bg-black/50 rounded-lg p-3 border border-gray-800">
                                    <div class="flex items-center justify-between mb-2">
                                        <span class="text-sm text-gray-400">Content Structure Score</span>
                                        <span class="text-sm font-bold">98/100</span>
                                    </div>
                                    <div class="w-full bg-gray-800 rounded-full h-2">
                                        <div class="bg-gradient-to-r from-purple-500 to-blue-500 h-2 rounded-full" style="width: 98%"></div>
                                    </div>
                                </div>
                                <div class="bg-black/50 rounded-lg p-3 border border-gray-800">
                                    <div class="flex items-center justify-between mb-2">
                                        <span class="text-sm text-gray-400">Schema Coverage</span>
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
        <section id="process" class="py-20 px-6 border-t border-gray-900">
            <div class="max-w-7xl mx-auto">
                <div class="text-center mb-16">
                    <h2 class="text-4xl lg:text-5xl font-bold mb-6">
                        Our Process
                    </h2>
                    <p class="text-xl text-gray-400 max-w-3xl mx-auto">
                        A systematic approach to search dominance
                    </p>
                </div>

                <div class="grid md:grid-cols-3 gap-8">
                    <div class="relative">
                        <div class="absolute top-8 left-1/2 w-full h-0.5 bg-gradient-to-r from-transparent via-purple-600 to-transparent hidden md:block"></div>
                        <div class="relative bg-white/5 backdrop-blur-2xl backdrop-saturate-200 rounded-3xl border border-white/10 p-8">
                            <div class="w-12 h-12 bg-purple-900/50 rounded-lg flex items-center justify-center mb-4">
                                <span class="text-xl font-bold text-purple-400">1</span>
                            </div>
                            <h3 class="text-xl font-bold mb-3">Audit & Analysis</h3>
                            <p class="text-gray-400">
                                Comprehensive evaluation of your current search presence and AI visibility
                            </p>
                        </div>
                    </div>

                    <div class="relative">
                        <div class="absolute top-8 left-1/2 w-full h-0.5 bg-gradient-to-r from-transparent via-blue-600 to-transparent hidden md:block"></div>
                        <div class="relative bg-white/5 backdrop-blur-2xl backdrop-saturate-200 rounded-3xl border border-white/10 p-8">
                            <div class="w-12 h-12 bg-blue-900/50 rounded-lg flex items-center justify-center mb-4">
                                <span class="text-xl font-bold text-blue-400">2</span>
                            </div>
                            <h3 class="text-xl font-bold mb-3">Strategy Development</h3>
                            <p class="text-gray-400">
                                Custom optimization roadmap aligned with your business objectives
                            </p>
                        </div>
                    </div>

                    <div class="relative">
                        <div class="relative bg-white/5 backdrop-blur-2xl backdrop-saturate-200 rounded-3xl border border-white/10 p-8">
                            <div class="w-12 h-12 bg-purple-900/50 rounded-lg flex items-center justify-center mb-4">
                                <span class="text-xl font-bold text-purple-400">3</span>
                            </div>
                            <h3 class="text-xl font-bold mb-3">Implementation & Scale</h3>
                            <p class="text-gray-400">
                                Execute optimizations and continuously refine based on performance data
                            </p>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <!-- CTA Section -->
        <section id="contact" class="py-20 px-6 border-t border-gray-900">
            <div class="max-w-4xl mx-auto text-center">
                <h2 class="text-4xl lg:text-5xl font-bold mb-6">
                    Ready to Optimize for
                    <span class="text-transparent bg-clip-text bg-gradient-to-r from-purple-400 to-blue-400">
                        AI-First Search?
                    </span>
                </h2>
                <p class="text-xl text-gray-400 mb-8">
                    Let's discuss how we can position your brand at the forefront of AI-powered discovery.
                </p>
                <div class="flex flex-col sm:flex-row gap-4 justify-center">
                    <a href="mailto:contact@aisearchlabs.com" class="inline-flex items-center justify-center px-8 py-4 bg-gradient-to-r from-purple-600 to-blue-600 rounded-lg font-semibold hover:from-purple-700 hover:to-blue-700 transition">
                        Schedule a Consultation
                        <ChevronRightIcon class="ml-2 h-5 w-5" />
                    </a>
                    <a href="#services" class="inline-flex items-center justify-center px-8 py-4 border border-gray-700 rounded-lg font-semibold hover:border-gray-600 hover:bg-gray-900/50 transition">
                        View Services
                    </a>
                </div>
            </div>
        </section>

        <!-- Footer -->
        <footer class="border-t border-gray-900 py-12 px-6">
            <div class="max-w-7xl mx-auto">
                <div class="flex flex-col md:flex-row items-center justify-between">
                    <div class="flex items-center space-x-2 mb-4 md:mb-0">
                        <SparklesIcon class="h-6 w-6 text-purple-500" />
                        <span class="font-bold">AI Search Labs</span>
                    </div>
                    <div class="text-sm text-gray-500">
                        &copy; 2024 AI Search Labs. Optimizing the future of search.
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