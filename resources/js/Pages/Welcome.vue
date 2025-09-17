<script setup>
import { Head } from "@inertiajs/vue3";
import { ref, onMounted, nextTick } from "vue";
import {
    ChevronRightIcon,
    SparklesIcon,
    MagnifyingGlassIcon,
    ChartBarIcon,
    DocumentTextIcon,
    UserGroupIcon,
    NewspaperIcon,
} from "@heroicons/vue/24/outline";
import { Chart, registerables } from "chart.js";

// Register Chart.js components
Chart.register(...registerables);

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
    "AI-Optimized Content",
    "Geographic SEO",
    "Digital PR",
    "Strategic Listicles",
];

// Chat simulation state
const chatMessages = ref([]);
const currentTyping = ref("");
const isTyping = ref(false);
const showCursor = ref(true);
const showSources = ref(false);

// Audit form state
const websiteUrl = ref("");
const showModal = ref(false);
const contactForm = ref({
    name: "",
    email: "",
    phone: "",
});

// Show visibility score after chat completes
const showVisibilityScore = ref(false);

// Chart reference for simple line chart
const visibilityChart = ref(null);

// Timeline hover state
const hoveredMonth = ref(null);

// Initialize chart function
const initializeChart = async () => {
    await nextTick();

    // Simple visibility growth chart
    if (visibilityChart.value) {
        new Chart(visibilityChart.value, {
            type: "line",
            data: {
                labels: [
                    "Start",
                    "Month 1",
                    "Month 2",
                    "Month 3",
                    "Month 4",
                    "Month 5",
                    "Month 6",
                ],
                datasets: [
                    {
                        label: "Your Brand",
                        data: [12, 18, 30, 52, 68, 81, 94],
                        borderColor: "rgb(59, 130, 246)",
                        backgroundColor: "rgba(59, 130, 246, 0.05)",
                        borderWidth: 2,
                        tension: 0.4,
                        fill: true,
                        pointRadius: 4,
                        pointHoverRadius: 6,
                        pointBackgroundColor: "white",
                        pointBorderColor: "rgb(59, 130, 246)",
                        pointBorderWidth: 2,
                    },
                ],
            },
            options: {
                responsive: true,
                maintainAspectRatio: false,
                plugins: {
                    legend: {
                        display: false,
                    },
                    tooltip: {
                        callbacks: {
                            label: function (context) {
                                return "Visibility: " + context.parsed.y + "%";
                            },
                        },
                    },
                },
                scales: {
                    y: {
                        beginAtZero: true,
                        max: 100,
                        ticks: {
                            callback: function (value) {
                                return value + "%";
                            },
                            stepSize: 25,
                        },
                        grid: {
                            color: "rgba(0, 0, 0, 0.05)",
                        },
                    },
                    x: {
                        grid: {
                            display: false,
                        },
                    },
                },
            },
        });
    }
};

// AI platforms for animated banner
const aiPlatforms = [
    { name: "ChatGPT", domain: "openai.com" },
    { name: "Gemini", domain: "gemini.google.com" },
    { name: "Perplexity", domain: "perplexity.ai" },
    { name: "Claude", domain: "claude.ai" },
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
    { domain: "hubspot.com", name: "HubSpot" },
    { domain: "g2.com", name: "G2" },
    { domain: "capterra.com", name: "Capterra" },
    { domain: "techradar.com", name: "TechRadar" },
    { domain: "forbes.com", name: "Forbes" },
];

const typeMessage = async (message, isUser = false) => {
    currentTyping.value = "";
    isTyping.value = true;

    for (let i = 0; i <= message.length; i++) {
        currentTyping.value = message.substring(0, i);
        await new Promise((resolve) => setTimeout(resolve, isUser ? 30 : 8));
    }

    isTyping.value = false;
    chatMessages.value.push({ text: message, isUser });
    currentTyping.value = "";
};

const handleSubmitWebsite = () => {
    if (websiteUrl.value.trim()) {
        showModal.value = true;
    }
};

const handleSubmitContact = () => {
    // Handle form submission
    console.log("Form submitted:", {
        website: websiteUrl.value,
        ...contactForm.value,
    });
    showModal.value = false;
    // Reset form
    websiteUrl.value = "";
    contactForm.value = { name: "", email: "", phone: "" };
    alert("Thank you! We'll send your free AEO audit within 24 hours.");
};

onMounted(() => {
    setInterval(() => {
        currentServiceIndex.value =
            (currentServiceIndex.value + 1) % services.length;
    }, 3000);

    // Rotate AI platforms (both banner and chat in sync)
    setInterval(() => {
        currentPlatformIndex.value =
            (currentPlatformIndex.value + 1) % aiPlatforms.length;
        currentChatPlatformIndex.value = currentPlatformIndex.value;
    }, 2500);

    // Initialize chart after component is mounted
    initializeChart();

    // Cursor blink
    setInterval(() => {
        showCursor.value = !showCursor.value;
    }, 500);

    // Start chat simulation after delay
    setTimeout(async () => {
        await typeMessage(userQuery, true);
        await new Promise((resolve) => setTimeout(resolve, 500));
        await typeMessage(aiResponse, false);
        // Show sources after message completes
        setTimeout(() => {
            showSources.value = true;
            // Show visibility score after sources appear
            setTimeout(() => {
                showVisibilityScore.value = true;
            }, 500);
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
                <p
                    class="text-sm text-gray-600 flex items-center justify-center gap-2"
                >
                    <span>Get your brand mentioned by</span>
                    <span class="relative inline-flex items-center">
                        <!-- AI Platform rotating display -->
                        <div class="relative w-32 h-6">
                            <div
                                v-for="(platform, index) in aiPlatforms"
                                :key="platform.name"
                                class="absolute inset-0 flex items-center gap-1.5 transition-all duration-500"
                                :class="{
                                    'opacity-100 transform translate-y-0':
                                        index === currentPlatformIndex,
                                    'opacity-0 transform -translate-y-full':
                                        index !== currentPlatformIndex,
                                }"
                            >
                                <img
                                    :src="`https://www.google.com/s2/favicons?domain=${platform.domain}&sz=32`"
                                    :alt="platform.name"
                                    class="w-4 h-4"
                                />
                                <span class="font-semibold">{{
                                    platform.name
                                }}</span>
                            </div>
                        </div>
                    </span>
                </p>
            </div>
        </div>
        <!-- Navigation -->
        <nav class="bg-white border-b border-gray-100">
            <div
                class="max-w-7xl mx-auto px-8 py-5 flex items-center justify-between"
            >
                <div class="flex items-center space-x-12">
                    <!-- Logo -->
                    <div class="flex items-center space-x-3">
                        <div
                            class="w-10 h-10 bg-blue-600 rounded-full flex items-center justify-center"
                        >
                            <SparklesIcon class="h-6 w-6 text-white" />
                        </div>
                        <span class="text-xl font-semibold text-black"
                            >AI Search Labs</span
                        >
                    </div>
                    <!-- Navigation Links -->
                    <div class="hidden md:flex items-center space-x-8">
                        <a
                            href="#product"
                            class="text-gray-900 font-medium hover:text-blue-600 transition"
                            >Product</a
                        >
                        <a
                            href="#solutions"
                            class="text-gray-900 font-medium hover:text-blue-600 transition"
                            >Solutions</a
                        >
                        <a
                            href="#case-studies"
                            class="text-gray-900 font-medium hover:text-blue-600 transition"
                            >Case studies</a
                        >
                        <a
                            href="#about"
                            class="text-gray-900 font-medium hover:text-blue-600 transition"
                            >About</a
                        >
                    </div>
                </div>
                <div class="flex items-center space-x-4">
                    <a
                        href="#contact"
                        class="text-gray-900 font-medium hover:text-blue-600 transition"
                        >Contact</a
                    >
                    <button
                        class="inline-flex items-center px-5 py-2.5 bg-black text-white rounded-full font-medium hover:bg-gray-800 transition"
                    >
                        <SparklesIcon class="h-4 w-4 mr-2" />
                        Get a demo
                    </button>
                </div>
            </div>
        </nav>

        <!-- Hero Section -->
        <section class="relative pt-20 pb-20 px-8">
            <!-- Grid background -->
            <div class="absolute inset-0 bg-gray-50">
                <div
                    class="absolute inset-0"
                    style="
                        background-image: linear-gradient(
                                rgba(0, 0, 0, 0.03) 1px,
                                transparent 1px
                            ),
                            linear-gradient(
                                90deg,
                                rgba(0, 0, 0, 0.03) 1px,
                                transparent 1px
                            );
                        background-size: 50px 50px;
                    "
                ></div>
            </div>
            <div class="relative max-w-7xl mx-auto">
                <div class="grid lg:grid-cols-2 gap-16 items-start">
                    <div class="pt-8">
                        <div
                            class="inline-flex items-center px-4 py-2 bg-blue-50 text-blue-700 rounded-full text-sm font-semibold mb-6"
                        >
                            <SparklesIcon class="h-4 w-4 mr-2" />
                            AI Search Optimization Agency
                        </div>
                        <h1
                            class="text-6xl lg:text-7xl font-bold mb-8 leading-tight"
                        >
                            Get your brand<br />
                            mentioned by
                            <span class="text-blue-600">AI<br /></span>
                        </h1>
                        <p
                            class="text-xl text-gray-600 mb-4 leading-relaxed max-w-xl"
                        >
                            <strong
                                >We are a UK-based AI search optimization
                                agency.</strong
                            >
                            We ensure your brand appears in AI-generated answers
                            when your customers ask questions.
                        </p>
                        <p
                            class="text-lg text-gray-500 mb-10 leading-relaxed max-w-xl"
                        >
                            Through strategic on-site optimization, AI-focused
                            content creation, digital PR, listicles, and
                            community signals, we make you the preferred
                            recommendation across ChatGPT, Claude, Gemini, and
                            Perplexity.
                        </p>
                        <div class="flex items-center gap-4">
                            <button
                                class="inline-flex items-center px-6 py-3 bg-black text-white rounded-full font-medium hover:bg-gray-800 transition"
                            >
                                <svg
                                    class="h-5 w-5 mr-2"
                                    fill="none"
                                    stroke="currentColor"
                                    viewBox="0 0 24 24"
                                >
                                    <path
                                        stroke-linecap="round"
                                        stroke-linejoin="round"
                                        stroke-width="2"
                                        d="M15 10l-4 4m0 0l-4-4m4 4V3"
                                    ></path>
                                </svg>
                                Get AEO Audit
                            </button>
                            <button
                                class="px-6 py-3 bg-white border-2 border-gray-200 text-black rounded-full font-medium hover:bg-gray-50 transition"
                            >
                                View Case Studies
                            </button>
                        </div>
                    </div>
                    <div class="relative">
                        <!-- ChatGPT Simulation Card -->
                        <div
                            class="bg-white rounded-2xl border border-gray-200 shadow-xl overflow-hidden"
                        >
                            <!-- AI Platform Header (Animated) -->
                            <div
                                class="bg-gray-50 border-b border-gray-200 px-4 py-3 flex items-center justify-between overflow-hidden"
                            >
                                <div class="flex items-center gap-2">
                                    <div class="relative w-32 h-6">
                                        <div
                                            v-for="(
                                                platform, index
                                            ) in aiPlatforms"
                                            :key="`chat-${platform.name}`"
                                            class="absolute inset-0 flex items-center gap-2 transition-all duration-500"
                                            :class="{
                                                'opacity-100 transform translate-y-0 blur-0':
                                                    index ===
                                                    currentChatPlatformIndex,
                                                'opacity-0 transform -translate-y-full blur-sm':
                                                    index !==
                                                    currentChatPlatformIndex,
                                            }"
                                        >
                                            <img
                                                :src="`https://www.google.com/s2/favicons?domain=${platform.domain}&sz=32`"
                                                :alt="platform.name"
                                                class="w-6 h-6"
                                            />
                                            <span
                                                class="text-sm font-semibold"
                                                >{{ platform.name }}</span
                                            >
                                        </div>
                                    </div>
                                </div>
                                <div class="flex items-center gap-2">
                                    <div
                                        class="w-2 h-2 bg-green-500 rounded-full animate-pulse"
                                    ></div>
                                    <span class="text-xs text-gray-500"
                                        >Live Demo</span
                                    >
                                </div>
                            </div>

                            <!-- Chat Messages -->
                            <div class="p-4 h-96 overflow-y-auto bg-gray-50">
                                <!-- Previous messages -->
                                <div
                                    v-for="(message, index) in chatMessages"
                                    :key="index"
                                    class="mb-4"
                                >
                                    <div
                                        v-if="message.isUser"
                                        class="flex justify-end"
                                    >
                                        <div
                                            class="bg-blue-600 text-white rounded-lg px-4 py-2 max-w-xs"
                                        >
                                            {{ message.text }}
                                        </div>
                                    </div>
                                    <div v-else>
                                        <div class="flex justify-start">
                                            <div
                                                class="bg-white text-gray-800 rounded-lg px-4 py-2 max-w-sm border border-gray-200"
                                            >
                                                <div
                                                    v-html="
                                                        message.text
                                                            .replace(
                                                                /\*\*(.*?)\*\*/g,
                                                                '<strong>$1</strong>'
                                                            )
                                                            .replace(
                                                                /\n/g,
                                                                '<br>'
                                                            )
                                                    "
                                                ></div>
                                            </div>
                                        </div>
                                        <!-- Sources section -->
                                        <div
                                            v-if="
                                                showSources &&
                                                index ===
                                                    chatMessages.length - 1
                                            "
                                            class="mt-3 ml-0"
                                        >
                                            <div
                                                class="text-xs text-gray-500 mb-2"
                                            >
                                                Sources
                                            </div>
                                            <div class="flex gap-2 flex-wrap">
                                                <a
                                                    v-for="source in sources"
                                                    :key="source.domain"
                                                    :href="`https://${source.domain}`"
                                                    target="_blank"
                                                    class="flex items-center gap-1.5 bg-white border border-gray-200 rounded-full px-3 py-1.5 hover:bg-gray-50 transition-colors"
                                                >
                                                    <img
                                                        :src="`https://www.google.com/s2/favicons?domain=${source.domain}&sz=32`"
                                                        :alt="source.name"
                                                        class="w-4 h-4 rounded-sm"
                                                    />
                                                    <span
                                                        class="text-xs text-gray-700"
                                                        >{{ source.name }}</span
                                                    >
                                                </a>
                                            </div>
                                        </div>
                                    </div>
                                </div>

                                <!-- Currently typing message -->
                                <div
                                    v-if="isTyping && currentTyping"
                                    class="mb-4"
                                >
                                    <div
                                        v-if="chatMessages.length === 0"
                                        class="flex justify-end"
                                    >
                                        <div
                                            class="bg-blue-600 text-white rounded-lg px-4 py-2 max-w-xs"
                                        >
                                            {{ currentTyping
                                            }}<span
                                                v-if="showCursor"
                                                class="inline-block w-0.5 h-4 bg-white ml-0.5 animate-pulse"
                                            ></span>
                                        </div>
                                    </div>
                                    <div v-else class="flex justify-start">
                                        <div
                                            class="bg-white text-gray-800 rounded-lg px-4 py-2 max-w-sm border border-gray-200"
                                        >
                                            <div
                                                v-html="
                                                    currentTyping
                                                        .replace(
                                                            /\*\*(.*?)\*\*/g,
                                                            '<strong>$1</strong>'
                                                        )
                                                        .replace(/\n/g, '<br>')
                                                "
                                            ></div>
                                            <span
                                                v-if="showCursor"
                                                class="inline-block w-0.5 h-4 bg-gray-600 ml-0.5 animate-pulse"
                                            ></span>
                                        </div>
                                    </div>
                                </div>
                            </div>

                            <!-- Bottom Stats -->
                            <div
                                class="bg-white border-t border-gray-200 px-4 py-3 transition-all duration-500"
                                :class="
                                    showVisibilityScore
                                        ? 'bg-gradient-to-r from-green-50 to-blue-50 shadow-lg'
                                        : ''
                                "
                            >
                                <div
                                    class="overflow-hidden transition-all duration-700"
                                    :class="
                                        showVisibilityScore
                                            ? 'max-h-20 opacity-100 scale-100'
                                            : 'max-h-0 opacity-0 scale-95'
                                    "
                                >
                                    <div
                                        class="flex items-center justify-between text-xs mb-3 animate-pulse-once"
                                    >
                                        <div class="flex items-center gap-4">
                                            <span class="text-gray-500"
                                                >Visibility Score:</span
                                            >
                                            <span
                                                class="font-bold text-green-600 text-base transition-all duration-500"
                                                :class="
                                                    showVisibilityScore
                                                        ? 'scale-110'
                                                        : 'scale-100'
                                                "
                                            >
                                                89% for "HubSpot"
                                            </span>
                                        </div>
                                        <div class="flex items-center gap-2">
                                            <div
                                                class="w-2 h-2 bg-blue-600 rounded-full animate-ping"
                                            ></div>
                                            <span class="text-gray-500"
                                                >#1 mentioned brand</span
                                            >
                                        </div>
                                    </div>
                                </div>

                                <!-- Website Audit Input -->
                                <div class="border-t border-gray-100 pt-3">
                                    <p class="text-xs text-gray-600 mb-2">
                                        I want a free audit for my website's AI
                                        visibility and opportunities.
                                    </p>
                                    <div class="flex gap-2">
                                        <input
                                            v-model="websiteUrl"
                                            type="url"
                                            placeholder="Enter your website URL"
                                            class="flex-1 px-3 py-1.5 text-sm border border-gray-200 rounded-lg focus:outline-none focus:border-blue-500"
                                            @keyup.enter="handleSubmitWebsite"
                                        />
                                        <button
                                            v-if="websiteUrl.trim()"
                                            @click="handleSubmitWebsite"
                                            class="px-4 py-1.5 bg-blue-600 text-white text-sm rounded-lg hover:bg-blue-700 transition-colors"
                                        >
                                            Next Step →
                                        </button>
                                    </div>
                                </div>
                            </div>
                        </div>

                        <!-- Contact Modal -->
                        <div
                            v-if="showModal"
                            class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50"
                            @click.self="showModal = false"
                        >
                            <div
                                class="bg-white rounded-2xl p-6 max-w-md w-full mx-4"
                            >
                                <h3 class="text-xl font-bold mb-4">
                                    Complete Your Free AEO Audit Request
                                </h3>
                                <p
                                    class="text-sm text-gray-600 mb-4 flex items-center gap-2"
                                >
                                    <span>We'll analyze</span>
                                    <span
                                        class="font-bold flex items-center gap-1"
                                    >
                                        <img
                                            :src="`https://www.google.com/s2/favicons?domain=${websiteUrl}&sz=32`"
                                            :alt="websiteUrl"
                                            class="w-4 h-4"
                                            @error="
                                                (e) =>
                                                    (e.target.style.display =
                                                        'none')
                                            "
                                        />
                                        {{ websiteUrl }}
                                    </span>
                                    <span>and send you a detailed report.</span>
                                </p>

                                <div class="space-y-4">
                                    <div>
                                        <label
                                            class="block text-sm font-medium text-gray-700 mb-1"
                                            >Full Name</label
                                        >
                                        <input
                                            v-model="contactForm.name"
                                            type="text"
                                            class="w-full px-3 py-2 border border-gray-200 rounded-lg focus:outline-none focus:border-blue-500"
                                            placeholder="John Doe"
                                        />
                                    </div>

                                    <div>
                                        <label
                                            class="block text-sm font-medium text-gray-700 mb-1"
                                            >Email</label
                                        >
                                        <input
                                            v-model="contactForm.email"
                                            type="email"
                                            class="w-full px-3 py-2 border border-gray-200 rounded-lg focus:outline-none focus:border-blue-500"
                                            placeholder="john@company.com"
                                        />
                                    </div>

                                    <div>
                                        <label
                                            class="block text-sm font-medium text-gray-700 mb-1"
                                            >Phone Number</label
                                        >
                                        <input
                                            v-model="contactForm.phone"
                                            type="tel"
                                            class="w-full px-3 py-2 border border-gray-200 rounded-lg focus:outline-none focus:border-blue-500"
                                            placeholder="+1 (555) 123-4567"
                                        />
                                    </div>
                                </div>

                                <div class="flex gap-3 mt-6">
                                    <button
                                        @click="showModal = false"
                                        class="flex-1 px-4 py-2 border border-gray-200 text-gray-700 rounded-lg hover:bg-gray-50 transition-colors"
                                    >
                                        Cancel
                                    </button>
                                    <button
                                        @click="handleSubmitContact"
                                        class="flex-1 px-4 py-2 bg-blue-600 text-white rounded-lg hover:bg-blue-700 transition-colors"
                                    >
                                        Get Free Audit
                                    </button>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <!-- AI Engines Section -->
        <section class="py-16 px-8 bg-gray-50">
            <div class="max-w-7xl mx-auto">
                <p
                    class="text-center text-sm text-gray-500 uppercase tracking-wider font-semibold mb-2"
                >
                    AI Search Optimization for Every Major Platform
                </p>
                <p class="text-center text-xs text-gray-400 mb-8">
                    Specialized strategies for each AI system's unique ranking
                    factors
                </p>
                <div
                    class="flex items-center justify-center space-x-12 flex-wrap gap-y-4"
                >
                    <div class="flex items-center gap-2">
                        <img
                            src="https://www.google.com/s2/favicons?domain=openai.com&sz=32"
                            alt="ChatGPT"
                            class="w-6 h-6"
                        />
                        <span class="text-lg font-medium text-gray-700"
                            >ChatGPT</span
                        >
                    </div>
                    <div class="flex items-center gap-2">
                        <img
                            src="https://www.google.com/s2/favicons?domain=gemini.google.com&sz=32"
                            alt="Gemini"
                            class="w-6 h-6"
                        />
                        <span class="text-lg font-medium text-gray-700"
                            >Gemini</span
                        >
                    </div>
                    <div class="flex items-center gap-2">
                        <img
                            src="https://www.google.com/s2/favicons?domain=claude.ai&sz=32"
                            alt="Claude"
                            class="w-6 h-6"
                        />
                        <span class="text-lg font-medium text-gray-700"
                            >Claude</span
                        >
                    </div>
                    <div class="flex items-center gap-2">
                        <img
                            src="https://www.google.com/s2/favicons?domain=perplexity.ai&sz=32"
                            alt="Perplexity"
                            class="w-6 h-6"
                        />
                        <span class="text-lg font-medium text-gray-700"
                            >Perplexity</span
                        >
                    </div>
                    <div class="flex items-center gap-2">
                        <img
                            src="https://www.google.com/s2/favicons?domain=copilot.microsoft.com&sz=32"
                            alt="Copilot"
                            class="w-6 h-6"
                        />
                        <span class="text-lg font-medium text-gray-700"
                            >Copilot</span
                        >
                    </div>
                    <div class="flex items-center gap-2">
                        <img
                            src="https://www.google.com/s2/favicons?domain=meta.ai&sz=32"
                            alt="Meta AI"
                            class="w-6 h-6"
                        />
                        <span class="text-lg font-medium text-gray-700"
                            >Meta AI</span
                        >
                    </div>
                </div>
            </div>
        </section>

        <!-- AEO Methodology Section -->
        <section id="solutions" class="py-20 px-8 bg-white">
            <div class="max-w-7xl mx-auto">
                <div class="text-center mb-16">
                    <h2 class="text-5xl font-bold mb-6">
                        Our AEO Framework:
                        <span class="text-blue-600"
                            >How We Get You Mentioned</span
                        >
                    </h2>
                    <p class="text-xl text-gray-600 max-w-3xl mx-auto">
                        Our proven 5-step methodology transforms your brand into
                        AI's preferred recommendation
                    </p>
                </div>

                <!-- AEO Steps Grid -->
                <div class="grid md:grid-cols-5 gap-4 mb-16">
                    <div
                        class="bg-gray-50 rounded-2xl p-6 hover:shadow-lg transition-shadow"
                    >
                        <div class="flex items-center mb-4">
                            <div
                                class="w-10 h-10 bg-blue-600 rounded-full flex items-center justify-center mr-3"
                            >
                                <span class="text-white font-bold">1</span>
                            </div>
                            <h3 class="text-base font-bold">Research</h3>
                        </div>
                        <p class="text-gray-600 text-sm leading-relaxed">
                            Identify high-value conversations your audience has
                            with AI tools.
                        </p>
                    </div>

                    <div
                        class="bg-gray-50 rounded-2xl p-6 hover:shadow-lg transition-shadow"
                    >
                        <div class="flex items-center mb-4">
                            <div
                                class="w-10 h-10 bg-blue-600 rounded-full flex items-center justify-center mr-3"
                            >
                                <span class="text-white font-bold">2</span>
                            </div>
                            <h3 class="text-base font-bold">Create Content</h3>
                        </div>
                        <p class="text-gray-600 text-sm leading-relaxed">
                            Build content clusters on your website specifically
                            designed to be cited by AI.
                        </p>
                    </div>

                    <div
                        class="bg-gray-50 rounded-2xl p-6 hover:shadow-lg transition-shadow"
                    >
                        <div class="flex items-center mb-4">
                            <div
                                class="w-10 h-10 bg-blue-600 rounded-full flex items-center justify-center mr-3"
                            >
                                <span class="text-white font-bold">3</span>
                            </div>
                            <h3 class="text-base font-bold">
                                External Placement
                            </h3>
                        </div>
                        <p class="text-gray-600 text-sm leading-relaxed">
                            Digital PR, listicles, and directory listings that
                            AI systems prioritize.
                        </p>
                    </div>

                    <div
                        class="bg-gray-50 rounded-2xl p-6 hover:shadow-lg transition-shadow"
                    >
                        <div class="flex items-center mb-4">
                            <div
                                class="w-10 h-10 bg-blue-600 rounded-full flex items-center justify-center mr-3"
                            >
                                <span class="text-white font-bold">4</span>
                            </div>
                            <h3 class="text-base font-bold">
                                Community Signals
                            </h3>
                        </div>
                        <p class="text-gray-600 text-sm leading-relaxed">
                            Build presence in specialized forums and platforms
                            AI uses for validation.
                        </p>
                    </div>

                    <div
                        class="bg-gray-50 rounded-2xl p-6 hover:shadow-lg transition-shadow"
                    >
                        <div class="flex items-center mb-4">
                            <div
                                class="w-10 h-10 bg-blue-600 rounded-full flex items-center justify-center mr-3"
                            >
                                <span class="text-white font-bold">5</span>
                            </div>
                            <h3 class="text-base font-bold">
                                Track & Optimize
                            </h3>
                        </div>
                        <p class="text-gray-600 text-sm leading-relaxed">
                            Monitor AI mentions and refine strategy based on
                            performance data.
                        </p>
                    </div>
                </div>

                <!-- Visibility Score Metrics -->
                <div class="mb-16">
                    <h3 class="text-3xl font-bold mb-8 text-center">
                        Your
                        <span class="text-blue-600">Visibility Score</span>
                        Across AI Platforms
                    </h3>
                    <div class="bg-gray-50 rounded-2xl p-8">
                        <div class="grid md:grid-cols-4 gap-6 mb-8">
                            <div class="text-center">
                                <div
                                    class="text-4xl font-bold text-blue-600 mb-2"
                                >
                                    87%
                                </div>
                                <div class="text-sm text-gray-600">ChatGPT</div>
                                <div class="text-xs text-gray-500">
                                    +23% MoM
                                </div>
                            </div>
                            <div class="text-center">
                                <div
                                    class="text-4xl font-bold text-purple-600 mb-2"
                                >
                                    92%
                                </div>
                                <div class="text-sm text-gray-600">
                                    Perplexity
                                </div>
                                <div class="text-xs text-gray-500">
                                    +18% MoM
                                </div>
                            </div>
                            <div class="text-center">
                                <div
                                    class="text-4xl font-bold text-blue-600 mb-2"
                                >
                                    79%
                                </div>
                                <div class="text-sm text-gray-600">Claude</div>
                                <div class="text-xs text-gray-500">
                                    +31% MoM
                                </div>
                            </div>
                            <div class="text-center">
                                <div
                                    class="text-4xl font-bold text-purple-600 mb-2"
                                >
                                    84%
                                </div>
                                <div class="text-sm text-gray-600">Gemini</div>
                                <div class="text-xs text-gray-500">
                                    +27% MoM
                                </div>
                            </div>
                        </div>
                        <p class="text-center text-gray-600">
                            <strong>Visibility Score:</strong> The percentage of
                            relevant AI-generated answers that mention your
                            brand
                        </p>
                    </div>
                </div>

                <!-- AEO Services -->
                <div>
                    <h3
                        class="text-2xl font-bold mb-8 text-transparent bg-clip-text bg-gradient-to-r from-purple-400 to-blue-400"
                    >
                        Comprehensive AEO Services
                    </h3>
                    <div class="grid md:grid-cols-2 lg:grid-cols-4 gap-6">
                        <div
                            class="bg-white/70 backdrop-blur-lg backdrop-saturate-150 rounded-2xl border border-gray-200/50 p-6 hover:border-purple-500/30 hover:bg-white/90 transition-all duration-500 hover:shadow-lg hover:shadow-purple-500/10 hover:-translate-y-1"
                        >
                            <NewspaperIcon
                                class="h-10 w-10 text-purple-500 mb-3"
                            />
                            <h4 class="text-lg font-bold mb-2">Digital PR</h4>
                            <p class="text-sm text-gray-600">
                                Strategic media placements that provide
                                authority signals AI systems trust
                            </p>
                        </div>

                        <div
                            class="bg-white/70 backdrop-blur-lg backdrop-saturate-150 rounded-2xl border border-gray-200/50 p-6 hover:border-blue-500/30 hover:bg-white/90 transition-all duration-500 hover:shadow-lg hover:shadow-blue-500/10 hover:-translate-y-1"
                        >
                            <DocumentTextIcon
                                class="h-10 w-10 text-blue-500 mb-3"
                            />
                            <h4 class="text-lg font-bold mb-2">
                                Strategic Listicles
                            </h4>
                            <p class="text-sm text-gray-600">
                                Curated placements in high-value comparison and
                                recommendation content
                            </p>
                        </div>

                        <div
                            class="bg-white/70 backdrop-blur-lg backdrop-saturate-150 rounded-2xl border border-gray-200/50 p-6 hover:border-purple-500/30 hover:bg-white/90 transition-all duration-500 hover:shadow-lg hover:shadow-purple-500/10 hover:-translate-y-1"
                        >
                            <ChartBarIcon
                                class="h-10 w-10 text-purple-500 mb-3"
                            />
                            <h4 class="text-lg font-bold mb-2">
                                Directory Optimization
                            </h4>
                            <p class="text-sm text-gray-600">
                                Comprehensive presence across industry-specific
                                and general directories
                            </p>
                        </div>

                        <div
                            class="bg-white/70 backdrop-blur-lg backdrop-saturate-150 rounded-2xl border border-gray-200/50 p-6 hover:border-blue-500/30 hover:bg-white/90 transition-all duration-500 hover:shadow-lg hover:shadow-blue-500/10 hover:-translate-y-1"
                        >
                            <UserGroupIcon
                                class="h-10 w-10 text-blue-500 mb-3"
                            />
                            <h4 class="text-lg font-bold mb-2">
                                Community Posting
                            </h4>
                            <p class="text-sm text-gray-600">
                                Authentic engagement in relevant communities and
                                forums
                            </p>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <!-- AI Approach Section -->
        <section
            id="approach"
            class="py-20 px-6 border-t border-gray-200 bg-gradient-to-br from-white to-gray-50"
        >
            <div class="max-w-7xl mx-auto">
                <div class="grid lg:grid-cols-2 gap-12 items-center">
                    <div>
                        <h2 class="text-4xl lg:text-5xl font-bold mb-6">
                            Built for the
                            <span class="text-blue-600">AI Era</span>
                        </h2>
                        <p class="text-xl text-gray-600 mb-8">
                            Search has evolved. AI models now mediate between
                            users and information. Our strategies ensure your
                            content is structured, validated, and positioned to
                            be the authoritative source AI systems reference.
                        </p>
                        <div class="space-y-4">
                            <div class="flex items-start">
                                <div
                                    class="flex-shrink-0 w-12 h-12 bg-blue-100 rounded-lg flex items-center justify-center mr-4"
                                >
                                    <SparklesIcon
                                        class="h-6 w-6 text-blue-600"
                                    />
                                </div>
                                <div>
                                    <h3 class="font-bold mb-1">
                                        Entity-First Architecture
                                    </h3>
                                    <p class="text-gray-600 text-sm">
                                        Content structured around entities and
                                        relationships that AI models understand
                                    </p>
                                </div>
                            </div>
                            <div class="flex items-start">
                                <div
                                    class="flex-shrink-0 w-12 h-12 bg-blue-100 rounded-lg flex items-center justify-center mr-4"
                                >
                                    <ChartBarIcon
                                        class="h-6 w-6 text-blue-600"
                                    />
                                </div>
                                <div>
                                    <h3 class="font-bold mb-1">
                                        Predictive Optimization
                                    </h3>
                                    <p class="text-gray-600 text-sm">
                                        Anticipate algorithm changes and adapt
                                        strategies proactively
                                    </p>
                                </div>
                            </div>
                            <div class="flex items-start">
                                <div
                                    class="flex-shrink-0 w-12 h-12 bg-blue-100 rounded-lg flex items-center justify-center mr-4"
                                >
                                    <MagnifyingGlassIcon
                                        class="h-6 w-6 text-blue-600"
                                    />
                                </div>
                                <div>
                                    <h3 class="font-bold mb-1">
                                        Multi-Model Coverage
                                    </h3>
                                    <p class="text-gray-600 text-sm">
                                        Optimized for GPT, Claude, Gemini, and
                                        emerging AI search systems
                                    </p>
                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="relative">
                        <div
                            class="bg-white rounded-2xl border border-gray-200 p-8 shadow-lg"
                        >
                            <h3
                                class="text-lg font-bold mb-6 text-center text-gray-900"
                            >
                                What We Track
                            </h3>
                            <div class="space-y-6">
                                <div>
                                    <h4
                                        class="font-semibold text-gray-900 mb-3"
                                    >
                                        Monthly AI Mentions
                                    </h4>
                                    <p class="text-sm text-gray-600 mb-2">
                                        How often your brand appears in AI
                                        responses
                                    </p>
                                    <div
                                        class="text-2xl font-bold text-blue-600"
                                    >
                                        2,847 mentions
                                    </div>
                                </div>
                                <div>
                                    <h4
                                        class="font-semibold text-gray-900 mb-3"
                                    >
                                        Top Conversation Topics
                                    </h4>
                                    <div class="space-y-2">
                                        <div
                                            class="flex items-center justify-between text-sm"
                                        >
                                            <span class="text-gray-600"
                                                >Product comparisons</span
                                            >
                                            <span class="font-medium">34%</span>
                                        </div>
                                        <div
                                            class="flex items-center justify-between text-sm"
                                        >
                                            <span class="text-gray-600"
                                                >Industry solutions</span
                                            >
                                            <span class="font-medium">28%</span>
                                        </div>
                                        <div
                                            class="flex items-center justify-between text-sm"
                                        >
                                            <span class="text-gray-600"
                                                >Best practices</span
                                            >
                                            <span class="font-medium">22%</span>
                                        </div>
                                    </div>
                                </div>
                                <div>
                                    <h4
                                        class="font-semibold text-gray-900 mb-3"
                                    >
                                        Platform Coverage
                                    </h4>
                                    <p class="text-sm text-gray-600">
                                        ChatGPT, Claude, Gemini, Perplexity
                                    </p>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <!-- 6-Month Implementation Timeline -->
        <section
            id="process"
            class="py-20 px-6 bg-white border-t border-gray-100"
        >
            <div class="max-w-7xl mx-auto">
                <div class="text-center mb-16">
                    <h2 class="text-4xl lg:text-5xl font-bold mb-6">
                        Your 6-Month
                        <span class="text-blue-600"
                            >Implementation Roadmap</span
                        >
                    </h2>
                    <p class="text-xl text-gray-600 max-w-3xl mx-auto">
                        A systematic approach to achieving AI visibility
                        leadership
                    </p>
                </div>

                <!-- Simplified Progress Visualization -->
                <div class="mb-12 bg-gray-50 rounded-2xl p-8">
                    <div class="flex items-center justify-between mb-4">
                        <h3 class="text-lg font-bold">
                            Expected Visibility Growth
                        </h3>
                        <span class="text-sm text-gray-500"
                            >From 12% to 94% in 6 months</span
                        >
                    </div>
                    <div class="h-48">
                        <canvas ref="visibilityChart"></canvas>
                    </div>
                </div>

                <!-- Timeline Progress Bar -->
                <div class="relative mb-16">
                    <!-- Progress Line -->
                    <div
                        class="absolute top-8 left-0 right-0 h-1 bg-gray-200 rounded-full hidden md:block"
                    >
                        <div
                            class="absolute inset-y-0 left-0 w-5/6 bg-gradient-to-r from-blue-600 to-blue-400 rounded-full"
                        ></div>
                    </div>

                    <!-- Timeline Points -->
                    <div class="relative grid grid-cols-6 gap-0 mb-8">
                        <div
                            v-for="month in 6"
                            :key="month"
                            class="relative flex flex-col items-center"
                        >
                            <div
                                class="w-16 h-16 bg-white border-4 rounded-full flex items-center justify-center font-bold text-sm z-10 cursor-pointer transition-all duration-300 hover:scale-110"
                                :class="
                                    month <= 6
                                        ? 'border-blue-600 text-blue-600 hover:bg-blue-50'
                                        : 'border-gray-300 text-gray-400'
                                "
                                @mouseenter="hoveredMonth = month"
                                @mouseleave="hoveredMonth = null"
                            >
                                M{{ month }}
                            </div>
                            <span class="text-xs text-gray-500 mt-2"
                                >Month {{ month }}</span
                            >
                        </div>
                    </div>
                </div>

                <!-- 6-Month Timeline Cards -->
                <div class="grid md:grid-cols-3 gap-6 mb-8">
                    <!-- Month 1 -->
                    <div
                        class="relative bg-white rounded-xl border-2 p-6 transition-all duration-300"
                        :class="
                            hoveredMonth === 1
                                ? 'border-blue-500 shadow-xl scale-105 bg-blue-50'
                                : 'border-gray-200 hover:border-blue-500 hover:shadow-lg'
                        "
                    >
                        <!-- Connection line to timeline -->
                        <div
                            class="absolute -top-px left-1/2 w-px h-4 bg-blue-600 hidden md:block"
                        ></div>
                        <div class="mb-4">
                            <div class="flex items-center gap-3">
                                <div class="flex items-center">
                                    <div
                                        class="w-10 h-10 bg-gradient-to-br from-blue-500 to-blue-600 text-white rounded-lg flex items-center justify-center shadow-sm"
                                    >
                                        <span class="text-lg font-semibold"
                                            >1</span
                                        >
                                    </div>
                                    <div class="ml-3">
                                        <p
                                            class="text-xs text-gray-400 uppercase tracking-wider"
                                        >
                                            Month One
                                        </p>
                                        <p
                                            class="text-sm font-medium text-gray-700"
                                        >
                                            Discovery Phase
                                        </p>
                                    </div>
                                </div>
                            </div>
                        </div>
                        <h3 class="text-lg font-bold mb-3 text-gray-900">
                            AI Landscape Audit
                        </h3>
                        <div
                            class="h-24 mb-4 flex items-center justify-center bg-gray-50 rounded-lg"
                        >
                            <div class="text-center">
                                <div class="text-2xl font-bold text-blue-600">
                                    500+
                                </div>
                                <div class="text-xs text-gray-500">
                                    Queries Analyzed
                                </div>
                            </div>
                        </div>
                        <div class="space-y-2">
                            <div class="flex items-start">
                                <div
                                    class="w-1.5 h-1.5 bg-blue-600 rounded-full mt-1.5 mr-2.5 flex-shrink-0"
                                ></div>
                                <div>
                                    <p
                                        class="text-sm font-medium text-gray-900"
                                    >
                                        Competitive Intelligence
                                    </p>
                                    <p class="text-xs text-gray-500">
                                        Map competitor AI visibility
                                    </p>
                                </div>
                            </div>
                            <div class="flex items-start">
                                <div
                                    class="w-1.5 h-1.5 bg-blue-600 rounded-full mt-1.5 mr-2.5 flex-shrink-0"
                                ></div>
                                <div>
                                    <p
                                        class="text-sm font-medium text-gray-900"
                                    >
                                        Query Mapping
                                    </p>
                                    <p class="text-xs text-gray-500">
                                        Identify 500+ high-value queries
                                    </p>
                                </div>
                            </div>
                        </div>
                        <div class="mt-4 pt-4 border-t border-gray-100">
                            <div class="flex items-center justify-between">
                                <span class="text-xs text-gray-500"
                                    >Baseline Score</span
                                >
                                <span class="text-sm font-bold text-gray-900"
                                    >Established</span
                                >
                            </div>
                        </div>
                    </div>

                    <!-- Month 2 -->
                    <div
                        class="relative bg-white rounded-xl border-2 p-6 transition-all duration-300"
                        :class="
                            hoveredMonth === 2
                                ? 'border-blue-500 shadow-xl scale-105 bg-blue-50'
                                : 'border-gray-200 hover:border-blue-500 hover:shadow-lg'
                        "
                    >
                        <!-- Connection line to timeline -->
                        <div
                            class="absolute -top-px left-1/2 w-px h-4 bg-blue-600 hidden md:block"
                        ></div>
                        <div class="mb-4">
                            <div class="flex items-center gap-3">
                                <div class="flex items-center">
                                    <div
                                        class="w-10 h-10 bg-gradient-to-br from-blue-500 to-blue-600 text-white rounded-lg flex items-center justify-center shadow-sm"
                                    >
                                        <span class="text-lg font-semibold"
                                            >2</span
                                        >
                                    </div>
                                    <div class="ml-3">
                                        <p
                                            class="text-xs text-gray-400 uppercase tracking-wider"
                                        >
                                            Month Two
                                        </p>
                                        <p
                                            class="text-sm font-medium text-gray-700"
                                        >
                                            Foundation Phase
                                        </p>
                                    </div>
                                </div>
                            </div>
                        </div>
                        <h3 class="text-lg font-bold mb-3 text-gray-900">
                            Entity Architecture
                        </h3>
                        <div
                            class="h-24 mb-4 flex items-center justify-center bg-gray-50 rounded-lg"
                        >
                            <div class="text-center">
                                <div class="text-2xl font-bold text-blue-600">
                                    +18%
                                </div>
                                <div class="text-xs text-gray-500">
                                    Visibility Gain
                                </div>
                            </div>
                        </div>
                        <div class="space-y-2">
                            <div class="flex items-start">
                                <div
                                    class="w-1.5 h-1.5 bg-blue-600 rounded-full mt-1.5 mr-2.5 flex-shrink-0"
                                ></div>
                                <div>
                                    <p
                                        class="text-sm font-medium text-gray-900"
                                    >
                                        Knowledge Graph Build
                                    </p>
                                    <p class="text-xs text-gray-500">
                                        Structure entity relationships
                                    </p>
                                </div>
                            </div>
                            <div class="flex items-start">
                                <div
                                    class="w-1.5 h-1.5 bg-blue-600 rounded-full mt-1.5 mr-2.5 flex-shrink-0"
                                ></div>
                                <div>
                                    <p
                                        class="text-sm font-medium text-gray-900"
                                    >
                                        Content Optimization
                                    </p>
                                    <p class="text-xs text-gray-500">
                                        AI-friendly content structure
                                    </p>
                                </div>
                            </div>
                        </div>
                        <div class="mt-4 pt-4 border-t border-gray-100">
                            <div class="flex items-center justify-between">
                                <span class="text-xs text-gray-500"
                                    >Cumulative Score</span
                                >
                                <span class="text-sm font-bold text-blue-600"
                                    >30%</span
                                >
                            </div>
                        </div>
                    </div>

                    <!-- Month 3 -->
                    <div
                        class="relative bg-white rounded-xl border-2 p-6 transition-all duration-300"
                        :class="
                            hoveredMonth === 3
                                ? 'border-blue-500 shadow-xl scale-105 bg-blue-50'
                                : 'border-gray-200 hover:border-blue-500 hover:shadow-lg'
                        "
                    >
                        <!-- Connection line to timeline -->
                        <div
                            class="absolute -top-px left-1/2 w-px h-4 bg-blue-600 hidden md:block"
                        ></div>
                        <div class="mb-4">
                            <div class="flex items-center gap-3">
                                <div class="flex items-center">
                                    <div
                                        class="w-10 h-10 bg-gradient-to-br from-blue-500 to-blue-600 text-white rounded-lg flex items-center justify-center shadow-sm"
                                    >
                                        <span class="text-lg font-semibold"
                                            >3</span
                                        >
                                    </div>
                                    <div class="ml-3">
                                        <p
                                            class="text-xs text-gray-400 uppercase tracking-wider"
                                        >
                                            Month Three
                                        </p>
                                        <p
                                            class="text-sm font-medium text-gray-700"
                                        >
                                            Authority Phase
                                        </p>
                                    </div>
                                </div>
                            </div>
                        </div>
                        <h3 class="text-lg font-bold mb-3 text-gray-900">
                            Authority Signals
                        </h3>
                        <div
                            class="h-24 mb-4 flex items-center justify-center bg-gray-50 rounded-lg"
                        >
                            <div class="text-center">
                                <div class="text-2xl font-bold text-blue-600">
                                    +35%
                                </div>
                                <div class="text-xs text-gray-500">
                                    Visibility Gain
                                </div>
                            </div>
                        </div>
                        <div class="space-y-2">
                            <div class="flex items-start">
                                <div
                                    class="w-1.5 h-1.5 bg-blue-600 rounded-full mt-1.5 mr-2.5 flex-shrink-0"
                                ></div>
                                <div>
                                    <p
                                        class="text-sm font-medium text-gray-900"
                                    >
                                        Digital PR Campaign
                                    </p>
                                    <p class="text-xs text-gray-500">
                                        Launch targeted media outreach
                                    </p>
                                </div>
                            </div>
                            <div class="flex items-start">
                                <div
                                    class="w-1.5 h-1.5 bg-blue-600 rounded-full mt-1.5 mr-2.5 flex-shrink-0"
                                ></div>
                                <div>
                                    <p
                                        class="text-sm font-medium text-gray-900"
                                    >
                                        Strategic Listicles
                                    </p>
                                    <p class="text-xs text-gray-500">
                                        Placement in comparison content
                                    </p>
                                </div>
                            </div>
                        </div>
                        <div class="mt-4 pt-4 border-t border-gray-100">
                            <div class="flex items-center justify-between">
                                <span class="text-xs text-gray-500"
                                    >Cumulative Score</span
                                >
                                <span class="text-sm font-bold text-blue-600"
                                    >52%</span
                                >
                            </div>
                        </div>
                    </div>
                </div>

                <!-- Second Row: Months 4-6 -->
                <div class="grid md:grid-cols-3 gap-6">
                    <!-- Month 4 -->
                    <div
                        class="relative bg-white rounded-xl border-2 p-6 transition-all duration-300"
                        :class="
                            hoveredMonth === 4
                                ? 'border-blue-500 shadow-xl scale-105 bg-blue-50'
                                : 'border-gray-200 hover:border-blue-500 hover:shadow-lg'
                        "
                    >
                        <!-- Connection line to timeline -->
                        <div
                            class="absolute -top-px left-1/2 w-px h-4 bg-blue-600 hidden md:block"
                        ></div>
                        <div class="mb-4">
                            <div class="flex items-center gap-3">
                                <div class="flex items-center">
                                    <div
                                        class="w-10 h-10 bg-gradient-to-br from-blue-500 to-blue-600 text-white rounded-lg flex items-center justify-center shadow-sm"
                                    >
                                        <span class="text-lg font-semibold"
                                            >4</span
                                        >
                                    </div>
                                    <div class="ml-3">
                                        <p
                                            class="text-xs text-gray-400 uppercase tracking-wider"
                                        >
                                            Month Four
                                        </p>
                                        <p
                                            class="text-sm font-medium text-gray-700"
                                        >
                                            Scale Phase
                                        </p>
                                    </div>
                                </div>
                            </div>
                        </div>
                        <h3 class="text-lg font-bold mb-3 text-gray-900">
                            Rapid Scaling
                        </h3>
                        <div
                            class="h-24 mb-4 flex items-center justify-center bg-gray-50 rounded-lg"
                        >
                            <div class="text-center">
                                <div class="text-2xl font-bold text-blue-600">
                                    +16%
                                </div>
                                <div class="text-xs text-gray-500">
                                    Visibility Gain
                                </div>
                            </div>
                        </div>
                        <div class="space-y-2">
                            <div class="flex items-start">
                                <div
                                    class="w-1.5 h-1.5 bg-blue-600 rounded-full mt-1.5 mr-2.5 flex-shrink-0"
                                ></div>
                                <div>
                                    <p
                                        class="text-sm font-medium text-gray-900"
                                    >
                                        Multi-Platform Push
                                    </p>
                                    <p class="text-xs text-gray-500">
                                        Expand to all major AI systems
                                    </p>
                                </div>
                            </div>
                            <div class="flex items-start">
                                <div
                                    class="w-1.5 h-1.5 bg-blue-600 rounded-full mt-1.5 mr-2.5 flex-shrink-0"
                                ></div>
                                <div>
                                    <p
                                        class="text-sm font-medium text-gray-900"
                                    >
                                        Community Presence
                                    </p>
                                    <p class="text-xs text-gray-500">
                                        Active in 50+ relevant forums
                                    </p>
                                </div>
                            </div>
                        </div>
                        <div class="mt-4 pt-4 border-t border-gray-100">
                            <div class="flex items-center justify-between">
                                <span class="text-xs text-gray-500"
                                    >Cumulative Score</span
                                >
                                <span class="text-sm font-bold text-blue-600"
                                    >68%</span
                                >
                            </div>
                        </div>
                    </div>

                    <!-- Month 5 -->
                    <div
                        class="relative bg-white rounded-xl border-2 p-6 transition-all duration-300"
                        :class="
                            hoveredMonth === 5
                                ? 'border-blue-500 shadow-xl scale-105 bg-blue-50'
                                : 'border-gray-200 hover:border-blue-500 hover:shadow-lg'
                        "
                    >
                        <!-- Connection line to timeline -->
                        <div
                            class="absolute -top-px left-1/2 w-px h-4 bg-blue-600 hidden md:block"
                        ></div>
                        <div class="mb-4">
                            <div class="flex items-center gap-3">
                                <div class="flex items-center">
                                    <div
                                        class="w-10 h-10 bg-gradient-to-br from-blue-500 to-blue-600 text-white rounded-lg flex items-center justify-center shadow-sm"
                                    >
                                        <span class="text-lg font-semibold"
                                            >5</span
                                        >
                                    </div>
                                    <div class="ml-3">
                                        <p
                                            class="text-xs text-gray-400 uppercase tracking-wider"
                                        >
                                            Month Five
                                        </p>
                                        <p
                                            class="text-sm font-medium text-gray-700"
                                        >
                                            Optimize Phase
                                        </p>
                                    </div>
                                </div>
                            </div>
                        </div>
                        <h3 class="text-lg font-bold mb-3 text-gray-900">
                            Fine-Tuning
                        </h3>
                        <div
                            class="h-24 mb-4 flex items-center justify-center bg-gray-50 rounded-lg"
                        >
                            <div class="text-center">
                                <div class="text-2xl font-bold text-blue-600">
                                    +13%
                                </div>
                                <div class="text-xs text-gray-500">
                                    Visibility Gain
                                </div>
                            </div>
                        </div>
                        <div class="space-y-2">
                            <div class="flex items-start">
                                <div
                                    class="w-1.5 h-1.5 bg-blue-600 rounded-full mt-1.5 mr-2.5 flex-shrink-0"
                                ></div>
                                <div>
                                    <p
                                        class="text-sm font-medium text-gray-900"
                                    >
                                        Performance Analysis
                                    </p>
                                    <p class="text-xs text-gray-500">
                                        Data-driven optimization
                                    </p>
                                </div>
                            </div>
                            <div class="flex items-start">
                                <div
                                    class="w-1.5 h-1.5 bg-blue-600 rounded-full mt-1.5 mr-2.5 flex-shrink-0"
                                ></div>
                                <div>
                                    <p
                                        class="text-sm font-medium text-gray-900"
                                    >
                                        Gap Filling
                                    </p>
                                    <p class="text-xs text-gray-500">
                                        Target missed opportunities
                                    </p>
                                </div>
                            </div>
                        </div>
                        <div class="mt-4 pt-4 border-t border-gray-100">
                            <div class="flex items-center justify-between">
                                <span class="text-xs text-gray-500"
                                    >Cumulative Score</span
                                >
                                <span class="text-sm font-bold text-blue-600"
                                    >81%</span
                                >
                            </div>
                        </div>
                    </div>

                    <!-- Month 6 -->
                    <div
                        class="relative bg-white rounded-xl border-2 p-6 transition-all duration-300"
                        :class="
                            hoveredMonth === 6
                                ? 'border-blue-500 shadow-xl scale-105 bg-blue-50'
                                : 'border-gray-200 hover:border-blue-500 hover:shadow-lg'
                        "
                    >
                        <!-- Connection line to timeline -->
                        <div
                            class="absolute -top-px left-1/2 w-px h-4 bg-blue-600 hidden md:block"
                        ></div>
                        <div class="mb-4">
                            <div class="flex items-center gap-3">
                                <div class="flex items-center">
                                    <div
                                        class="w-10 h-10 bg-gradient-to-br from-blue-500 to-blue-600 text-white rounded-lg flex items-center justify-center shadow-sm"
                                    >
                                        <span class="text-lg font-semibold"
                                            >6</span
                                        >
                                    </div>
                                    <div class="ml-3">
                                        <p
                                            class="text-xs text-gray-400 uppercase tracking-wider"
                                        >
                                            Month Six
                                        </p>
                                        <p
                                            class="text-sm font-medium text-gray-700"
                                        >
                                            Leadership Phase
                                        </p>
                                    </div>
                                </div>
                            </div>
                        </div>
                        <h3 class="text-lg font-bold mb-3 text-gray-900">
                            Market Leadership
                        </h3>
                        <div
                            class="h-24 mb-4 flex items-center justify-center bg-gray-50 rounded-lg"
                        >
                            <div class="text-center">
                                <div class="text-2xl font-bold text-blue-600">
                                    94%
                                </div>
                                <div class="text-xs text-gray-500">
                                    Final Score
                                </div>
                            </div>
                        </div>
                        <div class="space-y-2">
                            <div class="flex items-start">
                                <div
                                    class="w-1.5 h-1.5 bg-blue-600 rounded-full mt-1.5 mr-2.5 flex-shrink-0"
                                ></div>
                                <div>
                                    <p
                                        class="text-sm font-medium text-gray-900"
                                    >
                                        Category Authority
                                    </p>
                                    <p class="text-xs text-gray-500">
                                        Recognized as industry leader
                                    </p>
                                </div>
                            </div>
                            <div class="flex items-start">
                                <div
                                    class="w-1.5 h-1.5 bg-blue-600 rounded-full mt-1.5 mr-2.5 flex-shrink-0"
                                ></div>
                                <div>
                                    <p
                                        class="text-sm font-medium text-gray-900"
                                    >
                                        Continuous Growth
                                    </p>
                                    <p class="text-xs text-gray-500">
                                        Self-reinforcing visibility
                                    </p>
                                </div>
                            </div>
                        </div>
                        <div class="mt-4 pt-4 border-t border-gray-100">
                            <div class="flex items-center justify-between">
                                <span class="text-xs text-gray-500"
                                    >Achievement</span
                                >
                                <span class="text-sm font-bold text-blue-600"
                                    >Market Leader</span
                                >
                            </div>
                        </div>
                    </div>
                </div>

                <!-- Key Metrics -->
                <div class="mt-12 bg-gray-50 rounded-2xl p-8">
                    <h3 class="text-xl font-bold mb-6 text-gray-900">
                        Expected Outcomes
                    </h3>
                    <div class="grid md:grid-cols-4 gap-6">
                        <div class="text-center">
                            <div class="text-3xl font-bold mb-2 text-blue-600">
                                8x
                            </div>
                            <p class="text-sm text-gray-600">
                                Increase in AI mentions
                            </p>
                        </div>
                        <div class="text-center">
                            <div class="text-3xl font-bold mb-2 text-blue-600">
                                94%
                            </div>
                            <p class="text-sm text-gray-600">
                                Visibility score achieved
                            </p>
                        </div>
                        <div class="text-center">
                            <div class="text-3xl font-bold mb-2 text-blue-600">
                                Top 3
                            </div>
                            <p class="text-sm text-gray-600">
                                Category positioning
                            </p>
                        </div>
                        <div class="text-center">
                            <div class="text-3xl font-bold mb-2 text-blue-600">
                                All
                            </div>
                            <p class="text-sm text-gray-600">
                                Major AI platforms
                            </p>
                        </div>
                    </div>
                </div>
                <div class="flex items-start">
                    <div
                        class="w-2 h-2 bg-blue-600 rounded-full mt-1.5 mr-3 flex-shrink-0"
                    ></div>
                    <div>
                        <p class="text-sm font-medium text-gray-900">
                            Multi-Platform Coverage
                        </p>
                        <p class="text-xs text-gray-500">
                            Optimize for all AI models
                        </p>
                    </div>
                </div>
                <div class="flex items-start">
                    <div
                        class="w-2 h-2 bg-blue-600 rounded-full mt-1.5 mr-3 flex-shrink-0"
                    ></div>
                    <div>
                        <p class="text-sm font-medium text-gray-900">
                            Performance Refinement
                        </p>
                        <p class="text-xs text-gray-500">
                            Data-driven optimization
                        </p>
                    </div>
                </div>
            </div>
        </section>

        <!-- CTA Section -->
        <section id="contact" class="py-20 px-6 border-t border-gray-200">
            <div class="max-w-4xl mx-auto text-center">
                <h2 class="text-4xl lg:text-5xl font-bold mb-6">
                    Ready to Optimize for
                    <span
                        class="text-transparent bg-clip-text bg-gradient-to-r from-purple-400 to-blue-400"
                    >
                        AI-First Search?
                    </span>
                </h2>
                <p class="text-xl text-gray-600 mb-8">
                    Let's discuss how we can position your brand at the
                    forefront of AI-powered discovery.
                </p>
                <div class="flex flex-col sm:flex-row gap-4 justify-center">
                    <a
                        href="mailto:contact@aisearchlabs.com"
                        class="inline-flex items-center justify-center px-8 py-4 bg-gradient-to-r from-purple-600 to-blue-600 rounded-lg font-semibold hover:from-purple-700 hover:to-blue-700 transition"
                    >
                        Schedule a Consultation
                        <ChevronRightIcon class="ml-2 h-5 w-5" />
                    </a>
                    <a
                        href="#services"
                        class="inline-flex items-center justify-center px-8 py-4 border border-gray-700 rounded-lg font-semibold hover:border-gray-400 hover:bg-white/50 transition"
                    >
                        View Services
                    </a>
                </div>
            </div>
        </section>

        <!-- Footer -->
        <footer class="border-t border-gray-200 py-12 px-6 bg-white">
            <div class="max-w-7xl mx-auto">
                <div
                    class="flex flex-col md:flex-row items-center justify-between"
                >
                    <div class="flex items-center space-x-2 mb-4 md:mb-0">
                        <SparklesIcon class="h-6 w-6 text-blue-600" />
                        <span class="font-bold">AI Search Labs</span>
                        <span class="text-sm text-gray-500"
                            >• The AEO Experts</span
                        >
                    </div>
                    <div class="text-sm text-gray-700">
                        &copy; 2025 AI Search Labs. Pioneering Answer Engine
                        Optimization.
                    </div>
                </div>
            </div>
        </footer>
    </div>
</template>

<style scoped>
@keyframes gradient {
    0% {
        background-position: 0% 50%;
    }
    50% {
        background-position: 100% 50%;
    }
    100% {
        background-position: 0% 50%;
    }
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

/* One-time pulse animation for visibility score */
.animate-pulse-once {
    animation: pulse-once 1s ease-out;
}

@keyframes pulse-once {
    0%,
    100% {
        transform: scale(1);
    }
    50% {
        transform: scale(1.05);
    }
}
</style>
