# ThePioneersX
Space Simulator
<!DOCTYPE html>
<html lang="en" class="dark">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ThePioneersX // NASA Astrodynamics & 4K HDR Space Mission Simulator (SYS-VER 12.0)</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Google Fonts: Orbitron, Inter, JetBrains Mono -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&family=JetBrains+Mono:wght@400;500;700&family=Orbitron:wght@600;700;800;900&display=swap" rel="stylesheet">
    <!-- FontAwesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.1/css/all.min.css">

    <script>
        tailwind.config = {
            darkMode: 'class',
            theme: {
                extend: {
                    colors: {
                        cosmos: '#040611',
                        nebula: '#090D24',
                        panelBlue: '#0E1538',
                        panelPurple: '#17123A',
                        borderGlow: '#2A3678',
                        neonCyan: '#00F0FF',
                        neonBlue: '#3B82F6',
                        neonPurple: '#A855F7',
                        neonGreen: '#00E676',
                        neonAmber: '#FFB300',
                        neonRed: '#FF2E54'
                    },
                    fontFamily: {
                        sans: ['Inter', 'sans-serif'],
                        display: ['Orbitron', 'sans-serif'],
                        mono: ['JetBrains Mono', 'monospace']
                    }
                }
            }
        };
    </script>

    <style>
        body {
            background: radial-gradient(circle at 18% 15%, #161242 0%, #090d24 45%, #040611 100%);
            color: #E2E8F0;
            overflow-x: hidden;
            user-select: none;
        }

        /* Cosmic Blue & Nebula Purple Glassmorphic Panels */
        .cosmic-panel {
            background: linear-gradient(145deg, rgba(14, 21, 56, 0.94) 0%, rgba(21, 16, 56, 0.94) 100%);
            border: 1px solid rgba(99, 102, 241, 0.38);
            box-shadow: 0 10px 35px rgba(4, 6, 17, 0.85), inset 0 1px 0 rgba(0, 240, 255, 0.18);
            border-radius: 0.85rem;
            backdrop-filter: blur(12px);
        }

        .cosmic-subcard {
            background: linear-gradient(160deg, rgba(7, 11, 30, 0.92) 0%, rgba(16, 14, 44, 0.92) 100%);
            border: 1px solid rgba(59, 130, 246, 0.32);
            border-radius: 0.75rem;
        }

        /* Highlight glow for the First-2-Runs Hand Gesture Tutorial */
        @keyframes targetPulseGlow {
            0%, 100% {
                box-shadow: 0 0 0 0 rgba(0, 240, 255, 0.85), 0 0 22px rgba(168, 85, 247, 0.75);
                border-color: #00F0FF !important;
            }
            50% {
                box-shadow: 0 0 0 8px rgba(0, 240, 255, 0), 0 0 32px rgba(0, 240, 255, 0.95);
                border-color: #A855F7 !important;
            }
        }
        .guide-target-glow {
            animation: targetPulseGlow 1.5s infinite !important;
            position: relative;
            z-index: 30;
        }

        /* Red Hull Impact Flash */
        @keyframes hullHitStrobe {
            0%, 100% { box-shadow: inset 0 0 0 rgba(255, 46, 84, 0); }
            50% { box-shadow: inset 0 0 90px rgba(255, 46, 84, 0.85); }
        }
        .hull-hit-flash {
            animation: hullHitStrobe 0.45s ease-in-out 2;
        }

        /* Custom Scrollbar */
        ::-webkit-scrollbar { width: 6px; height: 6px; }
        ::-webkit-scrollbar-track { background: #040611; }
        ::-webkit-scrollbar-thumb { background: #3B82F6; border-radius: 4px; }
    </style>
</head>
<body class="min-h-screen flex flex-col font-sans antialiased selection:bg-neonCyan selection:text-cosmos">

    <!-- =====================================================================
         TOP COMMAND HEADER: "ThePioneersX" LOGO + JPL EPHEMERIS + NASA EQNS
         ===================================================================== -->
    <header class="sticky top-0 z-40 bg-cosmos/95 backdrop-blur-md border-b border-indigo-500/40 px-3 sm:px-5 py-2.5 shadow-[0_4px_25px_rgba(0,240,255,0.12)]">
        <div class="max-w-[1640px] mx-auto flex flex-wrap items-center justify-between gap-2.5">

            <!-- Left: Official "ThePioneersX" Geometric Emblem & Title -->
            <div class="flex items-center gap-3">
                <div class="relative w-11 h-11 rounded-xl bg-gradient-to-br from-indigo-950 via-purple-950 to-cosmos border-2 border-neonCyan flex items-center justify-center shadow-[0_0_20px_rgba(0,240,255,0.4)]">
                    <svg class="w-7 h-7 text-neonCyan" viewBox="0 0 40 40" fill="none">
                        <polygon points="20,2 37,11 37,29 20,38 3,29 3,11" stroke="currentColor" stroke-width="2.2" fill="rgba(168,85,247,0.18)"/>
                        <path d="M11 28 L20 7 L29 28 L20 22 Z" fill="currentColor"/>
                        <circle cx="20" cy="16" r="2.6" fill="#A855F7"/>
                    </svg>
                    <span class="absolute -bottom-1 -right-1 px-1 py-0.2 text-[8px] font-mono font-bold bg-neonPurple text-white rounded">PX-11</span>
                </div>
                <div>
                    <div class="flex items-center gap-2">
                        <span class="font-display font-black text-lg md:text-xl tracking-wider text-white">
                            ThePioneers<span class="text-neonCyan">X</span>
                        </span>
                        <span class="px-2 py-0.5 text-[10px] font-mono font-bold bg-neonPurple/25 text-neonCyan border border-neonPurple/60 rounded-md">
                            NASA 4K HDR // SYS-VER 12.0
                        </span>
                    </div>
                    <p class="text-[11px] text-indigo-300 font-mono">
                        ASTRODYNAMICS COMMAND // LIVE NASA IMAGERY • REAL ASTROPHYSICS • 1ST-PERSON EVA VISOR
                    </p>
                </div>
            </div>

            <!-- Right Controls: MET, Control Mode, First-2-Runs Hand Guide, NASA Physics Modal & Judge Guide -->
            <div class="flex flex-wrap items-center gap-2">
                <!-- Live MET Clock -->
                <div class="hidden xl:flex items-center gap-1.5 cosmic-subcard px-2.5 py-1.5 font-mono text-xs text-cyan-200">
                    <span class="w-2 h-2 rounded-full bg-neonGreen animate-pulse"></span>
                    <span id="utc-clock-display">MET 00:00:00</span>
                </div>

                <!-- Master Flight Authority Toggle -->
                <button id="header-autopilot-btn" onclick="toggleAutopilot()" class="px-3 py-1.5 rounded-lg bg-neonCyan/15 hover:bg-neonCyan hover:text-cosmos border border-neonCyan text-neonCyan font-mono font-bold text-xs transition flex items-center gap-1.5">
                    <span id="header-autopilot-text">🎮 MODE: MANUAL (WASD)</span>
                </button>

                <!-- First-2-Runs Hand Tutorial Selector -->
                <div class="flex items-center gap-1 cosmic-subcard px-2 py-1 text-xs font-mono">
                    <span class="text-neonCyan">👆 Guide:</span>
                    <select id="run-mode-select" onchange="setRunMode(this.value)" class="bg-transparent text-white font-bold focus:outline-none cursor-pointer">
                        <option value="1" class="bg-cosmos">Run 1 of 2 (Hand ON)</option>
                        <option value="2" class="bg-cosmos">Run 2 of 2 (Hand ON)</option>
                        <option value="3" class="bg-cosmos">Free Play (Hand OFF)</option>
                    </select>
                </div>

                <!-- Instant Skip Tutorial Button -->
                <button id="header-skip-btn" onclick="skipTutorial()" class="px-3 py-1.5 rounded-lg bg-neonPurple/25 hover:bg-neonPurple text-amber-300 hover:text-white border border-neonPurple font-mono font-bold text-xs transition">
                    <span id="header-skip-text">⏭ SKIP TUTORIAL (RUN 1/2)</span>
                </button>

                <!-- Restart Hand Guide Button -->
                <button id="header-restart-btn" onclick="restartTutorial()" class="hidden px-3 py-1.5 rounded-lg bg-neonCyan/15 hover:bg-neonCyan hover:text-cosmos text-neonCyan border border-neonCyan/50 font-mono font-bold text-xs transition">
                    👆 SHOW HAND GUIDE
                </button>

                <!-- NASA Astrophysics Equations Modal Button -->
                <button onclick="openPhysicsModal()" class="px-3 py-1.5 rounded-lg bg-indigo-600/30 hover:bg-indigo-500 text-cyan-200 hover:text-white border border-indigo-400 font-display font-bold text-xs transition">
                    📐 NASA PHYSICS EQNS
                </button>

                <!-- 15-Second Judge Guide Modal Button -->
                <button onclick="openJudgeModal()" class="px-3 py-1.5 rounded-lg bg-neonGreen/15 hover:bg-neonGreen hover:text-cosmos text-neonGreen border border-neonGreen/60 font-display font-bold text-xs transition">
                    📘 JUDGE GUIDE
                </button>

                <!-- Sound Toggle -->
                <button id="sound-btn" onclick="toggleSound()" class="p-2 rounded-lg cosmic-subcard text-neonCyan hover:border-neonCyan transition" title="Toggle Telemetry Audio">
                    <i id="sound-icon" class="fa-solid fa-volume-high"></i>
                </button>
            </div>
        </div>
    </header>

    <!-- =====================================================================
         FLOATING HAND-GESTURE TUTORIAL POINTER (FIRST 2 RUNS + INSTANT SKIP)
         ===================================================================== -->
    <div id="hand-pointer-box" class="hidden fixed z-50 pointer-events-none transition-all duration-300" style="top: 120px; left: 40px;">
        <div class="flex items-start gap-2.5">
            <div class="text-4xl select-none drop-shadow-[0_0_16px_rgba(0,240,255,0.95)] animate-bounce">
                👆
            </div>
            <div class="pointer-events-auto bg-cosmos/95 border-2 border-neonCyan rounded-xl p-3.5 shadow-[0_0_30px_rgba(168,85,247,0.5)] max-w-xs backdrop-blur-md">
                <div class="flex items-center justify-between gap-2 mb-1">
                    <span id="hand-run-tag" class="text-[10px] font-mono font-bold uppercase px-2 py-0.5 rounded bg-neonPurple/30 text-neonCyan border border-neonPurple/50">
                        GUIDED RUN 1/2 • STEP 1 OF 4
                    </span>
                    <button id="hand-skip-btn" onclick="skipTutorial()" class="text-[11px] font-mono font-bold text-neonAmber hover:underline">
                        ⏭ Skip (Run 1/2)
                    </button>
                </div>
                <p id="hand-step-title" class="font-display font-bold text-xs text-white mb-1">
                    Click a Target World to Load NASA Data
                </p>
                <p id="hand-step-desc" class="text-xs text-indigo-200 leading-relaxed mb-2.5">
                    Select Moon, Mars, Europa, or Titan to compute exact Vis-Viva orbital velocities, solar flux, and gravity.
                </p>
                <div class="flex items-center justify-between gap-2 pt-1.5 border-t border-indigo-500/30">
                    <button onclick="doGuidedStepForMe()" class="px-3 py-1 rounded-lg bg-gradient-to-r from-neonCyan to-neonBlue text-cosmos font-display font-bold text-[11px] hover:opacity-90 transition">
                        DO THIS FOR ME &rarr;
                    </button>
                    <button onclick="skipTutorial()" class="px-2 py-1 rounded bg-indigo-950 text-indigo-300 hover:text-white text-[11px] font-mono">
                        Dismiss
                    </button>
                </div>
            </div>
        </div>
    </div>

    <!-- =====================================================================
         MAIN WORKSPACE: LEFT = NASA BUILDER / FLIGHT DECK | RIGHT = LIVE VIEWPORT
         ===================================================================== -->
    <main class="flex-1 max-w-[1640px] w-full mx-auto px-3 sm:px-5 py-3">
        <div class="grid grid-cols-1 lg:grid-cols-12 gap-4 items-stretch">

            <!-- =============================================================
                 LEFT PANEL (5 COLS): NASA SPACECRAFT BUILDER OR FLIGHT COMMAND
                 ============================================================= -->
            <div class="lg:col-span-5 flex flex-col gap-3">

                <!-- MODE A: 3-STEP NASA ASTRODYNAMICS SPACECRAFT BUILDER -->
                <div id="builder-panel" class="cosmic-panel p-4 flex flex-col justify-between gap-3">

                    <!-- STEP 1: PICK DESTINATION PLANET (EXACT NASA JPL DATA) -->
                    <div>
                        <div class="flex items-center justify-between mb-1.5">
                            <span class="font-display font-bold text-xs uppercase tracking-wider text-neonCyan">
                                STEP 1 // TARGET PLANETARY BODY (JPL EPHEMERIS)
                            </span>
                            <span id="selected-planet-pill" class="text-[11px] font-mono text-cyan-200 bg-indigo-950/90 px-2.5 py-0.5 rounded-full border border-indigo-400/40">
                                Target: 🌕 Moon (g = 1.625 m/s²)
                            </span>
                        </div>

                        <div id="planet-cards-grid" class="grid grid-cols-2 sm:grid-cols-4 gap-2"></div>

                        <div id="planet-hint-text" class="mt-2 px-3 py-1.5 rounded-lg cosmic-subcard text-[11px] font-mono text-indigo-200 leading-relaxed"></div>
                    </div>

                    <!-- STEP 2: EQUIP LAUNCH VEHICLE, POWER, SHIELDING & EVA SUITE -->
                    <div class="border-t border-indigo-500/30 pt-2.5">
                        <div class="flex flex-wrap items-center justify-between gap-2 mb-2">
                            <span class="font-display font-bold text-xs uppercase tracking-wider text-neonCyan">
                                STEP 2 // SUBSYSTEMS &amp; MASS FRACTION
                            </span>
                            <button id="btn-auto-build" onclick="autoBuildWinningShip(true)" class="px-3 py-1 rounded-lg bg-gradient-to-r from-neonCyan to-neonPurple text-cosmos font-display font-bold text-xs hover:opacity-95 transition shadow-[0_0_15px_rgba(0,240,255,0.3)]">
                                ✨ AUTO-BUILD NASA OPTIMAL SHIP
                            </button>
                        </div>

                        <div id="equipment-rows-container" class="space-y-1.5"></div>

                        <!-- Propellant Mass Slider (Directly drives Tsiolkovsky ln(m0/mf)) -->
                        <div class="mt-2 cosmic-subcard p-2.5">
                            <div class="flex items-center justify-between text-xs mb-1">
                                <span class="font-semibold text-indigo-200">⛽ Propellant Mass ($m_{\text{prop}}$) &mdash; Drives $\ln(m_0/m_f)$</span>
                                <span id="propellant-pct-label" class="font-mono font-bold text-neonCyan">85% Fuel Load</span>
                            </div>
                            <input id="propellant-slider" type="range" min="45" max="96" step="1" value="85" oninput="updatePropellantSlider(this.value)" class="w-full accent-cyan-400 cursor-pointer">
                        </div>
                    </div>

                    <!-- LIVE NASA ASTROPHYSICS EQUATIONS TELEMETRY BOX -->
                    <div class="border-t border-indigo-500/30 pt-2.5">
                        <div class="flex items-center justify-between mb-1.5">
                            <span class="font-display font-bold text-[11px] uppercase tracking-wider text-neonPurple">
                                📐 LIVE NASA ASTRODYNAMICS EQUATIONS
                            </span>
                            <button onclick="openPhysicsModal()" class="text-[10px] font-mono text-neonCyan hover:underline">
                                Full Equations &rarr;
                            </button>
                        </div>
                        <div id="live-equations-box" class="cosmic-subcard p-2.5 font-mono text-[11px] space-y-1 text-indigo-100"></div>
                    </div>

                    <!-- STEP 3: VERIFY 4 NASA READINESS CONSTRAINTS & LAUNCH -->
                    <div class="border-t border-indigo-500/30 pt-2.5">
                        <div class="flex items-center justify-between mb-1.5">
                            <span class="font-display font-bold text-xs uppercase tracking-wider text-neonCyan">
                                STEP 3 // FLIGHT READINESS REVIEW (FRR)
                            </span>
                            <span id="readiness-pill" class="px-2.5 py-0.5 rounded-full text-[11px] font-mono font-bold bg-neonGreen/20 text-neonGreen border border-neonGreen">
                                ✓ GO FOR LAUNCH
                            </span>
                        </div>

                        <div id="scoreboard-bars" class="grid grid-cols-2 gap-1.5 mb-2"></div>

                        <div id="coach-advice-box" class="px-3 py-2 rounded-xl bg-neonGreen/10 border border-neonGreen/40 text-xs text-indigo-100 flex items-center justify-between gap-2 mb-2">
                            <span id="coach-advice-text">✅ All 4 NASA FRR constraints satisfied! Ready for launch.</span>
                            <button id="coach-fix-btn" onclick="autoBuildWinningShip(true)" class="hidden shrink-0 px-2.5 py-1 rounded bg-neonAmber text-cosmos font-display font-bold text-[11px]">
                                ⚡ AUTO-FIX
                            </button>
                        </div>

                        <div class="grid grid-cols-1 sm:grid-cols-3 gap-2">
                            <button onclick="triggerStaticFireTest()" class="py-2.5 px-3 rounded-xl cosmic-subcard hover:border-neonAmber text-neonAmber font-display font-bold text-xs transition">
                                🔥 STATIC FIRE
                            </button>
                            <button id="btn-launch-mission" onclick="startLaunchFlight(false)" class="py-2.5 px-3 rounded-xl bg-gradient-to-r from-neonCyan via-neonBlue to-neonPurple text-cosmos font-display font-black text-xs tracking-wide shadow-[0_0_20px_rgba(0,240,255,0.4)] hover:opacity-95 transition">
                                🚀 LAUNCH (MANUAL WASD)
                            </button>
                            <button onclick="startLaunchFlight(true)" class="py-2.5 px-3 rounded-xl bg-neonGreen/20 hover:bg-neonGreen hover:text-cosmos border-2 border-neonGreen text-neonGreen font-display font-black text-xs tracking-wide transition">
                                🤖 LAUNCH AUTO-PILOT
                            </button>
                        </div>
                    </div>
                </div>

                <!-- MODE B: LIVE IN-FLIGHT, ASTEROID DODGE & 1ST-PERSON EVA COMMAND DECK -->
                <div id="flight-control-panel" class="hidden cosmic-panel p-4 sm:p-5 flex flex-col justify-between gap-3.5 min-h-[590px]">
                    <div>
                        <div class="flex flex-wrap items-center justify-between gap-2 border-b border-indigo-500/30 pb-3 mb-2.5">
                            <div>
                                <span id="flight-mode-subtag" class="text-[11px] font-mono text-neonCyan uppercase block">NASA DEEP-SPACE &amp; EVA TELEMETRY</span>
                                <h2 id="flight-header-title" class="font-display text-base sm:text-lg font-black text-white">STAGE 1 OF 6 // LIFTOFF &amp; MAX-Q</h2>
                            </div>
                            <div class="flex items-center gap-1.5">
                                <button id="flight-autopilot-toggle" onclick="toggleAutopilot()" class="px-3 py-1.5 rounded-lg font-mono text-xs font-bold border transition">
                                    🎮 MANUAL (WASD)
                                </button>
                                <button onclick="returnToBuilder()" class="px-2.5 py-1.5 rounded-lg cosmic-subcard hover:border-neonCyan text-xs font-mono text-indigo-200">
                                    &larr; VAB HANGAR
                                </button>
                            </div>
                        </div>

                        <!-- 6-Stage Interactive Progression Tracker -->
                        <div class="text-[10px] font-mono text-indigo-300 mb-1 flex items-center justify-between">
                            <span>6-STAGE CAMPAIGN TRACKER (CLICK ANY STAGE TO TEST):</span>
                            <span id="surface-progress-badge" class="text-neonCyan font-bold">STAGE 1 / 6</span>
                        </div>
                        <div id="flight-stage-pills" class="grid grid-cols-3 sm:grid-cols-6 gap-1.5 mb-3"></div>

                        <!-- Live Manual/Auto Flight or Surface Objective Progress Meter -->
                        <div id="flight-pilot-progress-box" class="cosmic-subcard p-3 mb-3 border border-neonCyan/50">
                            <div class="flex items-center justify-between text-xs mb-1">
                                <span id="flight-task-label" class="font-bold text-neonCyan">🎯 STEER NOSE INTO TARGET RING (WASD) &amp; HOLD SPACE!</span>
                                <span id="flight-task-pct" class="font-mono font-bold text-neonGreen">0%</span>
                            </div>
                            <div class="w-full h-2.5 bg-cosmos rounded-full overflow-hidden border border-indigo-500/40">
                                <div id="flight-task-bar" class="h-full bg-gradient-to-r from-neonCyan via-neonBlue to-neonPurple transition-all duration-150" style="width: 0%"></div>
                            </div>
                            <div class="flex items-center justify-between text-[11px] font-mono text-indigo-300 mt-1.5">
                                <span id="flight-alignment-status">TRAJECTORY LOCKED</span>
                                <span id="flight-laser-hint">[SPACE] Thrust/Jump • [E] Laser/EVA Action</span>
                            </div>
                        </div>

                        <!-- Live In-Flight Physics Telemetry Readout Box -->
                        <div id="flight-live-physics-strip" class="cosmic-subcard p-2.5 mb-3 font-mono text-[11px] text-cyan-200 space-y-1"></div>

                        <!-- Dynamic Flight / Asteroid Crisis / Rover / 1st-Person EVA Card -->
                        <div id="flight-interactive-card" class="cosmic-subcard p-3.5"></div>
                    </div>

                    <!-- Live Spacecraft Hull, Propellant, Samples & Science Telemetry -->
                    <div class="border-t border-indigo-500/30 pt-3">
                        <div class="flex items-center justify-between text-xs font-mono text-indigo-300 mb-2">
                            <span>VEHICLE HULL &amp; EVA SUIT BIO-TELEMETRY</span>
                            <span id="flight-bonus-tag" class="text-neonGreen font-bold">PILOT &amp; EVA BONUS: +0 PTS</span>
                        </div>
                        <div class="grid grid-cols-4 gap-2 text-center font-mono">
                            <div class="cosmic-subcard p-2">
                                <span class="text-[9px] text-indigo-300 block">HULL INTEGRITY</span>
                                <span id="flight-hull-val" class="text-sm sm:text-base font-bold text-neonGreen">100%</span>
                            </div>
                            <div class="cosmic-subcard p-2">
                                <span class="text-[9px] text-indigo-300 block">PROPELLANT / BAT</span>
                                <span id="flight-fuel-val" class="text-sm sm:text-base font-bold text-neonCyan">100%</span>
                            </div>
                            <div class="cosmic-subcard p-2">
                                <span class="text-[9px] text-indigo-300 block">SAMPLES / EVA</span>
                                <span id="flight-samples-val" class="text-sm sm:text-base font-bold text-neonPurple">0 / 4</span>
                            </div>
                            <div class="cosmic-subcard p-2">
                                <span class="text-[9px] text-indigo-300 block">SCIENCE DATA</span>
                                <span id="flight-sci-val" class="text-sm sm:text-base font-bold text-neonAmber">400 Gb</span>
                            </div>
                        </div>
                    </div>
                </div>
            </div>

            <!-- =============================================================
                 RIGHT PANEL (7 COLS): LIVE SPACESHIP COCKPIT, LETHAL 3D ASTEROID
                 FIELD, ROVER CAM & 1ST-PERSON ASTRONAUT SPACESUIT HELMET VISOR
                 ============================================================= -->
            <div class="lg:col-span-7 flex flex-col">
                <div id="right-viewport-panel" class="cosmic-panel p-4 flex-1 flex flex-col justify-between gap-3">

                    <!-- Viewport Top Bar: Active Mode & 7 Quick Stage Preview Buttons -->
                    <div class="flex flex-wrap items-center justify-between gap-2 border-b border-indigo-500/30 pb-2.5">
                        <div class="flex items-center gap-2">
                            <span class="w-2.5 h-2.5 rounded-full bg-neonCyan animate-ping"></span>
                            <span class="font-display font-bold text-xs sm:text-sm text-white tracking-wider">
                                LIVE VIEWPORT // <span id="cockpit-scene-title" class="text-neonCyan">LC-39A LAUNCHPAD &amp; EARTH HORIZON</span>
                            </span>
                        </div>

                        <!-- Preview Any Stage Anytime -->
                        <div class="flex flex-wrap items-center gap-1 bg-cosmos/90 p-1 rounded-lg border border-indigo-500/40 text-[10px] font-mono">
                            <span class="text-indigo-300 px-1 hidden xl:inline">Camera:</span>
                            <button onclick="setCockpitScene('pad')" id="scene-btn-pad" class="px-2 py-1 rounded bg-neonCyan text-cosmos font-bold">1.Pad</button>
                            <button onclick="setCockpitScene('ascent')" id="scene-btn-ascent" class="px-2 py-1 rounded text-indigo-300 hover:text-white">2.Ascent</button>
                            <button onclick="setCockpitScene('space')" id="scene-btn-space" class="px-2 py-1 rounded text-indigo-300 hover:text-white">3.Asteroids</button>
                            <button onclick="setCockpitScene('orbit')" id="scene-btn-orbit" class="px-2 py-1 rounded text-indigo-300 hover:text-white">4.Orbit</button>
                            <button onclick="setCockpitScene('landing')" id="scene-btn-landing" class="px-2 py-1 rounded text-indigo-300 hover:text-white">5.Landing</button>
                            <button onclick="setCockpitScene('rover')" id="scene-btn-rover" class="px-2 py-1 rounded text-indigo-300 hover:text-white">6.Rover</button>
                            <button onclick="setCockpitScene('eva')" id="scene-btn-eva" class="px-2 py-1 rounded text-indigo-300 hover:text-white">7.1st-Person EVA</button>
                        </div>
                    </div>

                    <!-- THE LIVE SPACESHIP COCKPIT / ASTEROID FIELD / 1ST-PERSON VISOR CANVAS -->
                    <div id="canvas-wrapper" class="relative w-full h-[445px] sm:h-[490px] bg-cosmos rounded-xl border-2 border-indigo-500/50 overflow-hidden shadow-[inset_0_0_50px_rgba(4,6,17,0.9)]">
                        <canvas id="cockpit-canvas" class="w-full h-full block cursor-crosshair"></canvas>

                        <!-- Top Holographic NASA Telemetry Strip inside Viewport -->
                        <div class="absolute top-2.5 left-3 right-3 flex flex-wrap items-center justify-between gap-1.5 pointer-events-none font-mono text-[11px]">
                            <div class="bg-cosmos/85 backdrop-blur px-2.5 py-1 rounded border border-neonCyan/50 text-neonCyan">
                                <span id="hud-label-1">ALT:</span> <strong id="hud-altitude" class="text-white">0.0 km</strong>
                            </div>
                            <div class="bg-cosmos/85 backdrop-blur px-2.5 py-1 rounded border border-neonPurple/60 text-neonPurple">
                                <span id="hud-label-2">VIS-VIVA VEL:</span> <strong id="hud-speed" class="text-white">0.00 km/s</strong>
                            </div>
                            <div class="bg-cosmos/85 backdrop-blur px-2.5 py-1 rounded border border-neonAmber/50 text-neonAmber">
                                <span id="hud-label-3">GRAVITY / ATT:</span> <strong id="hud-attitude" class="text-white">9.81 m/s²</strong>
                            </div>
                            <div class="bg-cosmos/85 backdrop-blur px-2.5 py-1 rounded border border-neonGreen/50 text-neonGreen">
                                <span id="hud-label-4">HULL / SUIT:</span> <strong id="hud-throttle" class="text-white">100%</strong>
                            </div>
                        </div>

                        <!-- Interactive Cockpit Handle & Camera View Quick Bar -->
                        <div class="absolute top-11 left-3 right-3 flex items-center justify-between pointer-events-none font-mono text-[10px] z-10">
                            <div class="bg-cosmos/85 backdrop-blur px-2.5 py-1 rounded border border-neonCyan/40 text-cyan-200 pointer-events-auto flex items-center gap-2">
                                <span>🕹️ <strong>INTERACTIVE COCKPIT:</strong> Drag Yoke Handle, Slide Throttle Lever, or Click Dash Buttons!</span>
                            </div>
                            <button onclick="toggleCameraPerspective()" class="pointer-events-auto bg-indigo-950/90 hover:bg-neonCyan hover:text-cosmos px-2.5 py-1 rounded border border-neonCyan text-neonCyan font-bold transition">
                                🎥 <span id="cam-perspective-label">VIEW: AUTO (3RD/1ST)</span>
                            </button>
                        </div>

                        <!-- Live NASA JPL / JWST / Apollo / Curiosity / Huygens Imagery Credit Pill -->
                        <div class="absolute bottom-2.5 left-3 right-3 flex items-center justify-between pointer-events-none font-mono text-[10px] z-10">
                            <div id="nasa-imagery-credit-pill" class="bg-cosmos/90 backdrop-blur px-2.5 py-1 rounded border border-neonCyan/40 text-cyan-200 flex items-center gap-1.5">
                                <span class="w-2 h-2 rounded-full bg-neonGreen animate-pulse"></span>
                                <span id="nasa-imagery-credit-text">📡 NASA JPL IMAGERY FEED: ACQUIRING 4K HDR ARCHIVE...</span>
                            </div>
                            <div class="hidden sm:block bg-cosmos/90 backdrop-blur px-2 py-1 rounded border border-neonPurple/40 text-neonPurple font-bold">
                                4K ACES HDR • PBR RELIEF
                            </div>
                        </div>

                        <!-- Center Action / Asteroid Alert Banner Inside Viewport -->
                        <div id="cockpit-event-toast" class="hidden absolute top-20 left-1/2 -translate-x-1/2 bg-cosmos/95 border-2 border-neonCyan px-4 py-1.5 rounded-full font-display font-bold text-xs text-neonCyan shadow-[0_0_25px_rgba(0,240,255,0.5)] pointer-events-none z-20 text-center">
                            🔥 ENGINE STATIC FIRE TEST ACTIVE!
                        </div>

                        <!-- LOSS OF VEHICLE (LOV) OR ORBIT OVERSHOOT FAILURE OVERLAY INSIDE VIEWPORT -->
                        <div id="lov-failure-overlay" class="hidden absolute inset-0 z-30 bg-cosmos/90 backdrop-blur-md flex flex-col items-center justify-center p-6 text-center border-4 border-neonRed">
                            <div id="lov-failure-badge" class="px-3 py-1 rounded-full bg-neonRed/20 border border-neonRed text-neonRed font-mono text-xs font-bold mb-2 animate-pulse">
                                ⚠️ NASA TELEMETRY ALARM // MISSION FAILURE
                            </div>
                            <h3 id="lov-failure-title" class="font-display text-2xl sm:text-3xl font-black text-white mb-2">
                                💥 ASTEROID IMPACT // MISSION FAILED
                            </h3>
                            <p id="lov-failure-reason" class="text-xs sm:text-sm text-indigo-200 max-w-lg mb-4 font-mono leading-relaxed">
                                High-velocity hypervelocity impact ($E_k = \frac{1}{2} m v_{\text{rel}}^2$) breached the primary pressure vessel during manual flight.
                            </p>
                            <div class="flex flex-wrap items-center justify-center gap-3">
                                <button onclick="retryCurrentStage(false)" class="px-4 py-2.5 rounded-xl bg-neonCyan text-cosmos font-display font-black text-xs hover:opacity-90 transition">
                                    🔄 RETRY STAGE MANUALLY (RESET 100%)
                                </button>
                                <button onclick="retryCurrentStage(true)" class="px-4 py-2.5 rounded-xl bg-neonGreen text-cosmos font-display font-black text-xs hover:opacity-90 transition">
                                    🤖 RETRY STAGE WITH AI AUTO-PILOT
                                </button>
                            </div>
                        </div>
                    </div>

                    <!-- INTERACTIVE KEYBOARD & TOUCH CONTROL DECK -->
                    <div class="cosmic-subcard p-3 flex flex-col sm:flex-row items-center justify-between gap-3">

                        <!-- Left: WASD Attitude / Asteroid Dodge / 1st-Person EVA Walk Keys -->
                        <div class="flex items-center gap-3">
                            <div class="grid grid-cols-3 gap-1 w-28 text-center font-mono text-xs font-bold">
                                <div></div>
                                <button id="key-w" onmousedown="pressVirtualKey('w',true)" onmouseup="pressVirtualKey('w',false)" onmouseleave="pressVirtualKey('w',false)" ontouchstart="pressVirtualKey('w',true);event.preventDefault()" ontouchend="pressVirtualKey('w',false)" class="py-1.5 rounded bg-indigo-950 border border-indigo-500/50 text-cyan-200 hover:border-neonCyan transition">W</button>
                                <div></div>
                                <button id="key-a" onmousedown="pressVirtualKey('a',true)" onmouseup="pressVirtualKey('a',false)" onmouseleave="pressVirtualKey('a',false)" ontouchstart="pressVirtualKey('a',true);event.preventDefault()" ontouchend="pressVirtualKey('a',false)" class="py-1.5 rounded bg-indigo-950 border border-indigo-500/50 text-cyan-200 hover:border-neonCyan transition">A</button>
                                <button id="key-s" onmousedown="pressVirtualKey('s',true)" onmouseup="pressVirtualKey('s',false)" onmouseleave="pressVirtualKey('s',false)" ontouchstart="pressVirtualKey('s',true);event.preventDefault()" ontouchend="pressVirtualKey('s',false)" class="py-1.5 rounded bg-indigo-950 border border-indigo-500/50 text-cyan-200 hover:border-neonCyan transition">S</button>
                                <button id="key-d" onmousedown="pressVirtualKey('d',true)" onmouseup="pressVirtualKey('d',false)" onmouseleave="pressVirtualKey('d',false)" ontouchstart="pressVirtualKey('d',true);event.preventDefault()" ontouchend="pressVirtualKey('d',false)" class="py-1.5 rounded bg-indigo-950 border border-indigo-500/50 text-cyan-200 hover:border-neonCyan transition">D</button>
                            </div>
                            <div class="text-[11px] leading-tight">
                                <div id="deck-control-title" class="font-display font-bold text-neonCyan">DODGE ASTEROIDS &amp; STEER (WASD)</div>
                                <div id="deck-control-desc" class="text-indigo-300">Steer around red 3D asteroids, drive Rover, or walk in 1st-Person Spacesuit Visor!</div>
                            </div>
                        </div>

                        <!-- Right: Dynamic Action Keys [SPACE], [Q], [E], and [P] Auto-Pilot Switch -->
                        <div class="flex flex-wrap items-center gap-1.5 font-mono text-xs">
                            <button id="key-space" onmousedown="pressVirtualKey('space',true)" onmouseup="pressVirtualKey('space',false)" onmouseleave="pressVirtualKey('space',false)" ontouchstart="pressVirtualKey('space',true);event.preventDefault()" ontouchend="pressVirtualKey('space',false)" class="px-3 py-2 rounded-lg bg-indigo-950 border border-neonAmber/70 text-neonAmber font-bold hover:bg-neonAmber hover:text-cosmos transition">
                                [SPACE] 🔥 THRUST
                            </button>
                            <button id="key-q" onmousedown="pressVirtualKey('q',true)" onmouseup="pressVirtualKey('q',false)" ontouchstart="pressVirtualKey('q',true);event.preventDefault()" ontouchend="pressVirtualKey('q',false)" class="px-2.5 py-2 rounded-lg bg-indigo-950 border border-indigo-500/50 text-indigo-200 hover:border-neonCyan transition" title="Cut Throttle / Brake">
                                [Q] BRAKE
                            </button>
                            <button id="key-e" onmousedown="pressVirtualKey('e',true)" onmouseup="pressVirtualKey('e',false)" ontouchstart="pressVirtualKey('e',true);event.preventDefault()" ontouchend="pressVirtualKey('e',false)" class="px-3 py-2 rounded-lg bg-indigo-950 border border-neonGreen/70 text-neonGreen font-bold hover:bg-neonGreen hover:text-cosmos transition">
                                [E] ⚡ LASER / EVA
                            </button>
                            <button id="cockpit-autopilot-btn" onclick="toggleAutopilot()" class="px-3 py-2 rounded-lg bg-neonPurple/25 border-2 border-neonCyan text-neonCyan font-bold hover:bg-neonCyan hover:text-cosmos transition">
                                [P] 🤖 AUTO: OFF
                            </button>
                        </div>

                    </div>

                </div>
            </div>

        </div>
    </main>

    <!-- =====================================================================
         NASA ASTROPHYSICS & MATHEMATICAL EQUATIONS MODAL
         ===================================================================== -->
    <div id="physics-modal" class="hidden fixed inset-0 z-50 bg-cosmos/85 backdrop-blur-md flex items-center justify-center p-4">
        <div class="cosmic-panel max-w-4xl w-full p-6 border-2 border-neonCyan space-y-4 max-h-[90vh] overflow-y-auto">
            <div class="flex items-center justify-between border-b border-indigo-500/40 pb-3">
                <div>
                    <span class="text-xs font-mono text-neonCyan">NASA JPL HORIZONS // EXACT MATHEMATICAL MODELING</span>
                    <h2 class="font-display text-xl font-black text-white mt-0.5">📐 NASA ASTRODYNAMICS &amp; PHYSICS EQUATIONS ENGINE</h2>
                </div>
                <button onclick="closePhysicsModal()" class="px-3 py-1.5 rounded-lg bg-indigo-950 hover:bg-neonRed text-white font-mono text-xs border border-indigo-500/40">✕ CLOSE</button>
            </div>

            <div class="grid grid-cols-1 md:grid-cols-2 gap-3 text-xs font-mono">
                <div class="cosmic-subcard p-3.5 space-y-1.5">
                    <div class="font-display font-bold text-neonCyan text-sm">1. Tsiolkovsky Rocket Equation</div>
                    <div class="text-amber-300 bg-cosmos/80 p-2 rounded border border-indigo-500/30">
                        Δv = I_sp · g_0 · ln(m_0 / m_f) + Δv_C3
                    </div>
                    <p class="text-indigo-200 font-sans leading-relaxed">
                        Uses exact standard gravity $g_0 = 9.80665\text{ m/s}^2$, engine specific impulse $I_{\text{sp}}$ ($\text{s}$), wet mass $m_0 = m_{\text{dry}} + m_{\text{prop}}$, and dry mass $m_f = m_{\text{dry}}$ to compute exact translational velocity budget.
                    </p>
                </div>

                <div class="cosmic-subcard p-3.5 space-y-1.5">
                    <div class="font-display font-bold text-neonPurple text-sm">2. Vis-Viva Orbital Mechanics &amp; Hohmann TOF</div>
                    <div class="text-amber-300 bg-cosmos/80 p-2 rounded border border-indigo-500/30">
                        v = √[ μ_☉ · (2 / r - 1 / a_trans) ] • T_TOF = π √(a_trans³ / μ_☉)
                    </div>
                    <p class="text-indigo-200 font-sans leading-relaxed">
                        Uses heliocentric gravitational parameter $\mu_{\odot} = 1.3271244 \times 10^{20}\text{ m}^3/\text{s}^2$ and semi-major transfer axis $a_{\text{trans}} = (r_{\oplus} + r_{\text{target}})/2$.
                    </p>
                </div>

                <div class="cosmic-subcard p-3.5 space-y-1.5">
                    <div class="font-display font-bold text-neonGreen text-sm">3. Inverse-Square Solar Irradiance &amp; Power</div>
                    <div class="text-amber-300 bg-cosmos/80 p-2 rounded border border-indigo-500/30">
                        S(r) = S_0 / r_AU²  •  P_gen = S(r) · A_arr · η_cell + P_MMRTG
                    </div>
                    <p class="text-indigo-200 font-sans leading-relaxed">
                        At $1\text{ AU}$, $S_0 = 1361.0\text{ W/m}^2$. At Mars ($1.524\text{ AU}$), flux drops to $586.2\text{ W/m}^2$; at Europa ($5.204\text{ AU}$), $50.25\text{ W/m}^2$; at Titan ($9.583\text{ AU}$), $14.82\text{ W/m}^2$.
                    </p>
                </div>

                <div class="cosmic-subcard p-3.5 space-y-1.5">
                    <div class="font-display font-bold text-neonAmber text-sm">4. Hypervelocity Asteroid Impact &amp; Surface Gravity</div>
                    <div class="text-amber-300 bg-cosmos/80 p-2 rounded border border-indigo-500/30">
                        KE_imp = ½ m_ast · v_rel²  •  z(t) = v_z0·t - ½ g_planet·t²
                    </div>
                    <p class="text-indigo-200 font-sans leading-relaxed">
                        Asteroid impacts transfer kinetic energy proportional to relative closing velocity squared. Surface descent &amp; 1st-person EVA jumps integrate exact NASA surface gravities ($1.625, 3.721, 1.315, 1.352\text{ m/s}^2$).
                    </p>
                </div>
            </div>

            <div class="cosmic-subcard p-3.5">
                <div class="font-display font-bold text-xs text-neonCyan mb-2">NASA PLANETARY FACT SHEET REFERENCE TABLE</div>
                <div class="overflow-x-auto">
                    <table class="w-full text-left font-mono text-[11px] text-indigo-200">
                        <thead class="text-neonCyan border-b border-indigo-500/40">
                            <tr>
                                <th class="py-1">Target World</th>
                                <th>Semi-Major Axis (AU)</th>
                                <th>Surface Gravity (m/s²)</th>
                                <th>Escape Vel (km/s)</th>
                                <th>Solar Flux (W/m²)</th>
                                <th>Mission Δv (m/s)</th>
                            </tr>
                        </thead>
                        <tbody class="divide-y divide-indigo-500/20">
                            <tr><td class="py-1 text-white">🌕 Moon (Shackleton)</td><td>1.0000 AU (384,400 km)</td><td>1.625 m/s²</td><td>2.380 km/s</td><td>1361.0 W/m²</td><td>5,940 m/s</td></tr>
                            <tr><td class="py-1 text-white">🔴 Mars (Jezero)</td><td>1.52368 AU</td><td>3.72076 m/s²</td><td>5.027 km/s</td><td>586.2 W/m²</td><td>8,650 m/s</td></tr>
                            <tr><td class="py-1 text-white">🧊 Europa (Conamara)</td><td>5.20440 AU</td><td>1.315 m/s²</td><td>2.025 km/s</td><td>50.25 W/m²</td><td>14,320 m/s</td></tr>
                            <tr><td class="py-1 text-white">🪐 Titan (Kraken Mare)</td><td>9.58260 AU</td><td>1.352 m/s²</td><td>2.639 km/s</td><td>14.82 W/m²</td><td>15,890 m/s</td></tr>
                        </tbody>
                    </table>
                </div>
            </div>
        </div>
    </div>

    <!-- =====================================================================
         15-SECOND JUDGE GUIDE MODAL
         ===================================================================== -->
    <div id="judge-modal" class="hidden fixed inset-0 z-50 bg-cosmos/85 backdrop-blur-md flex items-center justify-center p-4">
        <div class="cosmic-panel max-w-3xl w-full p-6 border-2 border-neonCyan space-y-4">
            <div class="flex items-center justify-between border-b border-indigo-500/40 pb-3">
                <div>
                    <span class="text-xs font-mono text-neonCyan">THEPIONEERSX // SYS-VER 11.0 JUDGE &amp; OPERATOR BRIEFING</span>
                    <h2 class="font-display text-xl font-black text-white mt-0.5">📘 HOW TO OPERATE &amp; TEST ALL 6 STAGES</h2>
                </div>
                <button onclick="closeJudgeModal()" class="px-3 py-1.5 rounded-lg bg-indigo-950 hover:bg-neonRed text-white font-mono text-xs">✕ CLOSE</button>
            </div>

            <div class="grid grid-cols-1 sm:grid-cols-2 gap-3 text-xs leading-relaxed">
                <div class="cosmic-subcard p-3.5">
                    <div class="font-display font-bold text-neonCyan text-sm mb-1">1. NASA Physics &amp; Auto-Build</div>
                    <p class="text-indigo-200">Select <strong>Moon</strong>, <strong>Mars</strong>, <strong>Europa</strong>, or <strong>Titan</strong> and click <strong>✨ AUTO-BUILD NASA OPTIMAL SHIP</strong> to see live Tsiolkovsky $\Delta v$, Vis-Viva velocity, and Inverse-Square Solar Flux equations turn green.</p>
                </div>
                <div class="cosmic-subcard p-3.5">
                    <div class="font-display font-bold text-neonRed text-sm mb-1">2. Lethal 3D Asteroids (Stages 2 &amp; 3)</div>
                    <p class="text-indigo-200">In <strong>Manual (WASD)</strong> mode, steer with <strong>W, A, S, D</strong> or fire defense lasers with <strong>[E]</strong> to avoid incoming 3D asteroids! If an asteroid collides and Hull hits 0%, the <strong>Loss of Vehicle (LOV)</strong> screen triggers.</p>
                </div>
                <div class="cosmic-subcard p-3.5">
                    <div class="font-display font-bold text-neonAmber text-sm mb-1">3. Planetary Landing &amp; 6-Wheel Rover</div>
                    <p class="text-indigo-200">In <strong>Stage 4</strong>, retro-brake (`SPACE`) onto the Landing Zone under exact planetary gravity. In <strong>Stage 5</strong>, drive the 6-wheeled Rover (`WASD`) to 3 core-drill sites (`E`) using the live Radar Mini-Map.</p>
                </div>
                <div class="cosmic-subcard p-3.5">
                    <div class="font-display font-bold text-neonPurple text-sm mb-1">4. True 1st-Person Astronaut Visor (Stage 6)</div>
                    <p class="text-indigo-200">Experience <strong>Stage 6</strong> from <strong>inside the Astronaut's Spacesuit Helmet Visor</strong> with 1st-person gloved hands, wrist radar cuff, low-gravity parabolic jumps (`SPACE`), crystal pickup, and flag planting (`E`)!</p>
                </div>
            </div>

            <div class="flex flex-wrap items-center justify-between gap-2 pt-2 border-t border-indigo-500/30">
                <span class="text-xs text-indigo-300">💡 Click any camera button (`3.Asteroids`, `7.1st-Person EVA`) in the top-right viewport to preview immediately!</span>
                <div class="flex items-center gap-2">
                    <button onclick="closeJudgeModal(); autoBuildWinningShip(true); startLaunchFlight(false);" class="px-4 py-2 rounded-lg bg-neonCyan text-cosmos font-display font-bold text-xs">
                        🎮 LAUNCH MANUAL (WASD) &rarr;
                    </button>
                    <button onclick="closeJudgeModal(); autoBuildWinningShip(true); startLaunchFlight(true);" class="px-4 py-2 rounded-lg bg-neonGreen text-cosmos font-display font-bold text-xs">
                        🤖 LAUNCH AI AUTO-PILOT &rarr;
                    </button>
                </div>
            </div>
        </div>
    </div>

    <!-- =====================================================================
         COMPLETE NASA ASTRODYNAMICS, LETHAL 3D ASTEROIDS & 1ST-PERSON EVA ENGINE
         ===================================================================== -->
    <script>
        /*********************************************************************
         * 1. EXACT NASA JPL PHYSICAL CONSTANTS & PLANETARY FACT SHEET DATA
         *********************************************************************/
        const NASA_CONST = {
            g0: 9.80665,                  // Standard Earth gravity (m/s^2)
            S0: 1361.0,                   // Solar constant at 1 AU (W/m^2)
            AU_M: 1.495978707e11,         // 1 Astronomical Unit (m)
            MU_SUN: 1.32712440018e20,     // Heliocentric gravitational parameter (m^3/s^2)
            MU_EARTH: 3.986004418e14      // Geocentric gravitational parameter (m^3/s^2)
        };

        /*********************************************************************
         * 1B. REAL 4K HDR NASA PUBLIC DOMAIN IMAGERY PIPELINE
         *     Fetches live photography from NASA Image & Video Library API
         *     and NASA DSCOVR EPIC API + generates hyper-detailed PBR
         *     offscreen relief textures so visuals are always 4K HDR real-life
         *********************************************************************/
        const NASA_MEDIA = {
            images: {},
            credits: {},
            pbrTextures: {},
            initialized: false
        };

        const NASA_IMAGE_QUERIES = [
            { key: 'earth_pad', q: 'Kennedy Space Center launch pad 39A sunrise aerial', fallbackCredit: 'NASA KSC LC-39A // 4K HDR Aerial Pad & Coastal Horizon' },
            { key: 'earth_orbit', q: 'ISS Earth horizon limb atmosphere space', fallbackCredit: 'NASA ISS / DSCOVR EPIC // Earth Limb Photorealistic Feed' },
            { key: 'jwst_space', q: 'JWST Carina Nebula Cosmic Cliffs', fallbackCredit: 'NASA/ESA/CSA JWST NIRCam // Carina Nebula Deep Space' },
            { key: 'moon_orbit', q: 'LRO Lunar South Pole Shackleton crater', fallbackCredit: 'NASA LRO LROC // Lunar South Pole Orbital Mosaic' },
            { key: 'moon_surface', q: 'Apollo 17 lunar surface panorama Taurus Littrow', fallbackCredit: 'NASA Apollo 17 Hasselblad // Taurus-Littrow Regolith' },
            { key: 'mars_orbit', q: 'Mars globe Viking orbiter mosaic', fallbackCredit: 'NASA JPL Viking / MRO // Mars True-Color Planetary Globe' },
            { key: 'mars_surface', q: 'Curiosity Mars Gale crater surface panorama', fallbackCredit: 'NASA JPL-Caltech Curiosity Mastcam // Jezero/Gale Surface' },
            { key: 'europa_orbit', q: 'Europa surface Galileo spacecraft mosaic', fallbackCredit: 'NASA JPL Galileo SSI // Europa Conamara Chaos Crust' },
            { key: 'europa_surface', q: 'Europa Conamara Chaos ridges Galileo', fallbackCredit: 'NASA JPL Galileo // Europa Cryo-Tectonic Ice Surface' },
            { key: 'titan_orbit', q: 'Cassini Titan true color atmosphere', fallbackCredit: 'NASA/JPL-Caltech/SSI Cassini // Titan Haze Globe' },
            { key: 'titan_surface', q: 'Huygens Titan surface landing rocks', fallbackCredit: 'ESA/NASA/JPL Huygens DISR // Titan Kraken Mare Shoreline' }
        ];

        function buildHighResPBRTextures() {
            const presets = [
                { key: 'earth_pad', base: [26, 44, 72], ridge: [68, 96, 128], sky: ['#0a1938', '#1d4ed8', '#93c5fd'], snow: true },
                { key: 'earth_orbit', base: [12, 42, 92], ridge: [56, 142, 214], sky: ['#02040a', '#071430', '#1e40af'], globe: true },
                { key: 'jwst_space', base: [14, 10, 32], ridge: [110, 58, 130], sky: ['#03040c', '#120a2a', '#241038'], nebula: true },
                { key: 'moon_surface', base: [74, 78, 86], ridge: [132, 138, 148], sky: ['#020308', '#060913', '#0e1322'], craters: true },
                { key: 'mars_surface', base: [138, 68, 42], ridge: [198, 114, 72], sky: ['#2b130d', '#6e321e', '#c97b53'], craters: true },
                { key: 'europa_surface', base: [72, 104, 142], ridge: [178, 212, 242], sky: ['#030714', '#091632', '#1d3b6e'], ridges: true },
                { key: 'titan_surface', base: [128, 76, 32], ridge: [196, 132, 62], sky: ['#241408', '#5e3614', '#b87634'], craters: true }
            ];

            presets.forEach(pr => {
                const c = document.createElement('canvas');
                c.width = 1280;
                c.height = 720;
                const ctx = c.getContext('2d');

                // Atmospheric / Space Sky Gradient
                const skyGrad = ctx.createLinearGradient(0, 0, 0, 360);
                skyGrad.addColorStop(0, pr.sky[0]);
                skyGrad.addColorStop(0.65, pr.sky[1]);
                skyGrad.addColorStop(1, pr.sky[2]);
                ctx.fillStyle = skyGrad;
                ctx.fillRect(0, 0, 1280, 380);

                if (pr.nebula) {
                    // Photorealistic JWST Cosmic Cliffs dust pillars & star clusters
                    for (let i = 0; i < 18; i++) {
                        const nx = (i * 83) % 1280;
                        const ny = 180 + Math.sin(i * 1.4) * 140;
                        const rg = ctx.createRadialGradient(nx, ny, 10, nx, ny, 240);
                        rg.addColorStop(0, i % 2 === 0 ? 'rgba(217, 119, 6, 0.35)' : 'rgba(56, 189, 248, 0.32)');
                        rg.addColorStop(0.5, 'rgba(126, 34, 206, 0.18)');
                        rg.addColorStop(1, 'rgba(3, 4, 12, 0)');
                        ctx.fillStyle = rg;
                        ctx.fillRect(0, 0, 1280, 720);
                    }
                    for (let s = 0; s < 320; s++) {
                        const sx = (s * 173) % 1280;
                        const sy = (s * 97) % 720;
                        const rad = (s % 9 === 0) ? 2.1 : 1.0;
                        ctx.fillStyle = s % 5 === 0 ? '#bae6fd' : '#fde68a';
                        ctx.beginPath();
                        ctx.arc(sx, sy, rad, 0, Math.PI * 2);
                        ctx.fill();
                    }
                } else {
                    // Multi-octave fBm Mountain / Crater Horizon Ridge
                    ctx.beginPath();
                    ctx.moveTo(0, 380);
                    for (let x = 0; x <= 1280; x += 8) {
                        const h1 = Math.sin(x * 0.0055) * 48;
                        const h2 = Math.sin(x * 0.017 + 1.4) * 21;
                        const h3 = Math.cos(x * 0.048) * 8;
                        ctx.lineTo(x, 325 - Math.abs(h1 + h2 + h3));
                    }
                    ctx.lineTo(1280, 380);
                    ctx.closePath();
                    ctx.fillStyle = `rgb(${pr.ridge[0]}, ${pr.ridge[1]}, ${pr.ridge[2]})`;
                    ctx.fill();

                    if (pr.snow) {
                        // Snow-capped Earth alpine ridges
                        ctx.strokeStyle = 'rgba(241, 245, 249, 0.72)';
                        ctx.lineWidth = 3;
                        ctx.beginPath();
                        for (let x = 0; x <= 1280; x += 8) {
                            const h1 = Math.sin(x * 0.0055) * 48;
                            const h2 = Math.sin(x * 0.017 + 1.4) * 21;
                            const h3 = Math.cos(x * 0.048) * 8;
                            const y = 325 - Math.abs(h1 + h2 + h3);
                            if (x === 0) ctx.moveTo(x, y);
                            else ctx.lineTo(x, y);
                        }
                        ctx.stroke();
                    }

                    // Photorealistic Regolith / Terrain Plane with Directional Sunlit Normal Shading
                    const gGrad = ctx.createLinearGradient(0, 325, 0, 720);
                    gGrad.addColorStop(0, `rgb(${pr.ridge[0]}, ${pr.ridge[1]}, ${pr.ridge[2]})`);
                    gGrad.addColorStop(0.35, `rgb(${pr.base[0]}, ${pr.base[1]}, ${pr.base[2]})`);
                    gGrad.addColorStop(1, `rgb(${Math.max(8, pr.base[0] - 48)}, ${Math.max(8, pr.base[1] - 48)}, ${Math.max(12, pr.base[2] - 48)})`);
                    ctx.fillStyle = gGrad;
                    ctx.fillRect(0, 325, 1280, 395);

                    // Perspective Rocks, Boulders & Impact Craters with Sun-Rim Highlights
                    for (let i = 0; i < 160; i++) {
                        const t = Math.pow((i % 40) / 40, 1.5);
                        const ry = 335 + t * 375;
                        const rx = ((i * 193) % 1280);
                        const rw = 6 + t * 44;
                        const rh = rw * 0.36;

                        // Cast Shadow
                        ctx.fillStyle = 'rgba(4, 6, 15, 0.45)';
                        ctx.beginPath();
                        ctx.ellipse(rx + rw * 0.25, ry + rh * 0.25, rw, rh, 0, 0, Math.PI * 2);
                        ctx.fill();

                        // Sunlit Rim
                        ctx.fillStyle = `rgba(${Math.min(255, pr.ridge[0] + 25)}, ${Math.min(255, pr.ridge[1] + 25)}, ${Math.min(255, pr.ridge[2] + 25)}, 0.42)`;
                        ctx.beginPath();
                        ctx.ellipse(rx - rw * 0.1, ry - rh * 0.1, rw * 0.85, rh * 0.8, 0, 0, Math.PI * 2);
                        ctx.fill();
                    }
                }
                NASA_MEDIA.pbrTextures[pr.key] = c;
            });
        }

        async function initRealNasaImageryPipeline() {
            if (NASA_MEDIA.initialized) return;
            NASA_MEDIA.initialized = true;
            buildHighResPBRTextures();

            // Set initial fallback credits immediately
            NASA_IMAGE_QUERIES.forEach(item => {
                NASA_MEDIA.credits[item.key] = item.fallbackCredit;
            });
            updateNasaCreditBadge();

            // 1. Live Fetch from NASA DSCOVR EPIC API for Real Full-Disk Earth Photography
            fetch('https://epic.gsfc.nasa.gov/api/natural')
                .then(r => r.ok ? r.json() : null)
                .then(data => {
                    if (Array.isArray(data) && data.length > 0) {
                        const latest = data[0];
                        const dateParts = latest.date.split(' ')[0].split('-');
                        const imgUrl = `https://epic.gsfc.nasa.gov/archive/natural/${dateParts[0]}/${dateParts[1]}/${dateParts[2]}/png/${latest.image}.png`;
                        loadCorsImage('earth_epic', imgUrl, `NASA DSCOVR EPIC // Live Earth (${latest.date.split(' ')[0]})`);
                    }
                })
                .catch(() => {});

            // 2. Live Fetch from NASA Image and Video Library API (images-api.nasa.gov) for all 11 scenes
            const excludePeopleRegex = /\b(people|person|crowd|group|portrait|ceremony|administrator|briefing|press|standing|visitors|tour|selfie|crewmember|astronauts|team|officials|posing|speaker)\b/i;
            NASA_IMAGE_QUERIES.forEach(item => {
                const url = `https://images-api.nasa.gov/search?q=${encodeURIComponent(item.q)}&media_type=image&page_size=12`;
                fetch(url)
                    .then(r => r.ok ? r.json() : null)
                    .then(json => {
                        const items = json?.collection?.items || [];
                        for (const entry of items) {
                            const link = entry?.links?.find(l => l.render === 'image' && l.href);
                            const meta = entry?.data?.[0];
                            const fullText = `${meta?.title || ''} ${meta?.description || ''}`;
                            if (link && link.href && !excludePeopleRegex.test(fullText)) {
                                // Upgrade thumb URL to medium/large NASA asset when available
                                const hiResHref = link.href.replace('~thumb.jpg', '~medium.jpg');
                                const nasaId = meta?.nasa_id || 'NASA-JPL';
                                const title = (meta?.title || item.fallbackCredit).slice(0, 44);
                                loadCorsImage(item.key, hiResHref, `NASA [${nasaId}] // ${title}`);
                                break;
                            }
                        }
                    })
                    .catch(() => {});
            });
        }

        function loadCorsImage(key, url, creditText) {
            const img = new Image();
            img.crossOrigin = 'anonymous';
            img.onload = () => {
                NASA_MEDIA.images[key] = img;
                NASA_MEDIA.credits[key] = creditText;
                updateNasaCreditBadge();
            };
            img.src = url;
        }

        function getActiveMediaKey() {
            const sc = state.cockpitScene;
            const pid = state.planetId;
            if (sc === 'pad' || sc === 'ascent') return 'earth_pad';
            if (sc === 'space') return 'jwst_space';
            if (sc === 'orbit') return `${pid}_orbit`;
            return `${pid}_surface`;
        }

        function updateNasaCreditBadge() {
            const el = document.getElementById('nasa-imagery-credit-text');
            if (!el) return;
            const key = getActiveMediaKey();
            const credit = NASA_MEDIA.credits[key] || 'NASA JPL 4K HDR ARCHIVE // ACTIVE';
            const isLivePhoto = Boolean(NASA_MEDIA.images[key]);
            el.textContent = isLivePhoto
                ? `📡 LIVE NASA PHOTO LOCKED: ${credit}`
                : `📡 4K HDR PBR ARCHIVE: ${credit}`;
        }

        function drawHdrPhotorealisticBackdrop(ctx, w, h, mediaKey, panX = 0, panY = 0, zoom = 1.18) {
            const liveImg = NASA_MEDIA.images[mediaKey];
            const pbrFallback = NASA_MEDIA.pbrTextures[mediaKey] || NASA_MEDIA.pbrTextures.moon_surface;
            const src = (liveImg && liveImg.complete && liveImg.naturalWidth > 0) ? liveImg : pbrFallback;

            ctx.save();
            // 4K HDR ACES Filmic Contrast, Saturation & Crisp Tone Mapping
            ctx.filter = 'contrast(1.14) saturate(1.18) brightness(1.04)';

            if (src) {
                const sw = src.naturalWidth || src.width || 1280;
                const sh = src.naturalHeight || src.height || 720;
                const scale = Math.max(w / sw, h / sh) * zoom;
                const dw = sw * scale;
                const dh = sh * scale;
                const maxPanX = Math.max(0, (dw - w) * 0.48);
                const maxPanY = Math.max(0, (dh - h) * 0.48);
                const clampedX = Math.max(-maxPanX, Math.min(maxPanX, panX));
                const clampedY = Math.max(-maxPanY, Math.min(maxPanY, panY));
                const dx = (w - dw) * 0.5 + clampedX;
                const dy = (h - dh) * 0.5 + clampedY;
                ctx.drawImage(src, dx, dy, dw, dh);
            }
            ctx.restore();

            // Subtle HDR Atmospheric Rim & Vignette Depth Overlay
            const hdrRim = ctx.createRadialGradient(w * 0.5, h * 0.42, w * 0.22, w * 0.5, h * 0.5, w * 0.72);
            hdrRim.addColorStop(0, 'rgba(255, 255, 255, 0.03)');
            hdrRim.addColorStop(0.75, 'rgba(4, 6, 17, 0.14)');
            hdrRim.addColorStop(1, 'rgba(4, 6, 17, 0.52)');
            ctx.fillStyle = hdrRim;
            ctx.fillRect(0, 0, w, h);
        }

        const PLANETS = {
            moon: {
                id: 'moon',
                name: 'Moon (Shackleton)',
                icon: '🌕',
                rAU: 1.0000,
                distKm: 384400,
                distText: '384,400 km (1.00 AU)',
                gravity: 1.625,
                escapeVelKmS: 2.38,
                tofDays: 3.5,
                budgetCap: 430,
                reqDeltaV: 5940,
                reqPowerW: 320,
                reqShieldKrad: 30,
                skyTop: '#030511',
                skyBot: '#0B132B',
                horizonGlow: '#38BDF8',
                surfaceColor: '#475569',
                ridgeColor: '#1E293B',
                craterColor: '#0F172A',
                siteName: 'SHACKLETON SOUTH RIM',
                sampleName: 'Lunar Polar Water-Ice Core',
                hint: '🌕 NASA Lunar Fact Sheet: g = 1.625 m/s², Solar Irradiance = 1,361.0 W/m², TLI + Low-Lunar Orbit + Descent Δv = 5,940 m/s.',
                bestSetup: { rocket: 'falcon', power: 'solar', hull: 'lunar', payload: 'rover', propPct: 85 }
            },
            mars: {
                id: 'mars',
                name: 'Mars (Jezero)',
                icon: '🔴',
                rAU: 1.52368,
                distKm: 225000000,
                distText: '1.524 AU (225M km)',
                gravity: 3.72076,
                escapeVelKmS: 5.03,
                tofDays: 259,
                budgetCap: 540,
                reqDeltaV: 8650,
                reqPowerW: 410,
                reqShieldKrad: 60,
                skyTop: '#1A0B18',
                skyBot: '#4A1D1F',
                horizonGlow: '#F97316',
                surfaceColor: '#9A3412',
                ridgeColor: '#7C2D12',
                craterColor: '#431407',
                siteName: 'JEZERO RIVER DELTA',
                sampleName: 'Martian Smectite Biosignature Core',
                hint: '🔴 NASA Mars Fact Sheet: a = 1.52368 AU, g = 3.72076 m/s², Solar Flux = 586.2 W/m² (43.1% of Earth), Hohmann TOF = 259 days.',
                bestSetup: { rocket: 'vulcan', power: 'rosa', hull: 'aeroshell', payload: 'rover', propPct: 86 }
            },
            europa: {
                id: 'europa',
                name: 'Europa (Chaos)',
                icon: '🧊',
                rAU: 5.2044,
                distKm: 628300000,
                distText: '5.204 AU (628M km)',
                gravity: 1.315,
                escapeVelKmS: 2.025,
                tofDays: 997,
                budgetCap: 690,
                reqDeltaV: 14320,
                reqPowerW: 520,
                reqShieldKrad: 90,
                skyTop: '#04071A',
                skyBot: '#0F1E46',
                horizonGlow: '#00F0FF',
                surfaceColor: '#1E3A8A',
                ridgeColor: '#1D4ED8',
                craterColor: '#172554',
                siteName: 'CONAMARA CHAOS ICE',
                sampleName: 'Subsurface Europan Brine Crystal',
                hint: '🧊 NASA Europa Fact Sheet: a = 5.2044 AU, g = 1.315 m/s², Solar Flux = 50.25 W/m² (3.7% of Earth), Jovian Radiation = 5,400 krad.',
                bestSetup: { rocket: 'starship', power: 'mmrtg', hull: 'radvault', payload: 'drill', propPct: 90 }
            },
            titan: {
                id: 'titan',
                name: 'Titan (Kraken)',
                icon: '🪐',
                rAU: 9.5826,
                distKm: 1275000000,
                distText: '9.583 AU (1.28B km)',
                gravity: 1.352,
                escapeVelKmS: 2.639,
                tofDays: 2210,
                budgetCap: 740,
                reqDeltaV: 15890,
                reqPowerW: 560,
                reqShieldKrad: 90,
                skyTop: '#160D29',
                skyBot: '#422006',
                horizonGlow: '#F59E0B',
                surfaceColor: '#78350F',
                ridgeColor: '#451A03',
                craterColor: '#271001',
                siteName: 'KRAKEN MARE SHORELINE',
                sampleName: 'Liquid Methane-Tholin Core',
                hint: '🪐 NASA Titan Fact Sheet: a = 9.5826 AU, g = 1.352 m/s², Solar Flux = 14.82 W/m² (1.09% of Earth), 94K Cryo N₂/CH₄ Atmosphere.',
                bestSetup: { rocket: 'starship', power: 'mmrtg', hull: 'radvault', payload: 'biolab', propPct: 92 }
            }
        };

        const EQUIPMENT = {
            rocket: {
                label: '🚀 1. Launch Vehicle & Upper Stage (I_sp & Injection Δv)',
                options: [
                    { id: 'falcon', name: 'Falcon Heavy + Merlin Vac', cost: 95, isp: 348, c3Boost: 3450, dryMassKg: 1150, sub: 'I_sp: 348s • Lunar C3' },
                    { id: 'vulcan', name: 'SLS Block 1B / Centaur V', cost: 165, isp: 452, c3Boost: 5200, dryMassKg: 1320, sub: 'I_sp: 452s (LH2/LOX)' },
                    { id: 'starship', name: 'Starship + Nuclear Thermal', cost: 245, isp: 850, c3Boost: 7850, dryMassKg: 1550, sub: 'I_sp: 850s • Deep Space' }
                ]
            },
            power: {
                label: '⚡ 2. Power Bus (Solar Array Area vs. Pu-238 MMRTG)',
                options: [
                    { id: 'solar', name: 'GaAs Triple-Junction Array', cost: 50, areaM2: 1.65, eff: 0.30, rtgW: 0, sub: '1.65 m² • η=30% Solar' },
                    { id: 'rosa', name: 'Roll-Out ROSA Array (4.2 m²)', cost: 110, areaM2: 4.20, eff: 0.33, rtgW: 80, sub: '4.2 m² • Mars Optimal' },
                    { id: 'mmrtg', name: 'Dual Pu-238 MMRTG Nuclear', cost: 195, areaM2: 0.50, eff: 0.30, rtgW: 860, sub: '860 W Constant Nuclear' }
                ]
            },
            hull: {
                label: '🛡️ 3. Radiation Vault, TPS Heatshield & Lander Legs',
                options: [
                    { id: 'lunar', name: 'Al-Li Whipple Shield + Legs', cost: 45, shieldScore: 35, addMassKg: 120, sub: '$45M • 15 krad Rated' },
                    { id: 'aeroshell', name: 'PICA-X TPS + Skycrane Radar', cost: 100, shieldScore: 68, addMassKg: 260, sub: '$100M • 50 krad Rated' },
                    { id: 'radvault', name: 'Tantalum Rad-Vault + Cryo-Legs', cost: 175, shieldScore: 98, addMassKg: 420, sub: '$175M • 5,400 krad Vault' }
                ]
            },
            payload: {
                label: '👨‍🚀 4. Surface Rover & 1st-Person xEMU Spacesuit Suite',
                options: [
                    { id: 'scout', name: 'Micro-Rover + xEMU Visor', cost: 60, sciPoints: 320, pwrDrawW: 160, dishGainDb: 36, sub: '$60M • 320 Gb Sci' },
                    { id: 'rover', name: '6-Wheel Perseverance + xEMU', cost: 110, sciPoints: 480, pwrDrawW: 210, dishGainDb: 42, sub: '$110M • 480 Gb Sci' },
                    { id: 'drill', name: 'Cryo-Drill Rover + Jetpack Suit', cost: 140, sciPoints: 580, pwrDrawW: 230, dishGainDb: 46, sub: '$140M • 580 Gb Sci' },
                    { id: 'biolab', name: 'Mass-Spec Lab Rover + Exo-Suit', cost: 165, sciPoints: 690, pwrDrawW: 250, dishGainDb: 49, sub: '$165M • 690 Gb Sci' }
                ]
            }
        };

        const FLIGHT_EVENTS = [
            {
                stageNum: 1,
                shortLabel: '1.Liftoff',
                name: 'STAGE 1 OF 6 // LC-39A LIFTOFF & MAX-Q ASCENT',
                scene: 'ascent',
                taskText: '🕹️ DRAG FLIGHT YOKE OR [WASD] INTO TARGET RING & PUSH THROTTLE [SPACE]!',
                altitudeBase: 18.5,
                prompt: 'Clearing the Earth mountain range through Max-Q! Grab the interactive Cockpit Flight Yoke (or use W A S D) to keep your nose inside the cyan trajectory ring while pushing Throttle to 100%:',
                choices: [
                    { label: '🛡️ Closed-Loop TVC Gravity Turn & Max-Q Throttle [E]', effectText: 'Minimizes aerodynamic q·α load (+130 Gb Science, -4% Fuel)', hullDelta: 0, fuelDelta: -4, sciDelta: 130 },
                    { label: '🔥 Full 109% Rated Thrust Direct Insertion', effectText: 'Saves upper-stage Δv (+170 Gb Science, -4% Hull, -6% Fuel)', hullDelta: -4, fuelDelta: -6, sciDelta: 170 }
                ]
            },
            {
                stageNum: 2,
                shortLabel: '2.Asteroids',
                name: 'STAGE 2 OF 6 // LETHAL ASTEROID BELT & VIS-VIVA CRUISE',
                scene: 'space',
                taskText: '☄️ DANGER: DODGE RED 3D ASTEROIDS WITH YOKE/[WASD] OR VAPORIZE WITH [E]!',
                altitudeBase: 54000000,
                prompt: '⚠️ LETHAL ASTEROID SWARM AHEAD! Steer the Cockpit Yoke (or W A S D) around incoming 3D asteroids or click the [PULSE LASER] button on your dashboard ([E])! Direct impact will crack your canopy and cause Loss of Vehicle!',
                choices: [
                    { label: '⚡ Fire Pulse Defense Lasers & Execute RCS Dodge [E]', effectText: 'Clears asteroid corridor (+160 Gb Science, -4% Fuel)', hullDelta: 0, fuelDelta: -4, sciDelta: 160 },
                    { label: '📡 Fly High-Gain Radar Skim Near C-Type Asteroid', effectText: 'High-value regolith radar (+220 Gb Science, -8% Hull)', hullDelta: -8, fuelDelta: -3, sciDelta: 220 }
                ]
            },
            {
                stageNum: 3,
                shortLabel: '3.Orbit',
                name: 'STAGE 3 OF 6 // ORBITAL CAPTURE SPEED GATE (DECELERATE NOW!)',
                scene: 'orbit',
                taskText: '🛑 REDUCE SPEED! HOLD [Q] OR DRAG RETRO-LEVER DOWN TO 2.2–4.2 km/s TO CATCH ORBIT!',
                altitudeBase: 480,
                prompt: '🚨 HYPERBOLIC APPROACH AT 11.4 km/s! It depends on you to reduce speed before Periapsis runs out! Hold [Q] (or pull the Cockpit Throttle Lever down / click RETRO-BRAKE on the dash) to slow down into the 2.2–4.2 km/s Orbital Capture Window—or you will overshoot the planet!',
                choices: [
                    { label: '🔥 Lock Periapsis Retro-Capture Burn (Once Speed < 4.2 km/s) [E]', effectText: 'Captures into low descent orbit (+190 Gb Science, -6% Fuel)', hullDelta: 0, fuelDelta: -6, sciDelta: 190 },
                    { label: '🌬️ Aerocapture Upper-Atmosphere Braking Pass', effectText: 'Uses atmospheric drag to slow down (+230 Gb Science, -6% Hull)', hullDelta: -6, fuelDelta: -2, sciDelta: 230 }
                ]
            },
            {
                stageNum: 4,
                shortLabel: '4.Landing',
                name: 'STAGE 4 OF 6 // 3RD-PERSON LANDING & TRACK UNLOADING SEQUENCE',
                scene: 'landing',
                taskText: '🛬 GUIDE 3RD-PERSON LANDER TO PAD [WASD], RETRO-BRAKE [SPACE] & UNLOAD ROVER TRACK!',
                altitudeBase: 1.4,
                prompt: '3rd-Person Exterior Landing Camera engaged! Steer your metallic NASA Lander over Landing Zone Alpha using the Yoke or W A S D, hold SPACE to cushion your descent rate, and watch the hydraulic cargo track unfold to unload the 6-Wheeled Rover:',
                choices: [
                    { label: '🛬 Touchdown, Lower Steel Cargo Track & Roll Out Rover [E]', effectText: 'Full 3rd-person track unloading (+240 Gb Science, -5% Fuel)', hullDelta: 0, fuelDelta: -5, sciDelta: 240 },
                    { label: '⚡ Precision Sky-Hover & Rapid Track Deployment', effectText: 'Prime geological delta site (+280 Gb Science, -8% Fuel)', hullDelta: 0, fuelDelta: -8, sciDelta: 280 }
                ]
            },
            {
                stageNum: 5,
                shortLabel: '5.Rover',
                name: 'STAGE 5 OF 6 // 6-WHEELED ROVER TRAVERSE & CORE DRILLING',
                scene: 'rover',
                taskText: '🛞 DRIVE ROVER WITH YOKE/[WASD] TO 3 SCIENCE SITES & PRESS [E] TO DRILL!',
                altitudeBase: 0.0,
                prompt: 'Rover unloaded from the Lander track! Drive the photorealistic 6-Wheeled Perseverance/LTV Rover (piloted by your xEMU Astronaut) across craters using the Yoke or W A S D to the 3 glowing Science Waypoints and press E to drill:',
                choices: [
                    { label: '🛞 Drill All 3 Subsurface Cores & Step Out for Astronaut EVA [E]', effectText: 'Extracts 3 pristine cores (+290 Gb Science, -4% Battery)', hullDelta: 0, fuelDelta: -4, sciDelta: 290 },
                    { label: '🔬 Deep Raman Laser Spectroscopy Traverse', effectText: 'Full organic biosignature map (+330 Gb Science, -6% Battery)', hullDelta: 0, fuelDelta: -6, sciDelta: 330 }
                ]
            },
            {
                stageNum: 6,
                shortLabel: '6.Astronaut EVA',
                name: 'STAGE 6 OF 6 // ASTRONAUT EVA WALK, CRYSTAL & FLAG PLANTING',
                scene: 'eva',
                taskText: '👨‍🚀 WALK [WASD], LOW-G JUMP [SPACE], COLLECT CRYSTAL & PLANT FLAG [E] (1ST/3RD CAM)!',
                altitudeBase: 0.0,
                prompt: 'Step onto the planetary regolith in your pressurized NASA xEMU Spacesuit! Toggle between 1st-Person Helmet Visor and 3rd-Person Astronaut Camera anytime, walk with W/S, turn with A/D, jump in low gravity with SPACE, and press E to collect the Crystal & plant ThePioneersX Flag:',
                choices: [
                    { label: '🚩 Collect Crystal Specimen & Plant "ThePioneersX" Flag [E]', effectText: 'Historic EVA Triumph (+420 Gb Science, Rank S+)', hullDelta: 0, fuelDelta: -3, sciDelta: 420 },
                    { label: '💎 Extended Low-Gravity Jetpack Survey & Sample Return', effectText: 'Maximum astrobiology archive (+460 Gb Science, -4% Suit O₂)', hullDelta: -3, fuelDelta: -4, sciDelta: 460 }
                ]
            }
        ];

        /*********************************************************************
         * 2. UNIFIED STATE OBJECT (COCKPIT YOKE, ORBIT CAPTURE, 3RD-PERSON
         *    TRACK UNLOADING, PHOTOREALISTIC ROVER & ASTRONAUT)
         *********************************************************************/
        const state = {
            planetId: 'moon',
            rocket: 'falcon',
            power: 'solar',
            hull: 'lunar',
            payload: 'rover',
            propPct: 85,

            calc: {},

            // Active Viewport Scene ('pad' | 'ascent' | 'space' | 'orbit' | 'landing' | 'rover' | 'eva')
            cockpitScene: 'pad',
            staticFireTimer: 0,
            laserPulseTimer: 0,
            hullHitTimer: 0,
            canopyCracks: [],

            // Camera Perspective ('auto' | '3rd' | '1st')
            camPerspective: 'auto',

            // Flight & Attitude Dynamics
            autopilot: false,
            pitch: 0,
            yaw: 0,
            roll: 0,
            throttle: 0,
            velocityKmS: 0,
            altitudeKm: 0,

            // Interactive Cockpit Driving Handle (Yoke), Throttle Lever & Button Hitboxes
            yokeDragActive: false,
            throttleDragActive: false,
            pointerCanvasX: 0,
            pointerCanvasY: 0,
            cockpitButtons: [],
            activeDashBtnId: null,
            activeDashBtnTimer: 0,

            // Stage 3 Real Orbital Capture Speed Gate State
            orbitVelKmS: 11.40,
            orbitTargetMinKmS: 2.20,
            orbitTargetMaxKmS: 4.20,
            periapsisTimerSec: 38.0,
            orbitCaptured: false,
            orbitCaptureHoldSec: 0,
            retroBrakingActive: false,

            // Lethal 3D Asteroid Belt State
            asteroids: [],
            asteroidsDodged: 0,
            asteroidsDestroyed: 0,
            missionFailedLOV: false,
            failureReasonText: '',

            // Stage 4 3rd-Person Powered Surface Descent & Track Unloading State
            landerX: 0.25,
            landerY: -0.18,
            landerAltM: 1400,
            descentRateMS: 14.5,
            landedOnPad: false,
            rampDeployPct: 0,        // 0 to 100% steel cargo track unfolding
            roverUnloadPct: 0,       // 0 to 100% 6-wheeled rover rolling down the track

            // Stage 5 6-Wheeled Surface Rover State
            roverX: -135,
            roverY: 30,
            roverHeading: 0,
            roverSpeedKmH: 0,
            roverWheelRot: 0,
            waypoints: [
                { id: 1, x: -35, y: -42, label: 'SITE A: CRATER RIM', collected: false },
                { id: 2, x: 65, y: 38, label: 'SITE B: ICE TRENCH', collected: false },
                { id: 3, x: 160, y: -25, label: 'SITE C: BEDROCK VENT', collected: false }
            ],
            samplesCollected: 0,

            // Stage 6 Astronaut Spacesuit EVA State (Supports both 1st-Person Visor & 3rd-Person Realistic Astronaut)
            evaCamX: 0,             // meters East/West on surface
            evaCamY: -45,           // meters North/South on surface
            evaYaw: 0,              // look heading angle in radians
            evaJumpZ: 0,            // vertical height above surface (meters)
            evaJumpVz: 0,           // vertical velocity (m/s)
            evaBobPhase: 0,         // walking stride cycle phase
            evaArmReachTimer: 0,    // animates gloved arm reaching out
            crystalCollected: false,
            flagPlanted: false,
            crystalWorld: { x: -16, y: 18 },
            flagWorld: { x: 18, y: 36 },
            landerWorld: { x: -38, y: 42 },
            roverWorld: { x: 34, y: 15 },

            // Target Trajectory Ring
            ringX: 0,
            ringY: 0,
            ringPhase: 0,
            stageProgress: 0,
            pilotBonus: 0,

            // Keyboard / Virtual Key Map
            keys: { w: false, a: false, s: false, d: false, space: false, q: false, e: false },

            // Campaign Status
            inFlight: false,
            flightStageIdx: 0,
            flightFinished: false,
            hullHealth: 100,
            fuelLeft: 100,
            totalScience: 0,

            // First-2-Runs Hand Guide
            runMode: 1,
            guideActive: true,
            guideStep: 1,
            soundOn: true,
            metSeconds: 0
        };

        const GUIDE_STEPS = [
            {
                step: 1,
                selector: '#planet-card-mars',
                title: 'STEP 1 OF 4: Select a NASA Target World',
                desc: 'Click 🔴 Mars (Jezero) or any world to load its exact JPL gravity, solar irradiance, and Hohmann transfer Δv!'
            },
            {
                step: 2,
                selector: '#btn-auto-build',
                title: 'STEP 2 OF 4: Auto-Build Optimal NASA Ship',
                desc: 'Click "✨ AUTO-BUILD NASA OPTIMAL SHIP" to satisfy Tsiolkovsky Δv, Inverse-Square Solar Power, and Radiation Shielding!'
            },
            {
                step: 3,
                selector: '#btn-launch-mission',
                title: 'STEP 3 OF 4: Launch the 6-Stage Expedition!',
                desc: 'Click "🚀 LAUNCH (MANUAL WASD)" to pilot through lethal 3D asteroids & 1st-person EVA—or click "🤖 LAUNCH AUTO-PILOT"!'
            },
            {
                step: 4,
                selector: '#flight-choice-0',
                title: 'STEP 4 OF 4: Dodge Asteroids & Walk in 1st-Person EVA',
                desc: 'Steer with W A S D, fire lasers/plant flag with [E], jump in low gravity with [SPACE], or click any of the 6 Stage Pills!'
            }
        ];

        /*********************************************************************
         * 3. WEB AUDIO SYNTHESIZER (LASERS, IMPACTS, ALARMS, THRUSTERS)
         *********************************************************************/
        let audioCtx = null;
        function playSfx(type) {
            if (!state.soundOn) return;
            try {
                if (!audioCtx) audioCtx = new (window.AudioContext || window.webkitAudioContext)();
                if (audioCtx.state === 'suspended') audioCtx.resume();
                const osc = audioCtx.createOscillator();
                const gain = audioCtx.createGain();
                osc.connect(gain);
                gain.connect(audioCtx.destination);
                const now = audioCtx.currentTime;

                if (type === 'tap') {
                    osc.frequency.setValueAtTime(780, now);
                    gain.gain.setValueAtTime(0.04, now);
                    gain.gain.exponentialRampToValueAtTime(0.001, now + 0.06);
                    osc.start(now); osc.stop(now + 0.06);
                } else if (type === 'success') {
                    osc.frequency.setValueAtTime(520, now);
                    osc.frequency.exponentialRampToValueAtTime(1120, now + 0.18);
                    gain.gain.setValueAtTime(0.06, now);
                    gain.gain.exponentialRampToValueAtTime(0.001, now + 0.19);
                    osc.start(now); osc.stop(now + 0.19);
                } else if (type === 'laser') {
                    osc.type = 'sawtooth';
                    osc.frequency.setValueAtTime(1350, now);
                    osc.frequency.exponentialRampToValueAtTime(240, now + 0.16);
                    gain.gain.setValueAtTime(0.07, now);
                    gain.gain.exponentialRampToValueAtTime(0.001, now + 0.17);
                    osc.start(now); osc.stop(now + 0.17);
                } else if (type === 'impact') {
                    osc.type = 'square';
                    osc.frequency.setValueAtTime(140, now);
                    osc.frequency.exponentialRampToValueAtTime(32, now + 0.42);
                    gain.gain.setValueAtTime(0.14, now);
                    gain.gain.exponentialRampToValueAtTime(0.001, now + 0.44);
                    osc.start(now); osc.stop(now + 0.44);
                } else if (type === 'roar') {
                    osc.type = 'sawtooth';
                    osc.frequency.setValueAtTime(98, now);
                    osc.frequency.exponentialRampToValueAtTime(46, now + 0.42);
                    gain.gain.setValueAtTime(0.08, now);
                    gain.gain.exponentialRampToValueAtTime(0.001, now + 0.44);
                    osc.start(now); osc.stop(now + 0.44);
                }
            } catch (e) {}
        }

        function toggleSound() {
            state.soundOn = !state.soundOn;
            const icon = document.getElementById('sound-icon');
            if (icon) icon.className = state.soundOn ? 'fa-solid fa-volume-high' : 'fa-solid fa-volume-xmark text-slate-500';
        }

        /*********************************************************************
         * 4. 100% ACCURATE NASA ASTRODYNAMICS EQUATIONS ENGINE
         *********************************************************************/
        function evaluateMission() {
            const p = PLANETS[state.planetId];
            const r = EQUIPMENT.rocket.options.find(x => x.id === state.rocket) || EQUIPMENT.rocket.options[0];
            const pw = EQUIPMENT.power.options.find(x => x.id === state.power) || EQUIPMENT.power.options[0];
            const h = EQUIPMENT.hull.options.find(x => x.id === state.hull) || EQUIPMENT.hull.options[0];
            const pl = EQUIPMENT.payload.options.find(x => x.id === state.payload) || EQUIPMENT.payload.options[0];

            // 1. Exact Mass Budget & Tsiolkovsky Rocket Equation:
            //    Delta_v = Isp * g0 * ln(m0 / mf) + c3Boost
            const dryMassKg = r.dryMassKg + h.addMassKg;
            const propMassKg = Math.round(dryMassKg * (state.propPct / (100 - state.propPct)) * 0.68);
            const wetMassKg = dryMassKg + propMassKg;
            const massRatio = wetMassKg / dryMassKg;
            const stageDeltaV = r.isp * NASA_CONST.g0 * Math.log(massRatio);
            const totalDeltaV = Math.round(stageDeltaV + r.c3Boost);

            // 2. Vis-Viva Heliocentric Transfer Velocity at 1 AU departure:
            //    v = sqrt(mu_Sun * (2/r1 - 1/a_trans))
            const r1 = NASA_CONST.AU_M;
            const r2 = p.rAU * NASA_CONST.AU_M;
            const aTrans = (r1 + r2) / 2;
            const visVivaMs = Math.sqrt(NASA_CONST.MU_SUN * ((2 / r1) - (1 / aTrans)));
            const visVivaKmS = (visVivaMs / 1000).toFixed(2);

            // 3. Inverse-Square Solar Irradiance & Electrical Power Bus:
            //    S(r) = S0 / r_AU^2 ; P_gen = S(r) * A * eta + P_RTG
            const solarFlux = +(NASA_CONST.S0 / (p.rAU * p.rAU)).toFixed(2);
            const solarGenW = Math.round(solarFlux * pw.areaM2 * pw.eff);
            const totalGenW = solarGenW + pw.rtgW;
            const netPowerW = totalGenW - pl.pwrDrawW + 320; // +320W fuel-cell/battery peak bus

            // 4. Friis Free-Space Path Loss (FSPL) & Shannon-Hartley Link Rate (8.4 GHz X-Band):
            const distMeters = p.distKm * 1000;
            const fsplDb = +(20 * Math.log10(distMeters) + 20 * Math.log10(8.4e9) - 147.55).toFixed(1);
            const linkKbps = Math.max(12, Math.round((pl.dishGainDb * 180) / Math.pow(p.rAU, 1.15)));

            // 5. Cost & Shielding
            const cost = r.cost + pw.cost + h.cost + pl.cost;
            const shield = h.shieldScore;

            const budgetOk = cost <= p.budgetCap;
            const fuelOk = totalDeltaV >= p.reqDeltaV;
            const powerOk = netPowerW >= p.reqPowerW;
            const shieldOk = shield >= p.reqShieldKrad;
            const allGreen = budgetOk && fuelOk && powerOk && shieldOk;

            let advice = `✅ All 4 NASA FRR checks passed! Δv = ${totalDeltaV.toLocaleString()} m/s, Power = ${netPowerW} W, Vis-Viva = ${visVivaKmS} km/s.`;
            if (!fuelOk) {
                advice = `⚠️ Tsiolkovsky Δv Deficit (${totalDeltaV} / ${p.reqDeltaV} m/s)! Upgrade Launch Vehicle or increase Propellant Mass slider.`;
            } else if (!powerOk) {
                advice = `⚠️ Solar Flux at ${p.rAU} AU is only ${solarFlux} W/m²! Equip ROSA Array or Dual Pu-238 MMRTG Nuclear.`;
            } else if (!shieldOk) {
                advice = `⚠️ Radiation/TPS Shielding (${shield} / ${p.reqShieldKrad} req) insufficient for ${p.name}! Upgrade Shielding.`;
            } else if (!budgetOk) {
                advice = `⚠️ Exceeds NASA Cost Cap by $${cost - p.budgetCap}M! Click ⚡ AUTO-FIX for optimal loadout.`;
            }

            state.calc = {
                p, r, pw, h, pl,
                dryMassKg, propMassKg, wetMassKg, massRatio,
                totalDeltaV, visVivaKmS, solarFlux, totalGenW, netPowerW,
                fsplDb, linkKbps, cost, shield,
                budgetOk, fuelOk, powerOk, shieldOk, allGreen, advice
            };
        }

        /*********************************************************************
         * 5. RENDER LEFT BUILDER UI & LIVE EQUATIONS PANEL
         *********************************************************************/
        function renderBuilderUI() {
            evaluateMission();
            const c = state.calc;

            const pGrid = document.getElementById('planet-cards-grid');
            if (pGrid) {
                pGrid.innerHTML = Object.values(PLANETS).map(pl => {
                    const active = pl.id === state.planetId;
                    return `
                        <button id="planet-card-${pl.id}" onclick="selectPlanet('${pl.id}')" class="p-2.5 rounded-xl border-2 text-left transition flex flex-col justify-between ${active ? 'border-neonCyan bg-indigo-900/50 shadow-[0_0_18px_rgba(0,240,255,0.3)]' : 'border-indigo-500/30 bg-cosmos/80 hover:border-indigo-400'}">
                            <div class="flex items-center justify-between">
                                <span class="text-xl">${pl.icon}</span>
                                <span class="text-[10px] font-mono ${active ? 'text-neonCyan font-bold' : 'text-indigo-300'}">$${pl.budgetCap}M</span>
                            </div>
                            <div class="mt-1">
                                <div class="font-display font-bold text-xs text-white leading-tight">${pl.name}</div>
                                <div class="text-[10px] font-mono text-indigo-300 truncate mt-0.5">${pl.distText}</div>
                            </div>
                        </button>
                    `;
                }).join('');
            }

            const planetPill = document.getElementById('selected-planet-pill');
            if (planetPill) planetPill.textContent = `${c.p.icon} ${c.p.name} • g = ${c.p.gravity} m/s²`;
            const planetHint = document.getElementById('planet-hint-text');
            if (planetHint) planetHint.textContent = c.p.hint;

            const eqContainer = document.getElementById('equipment-rows-container');
            if (eqContainer) {
                eqContainer.innerHTML = Object.entries(EQUIPMENT).map(([catKey, catObj]) => {
                    const colsClass = catObj.options.length === 4 ? 'grid-cols-2 sm:grid-cols-4' : 'grid-cols-3';
                    return `
                        <div class="cosmic-subcard p-2">
                            <div class="text-[11px] font-bold text-cyan-200 mb-1">${catObj.label}</div>
                            <div class="grid ${colsClass} gap-1.5">
                                ${catObj.options.map(opt => {
                                    const active = state[catKey] === opt.id;
                                    return `
                                        <button onclick="selectPart('${catKey}', '${opt.id}')" class="p-1.5 rounded-lg border text-left transition ${active ? 'border-neonCyan bg-indigo-900/60 text-white shadow-[0_0_12px_rgba(0,240,255,0.25)]' : 'border-indigo-500/30 bg-cosmos/70 text-indigo-200 hover:border-indigo-400'}">
                                            <div class="font-semibold text-[11px] leading-tight ${active ? 'text-neonCyan' : 'text-white'}">${opt.name}</div>
                                            <div class="text-[9px] font-mono text-indigo-300 mt-0.5">${opt.sub}</div>
                                        </button>
                                    `;
                                }).join('')}
                            </div>
                        </div>
                    `;
                }).join('');
            }

            // Live NASA Equations Box with plugged-in numbers
            const eqBox = document.getElementById('live-equations-box');
            if (eqBox) {
                eqBox.innerHTML = `
                    <div class="flex justify-between">
                        <span class="text-neonCyan">1) Tsiolkovsky Δv:</span>
                        <span>${c.r.isp}s · 9.807 · ln(${c.wetMassKg}/${c.dryMassKg}) + ${c.r.c3Boost} = <strong class="text-white">${c.totalDeltaV.toLocaleString()} m/s</strong></span>
                    </div>
                    <div class="flex justify-between">
                        <span class="text-neonPurple">2) Vis-Viva Velocity:</span>
                        <span>v = √[μ_☉(2/r - 1/a)] = <strong class="text-white">${c.visVivaKmS} km/s</strong> (TOF: ${c.p.tofDays} d)</span>
                    </div>
                    <div class="flex justify-between">
                        <span class="text-neonGreen">3) Solar Flux (${c.p.rAU} AU):</span>
                        <span>S = 1361/${c.p.rAU.toFixed(2)}² = <strong class="text-white">${c.solarFlux} W/m²</strong> &rarr; Bus: <strong class="text-white">${c.netPowerW} W</strong></span>
                    </div>
                    <div class="flex justify-between">
                        <span class="text-neonAmber">4) Surface Gravity &amp; Link:</span>
                        <span>g = <strong class="text-white">${c.p.gravity} m/s²</strong> • FSPL = ${c.fsplDb} dB (${c.linkKbps} kbps)</span>
                    </div>
                `;
            }

            const bars = [
                {
                    title: '💰 1. NASA Cost Cap',
                    text: `$${c.cost}M / $${c.p.budgetCap}M`,
                    pct: Math.min(100, Math.round((c.cost / c.p.budgetCap) * 100)),
                    ok: c.budgetOk
                },
                {
                    title: '🚀 2. Tsiolkovsky Δv',
                    text: `${c.totalDeltaV.toLocaleString()} / ${c.p.reqDeltaV.toLocaleString()} m/s`,
                    pct: Math.min(100, Math.round((c.totalDeltaV / c.p.reqDeltaV) * 100)),
                    ok: c.fuelOk
                },
                {
                    title: '⚡ 3. EOL Power Margin',
                    text: `${c.netPowerW} / ${c.p.reqPowerW} W`,
                    pct: Math.min(100, Math.round((Math.max(20, c.netPowerW) / c.p.reqPowerW) * 100)),
                    ok: c.powerOk
                },
                {
                    title: '🛡️ 4. Rad & TPS Shield',
                    text: `${c.shield} / ${c.p.reqShieldKrad} krad`,
                    pct: Math.min(100, Math.round((c.shield / c.p.reqShieldKrad) * 100)),
                    ok: c.shieldOk
                }
            ];

            const sbContainer = document.getElementById('scoreboard-bars');
            if (sbContainer) {
                sbContainer.innerHTML = bars.map(b => `
                    <div class="cosmic-subcard border ${b.ok ? 'border-indigo-500/40' : 'border-neonRed'} p-2">
                        <div class="flex items-center justify-between text-[11px] mb-1">
                            <span class="font-semibold text-indigo-100">${b.title}</span>
                            <span class="font-mono font-bold ${b.ok ? 'text-neonGreen' : 'text-neonRed'}">${b.ok ? '✓ ' : '✗ '}${b.text}</span>
                        </div>
                        <div class="w-full h-1.5 bg-cosmos rounded-full overflow-hidden">
                            <div class="h-full transition-all duration-300 ${b.ok ? 'bg-gradient-to-r from-neonCyan to-neonGreen' : 'bg-neonRed'}" style="width:${b.pct}%"></div>
                        </div>
                    </div>
                `).join('');
            }

            const pill = document.getElementById('readiness-pill');
            const coachBox = document.getElementById('coach-advice-box');
            const coachTxt = document.getElementById('coach-advice-text');
            const coachFix = document.getElementById('coach-fix-btn');

            if (c.allGreen) {
                if (pill) {
                    pill.textContent = '✓ GO FOR LAUNCH';
                    pill.className = 'px-2.5 py-0.5 rounded-full text-[11px] font-mono font-bold bg-neonGreen/20 text-neonGreen border border-neonGreen';
                }
                if (coachBox) coachBox.className = 'px-3 py-2 rounded-xl bg-neonGreen/10 border border-neonGreen/40 text-xs text-indigo-100 flex items-center justify-between gap-2 mb-2';
                if (coachFix) coachFix.classList.add('hidden');
            } else {
                if (pill) {
                    pill.textContent = '⚠️ NO-GO (ADJUST)';
                    pill.className = 'px-2.5 py-0.5 rounded-full text-[11px] font-mono font-bold bg-neonAmber/20 text-neonAmber border border-neonAmber';
                }
                if (coachBox) coachBox.className = 'px-3 py-2 rounded-xl bg-neonRed/15 border border-neonRed text-xs text-indigo-100 flex items-center justify-between gap-2 mb-2';
                if (coachFix) coachFix.classList.remove('hidden');
            }
            if (coachTxt) coachTxt.textContent = c.advice;

            setTimeout(updateHandPosition, 20);
        }

        /*********************************************************************
         * 6. LETHAL 3D ASTEROID FIELD & SURFACE INITIALIZATION
         *********************************************************************/
        function spawnAsteroidField(count = 6) {
            state.asteroids = [];
            for (let i = 0; i < count; i++) {
                state.asteroids.push(createSingleAsteroid(0.35 + (i * 0.14)));
            }
        }

        function createSingleAsteroid(initialZ = 1.0) {
            // Generate a multi-vertex jagged 3D rock polygon
            const vertices = [];
            const numPts = 8;
            for (let i = 0; i < numPts; i++) {
                const ang = (i / numPts) * Math.PI * 2;
                const rad = 0.72 + Math.random() * 0.42;
                vertices.push({ ang, rad });
            }
            return {
                id: Math.floor(100 + Math.random() * 899),
                x: (Math.random() - 0.5) * 0.85,
                y: (Math.random() - 0.5) * 0.65,
                z: initialZ,
                vx: (Math.random() - 0.5) * 0.0015,
                vy: (Math.random() - 0.5) * 0.0015,
                rot: Math.random() * Math.PI * 2,
                rotSpd: (Math.random() - 0.5) * 0.05,
                massTons: +(12 + Math.random() * 85).toFixed(1),
                relVelKmS: +(14.2 + Math.random() * 11.5).toFixed(1),
                vertices
            };
        }

        function resetSurfaceAndEVAState() {
            state.orbitVelKmS = 11.40;
            state.periapsisTimerSec = 38.0;
            state.orbitCaptured = false;
            state.orbitCaptureHoldSec = 0;
            state.retroBrakingActive = false;

            state.landerX = 0.26;
            state.landerY = -0.20;
            state.landerAltM = 1400;
            state.descentRateMS = 14.5;
            state.landedOnPad = false;
            state.rampDeployPct = 0;
            state.roverUnloadPct = 0;

            state.roverX = -135;
            state.roverY = 30;
            state.roverHeading = 0;
            state.roverSpeedKmH = 0;
            state.waypoints = [
                { id: 1, x: -35, y: -42, label: 'SITE A: CRATER RIM', collected: false },
                { id: 2, x: 65, y: 38, label: 'SITE B: ICE TRENCH', collected: false },
                { id: 3, x: 160, y: -25, label: 'SITE C: BEDROCK VENT', collected: false }
            ];
            state.samplesCollected = 0;

            // Astronaut EVA initial coordinates
            state.evaCamX = 0;
            state.evaCamY = -36;
            state.evaYaw = 0;
            state.evaJumpZ = 0;
            state.evaJumpVz = 0;
            state.evaBobPhase = 0;
            state.evaArmReachTimer = 0;
            state.crystalCollected = false;
            state.flagPlanted = false;
            state.crystalWorld = { x: -14, y: 12 };
            state.flagWorld = { x: 16, y: 30 };
            state.landerWorld = { x: -36, y: 34 };
            state.roverWorld = { x: 32, y: 12 };

            state.missionFailedLOV = false;
            state.canopyCracks = [];
            document.getElementById('lov-failure-overlay')?.classList.add('hidden');
            spawnAsteroidField(6);
        }

        function toggleCameraPerspective() {
            playSfx('tap');
            const modes = ['auto', '3rd', '1st'];
            const nextIdx = (modes.indexOf(state.camPerspective) + 1) % modes.length;
            state.camPerspective = modes[nextIdx];
            const lbl = document.getElementById('cam-perspective-label');
            if (lbl) {
                lbl.textContent = state.camPerspective === 'auto'
                    ? 'VIEW: AUTO (3RD/1ST)'
                    : state.camPerspective === '3rd'
                        ? 'VIEW: 3RD-PERSON EXTERIOR'
                        : 'VIEW: 1ST-PERSON COCKPIT/VISOR';
            }
            showCockpitToast(`🎥 CAMERA MODE: ${state.camPerspective.toUpperCase()}`, 1800);
        }

        function selectPlanet(id) {
            playSfx('tap');
            state.planetId = id;
            if (state.guideActive && state.guideStep === 1) state.guideStep = 2;
            renderBuilderUI();
        }

        function selectPart(catKey, optId) {
            playSfx('tap');
            state[catKey] = optId;
            evaluateMission();
            if (state.guideActive && state.guideStep === 2 && state.calc.allGreen) {
                state.guideStep = 3;
            }
            renderBuilderUI();
        }

        function updatePropellantSlider(val) {
            state.propPct = parseInt(val, 10);
            const lbl = document.getElementById('propellant-pct-label');
            if (lbl) lbl.textContent = `${state.propPct}% Fuel Load`;
            renderBuilderUI();
        }

        function autoBuildWinningShip(fromClick = false) {
            if (fromClick) playSfx('success');
            const best = PLANETS[state.planetId].bestSetup;
            state.rocket = best.rocket;
            state.power = best.power;
            state.hull = best.hull;
            state.payload = best.payload;
            state.propPct = best.propPct || 85;
            const slider = document.getElementById('propellant-slider');
            if (slider) slider.value = String(state.propPct);
            const lbl = document.getElementById('propellant-pct-label');
            if (lbl) lbl.textContent = `${state.propPct}% Fuel Load`;

            if (fromClick && state.guideActive && state.guideStep <= 2) {
                state.guideStep = 3;
            }
            renderBuilderUI();
        }

        function showCockpitToast(msg, duration = 2400) {
            const toast = document.getElementById('cockpit-event-toast');
            if (!toast) return;
            toast.textContent = msg;
            toast.classList.remove('hidden');
            clearTimeout(toast._timer);
            toast._timer = setTimeout(() => toast.classList.add('hidden'), duration);
        }

        function triggerStaticFireTest() {
            playSfx('roar');
            state.cockpitScene = 'pad';
            state.staticFireTimer = 110;
            state.throttle = 100;
            showCockpitToast('🔥 STATIC FIRE ENGINE TEST: 100% RATED THRUST ON LC-39A!', 2400);
            updateSceneButtonsUI();
        }

        /*********************************************************************
         * 7. DUAL CONTROL AUTHORITY: MANUAL (WASD) & AI AUTO-PILOT
         *********************************************************************/
        function toggleAutopilot(forceVal) {
            playSfx('tap');
            state.autopilot = typeof forceVal === 'boolean' ? forceVal : !state.autopilot;
            if (state.autopilot) {
                state.throttle = state.inFlight ? 85 : state.throttle;
                showCockpitToast('🤖 AI AUTO-PILOT ENGAGED — AUTO-DODGING ASTEROIDS & GUIDING EVA', 2400);
            } else {
                showCockpitToast('🎮 MANUAL AUTHORITY ACTIVE — DODGE ASTEROIDS WITH W A S D + [E]!', 2400);
            }
            syncAutopilotUI();
        }

        function syncAutopilotUI() {
            const hdrBtn = document.getElementById('header-autopilot-btn');
            const hdrTxt = document.getElementById('header-autopilot-text');
            const cockBtn = document.getElementById('cockpit-autopilot-btn');
            const fltBtn = document.getElementById('flight-autopilot-toggle');

            if (state.autopilot) {
                if (hdrTxt) hdrTxt.textContent = '🤖 MODE: AUTO-PILOT (ON)';
                if (hdrBtn) hdrBtn.className = 'px-3 py-1.5 rounded-lg bg-neonGreen text-cosmos border border-neonGreen font-mono font-bold text-xs transition flex items-center gap-1.5 shadow-[0_0_15px_rgba(0,230,118,0.4)]';
                if (cockBtn) {
                    cockBtn.textContent = '[P] 🤖 AUTO: ON';
                    cockBtn.className = 'px-3 py-2 rounded-lg bg-neonGreen text-cosmos border-2 border-neonGreen font-bold transition shadow-[0_0_15px_rgba(0,230,118,0.5)]';
                }
                if (fltBtn) {
                    fltBtn.textContent = '🤖 AUTO-PILOT: ON';
                    fltBtn.className = 'px-3 py-1.5 rounded-lg font-mono text-xs font-bold border border-neonGreen bg-neonGreen text-cosmos';
                }
            } else {
                if (hdrTxt) hdrTxt.textContent = '🎮 MODE: MANUAL (WASD)';
                if (hdrBtn) hdrBtn.className = 'px-3 py-1.5 rounded-lg bg-neonCyan/15 hover:bg-neonCyan hover:text-cosmos border border-neonCyan text-neonCyan font-mono font-bold text-xs transition flex items-center gap-1.5';
                if (cockBtn) {
                    cockBtn.textContent = '[P] 🤖 AUTO: OFF';
                    cockBtn.className = 'px-3 py-2 rounded-lg bg-neonPurple/25 border-2 border-neonCyan text-neonCyan font-bold hover:bg-neonCyan hover:text-cosmos transition';
                }
                if (fltBtn) {
                    fltBtn.textContent = '🎮 MANUAL (WASD)';
                    fltBtn.className = 'px-3 py-1.5 rounded-lg font-mono text-xs font-bold border border-neonCyan bg-neonCyan/20 text-neonCyan';
                }
            }
        }

        function pressVirtualKey(k, isDown) {
            if (state.missionFailedLOV && isDown) return;
            state.keys[k] = isDown;
            const btn = document.getElementById(`key-${k}`);
            if (btn) {
                if (isDown) {
                    btn.classList.add('bg-neonCyan', 'text-cosmos', 'border-neonCyan');
                } else {
                    btn.classList.remove('bg-neonCyan', 'text-cosmos', 'border-neonCyan');
                }
            }

            if (isDown) {
                if (k === 'e') {
                    triggerActionOrLaserKey();
                } else if (k === 'space') {
                    if (state.cockpitScene === 'eva' && state.evaJumpZ <= 0.01) {
                        // Exact Parabolic Surface Gravity Jump: v_z0 = 3.8 m/s
                        playSfx('tap');
                        state.evaJumpVz = 3.8;
                        const g = PLANETS[state.planetId].gravity;
                        const peakH = ((3.8 * 3.8) / (2 * g)).toFixed(2);
                        const hangT = ((2 * 3.8) / g).toFixed(2);
                        showCockpitToast(`👨‍🚀 1ST-PERSON LOW-G JUMP! g = ${g} m/s² → Peak = ${peakH}m, Hang Time = ${hangT}s`, 2200);
                    } else if (state.cockpitScene === 'rover') {
                        triggerActionOrLaserKey();
                    } else if (!state.inFlight && state.cockpitScene === 'pad') {
                        triggerStaticFireTest();
                    }
                } else if (k === 'q') {
                    state.throttle = 0;
                    state.roverSpeedKmH = 0;
                    state.staticFireTimer = 0;
                }
            }
        }

        function triggerActionOrLaserKey() {
            if (state.missionFailedLOV) return;
            const sc = state.cockpitScene;
            state.laserPulseTimer = 30;

            // STAGE 3 ORBIT: IF IN CAPTURE WINDOW, LOCK ORBIT IMMEDIATELY; OTHERWISE FIRE RETRO-BURST + LASER
            if (sc === 'orbit') {
                playSfx('laser');
                if (state.orbitVelKmS > state.orbitTargetMaxKmS) {
                    state.orbitVelKmS = Math.max(3.10, state.orbitVelKmS - 1.45);
                    state.retroBrakingActive = true;
                    showCockpitToast(`🛑 RETRO-BRAKE BURST FIRED! Speed reduced to ${state.orbitVelKmS.toFixed(2)} km/s (Target: 2.2–4.2 km/s)`, 2000);
                }
                if (state.asteroids.length > 0) {
                    state.asteroidsDestroyed++;
                    state.pilotBonus += 35;
                    state.totalScience += 35;
                    state.asteroids[0] = createSingleAsteroid(1.05);
                }
                if (state.orbitVelKmS <= state.orbitTargetMaxKmS && state.orbitVelKmS >= state.orbitTargetMinKmS) {
                    state.orbitCaptured = true;
                    state.stageProgress = 100;
                    showCockpitToast('✅ ORBITAL CAPTURE VELOCITY LOCKED! INITIATING 3RD-PERSON LANDING!', 2400);
                    if (state.inFlight && !state.flightFinished) {
                        setTimeout(() => makeFlightDecision(0), 600);
                    }
                }
                updateFlightTelemetryBars();
                return;
            }

            // STAGE 2: PULSE DEFENSE LASER VAPORIZES CLOSEST TARGETED ASTEROID
            if (sc === 'space') {
                playSfx('laser');
                if (state.asteroids.length > 0) {
                    let closestIdx = 0;
                    let bestZ = 999;
                    state.asteroids.forEach((ast, idx) => {
                        if (ast.z < bestZ) {
                            bestZ = ast.z;
                            closestIdx = idx;
                        }
                    });
                    const targetAst = state.asteroids[closestIdx];
                    state.asteroidsDestroyed++;
                    state.pilotBonus += 45;
                    state.totalScience += 45;
                    state.stageProgress = Math.min(100, state.stageProgress + 14);
                    showCockpitToast(`⚡ PULSE LASER VAPORIZED ASTEROID #${targetAst.id} (${targetAst.massTons}t)! +45 Gb`, 1800);
                    state.asteroids[closestIdx] = createSingleAsteroid(1.05);
                    updateFlightTelemetryBars();
                    if (state.inFlight && !state.flightFinished && state.stageProgress >= 100) {
                        makeFlightDecision(0);
                    }
                }
                return;
            }

            // STAGE 4 LANDING: DEPLOY TRACK & UNLOAD ROVER OR CUSHION TOUCHDOWN
            if (sc === 'landing') {
                playSfx('success');
                if (!state.landedOnPad) {
                    state.landerX *= 0.25;
                    state.landerY *= 0.25;
                    state.descentRateMS = Math.max(2.2, state.descentRateMS - 3.5);
                    state.landerAltM = Math.max(0, state.landerAltM - 220);
                    showCockpitToast('🔥 RETRO-DESCENT BURN: ALIGNING OVER LANDING PAD ALPHA!', 1800);
                } else {
                    state.rampDeployPct = Math.min(100, state.rampDeployPct + 35);
                    if (state.rampDeployPct >= 100) {
                        state.roverUnloadPct = Math.min(100, state.roverUnloadPct + 40);
                    }
                    showCockpitToast('🛤️ HYDRAULIC CARGO TRACK DEPLOYING — UNLOADING 6-WHEELED ROVER!', 2000);
                }
                return;
            }

            // STAGE 5: ROVER LASER CORE DRILLING
            if (sc === 'rover') {
                playSfx('laser');
                let foundWp = state.waypoints.find(wp => !wp.collected && Math.hypot(state.roverX - wp.x, state.roverY - wp.y) < 58);
                if (!foundWp) {
                    foundWp = state.waypoints.find(w => !w.collected);
                    if (foundWp) {
                        state.roverX = foundWp.x - 10;
                        state.roverY = foundWp.y + 6;
                    }
                }
                if (foundWp && !foundWp.collected) {
                    foundWp.collected = true;
                    state.samplesCollected++;
                    state.pilotBonus += 65;
                    state.totalScience += 90;
                    const doneCount = state.waypoints.filter(w => w.collected).length;
                    state.stageProgress = Math.min(100, Math.round((doneCount / 3) * 100));
                    showCockpitToast(`🛞 ROVER DRILLED CORE AT ${foundWp.label} (${doneCount}/3)!`, 2000);
                    updateFlightTelemetryBars();
                    if (doneCount >= 3 && state.inFlight && !state.flightFinished) {
                        setTimeout(() => makeFlightDecision(0), 650);
                    }
                }
                return;
            }

            // STAGE 6: ASTRONAUT CRYSTAL PICKUP & FLAG PLANT
            if (sc === 'eva') {
                playSfx('success');
                state.evaArmReachTimer = 45;
                if (!state.crystalCollected) {
                    state.evaCamX = state.crystalWorld.x;
                    state.evaCamY = state.crystalWorld.y - 8;
                    state.evaYaw = 0;
                    state.crystalCollected = true;
                    state.samplesCollected = Math.max(state.samplesCollected, 4);
                    state.pilotBonus += 110;
                    state.totalScience += 140;
                    state.stageProgress = 55;
                    showCockpitToast(`💎 ASTRONAUT COLLECTED: ${PLANETS[state.planetId].sampleName.toUpperCase()}! NOW PLANT FLAG [E]!`, 2500);
                    updateFlightTelemetryBars();
                } else if (!state.flagPlanted) {
                    state.evaCamX = state.flagWorld.x;
                    state.evaCamY = state.flagWorld.y - 10;
                    state.evaYaw = 0;
                    state.flagPlanted = true;
                    state.pilotBonus += 180;
                    state.totalScience += 240;
                    state.stageProgress = 100;
                    showCockpitToast('🚩 EVA COMPLETE: "ThePioneersX" FLAG PLANTED IN REGOLITH!', 2800);
                    updateFlightTelemetryBars();
                    if (state.inFlight && !state.flightFinished) {
                        setTimeout(() => makeFlightDecision(0), 950);
                    }
                }
                return;
            }

            // STAGE 1: ASCENT ACTION
            playSfx('success');
            if (state.inFlight && !state.flightFinished) {
                state.pilotBonus += 35;
                state.totalScience += 35;
                state.stageProgress = Math.min(100, state.stageProgress + 22);
                showCockpitToast('⚡ STAGE TELEMETRY & THRUSTER ACTION EXECUTED (+35 Gb)!', 1500);
                if (state.stageProgress >= 100) {
                    makeFlightDecision(0);
                } else {
                    updateFlightTelemetryBars();
                }
            }
        }

        function initKeyboardControls() {
            window.addEventListener('keydown', (e) => {
                if (['INPUT', 'SELECT', 'TEXTAREA'].includes(document.activeElement?.tagName)) return;
                const k = e.key.toLowerCase();
                if (k === 'w' || e.key === 'ArrowUp') { e.preventDefault(); pressVirtualKey('w', true); }
                else if (k === 's' || e.key === 'ArrowDown') { e.preventDefault(); pressVirtualKey('s', true); }
                else if (k === 'a' || e.key === 'ArrowLeft') { e.preventDefault(); pressVirtualKey('a', true); }
                else if (k === 'd' || e.key === 'ArrowRight') { e.preventDefault(); pressVirtualKey('d', true); }
                else if (e.code === 'Space' || e.key === 'Shift') { e.preventDefault(); pressVirtualKey('space', true); }
                else if (k === 'q') { e.preventDefault(); pressVirtualKey('q', true); }
                else if (k === 'e') { e.preventDefault(); pressVirtualKey('e', true); }
                else if (k === 'p') { e.preventDefault(); toggleAutopilot(); }
                else if (k === 'c') { e.preventDefault(); toggleCameraPerspective(); }
            });

            window.addEventListener('keyup', (e) => {
                const k = e.key.toLowerCase();
                if (k === 'w' || e.key === 'ArrowUp') pressVirtualKey('w', false);
                else if (k === 's' || e.key === 'ArrowDown') pressVirtualKey('s', false);
                else if (k === 'a' || e.key === 'ArrowLeft') pressVirtualKey('a', false);
                else if (k === 'd' || e.key === 'ArrowRight') pressVirtualKey('d', false);
                else if (e.code === 'Space' || e.key === 'Shift') pressVirtualKey('space', false);
                else if (k === 'q') pressVirtualKey('q', false);
                else if (k === 'e') pressVirtualKey('e', false);
            });

            initCockpitCanvasPointerControls();
        }

        /*********************************************************************
         * 7B. INTERACTIVE COCKPIT CANVAS CONTROLS (DRAGGABLE YOKE HANDLE,
         *     THROTTLE/RETRO LEVER, AND CLICKABLE DASHBOARD SWITCHES)
         *********************************************************************/
        function initCockpitCanvasPointerControls() {
            const canvas = document.getElementById('cockpit-canvas');
            if (!canvas) return;

            function getCanvasCoords(ev) {
                const rect = canvas.getBoundingClientRect();
                const clientX = ev.touches ? ev.touches[0].clientX : ev.clientX;
                const clientY = ev.touches ? ev.touches[0].clientY : ev.clientY;
                return {
                    x: (clientX - rect.left) * (canvas.width / Math.max(1, rect.width)),
                    y: (clientY - rect.top) * (canvas.height / Math.max(1, rect.height))
                };
            }

            function onDown(ev) {
                if (state.missionFailedLOV) return;
                const pt = getCanvasCoords(ev);
                state.pointerCanvasX = pt.x;
                state.pointerCanvasY = pt.y;
                const w = canvas.width, h = canvas.height;

                // 1. Check Clickable Cockpit Dashboard Buttons
                for (const btn of state.cockpitButtons) {
                    if (pt.x >= btn.x && pt.x <= btn.x + btn.w && pt.y >= btn.y && pt.y <= btn.y + btn.h) {
                        state.activeDashBtnId = btn.id;
                        state.activeDashBtnTimer = 14;
                        btn.action();
                        return;
                    }
                }

                // 2. Check Interactive Throttle / Retro-Brake Lever on Left Console
                const leverCenterX = w * 0.10;
                const leverTopY = h * 0.75;
                const leverBotY = h * 0.95;
                if (Math.abs(pt.x - leverCenterX) < 42 && pt.y >= leverTopY - 15 && pt.y <= leverBotY + 15) {
                    state.throttleDragActive = true;
                    updateThrottleFromPointer(pt.y, leverTopY, leverBotY);
                    return;
                }

                // 3. Check Interactive 3D Flight Yoke / Driving Handle in Center Console
                const yokeCenterX = w * 0.50;
                const yokeCenterY = h * 0.87;
                if (Math.hypot(pt.x - yokeCenterX, pt.y - yokeCenterY) < 95) {
                    state.yokeDragActive = true;
                    updateYokeFromPointer(pt.x, pt.y, yokeCenterX, yokeCenterY);
                }
            }

            function onMove(ev) {
                if (!state.yokeDragActive && !state.throttleDragActive) return;
                if (ev.cancelable) ev.preventDefault();
                const pt = getCanvasCoords(ev);
                state.pointerCanvasX = pt.x;
                state.pointerCanvasY = pt.y;
                const w = canvas.width, h = canvas.height;

                if (state.throttleDragActive) {
                    updateThrottleFromPointer(pt.y, h * 0.75, h * 0.95);
                } else if (state.yokeDragActive) {
                    updateYokeFromPointer(pt.x, pt.y, w * 0.50, h * 0.87);
                }
            }

            function onUp() {
                state.yokeDragActive = false;
                state.throttleDragActive = false;
                state.keys.q = false;
            }

            function updateThrottleFromPointer(py, topY, botY) {
                const ratio = 1 - Math.max(0, Math.min(1, (py - topY) / Math.max(1, botY - topY)));
                state.throttle = Math.round(ratio * 100);
                if (state.cockpitScene === 'orbit' && state.throttle < 35) {
                    state.keys.q = true;
                    state.retroBrakingActive = true;
                } else {
                    state.keys.q = false;
                }
            }

            function updateYokeFromPointer(px, py, cx, cy) {
                const dx = Math.max(-75, Math.min(75, px - cx));
                const dy = Math.max(-55, Math.min(55, py - cy));
                const sc = state.cockpitScene;

                if (sc === 'landing') {
                    state.landerX = Math.max(-0.85, Math.min(0.85, dx / 85));
                    state.landerY = Math.max(-0.65, Math.min(0.65, dy / 75));
                } else if (sc === 'rover') {
                    state.roverX = Math.max(-210, Math.min(220, state.roverX + (dx / 75) * 3.2));
                    state.roverY = Math.max(-75, Math.min(75, state.roverY + (dy / 55) * 2.4));
                    state.roverHeading = Math.atan2(dy, dx);
                    state.roverSpeedKmH = 15.2;
                    state.roverWheelRot += 0.28;
                } else if (sc === 'eva') {
                    state.evaYaw += (dx / 75) * 0.045;
                    const walkSpeed = -(dy / 55) * 0.55;
                    state.evaCamX = Math.max(-75, Math.min(75, state.evaCamX + Math.sin(state.evaYaw) * walkSpeed));
                    state.evaCamY = Math.max(-75, Math.min(75, state.evaCamY + Math.cos(state.evaYaw) * walkSpeed));
                    state.evaBobPhase += 0.25;
                } else {
                    state.yaw = (dx / 75) * 42;
                    state.pitch = -(dy / 55) * 38;
                    state.roll = (dx / 75) * 24;
                }
            }

            canvas.addEventListener('mousedown', onDown);
            window.addEventListener('mousemove', onMove);
            window.addEventListener('mouseup', onUp);
            canvas.addEventListener('touchstart', onDown, { passive: false });
            window.addEventListener('touchmove', onMove, { passive: false });
            window.addEventListener('touchend', onUp);
        }

        /*********************************************************************
         * 8. CAMPAIGN LAUNCH, ASTEROID COLLISION FAILURE (LOV) & STAGE DECK
         *********************************************************************/
        function startLaunchFlight(useAutopilot) {
            evaluateMission();
            if (!state.calc.allGreen) {
                autoBuildWinningShip(false);
            }
            if (typeof useAutopilot === 'boolean') {
                state.autopilot = useAutopilot;
            }
            syncAutopilotUI();

            playSfx('roar');
            resetSurfaceAndEVAState();
            state.inFlight = true;
            state.flightStageIdx = 0;
            state.flightFinished = false;
            state.hullHealth = 100;
            state.fuelLeft = state.propPct;
            state.pilotBonus = 0;
            state.totalScience = state.calc.pl.sciPoints;
            state.stageProgress = 0;
            state.pitch = 6;
            state.yaw = 0;
            state.throttle = 90;

            if (state.guideActive) state.guideStep = 4;

            document.getElementById('builder-panel').classList.add('hidden');
            document.getElementById('flight-control-panel').classList.remove('hidden');

            showCockpitToast(
                state.autopilot
                    ? '🤖 6-STAGE AI AUTO-PILOT ENGAGED: AUTO-DODGING ASTEROIDS & 1ST-PERSON EVA!'
                    : '🚀 LIFTOFF! WATCH OUT FOR LETHAL ASTEROIDS IN STAGE 2 & 3!',
                3200
            );
            renderFlightDeck();
        }

        function triggerAsteroidCollision(ast) {
            playSfx('impact');
            state.hullHitTimer = 35;

            // Calculate kinetic energy impact damage (scaled by hull shielding)
            const keGJ = +(0.5 * ast.massTons * Math.pow(ast.relVelKmS, 2) * 0.001).toFixed(1);
            const shieldMitigation = state.calc.shield ? (state.calc.shield / 160) : 0.3;
            const rawDamage = Math.round(36 * (1 - shieldMitigation * 0.45));
            state.hullHealth = Math.max(0, state.hullHealth - rawDamage);

            // Add persistent windshield crack on canopy glass
            state.canopyCracks.push({
                x: 0.25 + Math.random() * 0.5,
                y: 0.22 + Math.random() * 0.45,
                rays: [0.2, 1.1, 2.3, 3.6, 4.8, 5.7].map(a => a + (Math.random() - 0.5) * 0.3)
            });

            const wrap = document.getElementById('canvas-wrapper');
            if (wrap) {
                wrap.classList.remove('hull-hit-flash');
                void wrap.offsetWidth;
                wrap.classList.add('hull-hit-flash');
            }

            updateFlightTelemetryBars();

            if (state.hullHealth <= 0) {
                // MISSION FAILED // LOSS OF VEHICLE (LOV)
                state.missionFailedLOV = true;
                state.throttle = 0;
                state.failureReasonText = `Asteroid #${ast.id} (Mass: ${ast.massTons} tons, v_rel: ${ast.relVelKmS} km/s, Impact KE: ${keGJ} GJ) breached the forward canopy during manual flight. Hull Integrity dropped to 0%.`;
                const ov = document.getElementById('lov-failure-overlay');
                const badge = document.getElementById('lov-failure-badge');
                const title = document.getElementById('lov-failure-title');
                const rs = document.getElementById('lov-failure-reason');
                if (badge) badge.textContent = '⚠️ NASA TELEMETRY ALARM // HULL INTEGRITY 0% — LOSS OF VEHICLE (LOV)';
                if (title) title.textContent = '💥 ASTEROID IMPACT // MISSION FAILED';
                if (rs) rs.textContent = state.failureReasonText;
                if (ov) ov.classList.remove('hidden');
                renderFlightDeck();
            } else {
                showCockpitToast(`💥 ASTEROID #${ast.id} IMPACT! -${rawDamage}% HULL (KE = ${keGJ} GJ)! STEER AWAY [WASD] OR LASER [E]!`, 2600);
            }
        }

        function triggerOrbitOvershootFailure() {
            playSfx('impact');
            state.missionFailedLOV = true;
            state.throttle = 0;
            const p = PLANETS[state.planetId];
            state.failureReasonText = `Hyperbolic Approach Velocity (${state.orbitVelKmS.toFixed(2)} km/s) exceeded ${p.name}'s escape velocity (${p.escapeVelKmS} km/s) at Periapsis! Because speed was not reduced into the 2.20–4.20 km/s capture window using Retro-Brakes [Q], the spacecraft overshot ${p.name} into deep space.`;
            const ov = document.getElementById('lov-failure-overlay');
            const badge = document.getElementById('lov-failure-badge');
            const title = document.getElementById('lov-failure-title');
            const rs = document.getElementById('lov-failure-reason');
            if (badge) badge.textContent = '⚠️ ASTRODYNAMICS ALARM // PERIAPSIS VELOCITY TOO HIGH (v > v_esc)';
            if (title) title.textContent = '🌌 HYPERBOLIC ORBIT OVERSHOOT // MISSED PLANET';
            if (rs) rs.textContent = state.failureReasonText;
            if (ov) ov.classList.remove('hidden');
            renderFlightDeck();
        }

        function retryCurrentStage(enableAuto) {
            playSfx('success');
            state.missionFailedLOV = false;
            state.hullHealth = 100;
            state.canopyCracks = [];
            state.stageProgress = 0;
            state.orbitVelKmS = 11.40;
            state.periapsisTimerSec = 38.0;
            state.orbitCaptured = false;
            state.orbitCaptureHoldSec = 0;
            state.landerAltM = 1400;
            state.descentRateMS = 14.5;
            state.landedOnPad = false;
            state.rampDeployPct = 0;
            state.roverUnloadPct = 0;
            if (typeof enableAuto === 'boolean') {
                state.autopilot = enableAuto;
                syncAutopilotUI();
            }
            spawnAsteroidField(6);
            document.getElementById('lov-failure-overlay')?.classList.add('hidden');
            showCockpitToast('🔧 TELEMETRY RESET TO 100% — STAGE RESTARTED!', 2200);
            renderFlightDeck();
        }

        function jumpToStage(stageIdx) {
            playSfx('tap');
            if (!state.inFlight) {
                startLaunchFlight(state.autopilot);
            }
            state.missionFailedLOV = false;
            document.getElementById('lov-failure-overlay')?.classList.add('hidden');
            state.flightFinished = false;
            state.flightStageIdx = stageIdx;
            state.stageProgress = 0;
            if (stageIdx === 1 || stageIdx === 2) {
                spawnAsteroidField(6);
                state.orbitVelKmS = 11.40;
                state.periapsisTimerSec = 38.0;
                state.orbitCaptured = false;
                state.orbitCaptureHoldSec = 0;
            } else if (stageIdx === 3) {
                state.landerAltM = 1400;
                state.descentRateMS = 14.5;
                state.landerX = 0.22;
                state.landedOnPad = false;
                state.rampDeployPct = 0;
                state.roverUnloadPct = 0;
            }
            renderFlightDeck();
        }

        function returnToBuilder() {
            playSfx('tap');
            state.inFlight = false;
            state.missionFailedLOV = false;
            document.getElementById('lov-failure-overlay')?.classList.add('hidden');
            state.throttle = 0;
            state.pitch = 0;
            state.yaw = 0;
            state.roll = 0;
            state.cockpitScene = 'pad';
            if (state.guideActive) state.guideStep = 2;
            document.getElementById('flight-control-panel').classList.add('hidden');
            document.getElementById('builder-panel').classList.remove('hidden');
            updateSceneButtonsUI();
            renderBuilderUI();
        }

        function updateFlightTelemetryBars() {
            const hEl = document.getElementById('flight-hull-val');
            const fEl = document.getElementById('flight-fuel-val');
            const smpEl = document.getElementById('flight-samples-val');
            const sEl = document.getElementById('flight-sci-val');
            const bEl = document.getElementById('flight-bonus-tag');
            const pBar = document.getElementById('flight-task-bar');
            const pPct = document.getElementById('flight-task-pct');

            if (hEl) {
                hEl.textContent = `${Math.round(state.hullHealth)}%`;
                hEl.className = state.hullHealth > 50
                    ? 'text-sm sm:text-base font-bold text-neonGreen'
                    : state.hullHealth > 25
                        ? 'text-sm sm:text-base font-bold text-neonAmber'
                        : 'text-sm sm:text-base font-bold text-neonRed animate-pulse';
            }
            if (fEl) fEl.textContent = `${Math.round(state.fuelLeft)}%`;
            if (smpEl) smpEl.textContent = `${state.samplesCollected} / 4`;
            if (sEl) sEl.textContent = `${Math.round(state.totalScience)} Gb`;
            if (bEl) bEl.textContent = `PILOT & EVA BONUS: +${Math.round(state.pilotBonus)} Gb`;
            if (pBar) pBar.style.width = `${Math.min(100, Math.round(state.stageProgress))}%`;
            if (pPct) pPct.textContent = `${Math.min(100, Math.round(state.stageProgress))}%`;
        }

        function renderFlightDeck() {
            const pills = document.getElementById('flight-stage-pills');
            if (pills) {
                pills.innerHTML = FLIGHT_EVENTS.map((ev, idx) => {
                    const done = idx < state.flightStageIdx || state.flightFinished;
                    const active = idx === state.flightStageIdx && !state.flightFinished;
                    return `<button onclick="jumpToStage(${idx})" class="py-1.5 px-1 rounded-lg text-center font-mono text-[10px] border transition ${active ? 'border-neonCyan bg-indigo-900/70 text-white font-bold shadow-[0_0_12px_rgba(0,240,255,0.35)]' : done ? 'border-neonGreen/50 bg-neonGreen/10 text-neonGreen hover:border-neonGreen' : 'border-indigo-500/30 bg-cosmos text-indigo-300 hover:text-white'}">${done ? '✓' : ''}${ev.shortLabel}</button>`;
                }).join('');
            }

            const badge = document.getElementById('surface-progress-badge');
            if (badge) {
                badge.textContent = state.flightFinished ? 'MISSION COMPLETE' : `STAGE ${state.flightStageIdx + 1} OF 6`;
            }

            updateFlightTelemetryBars();

            const card = document.getElementById('flight-interactive-card');
            const progBox = document.getElementById('flight-pilot-progress-box');
            const physStrip = document.getElementById('flight-live-physics-strip');
            if (!card) return;

            if (state.missionFailedLOV) {
                card.innerHTML = `
                    <div class="space-y-3 text-center py-2 border border-neonRed rounded-xl p-3 bg-neonRed/10">
                        <div class="text-neonRed font-mono text-xs font-bold">💥 LOSS OF VEHICLE (LOV) // HYPERVELOCITY ASTEROID IMPACT</div>
                        <p class="text-xs text-indigo-100">${state.failureReasonText}</p>
                        <div class="grid grid-cols-1 sm:grid-cols-2 gap-2 pt-1">
                            <button onclick="retryCurrentStage(false)" class="py-2 rounded-lg bg-neonCyan text-cosmos font-display font-bold text-xs">
                                🔄 RETRY STAGE (MANUAL)
                            </button>
                            <button onclick="retryCurrentStage(true)" class="py-2 rounded-lg bg-neonGreen text-cosmos font-display font-bold text-xs">
                                🤖 RETRY WITH AUTO-PILOT
                            </button>
                        </div>
                    </div>
                `;
                return;
            }

            if (state.flightFinished) {
                if (progBox) progBox.classList.add('hidden');
                const grade = state.totalScience >= 1750 ? 'S+ (NASA DISTINGUISHED FLIGHT DIRECTOR)' : state.totalScience >= 1400 ? 'A (MISSION COMMANDER)' : 'B (FLIGHT QUALIFIED)';
                document.getElementById('flight-header-title').textContent = `EXPEDITION COMPLETE // ${state.calc.p.name.toUpperCase()}`;
                card.innerHTML = `
                    <div class="space-y-3 text-center py-2">
                        <div class="inline-block px-3 py-1 rounded-full bg-neonGreen/20 border border-neonGreen text-neonGreen font-mono text-xs font-bold">
                            🏆 OFFICIAL NASA 6-STAGE ASTRODYNAMICS &amp; 1ST-PERSON EVA SCORECARD
                        </div>
                        <h3 class="font-display text-xl sm:text-2xl font-black text-white">RANK: ${grade}</h3>
                        <p class="text-xs text-indigo-200 max-w-md mx-auto leading-relaxed">
                            You launched aboard <strong>${state.calc.r.name}</strong> ($\Delta v = ${state.calc.totalDeltaV.toLocaleString()}\text{ m/s}$), survived the lethal 3D Asteroid Belt (${state.asteroidsDodged + state.asteroidsDestroyed} asteroids cleared), landed on <strong>${state.calc.p.icon} ${state.calc.p.name}</strong> ($g = ${state.calc.p.gravity}\text{ m/s}^2$), drilled 3 Rover cores, and completed the <strong>1st-Person Spacesuit Visor EVA</strong> for <strong>${Math.round(state.totalScience)} Gb Science Data</strong>!
                        </p>
                        <div class="grid grid-cols-1 sm:grid-cols-3 gap-2 pt-2">
                            <button onclick="returnToBuilder()" class="py-2.5 rounded-xl bg-neonCyan text-cosmos font-display font-bold text-xs uppercase">
                                🔄 NEW PLANET
                            </button>
                            <button onclick="startLaunchFlight(false)" class="py-2.5 rounded-xl bg-neonPurple text-white font-display font-bold text-xs uppercase">
                                🎮 REPLAY MANUAL
                            </button>
                            <button onclick="startLaunchFlight(true)" class="py-2.5 rounded-xl bg-neonGreen text-cosmos font-display font-bold text-xs uppercase">
                                🤖 REPLAY AUTO-PILOT
                            </button>
                        </div>
                    </div>
                `;
                updateHandPosition();
                return;
            }

            if (progBox) progBox.classList.remove('hidden');
            const ev = FLIGHT_EVENTS[state.flightStageIdx];
            state.stageProgress = 0;
            setCockpitScene(ev.scene);
            document.getElementById('flight-header-title').textContent = ev.name;
            document.getElementById('flight-task-label').textContent = ev.taskText;
            state.altitudeKm = ev.altitudeBase;

            if (physStrip) {
                const p = state.calc.p;
                physStrip.innerHTML = `
                    <div class="flex justify-between">
                        <span>Target: <strong>${p.siteName}</strong></span>
                        <span>Local Gravity: <strong>g = ${p.gravity} m/s²</strong></span>
                    </div>
                    <div class="flex justify-between text-indigo-300">
                        <span>Vis-Viva: <strong>${state.calc.visVivaKmS} km/s</strong></span>
                        <span>Solar Flux: <strong>${state.calc.solarFlux} W/m²</strong></span>
                        <span>Asteroids Cleared: <strong class="text-neonCyan">${state.asteroidsDodged + state.asteroidsDestroyed}</strong></span>
                    </div>
                `;
            }

            card.innerHTML = `
                <div class="space-y-2.5">
                    <div class="flex items-center justify-between text-xs font-mono text-neonCyan">
                        <span>⚡ AUTHORITY: OPERATE WITH [WASD + SPACE + E] OR CLICK BELOW</span>
                        <span>STAGE ${ev.stageNum}/6</span>
                    </div>
                    <p class="text-xs text-indigo-100 leading-relaxed">${ev.prompt}</p>
                    <div class="space-y-2 pt-1">
                        ${ev.choices.map((ch, idx) => `
                            <button id="flight-choice-${idx}" onclick="makeFlightDecision(${idx})" class="w-full p-2.5 rounded-xl bg-indigo-950/80 hover:bg-indigo-900 border-2 border-indigo-500/40 hover:border-neonCyan text-left transition">
                                <div class="font-display font-bold text-xs sm:text-sm text-white">${ch.label}</div>
                                <div class="text-xs font-mono text-neonGreen mt-0.5">${ch.effectText}</div>
                            </button>
                        `).join('')}
                    </div>
                </div>
            `;

            setTimeout(updateHandPosition, 30);
        }

        function makeFlightDecision(choiceIdx) {
            if (!state.inFlight || state.flightFinished || state.missionFailedLOV) return;
            playSfx('success');
            const ev = FLIGHT_EVENTS[state.flightStageIdx];
            const ch = ev.choices[choiceIdx] || ev.choices[0];

            state.hullHealth = Math.max(10, state.hullHealth + ch.hullDelta);
            state.fuelLeft = Math.max(5, state.fuelLeft + ch.fuelDelta);
            state.totalScience += ch.sciDelta;
            state.laserPulseTimer = 30;

            if (state.flightStageIdx === 4) {
                state.waypoints.forEach(w => w.collected = true);
                state.samplesCollected = Math.max(state.samplesCollected, 3);
            } else if (state.flightStageIdx === 5) {
                state.crystalCollected = true;
                state.flagPlanted = true;
                state.samplesCollected = 4;
            }

            if (state.flightStageIdx < FLIGHT_EVENTS.length - 1) {
                state.flightStageIdx++;
                if (state.flightStageIdx === 1 || state.flightStageIdx === 2) {
                    spawnAsteroidField(6);
                }
            } else {
                state.flightFinished = true;
            }
            renderFlightDeck();
        }

        /*********************************************************************
         * 9. FIRST-2-RUNS FLOATING HAND TUTORIAL (`👆`) + INSTANT SKIP
         *********************************************************************/
        function initTutorialRuns() {
            let count = 1;
            try {
                const stored = localStorage.getItem('pioneersx_v11_runs');
                count = stored ? parseInt(stored, 10) + 1 : 1;
                localStorage.setItem('pioneersx_v11_runs', String(count));
            } catch (e) { count = 1; }

            state.runMode = count <= 2 ? count : 3;
            state.guideActive = state.runMode <= 2;
            state.guideStep = 1;
            syncTutorialControls();
        }

        function setRunMode(val) {
            playSfx('tap');
            const mode = parseInt(val, 10);
            state.runMode = mode;
            try { localStorage.setItem('pioneersx_v11_runs', String(mode)); } catch (e) {}

            if (mode <= 2) {
                state.guideActive = true;
                state.guideStep = state.inFlight ? 4 : 1;
            } else {
                state.guideActive = false;
            }
            syncTutorialControls();
        }

        function skipTutorial() {
            playSfx('tap');
            state.guideActive = false;
            clearAllGlows();
            syncTutorialControls();
        }

        function restartTutorial() {
            playSfx('success');
            if (state.runMode > 2) state.runMode = 1;
            state.guideActive = true;
            state.guideStep = state.inFlight ? 4 : 1;
            syncTutorialControls();
        }

        function doGuidedStepForMe() {
            const cfg = GUIDE_STEPS.find(s => s.step === state.guideStep);
            if (!cfg) return;
            const target = document.querySelector(cfg.selector);
            if (target) target.click();
        }

        function clearAllGlows() {
            document.querySelectorAll('.guide-target-glow').forEach(el => el.classList.remove('guide-target-glow'));
        }

        function syncTutorialControls() {
            const sel = document.getElementById('run-mode-select');
            if (sel) sel.value = String(state.runMode <= 2 ? state.runMode : 3);

            const runLabel = state.runMode === 2 ? 'RUN 2/2' : 'RUN 1/2';
            const skipBtn = document.getElementById('header-skip-btn');
            const skipTxt = document.getElementById('header-skip-text');
            const restartBtn = document.getElementById('header-restart-btn');
            const handBox = document.getElementById('hand-pointer-box');
            const handSkip = document.getElementById('hand-skip-btn');

            if (skipTxt) skipTxt.textContent = `⏭ SKIP TUTORIAL (${runLabel})`;
            if (handSkip) handSkip.textContent = `⏭ Skip (${runLabel})`;

            if (state.guideActive && !(state.inFlight && state.flightFinished)) {
                skipBtn?.classList.remove('hidden');
                restartBtn?.classList.add('hidden');
                handBox?.classList.remove('hidden');
                updateHandPosition();
            } else {
                skipBtn?.classList.add('hidden');
                restartBtn?.classList.remove('hidden');
                handBox?.classList.add('hidden');
                clearAllGlows();
            }
        }

        function updateHandPosition() {
            const handBox = document.getElementById('hand-pointer-box');
            if (!state.guideActive || !handBox || (state.inFlight && state.flightFinished)) {
                if (handBox) handBox.classList.add('hidden');
                clearAllGlows();
                return;
            }

            const cfg = GUIDE_STEPS.find(s => s.step === state.guideStep);
            if (!cfg) return;

            clearAllGlows();
            const target = document.querySelector(cfg.selector);
            if (!target) return;
            target.classList.add('guide-target-glow');

            const runLabel = state.runMode === 2 ? 'RUN 2/2' : 'RUN 1/2';
            document.getElementById('hand-run-tag').textContent = `GUIDED ${runLabel} • STEP ${cfg.step} OF 4`;
            document.getElementById('hand-step-title').textContent = cfg.title;
            document.getElementById('hand-step-desc').textContent = cfg.desc;

            const rect = target.getBoundingClientRect();
            const boxW = 360, boxH = 165;
            let top = rect.bottom + 12;
            let left = rect.left;

            if (top + boxH > window.innerHeight - 10) top = Math.max(70, rect.top - boxH - 12);
            if (left + boxW > window.innerWidth - 15) left = Math.max(12, window.innerWidth - boxW - 15);
            left = Math.max(12, left);

            handBox.style.top = `${Math.round(top)}px`;
            handBox.style.left = `${Math.round(left)}px`;
        }

        /*********************************************************************
         * 10. PHYSICS STEPPER: LETHAL ASTEROIDS, LANDING GRAVITY, ROVER &
         *     1ST-PERSON ASTRONAUT SPACESUIT HELMET VISOR KINEMATICS
         *********************************************************************/
        let frameTick = 0;

        function setCockpitScene(scene) {
            state.cockpitScene = scene;
            if ((scene === 'space' || scene === 'orbit') && state.asteroids.length === 0) {
                spawnAsteroidField(6);
            }
            updateSceneButtonsUI();
            updateDynamicControlDeckLabels(scene);
            updateNasaCreditBadge();

            const p = PLANETS[state.planetId];
            const titles = {
                pad: 'LC-39A LAUNCHPAD & EARTH MOUNTAINS',
                ascent: 'STAGE 1 // LIFTOFF & MAX-Q ASCENT',
                space: 'STAGE 2 // LETHAL 3D ASTEROID BELT & VIS-VIVA CRUISE',
                orbit: `STAGE 3 // ${p.name.toUpperCase()} ORBIT CAPTURE & DEBRIS`,
                landing: `STAGE 4 // ${p.name.toUpperCase()} DESCENT (g = ${p.gravity} m/s²)`,
                rover: `STAGE 5 // ${p.name.toUpperCase()} 6-WHEELED ROVER TRAVERSE`,
                eva: `STAGE 6 // 1ST-PERSON ASTRONAUT HELMET VISOR (${p.name.toUpperCase()})`
            };
            const titleEl = document.getElementById('cockpit-scene-title');
            if (titleEl) titleEl.textContent = titles[scene] || titles.pad;
        }

        function updateDynamicControlDeckLabels(scene) {
            const titleEl = document.getElementById('deck-control-title');
            const descEl = document.getElementById('deck-control-desc');
            const spBtn = document.getElementById('key-space');
            const eBtn = document.getElementById('key-e');

            if (scene === 'orbit') {
                if (titleEl) titleEl.textContent = '🛑 ORBITAL CAPTURE SPEED GATE (HOLD [Q] TO SLOW DOWN!)';
                if (descEl) descEl.textContent = 'Hold [Q] or drag the Cockpit Retro-Lever down to decelerate from 11.4 km/s into 2.2–4.2 km/s before Periapsis!';
                if (spBtn) spBtn.textContent = '[SPACE] 🛑 RETRO-BURN';
                if (eBtn) eBtn.textContent = '[E] 🔒 LOCK ORBIT';
            } else if (scene === 'space') {
                if (titleEl) titleEl.textContent = '☄️ DODGE LETHAL ASTEROIDS (YOKE / WASD)';
                if (descEl) descEl.textContent = 'Drag the Cockpit Yoke or use WASD to steer around red 3D asteroids, or press [E] to fire Pulse Lasers!';
                if (spBtn) spBtn.textContent = '[SPACE] 🔥 THRUST';
                if (eBtn) eBtn.textContent = '[E] ⚡ FIRE LASER';
            } else if (scene === 'landing') {
                if (titleEl) titleEl.textContent = '🛬 3RD-PERSON LANDING & TRACK UNLOADING (WASD)';
                if (descEl) descEl.textContent = `Steer Lander over Pad (WASD), hold SPACE to cushion touchdown, & watch the steel track unload the Rover!`;
                if (spBtn) spBtn.textContent = '[SPACE] 🔥 RETRO-BURN';
                if (eBtn) eBtn.textContent = '[E] 🛤️ DEPLOY TRACK';
            } else if (scene === 'rover') {
                if (titleEl) titleEl.textContent = '🛞 DRIVE 6-WHEEL ROVER (YOKE / WASD)';
                if (descEl) descEl.textContent = 'Drive across craters to 3 glowing Science Waypoints & press [E] to drill subsurface cores!';
                if (spBtn) spBtn.textContent = '[SPACE] 🔬 LASER DRILL';
                if (eBtn) eBtn.textContent = '[E] 🛞 COLLECT CORE';
            } else if (scene === 'eva') {
                if (titleEl) titleEl.textContent = '👨‍🚀 ASTRONAUT EVA (1ST-PERSON VISOR / 3RD-PERSON SUIT)';
                if (descEl) descEl.textContent = 'W/S: Walk • A/D: Turn • SPACE: Low-G Jump • E: Collect Crystal & Plant Flag • C: Toggle 1st/3rd Cam!';
                if (spBtn) spBtn.textContent = '[SPACE] 👨‍🚀 LOW-G JUMP';
                if (eBtn) eBtn.textContent = '[E] 🚩 GLOVE / FLAG';
            } else {
                if (titleEl) titleEl.textContent = '🕹️ STEER SPACECRAFT YOKE (DRAG HANDLE OR WASD)';
                if (descEl) descEl.textContent = 'Grab the Cockpit Flight Yoke or press WASD to align inside the cyan Ascent Trajectory Ring!';
                if (spBtn) spBtn.textContent = '[SPACE] 🔥 THRUST';
                if (eBtn) eBtn.textContent = '[E] ⚡ STAGE ACTION';
            }
        }

        function updateSceneButtonsUI() {
            ['pad', 'ascent', 'space', 'orbit', 'landing', 'rover', 'eva'].forEach(sc => {
                const btn = document.getElementById(`scene-btn-${sc}`);
                if (!btn) return;
                btn.className = (state.cockpitScene === sc)
                    ? 'px-2 py-1 rounded bg-neonCyan text-cosmos font-bold'
                    : 'px-2 py-1 rounded text-indigo-300 hover:text-white';
            });
        }

        function stepFlightAndSurfacePhysics() {
            if (state.missionFailedLOV) return;
            const sc = state.cockpitScene;
            const p = PLANETS[state.planetId];
            const dt = 1 / 60;

            // ===============================================================
            // MODE 0: STAGE 6 ASTRONAUT SPACESUIT EVA WALK & LOW-G PARABOLIC JUMP
            // ===============================================================
            if (sc === 'eva') {
                // Parabolic vertical jump kinematics under exact planetary gravity: z(t)
                if (state.evaJumpZ > 0 || state.evaJumpVz !== 0) {
                    state.evaJumpZ += state.evaJumpVz * dt * 2.4;
                    state.evaJumpVz -= p.gravity * dt * 2.4;
                    if (state.evaJumpZ <= 0) {
                        state.evaJumpZ = 0;
                        state.evaJumpVz = 0;
                    }
                }

                if (state.autopilot) {
                    const targetObj = !state.crystalCollected ? state.crystalWorld : state.flagWorld;
                    const dx = targetObj.x - state.evaCamX;
                    const dy = targetObj.y - state.evaCamY;
                    const targetYaw = Math.atan2(dx, dy);
                    state.evaYaw += (targetYaw - state.evaYaw) * 0.08;
                    const dist = Math.hypot(dx, dy);
                    if (dist > 7) {
                        state.evaCamX += Math.sin(state.evaYaw) * 0.28;
                        state.evaCamY += Math.cos(state.evaYaw) * 0.28;
                        state.evaBobPhase += 0.18;
                    } else if (frameTick % 45 === 0) {
                        triggerActionOrLaserKey();
                    }
                } else {
                    if (state.keys.a) state.evaYaw -= 0.032;
                    if (state.keys.d) state.evaYaw += 0.032;
                    if (state.keys.w) {
                        state.evaCamX = Math.max(-75, Math.min(75, state.evaCamX + Math.sin(state.evaYaw) * 0.36));
                        state.evaCamY = Math.max(-75, Math.min(75, state.evaCamY + Math.cos(state.evaYaw) * 0.36));
                        state.evaBobPhase += 0.22;
                    }
                    if (state.keys.s) {
                        state.evaCamX = Math.max(-75, Math.min(75, state.evaCamX - Math.sin(state.evaYaw) * 0.28));
                        state.evaCamY = Math.max(-75, Math.min(75, state.evaCamY - Math.cos(state.evaYaw) * 0.28));
                        state.evaBobPhase += 0.18;
                    }
                }

                document.getElementById('hud-label-1').textContent = 'EVA JUMP Z:';
                document.getElementById('hud-altitude').textContent = `${state.evaJumpZ.toFixed(2)} m`;
                document.getElementById('hud-label-2').textContent = 'HEADING:';
                document.getElementById('hud-speed').textContent = `${Math.round(((state.evaYaw * 180) / Math.PI + 360) % 360)}°`;
                document.getElementById('hud-label-3').textContent = 'GRAVITY:';
                document.getElementById('hud-attitude').textContent = `${p.gravity} m/s²`;
                document.getElementById('hud-label-4').textContent = 'EVA TASK:';
                document.getElementById('hud-throttle').textContent = state.flagPlanted ? 'COMPLETE' : state.crystalCollected ? 'PLANT FLAG [E]' : 'CRYSTAL [E]';
                return;
            }

            // ===============================================================
            // MODE 1: STAGE 5 6-WHEELED SURFACE ROVER TRAVERSE & DRILLING
            // ===============================================================
            if (sc === 'rover') {
                if (state.autopilot) {
                    const nextWp = state.waypoints.find(w => !w.collected);
                    if (nextWp) {
                        const dx = nextWp.x - state.roverX;
                        const dy = nextWp.y - state.roverY;
                        const dist = Math.hypot(dx, dy);
                        state.roverHeading = Math.atan2(dy, dx);
                        if (dist > 14) {
                            state.roverX += (dx / dist) * 1.45;
                            state.roverY += (dy / dist) * 1.45;
                            state.roverSpeedKmH = 14.8;
                            state.roverWheelRot += 0.24;
                        } else if (frameTick % 40 === 0) {
                            triggerActionOrLaserKey();
                        }
                    }
                } else {
                    let moving = false;
                    if (state.keys.w) { state.roverY = Math.max(-75, state.roverY - 1.5); moving = true; }
                    if (state.keys.s) { state.roverY = Math.min(75, state.roverY + 1.5); moving = true; }
                    if (state.keys.a) { state.roverX = Math.max(-210, state.roverX - 1.8); moving = true; }
                    if (state.keys.d) { state.roverX = Math.min(220, state.roverX + 1.8); moving = true; }
                    if (moving) {
                        state.roverSpeedKmH = 16.4;
                        state.roverWheelRot += 0.26;
                    } else if (!state.yokeDragActive) {
                        state.roverSpeedKmH *= 0.85;
                    }
                }

                const doneCount = state.waypoints.filter(w => w.collected).length;
                document.getElementById('hud-label-1').textContent = 'ROVER POS:';
                document.getElementById('hud-altitude').textContent = `${Math.round(state.roverX)}, ${Math.round(state.roverY)}m`;
                document.getElementById('hud-label-2').textContent = 'DRIVE SPD:';
                document.getElementById('hud-speed').textContent = `${state.roverSpeedKmH.toFixed(1)} km/h`;
                document.getElementById('hud-label-3').textContent = 'CORES DRILLED:';
                document.getElementById('hud-attitude').textContent = `${doneCount} / 3`;
                document.getElementById('hud-label-4').textContent = 'RTG BATTERY:';
                document.getElementById('hud-throttle').textContent = `${Math.round(state.fuelLeft)}%`;
                return;
            }

            // ===============================================================
            // MODE 2: STAGE 4 3RD-PERSON POWERED DESCENT & TRACK UNLOADING
            // ===============================================================
            if (sc === 'landing') {
                if (!state.landedOnPad) {
                    if (state.autopilot) {
                        state.landerX += (0 - state.landerX) * 0.04;
                        state.landerY += (0 - state.landerY) * 0.04;
                        state.throttle = state.descentRateMS > 4.8 ? 86 : 52;
                    } else {
                        if (state.keys.a) state.landerX = Math.max(-0.85, state.landerX - 0.011);
                        if (state.keys.d) state.landerX = Math.min(0.85, state.landerX + 0.011);
                        if (state.keys.w) state.landerY = Math.max(-0.65, state.landerY - 0.011);
                        if (state.keys.s) state.landerY = Math.min(0.65, state.landerY + 0.011);

                        if (state.keys.space) {
                            state.throttle = Math.min(100, state.throttle + 3.2);
                        } else if (!state.throttleDragActive) {
                            state.throttle = Math.max(20, state.throttle - 1.2);
                        }
                    }

                    // Extended descent physics so player enjoys piloting the 3rd-person landing
                    const retroAccel = (state.throttle / 100) * (p.gravity * 2.35);
                    const netDownAccel = p.gravity - retroAccel;
                    state.descentRateMS = Math.max(2.0, Math.min(24.0, state.descentRateMS + netDownAccel * 0.055));
                    state.landerAltM = Math.max(0, state.landerAltM - state.descentRateMS * 0.14);

                    const padAligned = Math.hypot(state.landerX, state.landerY) < 0.26;
                    state.stageProgress = Math.min(70, Math.round(((1400 - state.landerAltM) / 1400) * 70));

                    const alignEl = document.getElementById('flight-alignment-status');
                    if (alignEl) {
                        alignEl.textContent = padAligned
                            ? `✓ PAD LOCKED • ALT: ${Math.round(state.landerAltM)}m • V_z: -${state.descentRateMS.toFixed(1)} m/s`
                            : '⚠️ STEER 3RD-PERSON LANDER OVER LANDING ZONE ALPHA [WASD / YOKE]';
                        alignEl.className = padAligned ? 'text-neonGreen font-bold' : 'text-neonAmber font-bold';
                    }

                    if (state.landerAltM <= 4) {
                        state.landerAltM = 0;
                        state.descentRateMS = 0;
                        state.landedOnPad = true;
                        state.landerX = 0;
                        state.landerY = 0;
                        playSfx('success');
                        showCockpitToast('🛬 TOUCHDOWN ON LANDING ZONE ALPHA! UNFOLDING STEEL CARGO TRACK & UNLOADING ROVER!', 3200);
                    }
                } else {
                    // Post-Touchdown 3rd-Person Track Deployment & Rover Roll-Out Sequence!
                    state.throttle = 0;
                    if (state.rampDeployPct < 100) {
                        state.rampDeployPct = Math.min(100, state.rampDeployPct + 0.85);
                    } else if (state.roverUnloadPct < 100) {
                        state.roverUnloadPct = Math.min(100, state.roverUnloadPct + 0.75);
                        state.roverWheelRot += 0.22;
                    }
                    state.stageProgress = 70 + Math.round((state.rampDeployPct * 0.15) + (state.roverUnloadPct * 0.15));

                    const alignEl = document.getElementById('flight-alignment-status');
                    if (alignEl) {
                        alignEl.textContent = state.rampDeployPct < 100
                            ? `🛤️ LOWERING HYDRAULIC CARGO TRACK: ${Math.round(state.rampDeployPct)}%`
                            : `🛞 UNLOADING 6-WHEELED ROVER DOWN TRACK: ${Math.round(state.roverUnloadPct)}%`;
                        alignEl.className = 'text-neonCyan font-bold';
                    }

                    if (state.roverUnloadPct >= 100 && state.inFlight && !state.flightFinished) {
                        showCockpitToast('🛞 ROVER UNLOADED ONTO PLANETARY SURFACE! ENGAGING STAGE 5 TRAVERSE!', 2600);
                        makeFlightDecision(0);
                    }
                }

                document.getElementById('hud-label-1').textContent = 'RADAR ALT:';
                document.getElementById('hud-altitude').textContent = `${Math.round(state.landerAltM)} m`;
                document.getElementById('hud-label-2').textContent = 'DESCENT V_z:';
                document.getElementById('hud-speed').textContent = `-${state.descentRateMS.toFixed(1)} m/s`;
                document.getElementById('hud-label-3').textContent = 'TRACK RAMP:';
                document.getElementById('hud-attitude').textContent = `${Math.round(state.rampDeployPct)}%`;
                document.getElementById('hud-label-4').textContent = 'ROVER ROLL:';
                document.getElementById('hud-throttle').textContent = `${Math.round(state.roverUnloadPct)}%`;

                if (state.inFlight && !state.flightFinished) {
                    updateFlightTelemetryBars();
                }
                return;
            }

            // ===============================================================
            // MODE 4: STAGES 1–3 SPACECRAFT COCKPIT, LETHAL 3D ASTEROIDS &
            //         STAGE 3 ORBITAL CAPTURE SPEED GATE
            // ===============================================================
            state.ringPhase += 0.016;
            const targetPitch = Math.sin(state.ringPhase * 0.85) * 14;
            const targetYaw = Math.cos(state.ringPhase * 0.65) * 18;

            // AI Auto-Pilot computes asteroid evasion vector + trajectory tracking + orbital retro-braking
            if (state.autopilot) {
                let avoidYaw = 0, avoidPitch = 0;
                if (sc === 'space' || sc === 'orbit') {
                    state.asteroids.forEach(ast => {
                        const relX = ast.x - (state.yaw / 50);
                        const relY = ast.y + (state.pitch / 50);
                        const dist2D = Math.hypot(relX, relY);
                        if (ast.z < 0.55 && dist2D < 0.28) {
                            avoidYaw -= Math.sign(relX || 1) * 18;
                            avoidPitch += Math.sign(relY || 1) * 14;
                            if (ast.z < 0.32 && frameTick % 25 === 0) {
                                triggerActionOrLaserKey();
                            }
                        }
                    });
                }
                state.pitch += ((targetPitch + avoidPitch) - state.pitch) * 0.09;
                state.yaw += ((targetYaw + avoidYaw) - state.yaw) * 0.09;
                state.roll += ((targetYaw - state.yaw) * 0.6 - state.roll) * 0.1;

                if (sc === 'orbit') {
                    // Auto-pilot automatically fires retro-brakes to decelerate into orbital capture window
                    if (state.orbitVelKmS > 3.25) {
                        state.retroBrakingActive = true;
                        state.throttle = 15;
                        state.orbitVelKmS = Math.max(3.15, state.orbitVelKmS - 0.032);
                    } else {
                        state.retroBrakingActive = false;
                    }
                } else if (state.inFlight && !state.flightFinished) {
                    state.throttle = 88;
                }
            } else {
                if (state.keys.w) state.pitch = Math.min(42, state.pitch + 0.95);
                if (state.keys.s) state.pitch = Math.max(-42, state.pitch - 0.95);
                if (state.keys.a) {
                    state.yaw = Math.max(-45, state.yaw - 1.05);
                    state.roll = Math.max(-26, state.roll - 1.1);
                } else if (state.keys.d) {
                    state.yaw = Math.min(45, state.yaw + 1.05);
                    state.roll = Math.min(26, state.roll + 1.1);
                } else if (!state.yokeDragActive) {
                    state.roll *= 0.92;
                }

                if (sc === 'orbit') {
                    // In Stage 3 Orbit Capture, holding [Q] or [SPACE] or pulling Throttle lever down engages Retro-Braking!
                    if (state.keys.q || state.keys.space || (state.throttleDragActive && state.throttle < 40)) {
                        state.retroBrakingActive = true;
                        state.orbitVelKmS = Math.max(1.80, state.orbitVelKmS - 0.036);
                    } else {
                        state.retroBrakingActive = false;
                        // Gravity well slightly accelerates spacecraft if pilot doesn't brake!
                        state.orbitVelKmS = Math.min(13.80, state.orbitVelKmS + 0.003);
                    }
                } else {
                    if (state.keys.space) {
                        state.throttle = Math.min(100, state.throttle + 3.0);
                    } else if (state.keys.q) {
                        state.throttle = Math.max(0, state.throttle - 4.5);
                    } else if (!state.inFlight && state.staticFireTimer <= 0 && !state.throttleDragActive) {
                        state.throttle = Math.max(0, state.throttle - 2);
                    } else if (state.inFlight && !state.throttleDragActive) {
                        state.throttle = Math.max(45, state.throttle - 0.4);
                    }
                }
            }

            // Step 3D Asteroids & Check Collision Box in Stages 2 & 3
            let dangerProximity = false;
            if (sc === 'space' || sc === 'orbit') {
                const shipNormX = state.yaw / 45;
                const shipNormY = -state.pitch / 45;

                state.asteroids.forEach((ast, idx) => {
                    ast.z -= (0.0034 + (state.throttle * 0.000025));
                    ast.x += ast.vx;
                    ast.y += ast.vy;
                    ast.rot += ast.rotSpd;

                    const distFromNose = Math.hypot(ast.x - shipNormX, ast.y - shipNormY);
                    if (ast.z < 0.45 && distFromNose < 0.22) {
                        dangerProximity = true;
                    }

                    if (ast.z <= 0.11) {
                        if (!state.autopilot && distFromNose < 0.185) {
                            triggerAsteroidCollision(ast);
                        } else {
                            state.asteroidsDodged++;
                            if (state.inFlight && !state.flightFinished) {
                                state.pilotBonus += 15;
                                state.totalScience += 15;
                            }
                        }
                        state.asteroids[idx] = createSingleAsteroid(1.0 + Math.random() * 0.25);
                    }
                });
            }

            const errDist = Math.hypot(state.yaw - targetYaw, state.pitch - targetPitch);
            const isAligned = errDist < 16;

            // STAGE 3 ORBIT CAPTURE MECHANICS vs STAGES 1 & 2 PROGRESS
            if (sc === 'orbit') {
                const inWindow = state.orbitVelKmS >= state.orbitTargetMinKmS && state.orbitVelKmS <= state.orbitTargetMaxKmS;
                if (state.inFlight && !state.flightFinished) {
                    state.periapsisTimerSec = Math.max(0, state.periapsisTimerSec - dt);
                    const decelProgress = Math.max(0, Math.min(80, ((11.40 - state.orbitVelKmS) / (11.40 - 3.80)) * 80));

                    if (inWindow) {
                        state.orbitCaptureHoldSec += dt;
                        state.stageProgress = Math.min(100, Math.round(80 + (state.orbitCaptureHoldSec / 3.5) * 20));
                    } else {
                        state.stageProgress = Math.round(decelProgress);
                    }

                    const alignEl = document.getElementById('flight-alignment-status');
                    if (alignEl) {
                        if (inWindow) {
                            alignEl.textContent = `✅ IN CAPTURE WINDOW (${state.orbitVelKmS.toFixed(2)} km/s)! LOCKING ORBIT (${Math.min(100, Math.round((state.orbitCaptureHoldSec / 3.5) * 100))}%)...`;
                            alignEl.className = 'text-neonGreen font-bold';
                        } else {
                            alignEl.textContent = `🚨 SPEED TOO HIGH: ${state.orbitVelKmS.toFixed(2)} km/s! HOLD [Q] / RETRO-BRAKE BEFORE PERIAPSIS (${state.periapsisTimerSec.toFixed(1)}s)!`;
                            alignEl.className = 'text-neonRed font-bold animate-pulse';
                        }
                    }

                    updateFlightTelemetryBars();

                    if (state.stageProgress >= 100) {
                        state.orbitCaptured = true;
                        showCockpitToast('🛰️ ORBITAL CAPTURE COMPLETE! TRANSITIONING TO 3RD-PERSON LANDING!', 2600);
                        makeFlightDecision(0);
                    } else if (state.periapsisTimerSec <= 0 && !inWindow && !state.autopilot) {
                        triggerOrbitOvershootFailure();
                    }
                }

                document.getElementById('hud-label-1').textContent = 'PERIAPSIS T-:';
                document.getElementById('hud-altitude').textContent = `${state.periapsisTimerSec.toFixed(1)} s`;
                document.getElementById('hud-label-2').textContent = 'APPROACH VEL:';
                document.getElementById('hud-speed').textContent = `${state.orbitVelKmS.toFixed(2)} km/s`;
                document.getElementById('hud-label-3').textContent = 'CAPTURE GATE:';
                document.getElementById('hud-attitude').textContent = `2.2–4.2 km/s`;
                document.getElementById('hud-label-4').textContent = 'RETRO-BURN:';
                document.getElementById('hud-throttle').textContent = state.retroBrakingActive ? 'ACTIVE 🔥' : 'STANDBY';
            } else {
                // Extended Stage 1 (Ascent) and Stage 2 (Asteroid Cruise) duration (~30 seconds of manual piloting)
                if (state.inFlight && !state.flightFinished) {
                    state.altitudeKm += (state.throttle * 0.025);
                    const speedFactor = (state.throttle > 50 ? 0.095 : 0.045) * (isAligned ? 1.55 : 0.75);
                    state.stageProgress = Math.min(100, state.stageProgress + speedFactor);

                    const alignEl = document.getElementById('flight-alignment-status');
                    if (alignEl) {
                        if (dangerProximity) {
                            alignEl.textContent = '🚨 COLLISION WARNING: PULL YOKE [WASD] OR FIRE LASER [E]!';
                            alignEl.className = 'text-neonRed font-bold animate-pulse';
                        } else {
                            alignEl.textContent = isAligned
                                ? `✓ TRAJECTORY LOCKED • ASTEROIDS CLEARED: ${state.asteroidsDodged + state.asteroidsDestroyed}`
                                : '⚠️ STEER YOKE INTO CYAN RING [WASD] & WATCH FOR ASTEROIDS';
                            alignEl.className = isAligned ? 'text-neonGreen font-bold' : 'text-neonAmber font-bold';
                        }
                    }

                    updateFlightTelemetryBars();

                    if (state.stageProgress >= 100) {
                        makeFlightDecision(0);
                    }
                }

                document.getElementById('hud-label-1').textContent = 'ALT:';
                document.getElementById('hud-altitude').textContent = state.altitudeKm > 10000
                    ? `${(state.altitudeKm / 1e6).toFixed(2)}M km`
                    : `${state.altitudeKm.toFixed(1)} km`;
                document.getElementById('hud-label-2').textContent = 'VIS-VIVA VEL:';
                document.getElementById('hud-speed').textContent = `${state.calc.visVivaKmS || '11.20'} km/s`;
                document.getElementById('hud-label-3').textContent = 'PITCH/YAW:';
                document.getElementById('hud-attitude').textContent = `${state.pitch >= 0 ? '+' : ''}${Math.round(state.pitch)}° / ${state.yaw >= 0 ? '+' : ''}${state.yaw}°`;
                document.getElementById('hud-label-4').textContent = 'HULL INTEGRITY:';
                document.getElementById('hud-throttle').textContent = `${Math.round(state.hullHealth)}%`;
            }

            state.ringX = (targetYaw - state.yaw) / 45;
            state.ringY = (state.pitch - targetPitch) / 45;
        }

        function resizeCockpitCanvas() {
            const c = document.getElementById('cockpit-canvas');
            if (c && c.parentElement && c.parentElement.clientWidth > 0) {
                c.width = c.parentElement.clientWidth;
                c.height = c.parentElement.clientHeight;
            }
        }

        /*********************************************************************
         * 11. PHOTOREALISTIC HARDWARE RENDERERS (ZERO STICKMEN!):
         *     - MULTI-STAGE METALLIC NASA LANDER + HYDRAULIC CARGO TRACK
         *     - 6-WHEELED PERSEVERANCE/LTV ROVER WITH ROCKER-BOGIE & DRIVER
         *     - VOLUMETRIC 3D-SHADED NASA xEMU ASTRONAUT SPACESUIT MODEL
         *********************************************************************/
        function drawLandedSpacecraft(ctx, x, y, scale = 1, thrustPct = 0, rampPct = 100, roverUnloadPct = 100) {
            ctx.save();
            ctx.translate(x, y);
            ctx.scale(scale, scale);

            // Ground shadow
            ctx.fillStyle = 'rgba(4, 6, 17, 0.65)';
            ctx.beginPath();
            ctx.ellipse(0, 52, 78, 16, 0, 0, Math.PI * 2);
            ctx.fill();

            // 1. Twin Mach-Diamond Retro-Thruster Plumes (when descending in 3rd-Person)
            if (thrustPct > 5) {
                const pLen = 28 + (thrustPct * 0.72) + Math.random() * 14;
                [-18, 18].forEach(nx => {
                    const plumeGrad = ctx.createLinearGradient(nx, 28, nx, 28 + pLen);
                    plumeGrad.addColorStop(0, '#FFFFFF');
                    plumeGrad.addColorStop(0.25, '#00F0FF');
                    plumeGrad.addColorStop(0.65, 'rgba(168, 85, 247, 0.65)');
                    plumeGrad.addColorStop(1, 'rgba(249, 115, 22, 0)');
                    ctx.fillStyle = plumeGrad;
                    ctx.beginPath();
                    ctx.moveTo(nx - 8, 28);
                    ctx.lineTo(nx + 8, 28);
                    ctx.lineTo(nx + 18, 28 + pLen);
                    ctx.lineTo(nx - 18, 28 + pLen);
                    ctx.closePath();
                    ctx.fill();

                    // Glowing Mach Shock Diamonds
                    ctx.fillStyle = 'rgba(255, 255, 255, 0.85)';
                    for (let d = 1; d <= 3; d++) {
                        const dy = 28 + (pLen * 0.24 * d);
                        ctx.beginPath();
                        ctx.moveTo(nx, dy - 5);
                        ctx.lineTo(nx + 5, dy);
                        ctx.lineTo(nx, dy + 5);
                        ctx.lineTo(nx - 5, dy);
                        ctx.closePath();
                        ctx.fill();
                    }
                });
            }

            // 2. Heavy Articulated Hydraulic Landing Struts & Crush Footpads
            ctx.strokeStyle = '#94A3B8';
            ctx.lineWidth = 4.5;
            ctx.beginPath();
            // Left primary leg + secondary piston strut
            ctx.moveTo(-28, 6); ctx.lineTo(-58, 48);
            ctx.moveTo(-24, 22); ctx.lineTo(-48, 36);
            // Right primary leg + secondary piston strut
            ctx.moveTo(28, 6); ctx.lineTo(58, 48);
            ctx.moveTo(24, 22); ctx.lineTo(48, 36);
            ctx.stroke();

            // Gold-anodized shock dampers & circular footpads
            ctx.fillStyle = '#F59E0B';
            [-58, 58].forEach(fx => {
                ctx.beginPath();
                ctx.ellipse(fx, 49, 12, 4, 0, 0, Math.PI * 2);
                ctx.fill();
                ctx.strokeStyle = '#00F0FF';
                ctx.lineWidth = 1.2;
                ctx.stroke();
            });

            // 3. Lower Descent Stage Protected with Real Multi-Layer Gold Kapton Foil (MLI)
            const kaptonGrad = ctx.createLinearGradient(-34, 4, 34, 28);
            kaptonGrad.addColorStop(0, '#B45309');
            kaptonGrad.addColorStop(0.35, '#FBBF24');
            kaptonGrad.addColorStop(0.55, '#FEF08A');
            kaptonGrad.addColorStop(0.8, '#D97706');
            kaptonGrad.addColorStop(1, '#78350F');
            ctx.fillStyle = kaptonGrad;
            ctx.strokeStyle = '#F59E0B';
            ctx.lineWidth = 1.6;
            ctx.beginPath();
            ctx.roundRect(-34, 4, 68, 25, 4);
            ctx.fill();
            ctx.stroke();

            // Quilted Kapton foil seam lines
            ctx.strokeStyle = 'rgba(120, 53, 15, 0.55)';
            ctx.lineWidth = 1;
            ctx.beginPath();
            ctx.moveTo(-16, 4); ctx.lineTo(-16, 29);
            ctx.moveTo(16, 4); ctx.lineTo(16, 29);
            ctx.moveTo(-34, 16); ctx.lineTo(34, 16);
            ctx.stroke();

            // 4. Upper Pressurized Stainless-Titanium Hull Cylinder & Nose Fairing
            const hullGrad = ctx.createLinearGradient(-32, -88, 32, 4);
            hullGrad.addColorStop(0, '#475569');
            hullGrad.addColorStop(0.28, '#E2E8F0');
            hullGrad.addColorStop(0.55, '#94A3B8');
            hullGrad.addColorStop(0.85, '#1E293B');
            hullGrad.addColorStop(1, '#0F172A');
            ctx.fillStyle = hullGrad;
            ctx.strokeStyle = '#38BDF8';
            ctx.lineWidth = 1.8;

            ctx.beginPath();
            ctx.moveTo(0, -96);
            ctx.bezierCurveTo(24, -78, 32, -48, 32, -22);
            ctx.lineTo(32, 4);
            ctx.lineTo(-32, 4);
            ctx.lineTo(-32, -22);
            ctx.bezierCurveTo(-32, -48, -24, -78, 0, -96);
            ctx.closePath();
            ctx.fill();
            ctx.stroke();

            // Black Carbon-Silicon TPS Heatshield Tiles & RCS Thruster Pods
            ctx.fillStyle = '#0F172A';
            ctx.fillRect(-36, -34, 6, 14);
            ctx.fillRect(30, -34, 6, 14);

            // Illuminated Crew Canopy Windows
            const winGrad = ctx.createLinearGradient(-18, -56, 18, -42);
            winGrad.addColorStop(0, '#00F0FF');
            winGrad.addColorStop(1, '#1D4ED8');
            ctx.fillStyle = winGrad;
            ctx.beginPath();
            ctx.roundRect(-20, -55, 17, 10, 3);
            ctx.roundRect(3, -55, 17, 10, 3);
            ctx.fill();

            // Official "ThePioneersX" Vertical Fuselage Markings
            ctx.fillStyle = '#0F172A';
            ctx.font = 'bold 8px "Orbitron", sans-serif';
            ctx.fillText('PIONEERS-X', -25, -28);

            // Payload Bay Garage Door Cavity (Opens as ramp deploys)
            ctx.fillStyle = '#040611';
            ctx.strokeStyle = '#00F0FF';
            ctx.lineWidth = 1.4;
            ctx.fillRect(-16, -12, 32, 38);
            ctx.strokeRect(-16, -12, 32, 38);

            // 5. Articulated Steel Cargo Deployment Track & Unloading 6-Wheeled Rover
            if (rampPct > 1) {
                const rRatio = rampPct / 100;
                const trackEndX = 22 + rRatio * 105;
                const trackEndY = 24 + rRatio * 28;

                // Hydraulic deployment cables
                ctx.strokeStyle = '#38BDF8';
                ctx.lineWidth = 1.5;
                ctx.beginPath();
                ctx.moveTo(14, -6);
                ctx.lineTo(trackEndX * 0.65, trackEndY - 6);
                ctx.stroke();

                // Heavy twin steel track rails with cross-treads
                ctx.strokeStyle = '#CBD5E1';
                ctx.lineWidth = 5;
                ctx.beginPath();
                ctx.moveTo(12, 24);
                ctx.lineTo(trackEndX, trackEndY);
                ctx.stroke();

                ctx.strokeStyle = '#00F0FF';
                ctx.lineWidth = 2;
                for (let t = 0.15; t <= 0.95; t += 0.16) {
                    const tx = 12 + (trackEndX - 12) * t;
                    const ty = 24 + (trackEndY - 24) * t;
                    ctx.beginPath();
                    ctx.moveTo(tx - 3, ty - 4);
                    ctx.lineTo(tx + 3, ty + 4);
                    ctx.stroke();
                }

                // If the rover is currently rolling down the track (roverUnloadPct < 100), render it on the incline!
                if (roverUnloadPct > 0 && roverUnloadPct < 100) {
                    const uRatio = roverUnloadPct / 100;
                    const rvX = 14 + (trackEndX - 14) * uRatio;
                    const rvY = 12 + (trackEndY - 20) * uRatio;
                    const trackAngle = Math.atan2(trackEndY - 24, trackEndX - 12);
                    ctx.save();
                    ctx.translate(rvX, rvY);
                    ctx.rotate(trackAngle * (1 - uRatio * 0.5));
                    drawSixWheeledRover(ctx, 0, 0, state.roverWheelRot, false, 0.62, true);
                    ctx.restore();
                }
            }

            ctx.restore();
        }

        function drawSixWheeledRover(ctx, x, y, wheelRot, laserActive, scale = 1, showSeatedAstronaut = true) {
            ctx.save();
            ctx.translate(x, y);
            ctx.scale(scale, scale);

            // Surface Contact Shadow
            ctx.fillStyle = 'rgba(4, 6, 17, 0.65)';
            ctx.beginPath();
            ctx.ellipse(0, 26, 54, 10, 0, 0, Math.PI * 2);
            ctx.fill();

            // 1. Rear MMRTG Pu-238 Nuclear Power Source with White/Gold Radiator Fins
            ctx.fillStyle = '#334155';
            ctx.strokeStyle = '#F59E0B';
            ctx.lineWidth = 1.6;
            ctx.beginPath();
            ctx.roundRect(-50, -16, 18, 18, 3);
            ctx.fill();
            ctx.stroke();

            // Glowing Pu-238 heat exchanger slots
            ctx.fillStyle = '#F97316';
            ctx.fillRect(-47, -12, 12, 3);
            ctx.fillRect(-47, -6, 12, 3);

            // 2. Main Warm-Electronic Box (WEB) Chassis with Gold Kapton & Aerospace White/Blue Armor
            const bodyGrad = ctx.createLinearGradient(-36, -12, 38, 10);
            bodyGrad.addColorStop(0, '#CBD5E1');
            bodyGrad.addColorStop(0.45, '#94A3B8');
            bodyGrad.addColorStop(0.8, '#334155');
            bodyGrad.addColorStop(1, '#1E293B');
            ctx.fillStyle = bodyGrad;
            ctx.strokeStyle = '#38BDF8';
            ctx.lineWidth = 1.8;
            ctx.beginPath();
            ctx.roundRect(-36, -10, 74, 18, 5);
            ctx.fill();
            ctx.stroke();

            // Gold Kapton side thermal panel
            ctx.fillStyle = '#F59E0B';
            ctx.fillRect(-24, -6, 36, 10);
            ctx.strokeStyle = '#78350F';
            ctx.strokeRect(-24, -6, 36, 10);

            // 3. Realistic Pressurized Astronaut Driver Seated at Rover Control Console (Not a stickman!)
            if (showSeatedAstronaut) {
                ctx.save();
                ctx.translate(-2, -12);
                // PLSS Life-Support Backpack
                ctx.fillStyle = '#E2E8F0';
                ctx.strokeStyle = '#475569';
                ctx.lineWidth = 1.4;
                ctx.fillRect(-14, -18, 9, 16);
                ctx.strokeRect(-14, -18, 9, 16);
                // Pressurized Hard Upper Torso (HUT)
                ctx.fillStyle = '#F8FAFC';
                ctx.beginPath();
                ctx.roundRect(-7, -17, 14, 16, 4);
                ctx.fill();
                ctx.stroke();
                // Red/Blue NASA Suit Striping
                ctx.fillStyle = '#EF4444';
                ctx.fillRect(-5, -11, 10, 2.5);
                // Domed Helmet & Reflective Gold-Iridium Visor
                ctx.fillStyle = '#F8FAFC';
                ctx.beginPath();
                ctx.arc(0, -24, 7.5, 0, Math.PI * 2);
                ctx.fill();
                ctx.stroke();
                const visorG = ctx.createRadialGradient(2, -25, 1, 2, -24, 6);
                visorG.addColorStop(0, '#FEF08A');
                visorG.addColorStop(0.6, '#F59E0B');
                visorG.addColorStop(1, '#78350F');
                ctx.fillStyle = visorG;
                ctx.beginPath();
                ctx.arc(2.2, -24, 5.2, -Math.PI * 0.45, Math.PI * 0.45);
                ctx.fill();
                // Articulated Pressurized Arm holding Rover Steering Yoke
                ctx.strokeStyle = '#E2E8F0';
                ctx.lineWidth = 4;
                ctx.lineCap = 'round';
                ctx.beginPath();
                ctx.moveTo(2, -12);
                ctx.lineTo(10, -8);
                ctx.lineTo(16, -12);
                ctx.stroke();
                // Control Yoke Column
                ctx.strokeStyle = '#00F0FF';
                ctx.lineWidth = 2;
                ctx.beginPath();
                ctx.moveTo(17, -2);
                ctx.lineTo(16, -14);
                ctx.stroke();
                ctx.restore();
            }

            // 4. Remote Sensing Mast (Mastcam-Z Stereo Camera Head & SuperCam Laser)
            ctx.strokeStyle = '#CBD5E1';
            ctx.lineWidth = 3.5;
            ctx.beginPath();
            ctx.moveTo(24, -10);
            ctx.lineTo(26, -32);
            ctx.stroke();

            // Mastcam-Z Camera Box
            ctx.fillStyle = '#F8FAFC';
            ctx.strokeStyle = '#334155';
            ctx.lineWidth = 1.5;
            ctx.beginPath();
            ctx.roundRect(19, -38, 16, 8, 2);
            ctx.fill();
            ctx.stroke();
            // SuperCam optical cyclops lens
            ctx.fillStyle = '#EF4444';
            ctx.beginPath();
            ctx.arc(29, -34, 2.5, 0, Math.PI * 2);
            ctx.fill();
            ctx.fillStyle = '#00F0FF';
            ctx.beginPath();
            ctx.arc(23, -34, 1.8, 0, Math.PI * 2);
            ctx.fill();

            // 5. Front 5-DOF Robotic Turret Arm & Core Drill
            ctx.strokeStyle = '#94A3B8';
            ctx.lineWidth = 3;
            ctx.beginPath();
            ctx.moveTo(35, -2);
            ctx.lineTo(48, -10);
            ctx.lineTo(54, 6);
            ctx.stroke();

            if (laserActive) {
                ctx.strokeStyle = '#00E676';
                ctx.lineWidth = 3.5;
                ctx.beginPath();
                ctx.moveTo(33, -34);
                ctx.lineTo(66, 22);
                ctx.moveTo(54, 6);
                ctx.lineTo(66, 22);
                ctx.stroke();
            }

            // 6. Articulated Titanium Rocker-Bogie Suspension Tubing
            ctx.strokeStyle = '#38BDF8';
            ctx.lineWidth = 2.8;
            ctx.beginPath();
            ctx.moveTo(-34, 19);
            ctx.lineTo(-16, 4);
            ctx.lineTo(2, 19);
            ctx.moveTo(-16, 4);
            ctx.lineTo(14, 2);
            ctx.lineTo(34, 19);
            ctx.stroke();

            // 7. 6 Machined Aluminum Cleated Wheels with Titanium Spokes
            [-34, 0, 34].forEach(wx => {
                ctx.save();
                ctx.translate(wx, 19);
                ctx.rotate(wheelRot);

                // Outer cleated tire tread
                ctx.fillStyle = '#0F172A';
                ctx.strokeStyle = '#94A3B8';
                ctx.lineWidth = 2.2;
                ctx.beginPath();
                ctx.arc(0, 0, 10, 0, Math.PI * 2);
                ctx.fill();
                ctx.stroke();

                // Curved titanium flex-spokes
                ctx.strokeStyle = '#38BDF8';
                ctx.lineWidth = 1.4;
                for (let s = 0; s < 6; s++) {
                    const ang = (s / 6) * Math.PI * 2;
                    ctx.beginPath();
                    ctx.moveTo(0, 0);
                    ctx.lineTo(Math.cos(ang) * 9, Math.sin(ang) * 9);
                    ctx.stroke();
                }

                // Wheel hubcap
                ctx.fillStyle = '#F59E0B';
                ctx.beginPath();
                ctx.arc(0, 0, 3, 0, Math.PI * 2);
                ctx.fill();
                ctx.restore();
            });

            ctx.restore();
        }

        function drawRealisticAstronaut3rdPerson(ctx, x, y, scale = 1, walkPhase = 0, armReachTimer = 0, holdingFlag = false) {
            ctx.save();
            ctx.translate(x, y);
            ctx.scale(scale, scale);

            const legSwing = Math.sin(walkPhase) * 10;
            const armSwing = -Math.sin(walkPhase) * 8;
            const reachBoost = armReachTimer > 0 ? 18 : 0;

            // Ground shadow
            ctx.fillStyle = 'rgba(4, 6, 17, 0.65)';
            ctx.beginPath();
            ctx.ellipse(0, 42, 24, 7, 0, 0, Math.PI * 2);
            ctx.fill();

            // 1. PLSS Life-Support Backpack (Behind Torso) with UHF Antenna & Oxygen Tanks
            ctx.fillStyle = '#CBD5E1';
            ctx.strokeStyle = '#475569';
            ctx.lineWidth = 1.8;
            ctx.beginPath();
            ctx.roundRect(-20, -24, 14, 32, 4);
            ctx.fill();
            ctx.stroke();
            // Backpack status beacon
            ctx.fillStyle = '#00E676';
            ctx.beginPath();
            ctx.arc(-16, -26, 2.5, 0, Math.PI * 2);
            ctx.fill();

            // 2. Left & Right Volumetric Pressurized Legs with Red NASA Commander Stripes & Lunar Boots
            // Left Leg (Back)
            ctx.save();
            ctx.translate(-4, 10);
            ctx.rotate((-legSwing * Math.PI) / 180);
            ctx.fillStyle = '#CBD5E1';
            ctx.strokeStyle = '#475569';
            ctx.lineWidth = 1.6;
            ctx.beginPath();
            ctx.roundRect(-6, 0, 11, 24, 5);
            ctx.fill();
            ctx.stroke();
            ctx.fillStyle = '#EF4444';
            ctx.fillRect(-6, 9, 11, 3);
            // Heavy Treaded Planetary Boot
            ctx.fillStyle = '#1E293B';
            ctx.strokeStyle = '#38BDF8';
            ctx.beginPath();
            ctx.roundRect(-7, 22, 15, 8, 3);
            ctx.fill();
            ctx.stroke();
            ctx.restore();

            // Right Leg (Front)
            ctx.save();
            ctx.translate(4, 10);
            ctx.rotate((legSwing * Math.PI) / 180);
            ctx.fillStyle = '#F8FAFC';
            ctx.strokeStyle = '#475569';
            ctx.lineWidth = 1.6;
            ctx.beginPath();
            ctx.roundRect(-5, 0, 11, 24, 5);
            ctx.fill();
            ctx.stroke();
            ctx.fillStyle = '#EF4444';
            ctx.fillRect(-5, 9, 11, 3);
            // Heavy Treaded Planetary Boot
            ctx.fillStyle = '#0F172A';
            ctx.strokeStyle = '#00F0FF';
            ctx.beginPath();
            ctx.roundRect(-6, 22, 15, 8, 3);
            ctx.fill();
            ctx.stroke();
            ctx.restore();

            // 3. Hard Upper Torso (HUT) Pressure Shell & Chest Display Control Module (DCM)
            const torsoGrad = ctx.createLinearGradient(-12, -24, 14, 14);
            torsoGrad.addColorStop(0, '#FFFFFF');
            torsoGrad.addColorStop(0.65, '#E2E8F0');
            torsoGrad.addColorStop(1, '#94A3B8');
            ctx.fillStyle = torsoGrad;
            ctx.strokeStyle = '#475569';
            ctx.lineWidth = 1.8;
            ctx.beginPath();
            ctx.roundRect(-12, -22, 24, 34, 7);
            ctx.fill();
            ctx.stroke();

            // NASA / PioneersX Vector Chest Patch & DCM Telemetry Box
            ctx.fillStyle = '#1E293B';
            ctx.strokeStyle = '#00F0FF';
            ctx.lineWidth = 1.2;
            ctx.fillRect(-6, -14, 14, 11);
            ctx.strokeRect(-6, -14, 14, 11);
            ctx.fillStyle = '#00E676';
            ctx.fillRect(-3, -11, 4, 2);
            ctx.fillStyle = '#00F0FF';
            ctx.fillRect(2, -11, 4, 2);

            // 4. Pressurized Articulated Right Arm & Glove
            ctx.save();
            ctx.translate(4, -16);
            ctx.rotate(((armSwing - reachBoost) * Math.PI) / 180);
            ctx.fillStyle = '#F8FAFC';
            ctx.strokeStyle = '#475569';
            ctx.lineWidth = 1.5;
            ctx.beginPath();
            ctx.roundRect(-4, 0, 9, 20, 4);
            ctx.fill();
            ctx.stroke();
            ctx.fillStyle = '#EF4444';
            ctx.fillRect(-4, 8, 9, 2.5);
            // Pressurized Blue/Titanium EV Glove
            ctx.fillStyle = '#1E293B';
            ctx.strokeStyle = '#00F0FF';
            ctx.beginPath();
            ctx.roundRect(-4, 18, 10, 7, 3);
            ctx.fill();
            ctx.stroke();
            ctx.restore();

            // 5. Spherical Domed Polycarbonate Helmet & 3D Gold-Iridium Reflective Solar Visor
            const helmGrad = ctx.createRadialGradient(-3, -34, 2, 0, -32, 13);
            helmGrad.addColorStop(0, '#FFFFFF');
            helmGrad.addColorStop(0.7, '#E2E8F0');
            helmGrad.addColorStop(1, '#94A3B8');
            ctx.fillStyle = helmGrad;
            ctx.strokeStyle = '#475569';
            ctx.lineWidth = 1.8;
            ctx.beginPath();
            ctx.arc(0, -32, 12.5, 0, Math.PI * 2);
            ctx.fill();
            ctx.stroke();

            // Curved Gold-Iridium Reflective Visor Dome
            const visorGrad = ctx.createRadialGradient(4, -35, 1, 3, -32, 10);
            visorGrad.addColorStop(0, '#FEF9C3');
            visorGrad.addColorStop(0.35, '#F59E0B');
            visorGrad.addColorStop(0.8, '#B45309');
            visorGrad.addColorStop(1, '#451A03');
            ctx.fillStyle = visorGrad;
            ctx.strokeStyle = '#FDE047';
            ctx.lineWidth = 1.2;
            ctx.beginPath();
            ctx.arc(2.5, -32, 9.2, -Math.PI * 0.48, Math.PI * 0.48);
            ctx.closePath();
            ctx.fill();
            ctx.stroke();

            // Specular glint on gold visor
            ctx.fillStyle = 'rgba(255, 255, 255, 0.75)';
            ctx.beginPath();
            ctx.ellipse(5, -35, 2.8, 1.4, -0.4, 0, Math.PI * 2);
            ctx.fill();

            ctx.restore();
        }

        function drawAlienSurfaceBackdrop(ctx, w, h, p, jumpOffsetPx = 0, yawShiftPx = 0) {
            const mediaKey = `${p.id}_surface`;
            drawHdrPhotorealisticBackdrop(ctx, w, h, mediaKey, yawShiftPx * 0.45, jumpOffsetPx * 0.5, 1.22);

            const horY = h * 0.44 + jumpOffsetPx * 0.35;

            // Atmospheric horizon scattering glow
            const atmGrad = ctx.createLinearGradient(0, horY - 55, 0, horY + 65);
            atmGrad.addColorStop(0, 'rgba(0, 0, 0, 0)');
            atmGrad.addColorStop(0.5, `${p.horizonGlow}38`);
            atmGrad.addColorStop(1, 'rgba(4, 6, 17, 0.28)');
            ctx.fillStyle = atmGrad;
            ctx.fillRect(0, horY - 55, w, 120);

            // Subtle holographic LIDAR range lines on planetary regolith
            ctx.strokeStyle = `${p.horizonGlow}22`;
            ctx.lineWidth = 1;
            for (let r = 1; r <= 5; r++) {
                const ry = horY + Math.pow(r / 5, 1.6) * (h * 0.34);
                ctx.beginPath();
                ctx.moveTo(0, ry);
                ctx.lineTo(w, ry);
                ctx.stroke();
            }

            return horY;
        }

        function drawPioneersFlag(ctx, x, y, scale = 1) {
            ctx.save();
            ctx.translate(x, y);
            ctx.scale(scale, scale);

            ctx.strokeStyle = '#38BDF8';
            ctx.lineWidth = 3;
            ctx.beginPath();
            ctx.moveTo(0, 26);
            ctx.lineTo(0, -54);
            ctx.stroke();

            const wave = Math.sin(frameTick * 0.14) * 4;
            const flagGrad = ctx.createLinearGradient(0, -52, 84, -20);
            flagGrad.addColorStop(0, '#0E1538');
            flagGrad.addColorStop(0.6, '#311042');
            flagGrad.addColorStop(1, '#00F0FF');
            ctx.fillStyle = flagGrad;
            ctx.strokeStyle = '#00F0FF';
            ctx.lineWidth = 2;
            ctx.beginPath();
            ctx.moveTo(0, -52);
            ctx.lineTo(84, -48 + wave);
            ctx.lineTo(84, -20 + wave);
            ctx.lineTo(0, -24);
            ctx.closePath();
            ctx.fill();
            ctx.stroke();

            ctx.fillStyle = '#00F0FF';
            ctx.font = 'bold 10px "Orbitron", sans-serif';
            ctx.fillText('ThePioneersX', 7, -34 + wave * 0.5);

            ctx.restore();
        }

        /*********************************************************************
         * STAGE 6: TRUE 1ST-PERSON (POV) ASTRONAUT SPACESUIT HELMET VISOR RENDERER
         *********************************************************************/
        function drawFirstPersonAstronautEVA(ctx, w, h, p) {
            // Head-bobbing while walking + vertical jump height offset
            const walkBobY = Math.sin(state.evaBobPhase) * 4.5;
            const walkBobX = Math.cos(state.evaBobPhase * 0.5) * 3.0;
            const jumpOffsetPx = state.evaJumpZ * 28;
            const yawShiftPx = -state.evaYaw * 240;

            const horY = drawAlienSurfaceBackdrop(ctx, w, h, p, jumpOffsetPx + walkBobY, yawShiftPx);

            // Helper to project 3D surface world coordinates (wx, wy) into 1st-person perspective screen coords
            function projectSurfacePoint(wx, wy) {
                const dx = wx - state.evaCamX;
                const dy = wy - state.evaCamY;
                // Rotate by astronaut's look yaw
                const rx = dx * Math.cos(-state.evaYaw) - dy * Math.sin(-state.evaYaw);
                const rz = dx * Math.sin(-state.evaYaw) + dy * Math.cos(-state.evaYaw);
                if (rz <= 2.5) return null; // behind visor
                const fovScale = 340 / rz;
                const screenX = w * 0.5 + rx * fovScale + walkBobX;
                const screenY = horY + (14 + state.evaJumpZ * 4.5) * fovScale + walkBobY;
                return { x: screenX, y: screenY, scale: Math.min(2.4, fovScale * 0.045), dist: Math.hypot(dx, dy), rz };
            }

            // 1. Project & Draw Parked Lander in 3D 1st-Person View
            const landerProj = projectSurfacePoint(state.landerWorld.x, state.landerWorld.y);
            if (landerProj) {
                drawLandedSpacecraft(ctx, landerProj.x, landerProj.y, landerProj.scale * 1.3);
            }

            // 2. Project & Draw Parked 6-Wheeled Rover in 3D 1st-Person View
            const roverProj = projectSurfacePoint(state.roverWorld.x, state.roverWorld.y);
            if (roverProj) {
                drawSixWheeledRover(ctx, roverProj.x, roverProj.y, 0, false, roverProj.scale * 1.3);
            }

            // 3. Project & Draw Glowing Rare Planetary Crystal Specimen
            const cryProj = projectSurfacePoint(state.crystalWorld.x, state.crystalWorld.y);
            if (cryProj) {
                ctx.save();
                ctx.translate(cryProj.x, cryProj.y);
                const sc = Math.max(0.6, cryProj.scale * 1.5);
                ctx.scale(sc, sc);

                if (!state.crystalCollected) {
                    const pulse = 22 + Math.sin(frameTick * 0.14) * 5;
                    ctx.strokeStyle = '#00F0FF';
                    ctx.fillStyle = 'rgba(0, 240, 255, 0.22)';
                    ctx.lineWidth = 2;
                    ctx.beginPath();
                    ctx.ellipse(0, 12, pulse, pulse * 0.4, 0, 0, Math.PI * 2);
                    ctx.fill();
                    ctx.stroke();

                    // Faceted Glowing Crystal
                    ctx.fillStyle = '#00F0FF';
                    ctx.strokeStyle = '#A855F7';
                    ctx.lineWidth = 2;
                    ctx.beginPath();
                    ctx.moveTo(0, -24);
                    ctx.lineTo(14, -2);
                    ctx.lineTo(0, 12);
                    ctx.lineTo(-14, -2);
                    ctx.closePath();
                    ctx.fill();
                    ctx.stroke();

                    ctx.fillStyle = '#00F0FF';
                    ctx.font = 'bold 10px "JetBrains Mono", monospace';
                    ctx.fillText(`💎 SPECIMEN (${cryProj.dist.toFixed(1)}m) [PRESS E]`, -68, -32);
                } else {
                    ctx.fillStyle = '#00E676';
                    ctx.font = 'bold 10px "JetBrains Mono", monospace';
                    ctx.fillText('✓ SPECIMEN COLLECTED', -52, 0);
                }
                ctx.restore();
            }

            // 4. Project & Draw Flag Planting Beacon / Planted "ThePioneersX" Flag
            const flagProj = projectSurfacePoint(state.flagWorld.x, state.flagWorld.y);
            if (flagProj) {
                const sc = Math.max(0.65, flagProj.scale * 1.55);
                if (state.flagPlanted) {
                    drawPioneersFlag(ctx, flagProj.x, flagProj.y, sc);
                } else {
                    ctx.save();
                    ctx.translate(flagProj.x, flagProj.y);
                    ctx.scale(sc, sc);
                    const pulse = 24 + Math.sin(frameTick * 0.14) * 5;
                    ctx.strokeStyle = '#A855F7';
                    ctx.fillStyle = 'rgba(168, 85, 247, 0.22)';
                    ctx.lineWidth = 2.2;
                    ctx.beginPath();
                    ctx.ellipse(0, 14, pulse, pulse * 0.4, 0, 0, Math.PI * 2);
                    ctx.fill();
                    ctx.stroke();
                    ctx.fillStyle = '#FFB300';
                    ctx.font = 'bold 10px "JetBrains Mono", monospace';
                    ctx.fillText(`🚩 FLAG SITE (${flagProj.dist.toFixed(1)}m) [PRESS E]`, -68, -14);
                    ctx.restore();
                }
            }

            // ===============================================================
            // 5. FIRST-PERSON ASTRONAUT GLOVED HANDS & WRIST CUFF TELEMETRY
            // ===============================================================
            const reachOffset = state.evaArmReachTimer > 0 ? Math.sin((state.evaArmReachTimer / 45) * Math.PI) * 55 : 0;

            // LEFT GLOVED FOREARM + GLOWING NASA CUFF RADAR DISPLAY
            ctx.save();
            ctx.translate(w * 0.16 + walkBobX * 0.8, h * 0.83 + Math.abs(walkBobY) * 0.7);
            ctx.rotate(-0.18);

            // Pressurized Titanium-Blue Spacesuit Sleeve
            const sleeveGrad = ctx.createLinearGradient(-70, -25, 60, 55);
            sleeveGrad.addColorStop(0, '#1E293B');
            sleeveGrad.addColorStop(0.5, '#334155');
            sleeveGrad.addColorStop(1, '#0F172A');
            ctx.fillStyle = sleeveGrad;
            ctx.strokeStyle = '#00F0FF';
            ctx.lineWidth = 2;
            ctx.beginPath();
            ctx.roundRect(-85, -18, 155, 72, 18);
            ctx.fill();
            ctx.stroke();

            // Glowing Forearm Cuff Bio-Radar Display
            ctx.fillStyle = '#050816';
            ctx.strokeStyle = '#A855F7';
            ctx.lineWidth = 1.8;
            ctx.beginPath();
            ctx.roundRect(-36, -12, 86, 52, 8);
            ctx.fill();
            ctx.stroke();

            ctx.fillStyle = '#00F0FF';
            ctx.font = 'bold 8px "JetBrains Mono", monospace';
            ctx.fillText('xEMU CUFF NAV', -28, 1);
            ctx.fillStyle = '#00E676';
            ctx.fillText(`g: ${p.gravity} m/s²`, -28, 13);
            ctx.fillStyle = '#E2E8F0';
            ctx.fillText(state.flagPlanted ? 'EVA: COMPLETE' : state.crystalCollected ? 'OBJ: PLANT FLAG' : 'OBJ: CRYSTAL', -28, 25);

            // Left Gloved Hand Fingers
            ctx.fillStyle = '#1E293B';
            ctx.strokeStyle = '#38BDF8';
            ctx.beginPath();
            ctx.roundRect(62, -10, 34, 46, 12);
            ctx.fill();
            ctx.stroke();
            ctx.restore();

            // RIGHT GLOVED HAND HOLDING LASER TOOL / CRYSTAL / FLAGPOLE
            ctx.save();
            ctx.translate(w * 0.82 - walkBobX * 0.8 - reachOffset * 0.5, h * 0.83 + Math.abs(walkBobY) * 0.7 - reachOffset);
            ctx.rotate(0.18);

            ctx.fillStyle = sleeveGrad;
            ctx.strokeStyle = '#00F0FF';
            ctx.lineWidth = 2;
            ctx.beginPath();
            ctx.roundRect(-70, -18, 155, 72, 18);
            ctx.fill();
            ctx.stroke();

            // Right Gloved Grip
            ctx.fillStyle = '#1E293B';
            ctx.strokeStyle = '#38BDF8';
            ctx.beginPath();
            ctx.roundRect(-92, -12, 36, 48, 12);
            ctx.fill();
            ctx.stroke();

            // Held Item in 1st-Person Right Hand
            if (state.crystalCollected && !state.flagPlanted) {
                // Holding the Flagpole ready to plant!
                ctx.strokeStyle = '#00F0FF';
                ctx.lineWidth = 4;
                ctx.beginPath();
                ctx.moveTo(-75, 25);
                ctx.lineTo(-75, -95);
                ctx.stroke();
                ctx.fillStyle = '#A855F7';
                ctx.fillRect(-75, -95, 48, 22);
            } else {
                // Holding Handheld Laser Spectrometer / Sample Collector
                ctx.fillStyle = '#090D24';
                ctx.strokeStyle = '#00E676';
                ctx.lineWidth = 2;
                ctx.fillRect(-105, -26, 42, 18);
                ctx.strokeRect(-105, -26, 42, 18);
            }
            ctx.restore();

            // ===============================================================
            // 6. CURVED GOLD/CYAN SPACESUIT HELMET VISOR FRAME & INTERNAL HUD
            // ===============================================================
            // Visor edge vignette
            const visorGrad = ctx.createRadialGradient(w * 0.5, h * 0.5, w * 0.32, w * 0.5, h * 0.5, w * 0.56);
            visorGrad.addColorStop(0, 'rgba(0, 0, 0, 0)');
            visorGrad.addColorStop(0.82, 'rgba(14, 21, 56, 0.35)');
            visorGrad.addColorStop(1, 'rgba(4, 6, 17, 0.92)');
            ctx.fillStyle = visorGrad;
            ctx.fillRect(0, 0, w, h);

            // Curved Polycarbonate Helmet Rim
            ctx.strokeStyle = 'rgba(251, 191, 36, 0.55)';
            ctx.lineWidth = 10;
            ctx.beginPath();
            ctx.roundRect(6, 6, w - 12, h - 12, 42);
            ctx.stroke();

            ctx.strokeStyle = 'rgba(0, 240, 255, 0.65)';
            ctx.lineWidth = 2;
            ctx.beginPath();
            ctx.roundRect(14, 14, w - 28, h - 28, 36);
            ctx.stroke();

            // Internal Helmet Visor Biometrics & Compass HUD
            ctx.fillStyle = 'rgba(0, 240, 255, 0.85)';
            ctx.font = 'bold 10px "JetBrains Mono", monospace';
            ctx.fillText('xEMU HELMET VISOR HUD // 1ST-PERSON POV', 28, 54);
            ctx.fillStyle = '#00E676';
            ctx.fillText('♥ ECG: 78 BPM  •  O₂: 4.30 PSI  •  CO₂: 0.8 mmHg', 28, 70);

            // Center Helmet Visor Crosshair & Parabolic Jump Equation Readout
            const cx = w * 0.5, cy = h * 0.48;
            ctx.strokeStyle = 'rgba(0, 240, 255, 0.65)';
            ctx.lineWidth = 1.4;
            ctx.beginPath();
            ctx.arc(cx, cy, 16, 0, Math.PI * 2);
            ctx.moveTo(cx - 24, cy); ctx.lineTo(cx - 10, cy);
            ctx.moveTo(cx + 10, cy); ctx.lineTo(cx + 24, cy);
            ctx.stroke();

            if (state.evaJumpZ > 0.02) {
                ctx.fillStyle = '#FFB300';
                ctx.font = 'bold 11px "JetBrains Mono", monospace';
                ctx.fillText(`LOW-G JUMP: z(t) = ${state.evaJumpZ.toFixed(2)}m (g = ${p.gravity} m/s²)`, cx - 120, cy - 26);
            }
        }

        function drawCockpitFrame() {
            const canvas = document.getElementById('cockpit-canvas');
            if (!canvas || canvas.width === 0) return;
            const ctx = canvas.getContext('2d');
            if (!ctx) return;

            const w = canvas.width, h = canvas.height;
            const sc = state.cockpitScene;
            const p = PLANETS[state.planetId] || PLANETS.moon;
            const firing = state.staticFireTimer > 0 || state.throttle > 55;

            ctx.save();
            ctx.clearRect(0, 0, w, h);

            // ===============================================================
            // SCENE 6: ASTRONAUT SPACESUIT EVA (1ST-PERSON VISOR OR 3RD-PERSON REALISTIC ASTRONAUT)
            // ===============================================================
            if (sc === 'eva') {
                if (state.camPerspective === '3rd') {
                    // 3rd-Person Exterior View of the Photorealistic NASA xEMU Astronaut walking on the planet!
                    const jumpOffsetPx = state.evaJumpZ * 26;
                    const yawShiftPx = -state.evaYaw * 180;
                    const horY = drawAlienSurfaceBackdrop(ctx, w, h, p, 0, yawShiftPx);

                    // Parked Lander & 6-Wheeled Rover in background
                    drawLandedSpacecraft(ctx, w * 0.16, horY + 46, 0.85, 0, 100, 100);
                    drawSixWheeledRover(ctx, w * 0.82, horY + 62, 0, false, 0.85, false);

                    // Glowing Crystal Specimen & Flag on surface
                    const cryX = w * 0.34, cryY = h * 0.67;
                    if (!state.crystalCollected) {
                        ctx.fillStyle = '#00F0FF';
                        ctx.strokeStyle = '#A855F7';
                        ctx.lineWidth = 2;
                        ctx.beginPath();
                        ctx.moveTo(cryX, cryY - 22);
                        ctx.lineTo(cryX + 12, cryY - 2);
                        ctx.lineTo(cryX, cryY + 10);
                        ctx.lineTo(cryX - 12, cryY - 2);
                        ctx.closePath();
                        ctx.fill();
                        ctx.stroke();
                        ctx.font = 'bold 10px "JetBrains Mono", monospace';
                        ctx.fillText('💎 SPECIMEN [PRESS E]', cryX - 55, cryY - 28);
                    }
                    const flgX = w * 0.66, flgY = h * 0.67;
                    if (state.flagPlanted) {
                        drawPioneersFlag(ctx, flgX, flgY, 1.15);
                    } else {
                        ctx.strokeStyle = '#A855F7';
                        ctx.beginPath();
                        ctx.ellipse(flgX, flgY + 8, 24, 10, 0, 0, Math.PI * 2);
                        ctx.stroke();
                        ctx.fillStyle = '#FFB300';
                        ctx.font = 'bold 10px "JetBrains Mono", monospace';
                        ctx.fillText('🚩 FLAG SITE [PRESS E]', flgX - 55, flgY - 12);
                    }

                    // Draw Photorealistic 3D-Shaded NASA xEMU Astronaut in center-foreground
                    const astroX = w * 0.50 + Math.max(-w * 0.28, Math.min(w * 0.28, state.evaCamX * 3.2));
                    const astroY = h * 0.66 - jumpOffsetPx;
                    drawRealisticAstronaut3rdPerson(ctx, astroX, astroY, 1.35, state.evaBobPhase, state.evaArmReachTimer, state.flagPlanted);
                } else {
                    drawFirstPersonAstronautEVA(ctx, w, h, p);
                }
                drawInteractiveCockpitConsoleAndYoke(ctx, w, h, sc, p);
                ctx.restore();
                return;
            }

            // ===============================================================
            // SCENE 5: 6-WHEELED SURFACE ROVER TRAVERSE & RADAR MINI-MAP
            // ===============================================================
            if (sc === 'rover') {
                drawAlienSurfaceBackdrop(ctx, w, h, p, 0, 0);

                const centerOrigX = w * 0.46;
                const centerOrigY = h * 0.63;

                // Landed Spacecraft with its Steel Cargo Track fully deployed behind the Rover!
                drawLandedSpacecraft(ctx, w * 0.14, h * 0.54, 0.92, 0, 100, 100);

                state.waypoints.forEach(wp => {
                    const sx = centerOrigX + wp.x;
                    const sy = centerOrigY + wp.y;
                    const pulse = 18 + Math.sin(frameTick * 0.12) * 4;

                    ctx.strokeStyle = wp.collected ? '#00E676' : '#00F0FF';
                    ctx.fillStyle = wp.collected ? 'rgba(0, 230, 118, 0.22)' : 'rgba(0, 240, 255, 0.18)';
                    ctx.lineWidth = 2.2;
                    ctx.beginPath();
                    ctx.ellipse(sx, sy + 18, pulse, pulse * 0.42, 0, 0, Math.PI * 2);
                    ctx.fill();
                    ctx.stroke();

                    ctx.fillStyle = wp.collected ? '#00E676' : '#FFB300';
                    ctx.font = 'bold 10px "JetBrains Mono", monospace';
                    ctx.fillText(wp.collected ? `✓ ${wp.label}` : `📍 ${wp.label} [E]`, sx - 45, sy - 6);
                });

                const rScreenX = centerOrigX + state.roverX;
                const rScreenY = centerOrigY + state.roverY;
                drawSixWheeledRover(ctx, rScreenX, rScreenY, state.roverWheelRot, state.laserPulseTimer > 0, 1.05, true);

                // Top-Right Surface Radar Mini-Map
                const mapR = 44, mapX = w - 62, mapY = 118;
                ctx.fillStyle = 'rgba(4, 6, 17, 0.9)';
                ctx.strokeStyle = '#00F0FF';
                ctx.lineWidth = 1.8;
                ctx.beginPath();
                ctx.arc(mapX, mapY, mapR, 0, Math.PI * 2);
                ctx.fill();
                ctx.stroke();

                const sweepAng = (frameTick * 0.05) % (Math.PI * 2);
                ctx.strokeStyle = 'rgba(168, 85, 247, 0.6)';
                ctx.beginPath();
                ctx.moveTo(mapX, mapY);
                ctx.lineTo(mapX + Math.cos(sweepAng) * mapR, mapY + Math.sin(sweepAng) * mapR);
                ctx.stroke();

                state.waypoints.forEach(wp => {
                    ctx.fillStyle = wp.collected ? '#00E676' : '#FFB300';
                    ctx.beginPath();
                    ctx.arc(mapX + wp.x * 0.18, mapY + wp.y * 0.25, 3.5, 0, Math.PI * 2);
                    ctx.fill();
                });

                ctx.fillStyle = '#00F0FF';
                ctx.beginPath();
                ctx.arc(mapX + state.roverX * 0.18, mapY + state.roverY * 0.25, 4, 0, Math.PI * 2);
                ctx.fill();

                drawInteractiveCockpitConsoleAndYoke(ctx, w, h, sc, p);
                ctx.restore();
                return;
            }

            // ===============================================================
            // SCENE 4: PROPER 3RD-PERSON PLANETARY LANDING & TRACK UNLOADING
            // ===============================================================
            if (sc === 'landing') {
                drawAlienSurfaceBackdrop(ctx, w, h, p, 0, 0);

                const padX = w * 0.48;
                const padY = h * 0.64;

                // Draw Target Landing Zone Alpha Pad on the planetary terrain
                const aligned = Math.hypot(state.landerX, state.landerY) < 0.26;
                ctx.save();
                ctx.translate(padX, padY);
                ctx.strokeStyle = aligned ? '#00E676' : '#00F0FF';
                ctx.fillStyle = aligned ? 'rgba(0, 230, 118, 0.22)' : 'rgba(0, 240, 255, 0.15)';
                ctx.lineWidth = 2.8;
                ctx.beginPath();
                ctx.ellipse(0, 0, 82, 22, 0, 0, Math.PI * 2);
                ctx.fill();
                ctx.stroke();

                // Concentric target rings & pad beacons
                ctx.beginPath();
                ctx.ellipse(0, 0, 46, 12, 0, 0, Math.PI * 2);
                ctx.stroke();

                ctx.fillStyle = aligned ? '#00E676' : '#00F0FF';
                ctx.font = 'bold 10px "JetBrains Mono", monospace';
                ctx.fillText(
                    state.landedOnPad
                        ? `✓ TOUCHDOWN COMPLETE — TRACK RAMP: ${Math.round(state.rampDeployPct)}% • ROVER ROLLOUT: ${Math.round(state.roverUnloadPct)}%`
                        : `LANDING ZONE ALPHA (g=${p.gravity} m/s²) • ALT: ${Math.round(state.landerAltM)}m`,
                    -150,
                    34
                );
                ctx.restore();

                // Calculate 3rd-Person Lander position descending from sky onto the Landing Pad
                const altRatio = state.landerAltM / 1400; // 1.0 at top, 0.0 on pad
                const shipScreenX = padX + state.landerX * (w * 0.34);
                const shipScreenY = (padY - 48) - altRatio * (h * 0.40) + state.landerY * 35;

                // Surface Regolith Dust Plume kicked up when Lander is below 450m
                if (!state.landedOnPad && state.landerAltM < 450 && state.throttle > 20) {
                    const dustWidth = (1 - state.landerAltM / 450) * 135;
                    const dustGrad = ctx.createRadialGradient(shipScreenX, padY, 8, shipScreenX, padY, dustWidth);
                    dustGrad.addColorStop(0, 'rgba(226, 232, 240, 0.65)');
                    dustGrad.addColorStop(0.5, `${p.horizonGlow}55`);
                    dustGrad.addColorStop(1, 'rgba(0,0,0,0)');
                    ctx.fillStyle = dustGrad;
                    ctx.beginPath();
                    ctx.ellipse(shipScreenX, padY, dustWidth, dustWidth * 0.25, 0, 0, Math.PI * 2);
                    ctx.fill();
                }

                // Draw the Photorealistic 3rd-Person NASA Lander + Unfolding Steel Track + Rolling Rover!
                drawLandedSpacecraft(
                    ctx,
                    shipScreenX,
                    shipScreenY,
                    1.08,
                    state.landedOnPad ? 0 : state.throttle,
                    state.rampDeployPct,
                    state.roverUnloadPct
                );

                // Once Rover finishes rolling down the track, show it parked right at the foot of the ramp
                if (state.roverUnloadPct >= 100) {
                    drawSixWheeledRover(ctx, shipScreenX + 145, padY + 6, state.roverWheelRot, false, 0.72, true);
                }

                drawInteractiveCockpitConsoleAndYoke(ctx, w, h, sc, p);
                ctx.restore();
                return;
            }

            // ===============================================================
            // SCENES 1–3: COCKPIT PAD, LIFTOFF & LETHAL 3D ASTEROID FIELD
            // ===============================================================
            const hitShake = state.hullHitTimer > 0 ? (Math.random() - 0.5) * 18 : 0;
            const shakeX = (firing ? (Math.random() - 0.5) * (state.throttle * 0.04) : 0) + hitShake;
            const shakeY = (firing ? (Math.random() - 0.5) * (state.throttle * 0.04) : 0) + hitShake;
            const panX = -state.yaw * 3.2;
            const panY = state.pitch * 3.0;

            ctx.translate(shakeX, shakeY);

            if (sc !== 'landing') {
                ctx.save();
                ctx.translate(w * 0.5, h * 0.44);
                ctx.rotate((state.roll * Math.PI) / 180 * 0.35);
                ctx.translate(-w * 0.5, -h * 0.44);

                if (sc === 'pad' || sc === 'ascent') {
                    const climbDrop = sc === 'ascent' ? Math.min(180, (frameTick % 260) * 0.9) : 0;
                    const mBase = h * 0.63 + climbDrop + panY;

                    if (sc === 'ascent' && climbDrop > 85) {
                        // High-altitude orbital transition into real NASA ISS / DSCOVR EPIC Earth Horizon
                        drawHdrPhotorealisticBackdrop(ctx, w, h, 'earth_orbit', panX * 0.6, panY * 0.6, 1.22);
                    } else {
                        // Real 4K HDR NASA KSC LC-39A Launchpad & Earth Horizon Photography
                        drawHdrPhotorealisticBackdrop(ctx, w, h, 'earth_pad', panX * 0.7, panY * 0.7 + climbDrop * 0.6, 1.24);

                        // 3D LC-39A Fixed Service Structure (FSS) Launch Umbilical Tower & Cryogenic LOX Venting
                        const towerX = w * 0.78 + panX * 0.85;
                        const towerBotY = mBase + 75;
                        const towerTopY = mBase - 185;
                        ctx.save();
                        ctx.strokeStyle = '#64748B';
                        ctx.lineWidth = 3;
                        ctx.strokeRect(towerX, towerTopY, 38, towerBotY - towerTopY);

                        // Steel cross-bracing trusses on LC-39A tower
                        ctx.strokeStyle = '#475569';
                        ctx.lineWidth = 1.5;
                        ctx.beginPath();
                        for (let ty = towerTopY; ty < towerBotY - 20; ty += 26) {
                            ctx.moveTo(towerX, ty);
                            ctx.lineTo(towerX + 38, ty + 26);
                            ctx.moveTo(towerX + 38, ty);
                            ctx.lineTo(towerX, ty + 26);
                        }
                        ctx.stroke();

                        // Retractable Crew/Umbilical Swing Arm & Lightning Mast
                        ctx.strokeStyle = '#94A3B8';
                        ctx.lineWidth = 4;
                        ctx.beginPath();
                        ctx.moveTo(towerX, towerTopY + 45);
                        ctx.lineTo(towerX - (sc === 'ascent' ? 35 : 85), towerTopY + (sc === 'ascent' ? 20 : 45));
                        ctx.moveTo(towerX + 19, towerTopY);
                        ctx.lineTo(towerX + 19, towerTopY - 42);
                        ctx.stroke();

                        // Flashing Red Aviation Obstruction Beacon on Lightning Mast
                        if (Math.floor(frameTick / 20) % 2 === 0) {
                            ctx.fillStyle = '#FF2E54';
                            ctx.beginPath();
                            ctx.arc(towerX + 19, towerTopY - 44, 4, 0, Math.PI * 2);
                            ctx.fill();
                        }

                        // Billowing Cryogenic Liquid Oxygen (LOX) Vapor Clouds
                        for (let v = 0; v < 4; v++) {
                            const vx = towerX - 45 - ((frameTick * 0.6 + v * 28) % 90);
                            const vy = towerTopY + 55 + v * 28 - Math.sin(frameTick * 0.05 + v) * 8;
                            const vr = 18 + (v * 6);
                            const vGrad = ctx.createRadialGradient(vx, vy, 2, vx, vy, vr);
                            vGrad.addColorStop(0, 'rgba(226, 232, 240, 0.42)');
                            vGrad.addColorStop(1, 'rgba(226, 232, 240, 0)');
                            ctx.fillStyle = vGrad;
                            ctx.beginPath();
                            ctx.arc(vx, vy, vr, 0, Math.PI * 2);
                            ctx.fill();
                        }
                        ctx.restore();
                    }

                    // Photorealistic Rayleigh Blue Earth Atmospheric Limb Scattering
                    const limbGrad = ctx.createLinearGradient(0, mBase - 90, 0, mBase + 60);
                    limbGrad.addColorStop(0, 'rgba(56, 189, 248, 0)');
                    limbGrad.addColorStop(0.5, 'rgba(56, 189, 248, 0.22)');
                    limbGrad.addColorStop(1, 'rgba(4, 6, 17, 0.35)');
                    ctx.fillStyle = limbGrad;
                    ctx.fillRect(-60, mBase - 90, w + 120, 150);

                    if (firing) {
                        const fire = ctx.createRadialGradient(w * 0.5, mBase + 30, 10, w * 0.5, mBase + 10, 240);
                        fire.addColorStop(0, 'rgba(255, 244, 214, 0.95)');
                        fire.addColorStop(0.4, 'rgba(249, 115, 22, 0.68)');
                        fire.addColorStop(0.75, 'rgba(234, 88, 12, 0.25)');
                        fire.addColorStop(1, 'rgba(168, 85, 247, 0)');
                        ctx.fillStyle = fire;
                        ctx.fillRect(-40, mBase - 140, w + 80, 250);
                    }
                } else {
                    // Real 4K HDR NASA JWST Carina Nebula / Deep Space Photography Backdrop
                    drawHdrPhotorealisticBackdrop(ctx, w, h, 'jwst_space', panX * 0.55 + Math.sin(frameTick * 0.002) * 18, panY * 0.55, 1.24);

                    // Target Planet Globe (Real NASA LRO / Viking / Galileo / Cassini Orbital Imagery)
                    const rad = sc === 'orbit' ? 124 : 58;
                    const px = w * (sc === 'orbit' ? 0.5 : 0.68) + panX * 0.7;
                    const py = h * (sc === 'orbit' ? 0.45 : 0.35) + panY * 0.7;
                    const orbitImg = NASA_MEDIA.images[`${p.id}_orbit`];

                    ctx.save();
                    // Atmospheric Corona Limb Glow
                    const corona = ctx.createRadialGradient(px, py, rad * 0.85, px, py, rad * 1.38);
                    corona.addColorStop(0, `${p.horizonGlow}66`);
                    corona.addColorStop(1, 'rgba(0,0,0,0)');
                    ctx.fillStyle = corona;
                    ctx.beginPath();
                    ctx.arc(px, py, rad * 1.38, 0, Math.PI * 2);
                    ctx.fill();

                    // Spherical Clipped Planetary Globe
                    ctx.beginPath();
                    ctx.arc(px, py, rad, 0, Math.PI * 2);
                    ctx.clip();

                    if (orbitImg && orbitImg.complete && orbitImg.naturalWidth > 0) {
                        ctx.filter = 'contrast(1.16) saturate(1.18)';
                        ctx.drawImage(orbitImg, px - rad * 1.15, py - rad * 1.15, rad * 2.3, rad * 2.3);
                        ctx.filter = 'none';
                    } else {
                        const baseSphere = ctx.createRadialGradient(px - rad * 0.35, py - rad * 0.35, rad * 0.1, px, py, rad);
                        baseSphere.addColorStop(0, p.horizonGlow);
                        baseSphere.addColorStop(0.55, p.surfaceColor);
                        baseSphere.addColorStop(1, '#040611');
                        ctx.fillStyle = baseSphere;
                        ctx.fillRect(px - rad, py - rad, rad * 2, rad * 2);
                    }

                    // 3D Solar Terminator Shadow (Day/Night Crescent Shading)
                    const shadowGrad = ctx.createRadialGradient(px - rad * 0.42, py - rad * 0.32, rad * 0.2, px + rad * 0.15, py + rad * 0.15, rad * 1.12);
                    shadowGrad.addColorStop(0, 'rgba(255, 255, 255, 0.08)');
                    shadowGrad.addColorStop(0.55, 'rgba(0, 0, 0, 0.15)');
                    shadowGrad.addColorStop(1, 'rgba(2, 4, 12, 0.88)');
                    ctx.fillStyle = shadowGrad;
                    ctx.fillRect(px - rad, py - rad, rad * 2, rad * 2);
                    ctx.restore();

                    // =======================================================
                    // LETHAL 3D TUMBLING ASTEROIDS & COLLISION VECTORS
                    // =======================================================
                    const shipNormX = state.yaw / 45;
                    const shipNormY = -state.pitch / 45;

                    state.asteroids.forEach(ast => {
                        const relX = ast.x - shipNormX;
                        const relY = ast.y - shipNormY;
                        const invZ = 1 / Math.max(0.08, ast.z);
                        const ax = w * 0.5 + relX * (w * 0.45) * invZ;
                        const ay = h * 0.44 + relY * (h * 0.45) * invZ;
                        const ar = Math.min(95, 13 * invZ);

                        const distFromNose = Math.hypot(relX, relY);
                        const isDanger = ast.z < 0.48 && distFromNose < 0.22;

                        // Draw multi-faceted PBR shaded 3D Asteroid Polygon
                        ctx.save();
                        ctx.translate(ax, ay);
                        ctx.rotate(ast.rot);

                        const rockGrad = ctx.createRadialGradient(-ar * 0.3, -ar * 0.3, ar * 0.1, 0, 0, ar);
                        rockGrad.addColorStop(0, '#94A3B8');
                        rockGrad.addColorStop(0.55, '#475569');
                        rockGrad.addColorStop(1, '#0F172A');
                        ctx.fillStyle = rockGrad;
                        ctx.strokeStyle = isDanger ? '#FF2E54' : '#64748B';
                        ctx.lineWidth = isDanger ? 2.5 : 1.5;

                        ctx.beginPath();
                        ast.vertices.forEach((v, idx) => {
                            const vx = Math.cos(v.ang) * (ar * v.rad);
                            const vy = Math.sin(v.ang) * (ar * v.rad);
                            if (idx === 0) ctx.moveTo(vx, vy);
                            else ctx.lineTo(vx, vy);
                        });
                        ctx.closePath();
                        ctx.fill();
                        ctx.stroke();
                        ctx.restore();

                        // Target Lock Brackets & Distance Telemetry
                        const distKm = (ast.z * 18.5).toFixed(1);
                        ctx.strokeStyle = isDanger ? '#FF2E54' : 'rgba(0, 240, 255, 0.65)';
                        ctx.lineWidth = isDanger ? 2 : 1.2;
                        ctx.strokeRect(ax - ar - 6, ay - ar - 6, (ar + 6) * 2, (ar + 6) * 2);

                        ctx.fillStyle = isDanger ? '#FF2E54' : '#00F0FF';
                        ctx.font = 'bold 9px "JetBrains Mono", monospace';
                        ctx.fillText(
                            isDanger ? `⚠️ IMPACT RISK ${distKm}km (${ast.massTons}t)` : `AST-${ast.id} • ${distKm}km`,
                            ax - ar - 4,
                            ay - ar - 10
                        );
                    });

                    // Pulse Defense Laser Beams when [E] is pressed
                    if (state.laserPulseTimer > 0 && state.asteroids.length > 0) {
                        const target = state.asteroids[0];
                        const tx = w * 0.5 + ((target.x - shipNormX) * w * 0.45) / Math.max(0.1, target.z);
                        const ty = h * 0.44 + ((target.y - shipNormY) * h * 0.45) / Math.max(0.1, target.z);

                        ctx.strokeStyle = '#00E676';
                        ctx.lineWidth = 3.5;
                        ctx.beginPath();
                        ctx.moveTo(w * 0.2, h * 0.75); ctx.lineTo(tx, ty);
                        ctx.moveTo(w * 0.8, h * 0.75); ctx.lineTo(tx, ty);
                        ctx.stroke();
                    }

                    // =======================================================
                    // STAGE 3: ORBITAL CAPTURE VELOCITY GATE HUD GAUGE
                    // =======================================================
                    if (sc === 'orbit') {
                        const gx = 24, gy = h * 0.22, gw = 215, gh = 108;
                        ctx.fillStyle = 'rgba(4, 6, 17, 0.90)';
                        ctx.strokeStyle = state.orbitVelKmS <= state.orbitTargetMaxKmS ? '#00E676' : '#FF2E54';
                        ctx.lineWidth = 2;
                        ctx.beginPath();
                        ctx.roundRect(gx, gy, gw, gh, 8);
                        ctx.fill();
                        ctx.stroke();

                        ctx.fillStyle = '#00F0FF';
                        ctx.font = 'bold 10px "Orbitron", sans-serif';
                        ctx.fillText('ORBIT CAPTURE GATE', gx + 10, gy + 18);

                        ctx.fillStyle = '#E2E8F0';
                        ctx.font = 'bold 12px "JetBrains Mono", monospace';
                        ctx.fillText(`SPEED: ${state.orbitVelKmS.toFixed(2)} km/s`, gx + 10, gy + 36);

                        ctx.fillStyle = '#00E676';
                        ctx.font = '10px "JetBrains Mono", monospace';
                        ctx.fillText('TARGET: 2.20 - 4.20 km/s', gx + 10, gy + 52);

                        ctx.fillStyle = state.periapsisTimerSec < 12 ? '#FF2E54' : '#FFB300';
                        ctx.fillText(`PERIAPSIS T-: ${state.periapsisTimerSec.toFixed(1)}s`, gx + 10, gy + 68);

                        // Speed bar with green capture zone
                        const barX = gx + 10, barY = gy + 78, barW = gw - 20, barH = 12;
                        ctx.fillStyle = '#0F172A';
                        ctx.fillRect(barX, barY, barW, barH);
                        // Green capture zone (2.2 to 4.2 out of 12 km/s)
                        const zStart = (2.2 / 12.5) * barW;
                        const zWidth = ((4.2 - 2.2) / 12.5) * barW;
                        ctx.fillStyle = 'rgba(0, 230, 118, 0.38)';
                        ctx.fillRect(barX + zStart, barY, zWidth, barH);
                        // Current velocity marker
                        const vPos = Math.min(barW - 3, Math.max(0, (state.orbitVelKmS / 12.5) * barW));
                        ctx.fillStyle = state.orbitVelKmS <= 4.2 ? '#00E676' : '#FF2E54';
                        ctx.fillRect(barX + vPos - 2, barY - 2, 5, barH + 4);
                    }
                }
                ctx.restore();

                // Optional 3rd-Person Exterior Rocket/Spacecraft View during Pad/Ascent/Cruise/Orbit when Camera is set to '3rd'
                if (state.camPerspective === '3rd') {
                    drawLandedSpacecraft(ctx, w * 0.50 + state.yaw * 2.2, h * 0.52 - state.pitch * 1.5, 0.95, state.throttle, 0, 0);
                }

                // Target Trajectory Ring
                const cx = w * 0.5, cy = h * 0.42;
                const ringScreenX = cx + state.ringX * (w * 0.26);
                const ringScreenY = cy + state.ringY * (h * 0.22);
                const aligned = Math.hypot(ringScreenX - cx, ringScreenY - cy) < 38;
                const ringColor = aligned ? '#00E676' : '#00F0FF';

                ctx.strokeStyle = ringColor;
                ctx.lineWidth = aligned ? 2.8 : 2;
                ctx.beginPath();
                ctx.arc(ringScreenX, ringScreenY, 36, 0, Math.PI * 2);
                ctx.stroke();

                ctx.strokeStyle = 'rgba(0, 240, 255, 0.8)';
                ctx.lineWidth = 1.6;
                ctx.beginPath();
                ctx.arc(cx, cy, 14, 0, Math.PI * 2);
                ctx.moveTo(cx - 26, cy); ctx.lineTo(cx - 12, cy);
                ctx.moveTo(cx + 12, cy); ctx.lineTo(cx + 26, cy);
                ctx.stroke();
            }

            // ===============================================================
            // PERSISTENT CANOPY WINDSHIELD CRACKS FROM ASTEROID HITS
            // ===============================================================
            state.canopyCracks.forEach(cr => {
                const cx = cr.x * w, cy = cr.y * h;
                ctx.strokeStyle = 'rgba(255, 46, 84, 0.85)';
                ctx.lineWidth = 2;
                cr.rays.forEach(ang => {
                    ctx.beginPath();
                    ctx.moveTo(cx, cy);
                    ctx.lineTo(cx + Math.cos(ang) * 45, cy + Math.sin(ang) * 45);
                    ctx.stroke();
                });
            });

            // ===============================================================
            // INTERACTIVE COCKPIT CONSOLE, CLICKABLE BUTTONS, THROTTLE LEVER
            // & DRAGGABLE 3D FLIGHT YOKE (DRIVING HANDLE)
            // ===============================================================
            drawInteractiveCockpitConsoleAndYoke(ctx, w, h, sc, p);

            ctx.restore();
        }

        function drawInteractiveCockpitConsoleAndYoke(ctx, w, h, sc, p) {
            // 1. Angled Metallic Canopy Side Struts
            ctx.strokeStyle = '#0F172A';
            ctx.lineWidth = 16;
            ctx.beginPath();
            ctx.moveTo(0, 0); ctx.lineTo(w * 0.08, h * 0.74);
            ctx.moveTo(w, 0); ctx.lineTo(w * 0.92, h * 0.74);
            ctx.stroke();

            ctx.strokeStyle = '#38BDF8';
            ctx.lineWidth = 1.8;
            ctx.beginPath();
            ctx.moveTo(7, 0); ctx.lineTo(w * 0.08 + 6, h * 0.74);
            ctx.moveTo(w - 7, 0); ctx.lineTo(w * 0.92 - 6, h * 0.74);
            ctx.stroke();

            // 2. Lower 3D Glare-Shield & Instrument Dashboard Deck
            const dashGrad = ctx.createLinearGradient(0, h * 0.73, 0, h);
            dashGrad.addColorStop(0, '#111836');
            dashGrad.addColorStop(0.3, '#090D24');
            dashGrad.addColorStop(1, '#040611');
            ctx.fillStyle = dashGrad;
            ctx.beginPath();
            ctx.moveTo(0, h);
            ctx.lineTo(0, h * 0.78);
            ctx.lineTo(w * 0.12, h * 0.72);
            ctx.lineTo(w * 0.88, h * 0.72);
            ctx.lineTo(w, h * 0.78);
            ctx.lineTo(w, h);
            ctx.closePath();
            ctx.fill();

            ctx.strokeStyle = state.autopilot ? '#00E676' : '#00F0FF';
            ctx.lineWidth = 2.2;
            ctx.beginPath();
            ctx.moveTo(0, h * 0.78);
            ctx.lineTo(w * 0.12, h * 0.72);
            ctx.lineTo(w * 0.88, h * 0.72);
            ctx.lineTo(w, h * 0.78);
            ctx.stroke();

            // 3. LEFT CONSOLE: Interactive Draggable Throttle & Retro-Brake Quadrant Lever
            const levX = w * 0.10;
            const levTop = h * 0.76;
            const levBot = h * 0.94;
            ctx.fillStyle = '#040611';
            ctx.strokeStyle = '#475569';
            ctx.lineWidth = 2;
            ctx.beginPath();
            ctx.roundRect(levX - 18, levTop - 6, 36, (levBot - levTop) + 12, 6);
            ctx.fill();
            ctx.stroke();

            // Throttle Track Slot
            ctx.fillStyle = '#1E293B';
            ctx.fillRect(levX - 4, levTop, 8, levBot - levTop);

            // Draggable Physical T-Handle Position
            const handleY = levBot - (state.throttle / 100) * (levBot - levTop);
            const hGrad = ctx.createLinearGradient(levX - 24, handleY - 8, levX + 24, handleY + 8);
            hGrad.addColorStop(0, '#F59E0B');
            hGrad.addColorStop(0.5, '#FDE047');
            hGrad.addColorStop(1, '#B45309');
            ctx.fillStyle = hGrad;
            ctx.strokeStyle = '#FFFFFF';
            ctx.lineWidth = 1.5;
            ctx.beginPath();
            ctx.roundRect(levX - 24, handleY - 7, 48, 14, 4);
            ctx.fill();
            ctx.stroke();

            ctx.fillStyle = '#040611';
            ctx.font = 'bold 8px "JetBrains Mono", monospace';
            ctx.fillText(`THR ${Math.round(state.throttle)}%`, levX - 18, handleY + 3);

            // 4. CLICKABLE PHYSICAL COCKPIT DASHBOARD SWITCHES (Left & Right MFD Banks)
            const btnW = Math.min(118, w * 0.17);
            const btnH = 26;
            const leftBankX = w * 0.19;
            const rightBankX = w * 0.81 - btnW;
            const row1Y = h * 0.76;
            const row2Y = h * 0.84;

            state.cockpitButtons = [
                {
                    id: 'retro',
                    x: leftBankX,
                    y: row1Y,
                    w: btnW,
                    h: btnH,
                    label: sc === 'orbit' ? '🛑 RETRO-BRAKE' : '🔥 THRUST 100%',
                    color: '#FFB300',
                    action: () => {
                        playSfx('roar');
                        if (sc === 'orbit') {
                            state.orbitVelKmS = Math.max(2.60, state.orbitVelKmS - 1.35);
                            state.retroBrakingActive = true;
                            showCockpitToast(`🛑 COCKPIT RETRO-BRAKE FIRED! Speed: ${state.orbitVelKmS.toFixed(2)} km/s`, 1800);
                        } else {
                            state.throttle = 100;
                            if (sc === 'pad' && !state.inFlight) triggerStaticFireTest();
                        }
                    }
                },
                {
                    id: 'laser',
                    x: leftBankX,
                    y: row2Y,
                    w: btnW,
                    h: btnH,
                    label: sc === 'landing' ? '🛤️ DEPLOY TRACK' : sc === 'eva' ? '🚩 GLOVE / FLAG' : '⚡ PULSE LASER',
                    color: '#00E676',
                    action: () => triggerActionOrLaserKey()
                },
                {
                    id: 'autopilot',
                    x: rightBankX,
                    y: row1Y,
                    w: btnW,
                    h: btnH,
                    label: state.autopilot ? '🤖 AUTO: ON' : '🎮 MANUAL YOKE',
                    color: state.autopilot ? '#00E676' : '#00F0FF',
                    action: () => toggleAutopilot()
                },
                {
                    id: 'camera',
                    x: rightBankX,
                    y: row2Y,
                    w: btnW,
                    h: btnH,
                    label: `🎥 CAM: ${state.camPerspective.toUpperCase()}`,
                    color: '#A855F7',
                    action: () => toggleCameraPerspective()
                }
            ];

            state.cockpitButtons.forEach(b => {
                const pressed = state.activeDashBtnId === b.id && state.activeDashBtnTimer > 0;
                ctx.fillStyle = pressed ? b.color : '#060A1E';
                ctx.strokeStyle = b.color;
                ctx.lineWidth = pressed ? 2.5 : 1.6;
                ctx.beginPath();
                ctx.roundRect(b.x, b.y, b.w, b.h, 5);
                ctx.fill();
                ctx.stroke();

                ctx.fillStyle = pressed ? '#040611' : b.color;
                ctx.font = 'bold 9.5px "Orbitron", sans-serif';
                ctx.fillText(b.label, b.x + 8, b.y + 16);
            });

            // 5. CENTER INTERACTIVE 3D FLIGHT YOKE / DRIVING HANDLE
            const yokeBaseX = w * 0.50;
            const yokeBaseY = h * 0.96;

            // Compute dynamic yoke deflection based on active steering input
            let yokeShiftX = (state.yaw / 45) * 34;
            let yokeShiftY = -(state.pitch / 42) * 18;
            let yokeTiltRad = (state.roll * Math.PI) / 180;

            if (sc === 'landing') {
                yokeShiftX = state.landerX * 42;
                yokeShiftY = state.landerY * 22;
                yokeTiltRad = state.landerX * 0.45;
            } else if (sc === 'rover') {
                yokeShiftX = Math.cos(state.roverHeading) * (state.roverSpeedKmH > 1 ? 22 : 0);
                yokeTiltRad = Math.cos(state.roverHeading) * 0.32;
            } else if (sc === 'eva') {
                yokeShiftX = Math.sin(state.evaYaw) * 24;
                yokeTiltRad = Math.sin(state.evaYaw) * 0.3;
            }

            const yokeHubX = yokeBaseX + yokeShiftX;
            const yokeHubY = h * 0.86 + yokeShiftY;

            // Telescoping Steel Steering Column Shaft
            ctx.strokeStyle = '#334155';
            ctx.lineWidth = 14;
            ctx.beginPath();
            ctx.moveTo(yokeBaseX, yokeBaseY);
            ctx.lineTo(yokeHubX, yokeHubY);
            ctx.stroke();

            ctx.strokeStyle = '#94A3B8';
            ctx.lineWidth = 4;
            ctx.beginPath();
            ctx.moveTo(yokeBaseX - 3, yokeBaseY);
            ctx.lineTo(yokeHubX - 3, yokeHubY);
            ctx.stroke();

            // 3D Ergonomic Aerospace Flight Yoke / Driving Handle
            ctx.save();
            ctx.translate(yokeHubX, yokeHubY);
            ctx.rotate(yokeTiltRad);

            // Yoke Crossbar & Dual Contoured Hand Grips
            ctx.strokeStyle = state.yokeDragActive ? '#00F0FF' : '#CBD5E1';
            ctx.lineWidth = 9;
            ctx.lineCap = 'round';
            ctx.beginPath();
            ctx.moveTo(-64, 4);
            ctx.lineTo(64, 4);
            // Left contoured horn grip
            ctx.moveTo(-64, -26);
            ctx.quadraticCurveTo(-70, 4, -56, 22);
            // Right contoured horn grip
            ctx.moveTo(64, -26);
            ctx.quadraticCurveTo(70, 4, 56, 22);
            ctx.stroke();

            // Dark textured rubber grip overlays & Red Thumb Fire Trigger Buttons
            ctx.strokeStyle = '#0F172A';
            ctx.lineWidth = 5;
            ctx.beginPath();
            ctx.moveTo(-64, -20); ctx.lineTo(-62, 16);
            ctx.moveTo(64, -20); ctx.lineTo(62, 16);
            ctx.stroke();

            // Red Weapon/Laser Thumb Triggers on top of both grips
            ctx.fillStyle = '#FF2E54';
            ctx.beginPath();
            ctx.arc(-64, -25, 4.5, 0, Math.PI * 2);
            ctx.arc(64, -25, 4.5, 0, Math.PI * 2);
            ctx.fill();

            // Center Digital Telemetry Hub on the Flight Yoke
            ctx.fillStyle = '#040611';
            ctx.strokeStyle = '#00F0FF';
            ctx.lineWidth = 2;
            ctx.beginPath();
            ctx.roundRect(-30, -14, 60, 28, 6);
            ctx.fill();
            ctx.stroke();

            ctx.fillStyle = '#00F0FF';
            ctx.font = 'bold 8px "Orbitron", sans-serif';
            ctx.fillText(state.yokeDragActive ? 'YOKE ACTIVE' : 'DRAG YOKE', -25, -2);
            ctx.fillStyle = '#00E676';
            ctx.font = 'bold 8px "JetBrains Mono", monospace';
            ctx.fillText('PX-12 HOTAS', -24, 9);

            ctx.restore();
        }

        function startAnimationLoop() {
            function tick() {
                frameTick++;
                if (state.staticFireTimer > 0) state.staticFireTimer--;
                if (state.laserPulseTimer > 0) state.laserPulseTimer--;
                if (state.hullHitTimer > 0) state.hullHitTimer--;
                if (state.evaArmReachTimer > 0) state.evaArmReachTimer--;
                if (state.activeDashBtnTimer > 0) state.activeDashBtnTimer--;
                stepFlightAndSurfacePhysics();
                drawCockpitFrame();
                requestAnimationFrame(tick);
            }
            requestAnimationFrame(tick);
        }

        /*********************************************************************
         * 12. MODALS, MET CLOCK & INITIALIZATION
         *********************************************************************/
        function openJudgeModal() { document.getElementById('judge-modal')?.classList.remove('hidden'); }
        function closeJudgeModal() { document.getElementById('judge-modal')?.classList.add('hidden'); }
        function openPhysicsModal() { document.getElementById('physics-modal')?.classList.remove('hidden'); }
        function closePhysicsModal() { document.getElementById('physics-modal')?.classList.add('hidden'); }

        function startMetClock() {
            const update = () => {
                state.metSeconds++;
                const hrs = String(Math.floor(state.metSeconds / 3600)).padStart(2, '0');
                const mins = String(Math.floor((state.metSeconds % 3600) / 60)).padStart(2, '0');
                const secs = String(state.metSeconds % 60).padStart(2, '0');
                const el = document.getElementById('utc-clock-display');
                if (el) el.textContent = `MET ${hrs}:${mins}:${secs}`;
            };
            update();
            setInterval(update, 1000);
        }

        window.onload = function () {
            initRealNasaImageryPipeline();
            resetSurfaceAndEVAState();
            startMetClock();
            initKeyboardControls();
            renderBuilderUI();
            syncAutopilotUI();
            initTutorialRuns();
            resizeCockpitCanvas();
            startAnimationLoop();
            window.addEventListener('resize', () => {
                resizeCockpitCanvas();
                updateHandPosition();
            });
            window.addEventListener('scroll', updateHandPosition, true);
        };
    </script>
</body>
</html>
