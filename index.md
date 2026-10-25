<!DOCTYPE html>
<html lang="en" class="scroll-smooth">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Assignment 9.2 - Responsive Typography & Images</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            darkMode: 'class',
            theme: {
                extend: {
                    colors: {
                        brand: {
                            50: '#f0f7ff',
                            100: '#e0effe',
                            500: '#0284c7',
                            600: '#0369a1',
                            900: '#0c4a6e',
                        }
                    }
                }
            }
        }
    </script>
    <!-- Font Awesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <style>
        /* ==========================================================================
           ASSIGNMENT REQUIREMENT 3: CSS FLUID TYPOGRAPHY USING clamp(), rem, em
           ========================================================================== */

        :root {
            /* Base root font sizing */
            font-size: 100%; /* 16px default */
            
            /* Fluid Typographic Scales using clamp(MIN, VAL, MAX) */
            --font-display: clamp(2.25rem, 1.5rem + 3.2vw, 4rem);    /* 36px to 64px */
            --font-h1: clamp(1.75rem, 1.25rem + 2vw, 2.75rem);        /* 28px to 44px */
            --font-h2: clamp(1.35rem, 1.1rem + 1.2vw, 2rem);         /* 21.6px to 32px */
            --font-h3: clamp(1.15rem, 1.05rem + 0.6vw, 1.45rem);      /* 18.4px to 23.2px */
            --font-body: clamp(1rem, 0.95rem + 0.35vw, 1.2rem);       /* 16px to 19.2px */
            --font-small: clamp(0.85rem, 0.8rem + 0.25vw, 0.95rem);   /* 13.6px to 15.2px */
            
            /* Spacing using ems and rems */
            --spacing-container-p: clamp(1rem, 4vw, 3rem);
            --line-height-body: 1.75;
            --max-prose-width: 68ch; /* Optimal line length requirement */
        }

        /* Typography Helper Classes applying custom clamp variables */
        .text-fluid-display { font-size: var(--font-display); line-height: 1.1; }
        .text-fluid-h1 { font-size: var(--font-h1); line-height: 1.2; }
        .text-fluid-h2 { font-size: var(--font-h2); line-height: 1.3; }
        .text-fluid-h3 { font-size: var(--font-h3); line-height: 1.4; }
        .text-fluid-body { font-size: var(--font-body); line-height: var(--line-height-body); }
        .text-fluid-small { font-size: var(--font-small); line-height: 1.5; }

        /* Requirement 6: Optimal line length and prose container */
        .prose-container {
            max-width: var(--max-prose-width);
            margin-left: auto;
            margin-right: auto;
        }

        /* ==========================================================================
           ASSIGNMENT REQUIREMENT 4: RESPONSIVE MEDIA & IMAGES
           ========================================================================== */
        
        /* Rule: Prevent image stretching or overflow on all screen sizes */
        img, picture, video, svg {
            max-width: 100%;
            height: auto;
            object-fit: cover;
            display: block;
        }

        /* Aspect ratio wrappers to prevent Layout Shift */
        .aspect-hero {
            aspect-ratio: 16 / 9;
        }
        @media (min-width: 768px) {
            .aspect-hero {
                aspect-ratio: 21 / 9;
            }
        }

        .aspect-card {
            aspect-ratio: 4 / 3;
        }

        /* Inspector Highlight Styles when Unit Inspector is Active */
        body.inspector-active [data-inspect] {
            outline: 2px dashed #f59e0b !important;
            outline-offset: 2px;
            position: relative;
        }

        body.inspector-active [data-inspect]::after {
            content: attr(data-inspect-info);
            position: absolute;
            top: -24px;
            left: 0;
            background: #1e293b;
            color: #f59e0b;
            font-size: 11px;
            font-family: monospace;
            padding: 2px 6px;
            border-radius: 4px;
            z-index: 50;
            white-space: nowrap;
            box-shadow: 0 2px 8px rgba(0,0,0,0.3);
            pointer-events: none;
        }

        /* Smooth Viewport Switcher Container Frame Transitions */
        #viewport-frame-wrapper {
            transition: width 0.4s cubic-bezier(0.4, 0, 0.2, 1), margin 0.4s ease;
        }

        /* Custom Scrollbar for sleek UI */
        ::-webkit-scrollbar {
            width: 8px;
            height: 8px;
        }
        ::-webkit-scrollbar-track {
            background: rgba(0,0,0,0.05);
        }
        ::-webkit-scrollbar-thumb {
            background: rgba(156, 163, 175, 0.5);
            border-radius: 4px;
        }
    </style>
</head>
<body class="bg-slate-900 text-slate-100 antialiased min-h-screen flex flex-col font-sans transition-colors duration-200">

    <!-- ==========================================================================
         TOP INTERACTIVE CONTROL BAR (Viewport Previewer + Unit Inspector Toggle)
         ========================================================================== -->
    <header class="sticky top-0 z-50 bg-slate-950/90 backdrop-blur border-b border-slate-800 px-4 py-3 shadow-md">
        <div class="max-w-7xl mx-auto flex flex-wrap items-center justify-between gap-4">
            
            <!-- Assignment Header Badge -->
            <div class="flex items-center space-x-3">
                <span class="bg-brand-600 text-white text-xs font-bold px-2.5 py-1 rounded-md uppercase tracking-wider">Assignment 9.2</span>
                <h1 class="text-sm md:text-base font-semibold text-slate-200">Responsive Typography & Images</h1>
            </div>

            <!-- Viewport Switcher Controls (Mobile / Tablet / Desktop) -->
            <div class="flex items-center bg-slate-800/80 p-1 rounded-lg border border-slate-700 text-xs font-medium">
                <span class="text-slate-400 px-2 hidden sm:inline"><i class="fa-solid fa-desktop mr-1"></i> Viewport:</span>
                
                <button onclick="setViewport('full')" id="btn-full" class="vp-btn px-3 py-1.5 rounded-md bg-brand-600 text-white transition">
                    <i class="fa-solid fa-expand mr-1 sm:hidden"></i><span class="hidden sm:inline">Full (100%)</span>
                </button>
                <button onclick="setViewport('desktop')" id="btn-desktop" class="vp-btn px-3 py-1.5 rounded-md text-slate-300 hover:text-white transition">
                    <i class="fa-solid fa-laptop mr-1"></i><span class="hidden sm:inline">Desktop (1200px)</span>
                </button>
                <button onclick="setViewport('tablet')" id="btn-tablet" class="vp-btn px-3 py-1.5 rounded-md text-slate-300 hover:text-white transition">
                    <i class="fa-solid fa-tablet-screen-button mr-1"></i><span class="hidden sm:inline">Tablet (768px)</span>
                </button>
                <button onclick="setViewport('mobile')" id="btn-mobile" class="vp-btn px-3 py-1.5 rounded-md text-slate-300 hover:text-white transition">
                    <i class="fa-solid fa-mobile-screen mr-1"></i><span class="hidden sm:inline">Mobile (375px)</span>
                </button>
            </div>

            <!-- Utility Toggles (CSS Inspector & Dark/Light Mode) -->
            <div class="flex items-center space-x-2">
                <!-- CSS Inspector Toggle -->
                <button id="inspector-toggle" onclick="toggleInspector()" class="px-3 py-1.5 rounded-lg border border-amber-500/40 text-amber-400 bg-amber-500/10 hover:bg-amber-500/20 text-xs font-medium flex items-center transition">
                    <i class="fa-solid fa-code mr-1.5"></i>
                    <span>Inspect Units</span>
                </button>

                <!-- Light/Dark Mode Switcher -->
                <button onclick="toggleTheme()" class="p-1.5 rounded-lg border border-slate-700 text-slate-300 hover:bg-slate-800 text-xs">
                    <i id="theme-icon" class="fa-solid fa-moon"></i>
                </button>
            </div>
        </div>
    </header>

    <!-- Active Viewport Info Badge (Visible when viewport scaled) -->
    <div id="viewport-info" class="hidden bg-brand-900/60 text-brand-200 text-xs py-1 px-4 text-center border-b border-brand-800/50">
        Simulating Viewport: <span id="current-viewport-label" class="font-bold">Full Width</span> | Resizable canvas enabled for testing responsiveness.
    </div>


    <!-- ==========================================================================
         MAIN VIEWPORT CONTAINER FRAME (Can shrink for mobile/tablet simulation)
         ========================================================================== -->
    <div id="viewport-frame-wrapper" class="w-full mx-auto my-0 flex-1 flex flex-col bg-slate-900 text-slate-100 transition-all">
        <div id="app-canvas" class="w-full flex-1 flex flex-col bg-white dark:bg-slate-900 text-slate-800 dark:text-slate-100 transition-colors">
            
            <!-- ARTICLE / PORTFOLIO SITE HEADER -->
            <nav class="border-b border-slate-200 dark:border-slate-800 px-4 sm:px-8 py-4 bg-white/80 dark:bg-slate-900/80 backdrop-blur sticky top-[57px] z-40">
                <div class="max-w-7xl mx-auto flex items-center justify-between">
                    <a href="#" class="flex items-center gap-2 font-bold text-lg sm:text-xl text-slate-900 dark:text-white tracking-tight">
                        <div class="w-8 h-8 rounded-lg bg-gradient-to-tr from-brand-500 to-indigo-600 flex items-center justify-center text-white">
                            <i class="fa-solid fa-layer-group text-sm"></i>
                        </div>
                        <span>Aesthetics<span class="text-brand-500">Lab</span></span>
                    </a>
                    <div class="flex items-center space-x-4 sm:space-x-6 text-sm font-medium text-slate-600 dark:text-slate-300">
                        <a href="#article" class="hover:text-brand-500 transition">Article</a>
                        <a href="#gallery" class="hover:text-brand-500 transition">Portfolio</a>
                        <a href="#author" class="hover:text-brand-500 transition">Author</a>
                    </div>
                </div>
            </nav>

            <main class="flex-1">

                <!-- ==========================================================================
                     HERO ARTICLE SECTION
                     ========================================================================== -->
                <article id="article" class="px-4 sm:px-6 lg:px-8 py-8 sm:py-12">
                    
                    <!-- Main Outer Article Container (Requirement 5: Suitable max-width container) -->
                    <div class="max-w-4xl mx-auto">
                        
                        <!-- Header / Metadata -->
                        <header class="text-center mb-8 sm:mb-12">
                            <div class="inline-flex items-center gap-2 px-3 py-1 rounded-full bg-brand-50 dark:bg-brand-900/30 text-brand-600 dark:text-brand-400 text-xs font-semibold mb-4">
                                <i class="fa-solid fa-newspaper text-xs"></i> Responsive Web Design Guide
                            </div>
                            
                            <!-- Display Fluid Heading -->
                            <h1 class="text-fluid-display font-extrabold text-slate-900 dark:text-white tracking-tight mb-4"
                                data-inspect
                                data-inspect-info="font-size: clamp(2.25rem, 1.5rem + 3.2vw, 4rem)">
                                Mastering Responsive Typography & Media Elements
                            </h1>

                            <p class="text-fluid-body text-slate-600 dark:text-slate-400 max-w-2xl mx-auto"
                               data-inspect
                               data-inspect-info="font-size: clamp(1rem, 0.95rem + 0.35vw, 1.2rem)">
                                A comprehensive look at building robust fluid layouts using CSS <code class="text-brand-500">clamp()</code>, relative units, and constrained media rules.
                            </p>

                            <!-- Article Meta -->
                            <div class="flex items-center justify-center gap-4 mt-6 text-xs sm:text-sm text-slate-500 dark:text-slate-400 border-y border-slate-200 dark:border-slate-800 py-3">
                                <div class="flex items-center gap-2">
                                    <img src="https://images.unsplash.com/photo-1534528741775-53994a69daeb?auto=format&fit=crop&w=120&q=80" 
                                         alt="Author Avatar" 
                                         class="w-7 h-7 rounded-full object-cover">
                                    <span class="font-medium text-slate-800 dark:text-slate-200">Elena Rostova</span>
                                </div>
                                <span>•</span>
                                <span><i class="fa-regular fa-calendar mr-1"></i> Oct 24, 2026</span>
                                <span>•</span>
                                <span><i class="fa-regular fa-clock mr-1"></i> 5 min read</span>
                            </div>
                        </header>

                        <!-- HERO BANNER IMAGE (Requirement 4: max-width 100%, height auto, object-fit) -->
                        <figure class="mb-12 rounded-2xl overflow-hidden shadow-xl border border-slate-200 dark:border-slate-800 bg-slate-100 dark:bg-slate-800">
                            <div class="aspect-hero w-full relative">
                                <img src="https://images.unsplash.com/photo-1507238691740-187a5b1d37b8?auto=format&fit=crop&w=1600&q=80" 
                                     alt="Responsive Design Canvas" 
                                     class="w-full h-full object-cover"
                                     data-inspect
                                     data-inspect-info="max-width: 100%; height: auto; object-fit: cover;">
                            </div>
                            <figcaption class="p-3 text-center text-fluid-small text-slate-500 dark:text-slate-400 bg-slate-50 dark:bg-slate-800/50 border-t border-slate-200/50 dark:border-slate-800/50">
                                <i class="fa-solid fa-camera mr-1"></i> Modern responsive interfaces adapt seamlessly between varied display devices and viewports.
                            </figcaption>
                        </figure>

                        <!-- ARTICLE BODY CONTENT (Requirement 6: Optimal line length max-width ~ 68ch) -->
                        <div class="prose-container space-y-6 text-fluid-body text-slate-700 dark:text-slate-300">
                            
                            <p data-inspect data-inspect-info="line-height: 1.75; max-width: 68ch;">
                                Web design has evolved far beyond fixed pixel layouts. Today's web operates across thousands of unique viewport sizes—from handheld smartphones to high-resolution 4K desktop displays. Creating readable, visually appealing content requires combining <strong>fluid typography</strong> with <strong>flexible media constraints</strong>.
                            </p>

                            <h2 class="text-fluid-h1 font-bold text-slate-900 dark:text-white pt-4 tracking-tight"
                                data-inspect
                                data-inspect-info="font-size: clamp(1.75rem, 1.25rem + 2vw, 2.75rem)">
                                1. Why Fluid Typography with <code class="text-brand-500">clamp()</code> Wins
                            </h2>

                            <p>
                                Traditional responsive typography relied heavily on multiple media queries at arbitrary breakpoints (e.g., 320px, 768px, 1024px). This often caused jarring text size jumps as the screen resized. With modern CSS functions like <code class="bg-slate-100 dark:bg-slate-800 px-1.5 py-0.5 rounded text-brand-600 dark:text-brand-400 text-sm font-mono">clamp(minimum, preferred, maximum)</code>, font sizes scale smoothly across viewport dimensions.
                            </p>

                            <!-- Interactive Live Code Demo Block -->
                            <div class="my-6 p-4 rounded-xl bg-slate-950 text-slate-100 font-mono text-xs sm:text-sm border border-slate-800 shadow-inner overflow-x-auto">
                                <div class="text-slate-500 mb-2">// CSS Rules Applied on this Page</div>
                                <div class="text-purple-400">h1 {</div>
                                <div class="pl-4 text-emerald-400">font-size: <span class="text-amber-300">clamp(2.25rem, 1.5rem + 3.2vw, 4rem)</span>;</div>
                                <div class="text-purple-400">}</div>
                                <div class="text-purple-400">p {</div>
                                <div class="pl-4 text-emerald-400">font-size: <span class="text-amber-300">clamp(1rem, 0.95rem + 0.35vw, 1.2rem)</span>;</div>
                                <div class="pl-4 text-emerald-400">max-width: <span class="text-amber-300">68ch</span>; <span class="text-slate-500">/* Ideal line length (~60-75 chars) */</span></div>
                                <div class="pl-4 text-emerald-400">line-height: <span class="text-amber-300">1.75</span>;</div>
                                <div class="text-purple-400">}</div>
                            </div>

                            <blockquote class="my-8 border-l-4 border-brand-500 pl-4 py-2 italic text-slate-800 dark:text-slate-200 bg-brand-50/50 dark:bg-brand-900/10 rounded-r-lg">
                                <p class="text-fluid-h3 font-medium text-slate-900 dark:text-slate-100"
                                   data-inspect
                                   data-inspect-info="font-size: clamp(1.15rem, 1.05rem + 0.6vw, 1.45rem)">
                                    "Typography is the backbone of web UX. If your text isn't readable across screen sizes, design details lose their value."
                                </p>
                                <cite class="block text-xs uppercase tracking-wider font-semibold text-brand-600 dark:text-brand-400 mt-2 not-italic">— Responsive Web Standards Guidelines</cite>
                            </blockquote>

                            <h2 class="text-fluid-h2 font-bold text-slate-900 dark:text-white pt-4 tracking-tight"
                                data-inspect
                                data-inspect-info="font-size: clamp(1.35rem, 1.1rem + 1.2vw, 2rem)">
                                2. Defensive Image Styling
                            </h2>

                            <p>
                                Unconstrained images are a frequent cause of horizontal overflow and broken web page layouts on mobile devices. By enforcing global image constraints, media assets scale dynamically within their parent containers:
                            </p>

                            <ul class="list-disc list-inside space-y-2 pl-2 text-slate-700 dark:text-slate-300">
                                <li><strong class="text-slate-900 dark:text-white">max-width: 100%;</strong> — Ensures images never exceed their containing element's width.</li>
                                <li><strong class="text-slate-900 dark:text-white">height: auto;</strong> — Preserves aspect ratio during resizing to prevent vertical stretching.</li>
                                <li><strong class="text-slate-900 dark:text-white">object-fit: cover;</strong> — Maintains crop consistency inside fixed-aspect cards.</li>
                            </ul>

                        </div>
                    </div>
                </article>


                <!-- ==========================================================================
                     PORTFOLIO / FEATURE GRID SECTION (Requirement 1 & 4)
                     ========================================================================== -->
                <section id="gallery" class="px-4 sm:px-6 lg:px-8 py-12 bg-slate-50 dark:bg-slate-950/50 border-t border-slate-200 dark:border-slate-800 transition-colors">
                    <div class="max-w-7xl mx-auto">
                        
                        <div class="text-center max-w-2xl mx-auto mb-10">
                            <h2 class="text-fluid-h1 font-bold text-slate-900 dark:text-white tracking-tight">Responsive Image Gallery</h2>
                            <p class="text-fluid-body text-slate-600 dark:text-slate-400 mt-2">
                                Cards adjust automatically from a single column on mobile screens to multi-column grids on desktop.
                            </p>
                        </div>

                        <!-- Responsive Grid: 1 col (mobile), 2 col (tablet), 3 col (desktop) -->
                        <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6 sm:gap-8">
                            
                            <!-- Card 1 -->
                            <div class="bg-white dark:bg-slate-800 rounded-2xl overflow-hidden shadow-md hover:shadow-xl transition-all border border-slate-200 dark:border-slate-700/60 flex flex-col">
                                <div class="aspect-card w-full overflow-hidden bg-slate-100 dark:bg-slate-900">
                                    <img src="https://images.unsplash.com/photo-1555066931-4365d14bab8c?auto=format&fit=crop&w=600&q=80" 
                                         alt="UI Development Work" 
                                         class="w-full h-full object-cover hover:scale-105 transition duration-500"
                                         data-inspect
                                         data-inspect-info="max-width: 100%; aspect-ratio: 4/3">
                                </div>
                                <div class="p-5 flex-1 flex flex-col justify-between">
                                    <div>
                                        <span class="text-xs font-semibold text-brand-600 dark:text-brand-400 uppercase tracking-wider">Engineering</span>
                                        <h3 class="text-fluid-h3 font-bold text-slate-900 dark:text-white mt-1">Design System Engineering</h3>
                                        <p class="text-fluid-small text-slate-600 dark:text-slate-400 mt-2">
                                            Building standardized fluid components across design tokens and mobile devices.
                                        </p>
                                    </div>
                                    <div class="mt-4 pt-4 border-t border-slate-100 dark:border-slate-700/50 flex items-center justify-between text-xs text-slate-500">
                                        <span><i class="fa-solid fa-code text-brand-500 mr-1"></i> CSS3 / Tailwind</span>
                                        <a href="#" class="text-brand-600 dark:text-brand-400 font-semibold hover:underline">View Project →</a>
                                    </div>
                                </div>
                            </div>

                            <!-- Card 2 -->
                            <div class="bg-white dark:bg-slate-800 rounded-2xl overflow-hidden shadow-md hover:shadow-xl transition-all border border-slate-200 dark:border-slate-700/60 flex flex-col">
                                <div class="aspect-card w-full overflow-hidden bg-slate-100 dark:bg-slate-900">
                                    <img src="https://images.unsplash.com/photo-1512941937669-90a1b58e7e9c?auto=format&fit=crop&w=600&q=80" 
                                         alt="Mobile UX Design" 
                                         class="w-full h-full object-cover hover:scale-105 transition duration-500"
                                         data-inspect
                                         data-inspect-info="max-width: 100%; aspect-ratio: 4/3">
                                </div>
                                <div class="p-5 flex-1 flex flex-col justify-between">
                                    <div>
                                        <span class="text-xs font-semibold text-brand-600 dark:text-brand-400 uppercase tracking-wider">Mobile UX</span>
                                        <h3 class="text-fluid-h3 font-bold text-slate-900 dark:text-white mt-1">Touch & Gesture Design</h3>
                                        <p class="text-fluid-small text-slate-600 dark:text-slate-400 mt-2">
                                            Optimizing interactive targets for thumb zones and varied mobile screen sizes.
                                        </p>
                                    </div>
                                    <div class="mt-4 pt-4 border-t border-slate-100 dark:border-slate-700/50 flex items-center justify-between text-xs text-slate-500">
                                        <span><i class="fa-solid fa-mobile-screen text-brand-500 mr-1"></i> Mobile First</span>
                                        <a href="#" class="text-brand-600 dark:text-brand-400 font-semibold hover:underline">View Project →</a>
                                    </div>
                                </div>
                            </div>

                            <!-- Card 3 -->
                            <div class="bg-white dark:bg-slate-800 rounded-2xl overflow-hidden shadow-md hover:shadow-xl transition-all border border-slate-200 dark:border-slate-700/60 flex flex-col sm:col-span-2 lg:col-span-1">
                                <div class="aspect-card w-full overflow-hidden bg-slate-100 dark:bg-slate-900">
                                    <img src="https://images.unsplash.com/photo-1460925895917-afdab827c52f?auto=format&fit=crop&w=600&q=80" 
                                         alt="Performance Optimization" 
                                         class="w-full h-full object-cover hover:scale-105 transition duration-500"
                                         data-inspect
                                         data-inspect-info="max-width: 100%; aspect-ratio: 4/3">
                                </div>
                                <div class="p-5 flex-1 flex flex-col justify-between">
                                    <div>
                                        <span class="text-xs font-semibold text-brand-600 dark:text-brand-400 uppercase tracking-wider">Performance</span>
                                        <h3 class="text-fluid-h3 font-bold text-slate-900 dark:text-white mt-1">Responsive Asset Delivery</h3>
                                        <p class="text-fluid-small text-slate-600 dark:text-slate-400 mt-2">
                                            Preventing Layout Shift (CLS) through fixed aspect ratios and responsive images.
                                        </p>
                                    </div>
                                    <div class="mt-4 pt-4 border-t border-slate-100 dark:border-slate-700/50 flex items-center justify-between text-xs text-slate-500">
                                        <span><i class="fa-solid fa-bolt text-brand-500 mr-1"></i> Core Web Vitals</span>
                                        <a href="#" class="text-brand-600 dark:text-brand-400 font-semibold hover:underline">View Project →</a>
                                    </div>
                                </div>
                            </div>

                        </div>
                    </div>
                </section>


                <!-- ==========================================================================
                     AUTHOR & FOOTER SECTION
                     ========================================================================== -->
                <footer id="author" class="px-4 sm:px-6 lg:px-8 py-12 bg-white dark:bg-slate-900 border-t border-slate-200 dark:border-slate-800">
                    <div class="max-w-4xl mx-auto flex flex-col sm:flex-row items-center justify-between gap-6">
                        
                        <!-- Profile -->
                        <div class="flex items-center gap-4 text-center sm:text-left">
                            <img src="https://images.unsplash.com/photo-1534528741775-53994a69daeb?auto=format&fit=crop&w=160&q=80" 
                                 alt="Elena Rostova" 
                                 class="w-16 h-16 rounded-full object-cover border-2 border-brand-500 shadow-md">
                            <div>
                                <h3 class="text-fluid-h3 font-bold text-slate-900 dark:text-white">Elena Rostova</h3>
                                <p class="text-fluid-small text-slate-500 dark:text-slate-400">Senior UI Engineer & Technical Writer</p>
                            </div>
                        </div>

                        <!-- Assignment Metadata Footer Note -->
                        <div class="text-xs text-slate-400 dark:text-slate-500 text-center sm:text-right">
                            <p class="font-semibold text-slate-600 dark:text-slate-400">Assignment 9.2 Submission</p>
                            <p class="mt-1">Built with Fluid Typography & Responsive Media</p>
                        </div>
                    </div>
                </footer>

            </main>
        </div>
    </div>

    <!-- ==========================================================================
         JAVASCRIPT CONTROLLERS (Viewport Resizer & Unit Inspector)
         ========================================================================== -->
    <script>
        // Viewport Switcher Controller
        function setViewport(mode) {
            const wrapper = document.getElementById('viewport-frame-wrapper');
            const info = document.getElementById('viewport-info');
            const infoLabel = document.getElementById('current-viewport-label');
            const buttons = document.querySelectorAll('.vp-btn');

            // Reset Active Button Styling
            buttons.forEach(btn => {
                btn.classList.remove('bg-brand-600', 'text-white');
                btn.classList.add('text-slate-300');
            });

            const activeBtn = document.getElementById(`btn-${mode}`);
            if(activeBtn) {
                activeBtn.classList.add('bg-brand-600', 'text-white');
                activeBtn.classList.remove('text-slate-300');
            }

            if (mode === 'full') {
                wrapper.style.width = '100%';
                wrapper.style.margin = '0 auto';
                info.classList.add('hidden');
            } else {
                info.classList.remove('hidden');
                wrapper.style.margin = '1.5rem auto';
                
                if (mode === 'desktop') {
                    wrapper.style.width = '1200px';
                    wrapper.style.maxWidth = '95%';
                    infoLabel.textContent = 'Desktop (1200px Container Frame)';
                } else if (mode === 'tablet') {
                    wrapper.style.width = '768px';
                    wrapper.style.maxWidth = '95%';
                    infoLabel.textContent = 'Tablet Viewport (768px Frame)';
                } else if (mode === 'mobile') {
                    wrapper.style.width = '375px';
                    wrapper.style.maxWidth = '95%';
                    infoLabel.textContent = 'Mobile Viewport (375px Frame)';
                }
            }
        }

        // Live CSS Unit Inspector Toggle
        function toggleInspector() {
            document.body.classList.toggle('inspector-active');
            const btn = document.getElementById('inspector-toggle');
            
            if(document.body.classList.contains('inspector-active')) {
                btn.classList.remove('border-amber-500/40', 'bg-amber-500/10', 'text-amber-400');
                btn.classList.add('border-amber-500', 'bg-amber-500', 'text-slate-950', 'font-bold');
            } else {
                btn.classList.add('border-amber-500/40', 'bg-amber-500/10', 'text-amber-400');
                btn.classList.remove('border-amber-500', 'bg-amber-500', 'text-slate-950', 'font-bold');
            }
        }

        // Theme Switcher Controller (Light / Dark)
        function toggleTheme() {
            const html = document.documentElement;
            const icon = document.getElementById('theme-icon');
            
            if (html.classList.contains('dark')) {
                html.classList.remove('dark');
                icon.classList.remove('fa-moon');
                icon.classList.add('fa-sun');
            } else {
                html.classList.add('dark');
                icon.classList.remove('fa-sun');
                icon.classList.add('fa-moon');
            }
        }

        // Default to Dark Mode on Load
        document.documentElement.classList.add('dark');
    </script>
</body>
</html>