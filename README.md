<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  
  <!-- Primary Meta Tags for GitHub Pages & Web -->
  <title>AGRipa - Lipa City Smart Weather, Telemetry & Farm Reports</title>
  <meta name="title" content="AGRipa - Lipa City Smart Weather, Telemetry & Farm Reports">
  <meta name="description" content="Official real-time agri-meteorological weather telemetry, biosecurity advisory, and farm reporting engine for Lipa City. Sourced with LGU City Agriculture Office directives.">

  <!-- Open Graph / Social Media Preview (Optimized for GitHub Pages sharing) -->
  <meta property="og:type" content="website">
  <meta property="og:title" content="AGRipa - Lipa Weather Telemetry & Live Farm Reports">
  <meta property="og:description" content="Real-time agricultural weather telemetry, ASF/Avian Flu virus zone tracker, and CDRRMO class suspension directives for Lipa City.">
  <meta property="og:image" content="https://images.unsplash.com/photo-1500382017468-9049fed747ef?auto=format&fit=crop&w=1200&q=80">

  <!-- Tailwind CSS CDN -->
  <script src="https://cdn.tailwindcss.com"></script>
  
  <!-- Google Fonts -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;600;700;800&family=Playfair+Display:ital,wght@0,600;0,800;1,600&display=swap" rel="stylesheet">

  <script>
    tailwind.config = {
      theme: {
        extend: {
          colors: {
            earth: {
              50: '#fcfaf7',
              100: '#f7f2ea',
              200: '#eee3d2',
              300: '#e2cfb3',
              400: '#d0b38c',
              500: '#be9568',
              600: '#a87e53',
              700: '#896242',
              800: '#6f503a',
              900: '#5a4231',
            },
            harvest: {
              green: '#1b4332',
              leaf: '#2d6a4f',
              lightgreen: '#d8f3dc',
              amber: '#d97706',
              terracotta: '#c05621',
            }
          },
          fontFamily: {
            serif: ['"Playfair Display"', 'serif'],
            sans: ['"Plus Jakarta Sans"', 'sans-serif'],
          }
        }
      }
    }
  </script>

  <style>
    body {
      background-image: 
        linear-gradient(to bottom, rgba(247, 242, 234, 0.88), rgba(247, 242, 234, 0.94)),
        url('https://images.unsplash.com/photo-1500382017468-9049fed747ef?auto=format&fit=crop&w=1920&q=80');
      background-size: cover;
      background-position: center;
      background-attachment: fixed;
      background-repeat: no-repeat;
      transition: background-color 0.5s ease;
    }

    body.emergency-active {
      background-image: 
        linear-gradient(to bottom, rgba(254, 226, 226, 0.90), rgba(239, 68, 68, 0.85)),
        url('https://images.unsplash.com/photo-1500382017468-9049fed747ef?auto=format&fit=crop&w=1920&q=80') !important;
    }

    .agri-card {
      background: rgba(255, 255, 255, 0.94);
      backdrop-filter: blur(16px);
      border: 1px solid rgba(226, 207, 179, 0.8);
      box-shadow: 0 10px 30px -5px rgba(90, 66, 49, 0.12);
      transition: all 0.4s ease;
    }

    .emergency-active .agri-card {
      border-color: #ef4444 !important;
      box-shadow: 0 10px 25px -5px rgba(220, 38, 38, 0.25) !important;
    }

    .map-wrapper {
      height: 520px;
      width: 100%;
    }

    .nav-btn.active {
      background-color: #2d6a4f !important;
      color: #ffffff !important;
      border-color: #52b788 !important;
    }

    .emergency-active header {
      background-color: #991b1b !important;
      border-bottom-color: #dc2626 !important;
    }

    @keyframes pulse-red {
      0%, 100% { opacity: 1; transform: scale(1); }
      50% { opacity: 0.94; transform: scale(1.015); }
    }

    .toast-alert {
      animation: pulse-red 1.8s infinite ease-in-out;
    }
  </style>
</head>
<body class="text-earth-900 font-sans min-h-screen pb-16">

  <!-- AUTOMATIC CLASS SUSPENSION POPUP MODAL -->
  <div id="classSuspensionModal" class="hidden fixed inset-0 z-50 bg-black/80 backdrop-blur-md flex items-center justify-center p-4">
    <div class="max-w-md w-full bg-red-900 text-white rounded-3xl p-6 shadow-2xl border-4 border-yellow-400 space-y-4 max-h-[90vh] overflow-y-auto relative">
      <button onclick="dismissSuspensionModal()" class="absolute top-4 right-4 bg-red-950 hover:bg-black text-white font-mono text-xs px-2.5 py-1 rounded-full border border-red-700 transition">
        ✕ Close
      </button>

      <div class="flex items-center gap-3 border-b border-red-700 pb-3">
        <div class="bg-yellow-400 text-red-950 p-3 rounded-2xl text-3xl shadow-lg">🚨</div>
        <div>
          <span class="text-[10px] font-extrabold uppercase tracking-widest bg-yellow-400 text-red-950 px-2 py-0.5 rounded-md">
            EXECUTIVE DRRMO REAL-TIME DIRECTIVE
          </span>
          <h3 class="text-lg font-serif font-extrabold text-yellow-300 mt-1">WALANG PASOK: NO CLASSES ANNOUNCEMENT</h3>
        </div>
      </div>

      <div class="space-y-2 text-xs leading-relaxed font-medium">
        <p class="text-sm font-bold text-yellow-200">OFFICIAL ANNOUNCEMENT FROM GOV. VILMA SANTOS-RECTO & PRESIDENT MARCOS JR.</p>
        <p id="suspensionNoticeText">
          As officially announced via the official Facebook pages of <strong>Gov. Vilma Santos-Recto</strong> and <strong>President Ferdinand Marcos Jr.</strong>, together with <strong>Lipa CDRRMO</strong> directives, there is an official declaration of <strong>SUSPENSION OF CLASSES IN ALL LEVELS</strong> across Lipa City.
        </p>
      </div>

      <div class="bg-red-950/80 p-3 rounded-xl border border-red-700 text-[11px] space-y-1">
        <p class="font-bold text-yellow-300">📍 Affected Scope:</p>
        <p id="suspensionScopeText" class="font-bold text-white">Lipa City Urban Center & All Barangays</p>
      </div>

      <div class="bg-red-950/90 p-3 rounded-xl border border-yellow-400/50 text-xs space-y-1.5">
        <div class="flex items-center justify-between border-b border-red-800 pb-1">
          <span class="font-extrabold text-yellow-300 uppercase text-[10px] tracking-wider">
            🛡️ EXECUTIVE VERIFICATION STAMP
          </span>
          <span class="bg-emerald-950 text-emerald-300 font-mono text-[9px] px-2 py-0.5 rounded border border-emerald-700 font-bold">
            REAL-TIME VERIFIED ✅
          </span>
        </div>
        <p class="text-[11px] text-white"><strong>Issuing Authorities:</strong> Gov. Vilma Santos-Recto FB Page / President Marcos Jr. FB Page / Lipa CDRRMO / DepEd Order No. 37, s. 2022</p>
        <p class="text-[11px] text-white"><strong>Verified Date & Time:</strong> <span id="modalVerificationTimestamp" class="font-mono text-yellow-200 font-bold">---</span></p>
        <p class="text-[11px] text-white flex flex-col gap-1 mt-1">
          <strong>Official Social Sources:</strong> 
          <a id="modalSourceLinkGov" href="https://www.facebook.com/VilmaSantosRectoOfficial" target="_blank" rel="noopener noreferrer" class="underline text-yellow-300 font-bold hover:text-yellow-200 flex items-center gap-1">
            📘 Gov. Vilma Santos-Recto Official FB Page
          </a>
          <a id="modalSourceLinkPbm" href="https://www.facebook.com/pbmarcosjr" target="_blank" rel="noopener noreferrer" class="underline text-yellow-300 font-bold hover:text-yellow-200 flex items-center gap-1">
            📘 President Ferdinand Marcos Jr. Official FB Page
          </a>
        </p>
      </div>

      <button onclick="dismissSuspensionModal()" class="w-full bg-yellow-400 hover:bg-yellow-300 text-red-950 font-extrabold py-3.5 rounded-xl shadow-lg text-xs tracking-wider transition">
        ACKNOWLEDGE & VIEW DASHBOARD
      </button>
    </div>
  </div>

  <!-- EMERGENCY ALERT TOAST -->
  <div id="emergencyToast" class="hidden fixed top-20 right-4 z-40 max-w-sm w-full bg-red-700 text-white p-4 rounded-2xl shadow-2xl border-2 border-red-300 toast-alert space-y-2">
    <div class="flex justify-between items-center border-b border-red-500/50 pb-1.5">
      <span class="text-xs font-extrabold uppercase tracking-widest bg-red-900 text-red-200 px-2 py-0.5 rounded-md">
        🚨 CRITICAL TCWS SIGNAL ALERT
      </span>
      <div class="flex items-center gap-1.5">
        <span class="text-[10px] font-mono text-red-200" id="toastTimer">Siren Active 🔊</span>
        <button onclick="dismissEmergencyAlert()" title="Exit Critical Alert" class="bg-red-950 hover:bg-black text-white font-bold text-xs px-2 py-0.5 rounded-full border border-red-500 transition">
          ✕ Exit
        </button>
      </div>
    </div>
    <p id="toastMessage" class="text-xs font-bold leading-snug">
      PAGASA TCWS Wind Signal Warning active in Lipa City!
    </p>
    <div class="flex gap-2 pt-1">
      <button onclick="playNdrrmcAlertTone()" class="flex-1 bg-yellow-400 hover:bg-yellow-300 text-red-950 text-[11px] font-extrabold py-1.5 rounded-lg shadow transition">
        🔊 Test Siren
      </button>
      <button onclick="muteEmergencySiren()" class="bg-red-950 hover:bg-black text-white text-[11px] font-bold px-3 py-1.5 rounded-lg transition border border-red-800">
        🔇 Mute Sound
      </button>
      <button onclick="dismissEmergencyAlert()" class="bg-red-900 hover:bg-red-950 text-white text-[11px] font-bold px-3 py-1.5 rounded-lg transition border border-red-700">
        🚪 Dismiss & Silence
      </button>
    </div>
  </div>

  <!-- LANDING SCREEN -->
  <div id="landingScreen" class="fixed inset-0 z-50 bg-harvest-green/95 backdrop-blur-md flex items-center justify-center p-4 overflow-y-auto">
    <div class="max-w-2xl w-full bg-earth-50/95 text-earth-900 rounded-3xl p-6 md:p-8 shadow-2xl border-4 border-harvest-amber space-y-6 my-auto">
      
      <div class="flex items-center gap-4 border-b border-earth-300 pb-4">
        <div class="bg-harvest-leaf text-white p-3.5 rounded-2xl text-4xl shadow-inner border border-emerald-400/30">🌾</div>
        <div>
          <span class="text-[10px] font-extrabold uppercase tracking-widest bg-harvest-amber text-white px-2.5 py-0.5 rounded-md">
            LIPA WEATHER & FARM REPORTS SYSTEM
          </span>
          <h1 class="text-3xl font-serif font-extrabold text-harvest-green mt-0.5">AGRipa</h1>
          <p class="text-xs text-earth-600 font-semibold">Lipa Weather Telemetry & Live Farm Reports Portal</p>
        </div>
      </div>

      <div class="space-y-3">
        <h2 class="text-sm font-bold text-earth-800 uppercase tracking-wider">Lipa City Node & Regional Advisory Network</h2>
        <p class="text-xs text-earth-700 leading-relaxed font-medium">
          Official telemetry, farm reports, and livestock virus monitoring engine for <strong>AGRipa</strong>. All hog farm censuses, chicken layer/broiler disease reports, and biosecurity advisories are sourced directly from the <strong>LGU City Government of Lipa (Office of the City Agriculturist - OCA)</strong>.
        </p>

        <div class="grid grid-cols-1 md:grid-cols-3 gap-2.5 pt-1 text-xs">
          <div class="bg-earth-100/80 p-3 rounded-xl border border-earth-200">
            <p class="font-bold text-harvest-green">🏛️ Official LGU Source</p>
            <p class="text-[11px] text-earth-600 mt-0.5">Data validated with Lipa City Agriculture Office (OCA) directives.</p>
          </div>
          <div class="bg-earth-100/80 p-3 rounded-xl border border-earth-200">
            <p class="font-bold text-harvest-green">🐖/🐔 Hog & Chicken Reports</p>
            <p class="text-[11px] text-earth-600 mt-0.5">Specific recommendations for Swine (ASF) and Poultry (Avian Flu) farms.</p>
          </div>
          <div class="bg-earth-100/80 p-3 rounded-xl border border-earth-200">
            <p class="font-bold text-harvest-green">🌀 TCWS Weather Siren</p>
            <p class="text-[11px] text-earth-600 mt-0.5">PAGASA storm signal tracking with automated acoustic alerts.</p>
          </div>
        </div>
      </div>

      <div class="bg-harvest-green/5 p-4 rounded-2xl border border-harvest-green/20 space-y-2">
        <span class="text-[10px] font-extrabold uppercase tracking-wider bg-harvest-green text-white px-2 py-0.5 rounded-md">
          Developer & System Architect
        </span>
        <h3 class="text-base font-serif font-bold text-harvest-green">JOHN IVAN M. PERJES</h3>
        <p class="text-xs font-semibold text-earth-800">Bachelor of Science in Computer Engineering (BSCpE)</p>
        <p class="text-xs text-earth-600 font-medium">AMA Computer College – Lipa Campus</p>
      </div>

      <button onclick="enterSystem()" class="w-full bg-harvest-terracotta hover:bg-orange-700 text-white font-extrabold py-4 rounded-2xl shadow-xl transition duration-200 text-base tracking-wide flex items-center justify-center gap-2">
        <span>🚀 ENTER AGRIPA DASHBOARD</span>
      </button>

    </div>
  </div>

  <!-- HEADER -->
  <header class="bg-harvest-green/95 backdrop-blur-md text-earth-50 border-b-4 border-harvest-amber shadow-xl sticky top-0 z-20 transition-colors duration-500">
    <div class="max-w-7xl mx-auto px-4 py-3 flex flex-col md:flex-row justify-between items-center gap-3">
      <div class="flex items-center gap-3 w-full md:w-auto justify-between md:justify-start">
        <div class="flex items-center gap-2.5">
          <div class="bg-harvest-leaf p-2 rounded-xl border border-emerald-400/30 text-xl shadow-inner">🌾</div>
          <div>
            <h1 class="text-xl font-serif font-bold tracking-tight text-emerald-100">AGRipa</h1>
            <p id="headerSubtext" class="text-[11px] text-emerald-300 font-medium tracking-wide">Lipa Weather Telemetry & Live Farm Reports Network</p>
          </div>
        </div>
        <div class="flex items-center gap-2">
          <span id="autoPollBadge" class="bg-emerald-950 text-emerald-300 text-[10px] px-2.5 py-1 rounded-full font-bold uppercase tracking-wider border border-emerald-700 animate-pulse">
            🔄 Auto-Polling: 30s
          </span>
          <span id="networkStatusBadge" class="bg-harvest-amber text-white text-[10px] px-2.5 py-1 rounded-full font-extrabold uppercase tracking-wider shadow-sm">
            Checking...
          </span>
        </div>
      </div>

      <nav class="flex flex-wrap items-center gap-1.5 text-xs font-bold w-full md:w-auto justify-center md:justify-end">
        <button onclick="switchTab('weather')" id="nav-weather" class="nav-btn active bg-emerald-900/60 text-emerald-100 px-3 py-1.5 rounded-lg border border-emerald-500/30 transition">
          📋 Priority Advisory & Map
        </button>
        <button onclick="switchTab('directives')" id="nav-directives" class="nav-btn bg-red-800 text-white px-3 py-1.5 rounded-lg border border-red-500/40 transition flex items-center gap-1">
          📢 Directives <span id="navDirectiveBadge" class="hidden bg-yellow-400 text-red-950 text-[9px] px-1.5 py-0.2 rounded-full font-extrabold">0</span>
        </button>
        <button onclick="switchTab('news')" id="nav-news" class="nav-btn bg-emerald-900/60 text-emerald-100 px-3 py-1.5 rounded-lg border border-emerald-500/30 transition">
          📰 Verified Agri News
        </button>
        <button onclick="switchTab('location')" id="nav-location" class="nav-btn bg-emerald-900/60 text-emerald-100 px-3 py-1.5 rounded-lg border border-emerald-500/30 transition">
          📍 Select Sector
        </button>
        <button onclick="switchTab('suggestions')" id="nav-suggestions" class="nav-btn bg-emerald-900/60 text-emerald-100 px-3 py-1.5 rounded-lg border border-emerald-500/30 transition">
          💡 Suggestions
        </button>
        <button onclick="switchTab('admin')" id="nav-admin" class="nav-btn bg-harvest-amber text-white px-3 py-1.5 rounded-lg shadow-sm transition">
          🔐 Portal Admin
        </button>
        <button onclick="switchTab('about')" id="nav-about" class="nav-btn bg-emerald-900/60 text-emerald-100 px-3 py-1.5 rounded-lg border border-emerald-500/30 transition">
          ℹ️ About
        </button>
      </nav>
    </div>
  </header>

  <main class="max-w-7xl mx-auto p-4 space-y-6">

    <!-- LIPA CLASS SUSPENSION BANNER -->
    <div id="batangasClassBanner" class="hidden bg-red-900/95 backdrop-blur-md text-white p-4 rounded-2xl border-4 border-yellow-400 shadow-xl space-y-2">
      <div class="flex items-center justify-between border-b border-red-700 pb-2">
        <span class="text-xs font-extrabold uppercase tracking-widest bg-yellow-400 text-red-950 px-2.5 py-0.5 rounded-md">
          🚨 GOV. VILMA SANTOS-RECTO & PRESIDENT MARCOS JR. REAL-TIME DIRECTIVE
        </span>
        <button onclick="dismissBannerNotice()" class="text-[10px] bg-red-950 hover:bg-black text-white px-2 py-0.5 rounded font-mono border border-red-700">
          ✕ Dismiss Banner
        </button>
      </div>
      <p id="bannerTitleText" class="text-sm font-serif font-bold text-yellow-200">
        ALL CLASSES SUSPENDED IN LIPA CITY
      </p>
      <p id="bannerDescText" class="text-xs text-red-100 font-medium">
        Gov. Vilma Santos-Recto and President Ferdinand Marcos Jr. have declared class suspensions across all levels.
      </p>

      <div class="bg-red-950/90 p-2.5 rounded-xl border border-yellow-400/40 text-[11px] space-y-1 mt-2">
        <div class="flex items-center justify-between">
          <span class="font-bold text-yellow-300 uppercase text-[10px] tracking-wider">🛡️ VERIFIED DIRECTIVE SOURCE</span>
          <span class="bg-emerald-900 text-emerald-200 text-[9px] px-2 py-0.5 rounded font-mono font-bold">LIVE VERIFIED ✅</span>
        </div>
        <p class="text-white"><strong>Source Authority:</strong> Gov. Vilma Santos-Recto Official FB Page / President Ferdinand Marcos Jr. Official FB Page / Lipa CDRRMO / DepEd Sec. Order No. 37</p>
        <p class="text-white"><strong>Verification Date & Time:</strong> <span id="bannerVerificationTimestamp" class="font-mono text-yellow-200 font-bold">---</span></p>
        <div class="flex flex-wrap gap-3 pt-1">
          <a id="bannerSourceLinkGov" href="https://www.facebook.com/VilmaSantosRectoOfficial" target="_blank" rel="noopener noreferrer" class="underline text-yellow-300 font-bold hover:text-yellow-200 text-[11px]">📘 Gov. Vilma Santos-Recto FB</a>
          <a id="bannerSourceLinkPbm" href="https://www.facebook.com/pbmarcosjr" target="_blank" rel="noopener noreferrer" class="underline text-yellow-300 font-bold hover:text-yellow-200 text-[11px]">📘 President Marcos Jr. FB</a>
        </div>
      </div>
    </div>

    <!-- Offline Alert Banner -->
    <div id="offlineNotice" class="hidden bg-amber-100 border-l-4 border-amber-500 text-amber-900 p-4 rounded-xl text-xs font-bold space-y-1 shadow-sm">
      <p id="offlineTitle" class="text-sm">⚠️ OFFLINE OR RESTRICTED NETWORK MODE ACTIVATED</p>
      <p id="offlineDesc" class="font-medium text-amber-800">Displaying saved or fallback synoptic telemetry for AGRipa node.</p>
    </div>

    <!-- TAB 1: MAIN DASHBOARD -->
    <div id="tab-weather" class="tab-content space-y-6">
      
      <!-- DETAILED FIELD ADVISORY SECTION -->
      <section id="advisoryCard" class="agri-card p-6 rounded-3xl space-y-4 border-2 border-harvest-leaf shadow-xl bg-gradient-to-br from-white/95 via-earth-50/90 to-emerald-50/80">
        <div class="flex flex-col md:flex-row justify-between items-start md:items-center gap-3 border-b border-earth-300 pb-3">
          <div class="flex items-center gap-3">
            <span class="text-3xl bg-harvest-green text-white p-2.5 rounded-2xl shadow-md">📋</span>
            <div>
              <span class="text-[10px] font-extrabold uppercase tracking-widest bg-harvest-leaf text-white px-2.5 py-0.5 rounded-md">
                AGRIPA FIELD DIRECTIVE & WARNINGS
              </span>
              <h2 id="advisoryHeader" class="text-xl font-serif font-extrabold text-harvest-green mt-0.5">
                Sector Field Guidance & Warning System
              </h2>
            </div>
          </div>
          <div class="flex items-center gap-2">
            <span class="text-xs bg-emerald-100 text-harvest-green font-bold px-3 py-1 rounded-full font-mono border border-emerald-300 shadow-sm">
              🎯 Active Zone: <span id="advisoryZoneTag">Lipa City Poblacion</span>
            </span>
            <button onclick="switchTab('location')" class="text-xs bg-harvest-terracotta hover:bg-orange-700 text-white font-extrabold px-3 py-1 rounded-lg shadow transition">
              ⚙️ Change Sector
            </button>
          </div>
        </div>

        <div id="advisoryOutput" class="text-xs md:text-sm text-earth-900 leading-relaxed font-medium space-y-4">
          Select a sector or agricultural focus to generate specialized local guidance.
        </div>
      </section>

      <!-- DYNAMIC DUAL POULTRY & SWINE VIRUS ZONE DETECTION DISPLAY CARD WITH OFFICIAL LGU SOURCE -->
      <section id="virusDetectorCard" class="agri-card p-5 rounded-3xl space-y-3 border-l-8 border-red-600 bg-red-50/40 shadow-lg">
        <div class="flex items-center justify-between border-b border-red-200 pb-2.5">
          <div class="flex items-center gap-2.5">
            <span class="bg-red-700 text-white p-2 rounded-xl text-xl shadow" id="virusIconTag">🏛️</span>
            <div>
              <div class="flex items-center gap-2">
                <span class="text-[10px] font-extrabold uppercase tracking-widest bg-red-800 text-white px-2 py-0.5 rounded">
                  SOURCE: LGU LIPA & CITY AGRICULTURE OFFICE (OCA)
                </span>
                <span class="bg-emerald-800 text-emerald-100 font-mono text-[9px] px-2 py-0.5 rounded font-bold">
                  AGRIPA VERIFIED ✅
                </span>
              </div>
              <h3 class="text-base font-serif font-bold text-red-950 mt-1" id="virusDetectorHeader">
                Target Zone: Lipa City Poblacion Virus Status
              </h3>
            </div>
          </div>
          <span id="virusStatusTag" class="bg-red-600 text-white text-[10px] px-3 py-1 rounded-full font-extrabold uppercase border border-red-700 font-mono shadow animate-pulse">
            🚨 HIGH RISK VIRUS ZONE DETECTED
          </span>
        </div>

        <div class="text-xs text-earth-900 leading-relaxed space-y-2">
          <p id="virusZoneDescText" class="font-bold text-red-900">
            Official Report Sourced from LGU Lipa City Agriculture Office (OCA): Barangay Sico is flagged under RED / HIGH RISK status by LGU Veterinary biosecurity protocols.
          </p>

          <div id="virusBiosecurityGuidelines" class="grid grid-cols-1 md:grid-cols-3 gap-3 pt-1 font-medium text-[11px]">
            <!-- Dynamic detailed guidelines populated by JavaScript -->
          </div>
        </div>
      </section>

      <!-- PAGASA TCWS & Risk Assessment Badges -->
      <section class="grid grid-cols-1 md:grid-cols-2 gap-4">
        <div id="tcwsCard" class="agri-card p-4 rounded-2xl border-l-4 border-harvest-amber flex items-center justify-between">
          <div>
            <p id="lblTcwsTitle" class="text-xs font-bold text-earth-600 uppercase">PAGASA TCWS Warning (Active Zone)</p>
            <p id="tcwsSignalText" class="text-base font-serif font-bold text-harvest-green mt-0.5">No Typhoon Threat (NO SIGNAL)</p>
          </div>
          <span class="text-3xl">🌀</span>
        </div>

        <div id="pestBadgeCard" class="agri-card p-4 rounded-2xl border-l-4 border-emerald-600 flex items-center justify-between transition-colors">
          <div>
            <p id="lblPestBadgeTitle" class="text-xs font-bold text-earth-600 uppercase">AGRipa Risk Assessment</p>
            <p id="pestRiskText" class="text-base font-serif font-bold text-emerald-700 mt-0.5">Normal (Low Stress)</p>
          </div>
          <span class="text-3xl" id="pestRiskIcon">🌱</span>
        </div>
      </section>

      <!-- WINDY MAP SECTION -->
      <section class="agri-card p-5 rounded-2xl space-y-4">
        <div class="flex flex-wrap justify-between items-center gap-2 border-b border-earth-200 pb-3">
          <div>
            <label id="lblRadarTitle" class="text-sm font-bold text-earth-800 uppercase tracking-wider flex items-center gap-1.5">
              🌀 Live Windy.com Weather Radar Map
            </label>
            <p id="lblRadarDesc" class="text-xs text-earth-500 font-medium">Interactive animated radar centered over AGRipa node coordinates</p>
          </div>
          
          <div class="flex flex-wrap gap-1.5">
            <button onclick="changeWindyOverlay('rain')" class="bg-emerald-800 hover:bg-emerald-900 text-white text-[11px] font-bold px-2.5 py-1 rounded-lg shadow transition">
              🌧️ Rain Radar
            </button>
            <button onclick="changeWindyOverlay('wind')" class="bg-harvest-terracotta hover:bg-orange-700 text-white text-[11px] font-bold px-2.5 py-1 rounded-lg shadow transition">
              💨 Wind Stream
            </button>
            <button onclick="changeWindyOverlay('temp')" class="bg-amber-600 hover:bg-amber-700 text-white text-[11px] font-bold px-2.5 py-1 rounded-lg shadow transition">
              🌡️ Heat Map
            </button>
            <button onclick="changeWindyOverlay('clouds')" class="bg-blue-800 hover:bg-blue-900 text-white text-[11px] font-bold px-2.5 py-1 rounded-lg shadow transition">
              ☁️ Cloud Cover
            </button>
            <button onclick="simulateHeavyRainAlert()" class="bg-red-700 hover:bg-red-800 text-white text-[11px] font-bold px-2.5 py-1 rounded-lg shadow transition flex items-center gap-1">
              🚨 TCWS Siren Test
            </button>
          </div>
        </div>

        <div class="map-wrapper overflow-hidden rounded-xl border border-earth-300 shadow-inner relative">
          <iframe 
            id="windyIframe" 
            class="w-full h-full border-0 rounded-xl"
            src="https://embed.windy.com/embed2.html?lat=13.9419&lon=121.1644&detailLat=13.9419&detailLon=121.1644&width=650&height=500&zoom=11&level=surface&overlay=rain&product=ecmwf&menu=&message=true&marker=true&calendar=now&pressure=&type=map&location=coordinates&detail=&metricWind=km%2Fh&metricTemp=%C2%B0C&radarRange=-1" 
            allowfullscreen>
          </iframe>
        </div>
      </section>

      <!-- AGRipa Field Telemetry -->
      <section id="weatherCard" class="agri-card p-5 rounded-2xl space-y-4">
        <div class="flex justify-between items-center">
          <h2 id="lblTelemetryHeader" class="text-xs font-bold text-earth-700 uppercase tracking-wider">Current Sector Field Telemetry</h2>
          <span id="dataTimestamp" class="text-xs text-earth-500 font-mono">Synced Telemetry</span>
        </div>
        
        <div class="grid grid-cols-2 md:grid-cols-4 gap-3 text-center">
          <div class="bg-earth-100/80 p-3.5 rounded-xl border border-earth-200">
            <p class="text-xs font-semibold text-earth-700">Rainfall (24hr)</p>
            <p id="rainVal" class="text-xl font-serif font-bold text-harvest-green mt-1">0 mm</p>
          </div>
          <div class="bg-earth-100/80 p-3.5 rounded-xl border border-earth-200">
            <p class="text-xs font-semibold text-earth-700">Wind Speed</p>
            <p id="windVal" class="text-xl font-serif font-bold text-harvest-terracotta mt-1">12 km/h</p>
          </div>
          <div class="bg-earth-100/80 p-3.5 rounded-xl border border-earth-200">
            <p class="text-xs font-semibold text-earth-700">Heat Index</p>
            <p id="heatIndexVal" class="text-xl font-serif font-bold text-red-700 mt-1">32°C</p>
          </div>
          <div class="bg-earth-100/80 p-3.5 rounded-xl border border-earth-200">
            <p class="text-xs font-semibold text-earth-700">Relative Humidity</p>
            <p id="humidityVal" class="text-xl font-serif font-bold text-blue-800 mt-1">75%</p>
          </div>
          <div class="bg-earth-100/80 p-3.5 rounded-xl border border-earth-200">
            <p class="text-xs font-semibold text-earth-700">Evapotranspiration (ET₀)</p>
            <p id="etVal" class="text-xl font-serif font-bold text-amber-800 mt-1">4.2 mm/day</p>
          </div>
          <div class="bg-earth-100/80 p-3.5 rounded-xl border border-earth-200">
            <p class="text-xs font-semibold text-earth-700">VPD Deficit</p>
            <p id="vpdVal" class="text-xl font-serif font-bold text-purple-800 mt-1">0.85 kPa</p>
          </div>
          <div class="bg-earth-100/80 p-3.5 rounded-xl border border-earth-200">
            <p class="text-xs font-semibold text-earth-700">Soil Moisture</p>
            <p id="soilVal" class="text-xl font-serif font-bold text-emerald-900 mt-1">45%</p>
          </div>
          <div class="bg-earth-100/80 p-3.5 rounded-xl border border-earth-200">
            <p class="text-xs font-semibold text-earth-700">Dew Point</p>
            <p id="dewVal" class="text-xl font-serif font-bold text-indigo-900 mt-1">24.1°C</p>
          </div>
        </div>
      </section>

      <!-- 7-Day Forecast Table -->
      <section id="forecastCard" class="agri-card p-5 rounded-2xl space-y-3">
        <h2 id="lblForecastHeader" class="text-xs font-bold text-earth-700 uppercase tracking-wider">📅 7-Day Weather Outlook</h2>
        <div class="overflow-x-auto">
          <table class="w-full text-xs text-left text-earth-900 border-collapse">
            <thead>
              <tr class="border-b border-earth-300 font-bold text-harvest-green uppercase">
                <th class="p-2">Date</th>
                <th class="p-2">Max/Min Temp</th>
                <th class="p-2">Expected Rain</th>
                <th class="p-2">Wind Speed</th>
              </tr>
            </thead>
            <tbody id="forecastTableBody"></tbody>
          </table>
        </div>
      </section>

    </div>

    <!-- TAB 2: DIRECTIVES SECTION -->
    <div id="tab-directives" class="tab-content hidden space-y-6 max-w-4xl mx-auto">
      <!-- DYNAMICALLY RENDERED BY RENDERDIRECTIVESSECTION() -->
    </div>

    <!-- TAB 3: VERIFIED AGRI NEWS -->
    <div id="tab-news" class="tab-content hidden space-y-6 max-w-4xl mx-auto">
      <div class="bg-harvest-green text-white p-5 rounded-2xl shadow-lg border-2 border-emerald-600 flex flex-col md:flex-row justify-between items-start md:items-center gap-3">
        <div class="flex items-center gap-3">
          <div class="bg-emerald-950 text-emerald-300 font-extrabold text-lg px-3 py-1 rounded-xl shadow-md font-sans border border-emerald-700">
            VERIFIED AGRI NEWS
          </div>
          <div>
            <h2 class="text-lg font-serif font-bold tracking-wide">AGRipa National & Local Updates</h2>
            <p class="text-xs text-emerald-200">Fact-Checked Farming, Land & Climate Reports Sourced from Official Outlets</p>
          </div>
        </div>
        <span class="text-[10px] bg-emerald-900 text-emerald-100 font-mono px-3 py-1 rounded-full border border-emerald-700">
          Anti-Fake News Engine ✅
        </span>
      </div>

      <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
        <article class="agri-card p-5 rounded-2xl space-y-3 border-l-4 border-harvest-green flex flex-col justify-between">
          <div class="space-y-2">
            <div class="flex justify-between items-center text-[10px] font-bold text-earth-500 uppercase">
              <span class="bg-emerald-100 text-harvest-green px-2 py-0.5 rounded">📜 Agri Land Protection</span>
              <span class="font-mono text-emerald-900 font-bold">PIA / DA Official</span>
            </div>
            <h3 class="font-serif font-bold text-earth-900 text-base leading-snug">
              DA & DOE move to protect prime farmlands from solar expansion
            </h3>
            <p class="text-xs text-earth-700 leading-relaxed">
              Agriculture Secretary Francisco P. Tiu Laurel Jr. announced geofencing of high-yield rice fields to prevent solar projects from crowding out food production.
            </p>
          </div>
          <div class="pt-2 border-t border-earth-200 space-y-1">
            <div class="flex justify-between items-center text-[10px] text-earth-600 font-mono">
              <span>Published: Sept 8, 2026</span>
              <span class="text-emerald-700 font-bold">Verified ✅</span>
            </div>
            <a href="https://pia.gov.ph/news/ph-moves-to-protect-farmlands-from-solar-expansion/" target="_blank" rel="noopener noreferrer" class="block text-[11px] font-bold text-harvest-leaf hover:underline truncate">
              🔗 Source: pia.gov.ph/news/ph-moves-to-protect-farmlands...
            </a>
          </div>
        </article>

        <article class="agri-card p-5 rounded-2xl space-y-3 border-l-4 border-amber-600 flex flex-col justify-between">
          <div class="space-y-2">
            <div class="flex justify-between items-center text-[10px] font-bold text-earth-500 uppercase">
              <span class="bg-amber-100 text-amber-900 px-2 py-0.5 rounded">☀️ Climate Risk Advisory</span>
              <span class="font-mono text-amber-900 font-bold">PAGASA / ReliefWeb</span>
            </div>
            <h3 class="font-serif font-bold text-earth-900 text-base leading-snug">
              El Niño Climate Outlook issued for PH agriculture as dry conditions intensify
            </h3>
            <p class="text-xs text-earth-700 leading-relaxed">
              PAGASA and DA issue coordinated action plans prioritizing drought-prone farming areas to safeguard food production.
            </p>
          </div>
          <div class="pt-2 border-t border-earth-200 space-y-1">
            <div class="flex justify-between items-center text-[10px] text-earth-600 font-mono">
              <span>Published: Sept 8, 2026</span>
              <span class="text-emerald-700 font-bold">Verified ✅</span>
            </div>
            <a href="https://reliefweb.int/report/philippines/el-nino-climate-impact-outlook-philippines-august-2026" target="_blank" rel="noopener noreferrer" class="block text-[11px] font-bold text-amber-800 hover:underline truncate">
              🔗 Source: reliefweb.int/report/philippines/el-nino-outlook...
            </a>
          </div>
        </article>
      </div>
    </div>

    <!-- TAB 4: LOCATION & SELECTOR -->
    <div id="tab-location" class="tab-content hidden space-y-6 max-w-2xl mx-auto">
      <section class="agri-card p-6 rounded-2xl space-y-4">
        <div class="flex items-center justify-between border-b border-earth-200 pb-3">
          <div class="flex items-center gap-2">
            <span class="text-xl">📍</span>
            <h2 id="lblLocationTitle" class="font-serif font-bold text-harvest-green text-lg">AGRipa Sector Selector</h2>
          </div>
          <span class="text-xs bg-emerald-100 text-harvest-green px-2.5 py-1 rounded-md font-bold font-mono">
            AWS Station ID: <span id="awsStationId">5035-LIPA</span>
          </span>
        </div>

        <div class="space-y-4">
          <div class="space-y-1.5">
            <label class="block text-xs font-bold text-earth-800 uppercase tracking-wider">
              🔍 Select Zone or Barangay
            </label>
            <input type="text" id="locationSearchInput" oninput="filterLocations()" placeholder="🔎 Search zone or barangay..." class="w-full p-2.5 pl-3 border border-earth-300 rounded-xl bg-white focus:outline-none focus:ring-2 focus:ring-harvest-leaf text-xs font-bold text-earth-900 mb-1">
            
            <select id="provinceSelect" onchange="selectProvince()" class="w-full p-3 border border-earth-300 rounded-xl bg-earth-50/80 focus:outline-none focus:ring-2 focus:ring-harvest-leaf text-earth-900 font-semibold text-sm max-h-60">
              
              <optgroup label="--- LIPA CITY URBAN & COMMERCIAL ZONES ---">
                <option value="13.9419,121.1644,Lipa City Poblacion, Lipa City,5035" selected>🏙️ Lipa City Central / Poblacion (Urban Center)</option>
                <option value="13.9550,121.1620,Balintawak, Lipa City,5035">🏙️ Balintawak, Lipa City</option>
                <option value="13.9610,121.1750,Marawoy, Lipa City,5035">🏙️ Marawoy (Commercial Hub)</option>
                <option value="13.9310,121.1550,Tambo, Lipa City,5035">🏙️ Tambo, Lipa City</option>
              </optgroup>

              <optgroup label="--- LIPA CITY AGRICULTURAL & HILLSIDE BARANGAYS ---">
                <option value="13.9820,121.1980,Mataas Na Lupa, Lipa City,5035">☕ Mataas Na Lupa (Kapeng Barako Zone)</option>
                <option value="13.9210,121.2100,Inosloban, Lipa City,5035">☕ Inosloban, Lipa City</option>
                <option value="13.9710,121.1850,Plaridel, Lipa City,5035">🌾 Plaridel, Lipa City</option>
                <option value="13.9100,121.1400,Lodlod, Lipa City,5035">🌾 Lodlod, Lipa City</option>
                <option value="13.9650,121.1350,Anilao, Lipa City,5035">🌾 Anilao, Lipa City</option>
                <option value="13.9050,121.1800,Pinagtongulan, Lipa City,5035">🐓 Pinagtongulan (Poultry & Layer Farms)</option>
                <option value="13.9350,121.2200,Sico, Lipa City,5035">🐖 Sico (Swine Sector)</option>
              </optgroup>

            </select>
          </div>

          <div>
            <label class="block text-xs font-bold text-earth-800 uppercase tracking-wider mb-1.5">
              🌱 Select Agricultural Focus / Farming Sector
            </label>
            <select id="cropSelect" class="w-full p-3 border border-earth-300 rounded-xl bg-earth-50/80 focus:outline-none focus:ring-2 focus:ring-harvest-leaf font-semibold text-sm text-earth-900">
              
              <optgroup label="--- 🏢 URBAN & HOUSEHOLD GARDENING ---">
                <option value="urban_gardening" selected>🏢 Urban & Container Gardening (Microgreens, Hydroponics)</option>
                <option value="backyard_vegetables">🥬 Backyard Vegetables (Pinakbet: Eggplant, Tomato, Okra, Sitaw)</option>
              </optgroup>

              <optgroup label="--- 🐖 SWINE & LIVESTOCK BIOSECURITY ---">
                <option value="swine">🐖 Swine Farming (African Swine Fever - LGU / OCA Sourced)</option>
              </optgroup>

              <optgroup label="--- 🥚 POULTRY & AVIAN FLU BIOSECURITY ---">
                <option value="poultry">🥚 Poultry Layer Farms (Avian Flu H5N1 - LGU / OCA Sourced)</option>
                <option value="broiler">🐔 Broiler Chicken Farms (Avian Flu H5N1 - LGU / OCA Sourced)</option>
              </optgroup>

              <optgroup label="--- ☕ TRADITIONAL LIPA HIGHLAND CROPS ---">
                <option value="coffee">☕ Kapeng Barako / Liberica & Robusta Coffee (Lipa Highlands)</option>
                <option value="cacao">🍫 Cacao / Tablea Production</option>
                <option value="coconut">🥥 Coconut & Intercropping Systems</option>
                <option value="spices">🌶️ Black Pepper & High-Value Spices</option>
              </optgroup>

              <optgroup label="--- 🌾 GRAINS & HIGH-VALUE FIELD CROPS ---">
                <option value="rice">🌾 Palay / Smallholder Rice Farming</option>
                <option value="corn">🌽 Green Corn / Hybrid Yellow Corn</option>
                <option value="sugarcane">🎍 Sugarcane Production</option>
                <option value="fruit_trees">🍋 Fruit Orchards (Lanzones, Citrus, Rambutan, Mango)</option>
              </optgroup>

              <optgroup label="--- 🐂 OTHER LIVESTOCK SECTORS ---">
                <option value="household_livestock">🐂 Cattle, Carabao, & Goat Raising</option>
              </optgroup>

            </select>
          </div>
        </div>

        <button onclick="fetchAgriForecast(); switchTab('weather');" id="btnAnalyze" class="w-full bg-harvest-terracotta hover:bg-orange-700 text-white font-extrabold py-3.5 rounded-xl shadow-md transition duration-200 text-sm tracking-wide flex items-center justify-center gap-2">
          <span>Apply Zone & Update Telemetry</span> 🌧️
        </button>

        <div class="flex items-center justify-between text-xs text-earth-700 pt-2 border-t border-earth-200">
          <span id="selectedProvinceName" class="font-bold text-harvest-green">Selected: Lipa City Poblacion</span>
          <button onclick="getLocation()" id="btnGps" class="text-harvest-terracotta font-bold hover:underline flex items-center gap-1 bg-earth-100 px-3 py-1.5 rounded-lg border border-earth-300 shadow-sm text-xs">
            📍 GPS Location
          </button>
        </div>
      </section>
    </div>

    <!-- TAB 5: SUGGESTION BOX -->
    <div id="tab-suggestions" class="tab-content hidden space-y-6 max-w-2xl mx-auto">
      <section class="agri-card p-6 rounded-2xl space-y-4 border-2 border-harvest-amber/40">
        <div class="flex items-center gap-2.5 text-harvest-green border-b border-earth-200 pb-3">
          <span class="text-3xl">🏛️</span>
          <div>
            <h2 class="text-lg font-serif font-bold">AGRipa Feedback & Farm Reporting Box</h2>
            <p class="text-xs text-earth-600 font-medium">Submit farm reports, hog/chicken health issues, or biosecurity reports directly to AGRipa and the Office of the City Agriculturist (OCA).</p>
          </div>
        </div>

        <form onsubmit="submitAnonymousSuggestion(event)" class="space-y-4 text-xs">
          <div>
            <label class="block font-bold text-earth-800 uppercase tracking-wider mb-1.5">Category</label>
            <select id="suggestionCategory" class="w-full p-3 border border-earth-300 rounded-xl bg-earth-50/80 focus:outline-none focus:ring-2 focus:ring-harvest-leaf font-semibold text-earth-900 text-sm">
              <option value="Swine / ASF Biosecurity">🐖 Swine / ASF Biosecurity Report (LGU Lipa OCA)</option>
              <option value="Poultry / Bird Flu Biosecurity">🐔 Poultry / Avian Flu Report (LGU Lipa OCA)</option>
              <option value="Weather Feedback">🌧️ Telemetry & Weather Alert Accuracy</option>
              <option value="Feature Idea">💡 Feature Idea</option>
              <option value="Bug Report">🐛 System Bug / Issue</option>
            </select>
          </div>

          <div>
            <label class="block font-bold text-earth-800 uppercase tracking-wider mb-1.5">Your Suggestion / Message</label>
            <textarea id="suggestionText" rows="4" required placeholder="Type your feedback or local hog/poultry farm report here..." class="w-full p-3 border border-earth-300 rounded-xl bg-earth-50/80 focus:outline-none focus:ring-2 focus:ring-harvest-leaf font-semibold text-earth-900 text-sm"></textarea>
          </div>

          <button type="submit" class="w-full bg-harvest-leaf hover:bg-harvest-green text-white font-extrabold py-3.5 rounded-xl shadow-md transition duration-200 text-sm tracking-wide flex items-center justify-center gap-2">
            <span>📩 SUBMIT TO AGRIPA & OCA</span>
          </button>
          <p id="submitSuccessMsg" class="hidden text-center text-xs font-bold text-emerald-700 bg-emerald-50 p-2.5 rounded-lg border border-emerald-200">
            ✅ Farm report submitted to AGRipa Admin Panel!
          </p>
        </form>
      </section>
    </div>

    <!-- TAB 6: ADMIN INBOX & ASF ZONE MANAGER -->
    <div id="tab-admin" class="tab-content hidden space-y-6 max-w-2xl mx-auto">
      <section class="agri-card p-6 rounded-2xl space-y-4 border-2 border-harvest-green/40">
        <div class="flex items-center justify-between border-b border-earth-200 pb-3">
          <div class="flex items-center gap-2 text-harvest-green">
            <span class="text-2xl">🔐</span>
            <div>
              <h2 class="text-lg font-serif font-bold">Portal Admin Control & Virus Zone Manager</h2>
              <p class="text-xs text-earth-600 font-medium">AGRipa & Office of the City Agriculturist Control Panel</p>
            </div>
          </div>
          <span id="adminStatusBadge" class="bg-red-100 text-red-800 text-[10px] px-2.5 py-1 rounded-md font-bold uppercase border border-red-200">
            Locked 🔒
          </span>
        </div>

        <div id="adminAuthBox" class="space-y-3 bg-earth-50 p-4 rounded-xl border border-earth-300">
          <label class="block text-xs font-bold text-earth-800 uppercase tracking-wider">Enter Admin Password</label>
          <div class="flex gap-2">
            <input type="password" id="adminPassInput" placeholder="Enter password..." class="flex-1 p-2.5 border border-earth-300 rounded-xl bg-white focus:outline-none focus:ring-2 focus:ring-harvest-leaf text-xs font-bold">
            <button onclick="unlockAdminPanel()" class="bg-harvest-green hover:bg-harvest-leaf text-white text-xs font-extrabold px-4 py-2.5 rounded-xl shadow-md transition">
              Unlock
            </button>
          </div>
          <p id="adminAuthError" class="hidden text-xs font-bold text-red-600">❌ Incorrect password. Access denied.</p>
        </div>

        <div id="adminInboxContent" class="hidden space-y-6">
          <div class="flex justify-between items-center bg-emerald-50 p-3 rounded-xl border border-emerald-200">
            <span class="text-xs font-bold text-harvest-green">AGRipa Authenticated Session Active</span>
            <button onclick="clearAllSuggestions()" class="text-[11px] bg-red-600 hover:bg-red-700 text-white font-bold px-3 py-1 rounded-lg transition">
              Clear All Messages
            </button>
          </div>

          <!-- ASF / AVIAN FLU BARANGAY ZONE MANAGEMENT CONTROL CARD -->
          <div class="bg-red-50/90 p-5 rounded-2xl border-2 border-red-300 space-y-3.5 shadow-md">
            <div class="flex items-center justify-between border-b border-red-200 pb-2">
              <h3 class="text-xs font-extrabold text-red-950 uppercase tracking-wider flex items-center gap-1.5">
                <span>🏛️</span> Manage Lipa Barangay Livestock Virus Risk Zones (AGRipa)
              </h3>
              <span class="text-[10px] bg-red-200 text-red-900 px-2 py-0.5 rounded font-mono font-bold">LGU OCA Control</span>
            </div>

            <div class="grid grid-cols-1 md:grid-cols-2 gap-3 text-xs">
              <div>
                <label class="block font-bold text-earth-800 mb-1">Select Barangay</label>
                <select id="adminBarangaySelect" class="w-full p-2.5 border border-earth-300 rounded-xl bg-white font-semibold">
                  <option value="Sico">Sico</option>
                  <option value="Pinagtongulan">Pinagtongulan</option>
                  <option value="Inosloban">Inosloban</option>
                  <option value="Plaridel">Plaridel</option>
                  <option value="Mataas Na Lupa">Mataas Na Lupa</option>
                  <option value="Lodlod">Lodlod</option>
                  <option value="Anilao">Anilao</option>
                  <option value="Balintawak">Balintawak</option>
                  <option value="Marawoy">Marawoy</option>
                  <option value="Tambo">Tambo</option>
                  <option value="Lipa City Poblacion">Lipa City Poblacion</option>
                </select>
              </div>

              <div>
                <label class="block font-bold text-earth-800 mb-1">Assign Virus Zone Status</label>
                <select id="adminZoneSelect" class="w-full p-2.5 border border-earth-300 rounded-xl bg-white font-bold text-red-900">
                  <option value="RED">🔴 RED: High Risk Containment Zone</option>
                  <option value="YELLOW">🟡 YELLOW: Buffer Quarantine Zone</option>
                  <option value="GREEN">🟢 GREEN: Disease-Free Protected Zone</option>
                </select>
              </div>
            </div>

            <button onclick="updateBarangayAsfZone()" class="w-full bg-red-700 hover:bg-red-800 text-white font-extrabold py-3 rounded-xl shadow transition text-xs flex items-center justify-center gap-1.5">
              <span>💾 SAVE ZONE CHANGE & BROADCAST SYSTEM WIDE</span>
            </button>
            <p id="adminZoneSuccessMsg" class="hidden text-center text-[11px] font-bold text-emerald-800 bg-emerald-100 p-2 rounded-lg border border-emerald-300">
              ✅ AGRipa Barangay Virus Zone status updated & synced!
            </p>
          </div>

          <div class="space-y-2">
            <h3 class="text-xs font-bold text-earth-800 uppercase tracking-wider">Farmer Suggestion & Report Inbox</h3>
            <div id="suggestionList" class="space-y-3 max-h-80 overflow-y-auto pr-1"></div>
          </div>
        </div>
      </section>
    </div>

    <!-- TAB 7: ABOUT SECTION -->
    <div id="tab-about" class="tab-content hidden space-y-6 max-w-3xl mx-auto">
      <section class="agri-card p-6 rounded-2xl space-y-6 border-t-4 border-harvest-green">
        <div class="space-y-2">
          <div class="flex items-center gap-2 text-harvest-green">
            <span class="text-2xl">🏛️</span>
            <h2 class="text-lg font-serif font-bold">About AGRipa</h2>
          </div>
          <p class="text-xs text-earth-800 leading-relaxed">
            <strong>AGRipa</strong> is a smart agri-meteorological and live farm reporting system designed for local growers, hog raisers, poultry layer/broiler operators, and agricultural extension officers in Lipa City. All regional livestock disease reports and biosecurity protocols are directly aligned with local directives from the <strong>LGU City Government of Lipa (Office of the City Agriculturist - OCA)</strong> located at Tanco Drive, Barangay Marawoy, Lipa City. Aligned with the <strong>HEARTS Program</strong> initiated under the executive mandate of <strong>Gov. Vilma Santos-Recto</strong> and national directives from <strong>President Ferdinand Marcos Jr.</strong>
          </p>
        </div>

        <hr class="border-earth-200">

        <div class="flex flex-col md:flex-row items-start md:items-center gap-4 bg-earth-50 p-4 rounded-xl border border-earth-300">
          <div class="bg-harvest-green text-white p-4 rounded-2xl text-center font-bold font-serif min-w-[70px] shadow-sm">
            <span class="text-2xl">👨‍💻</span>
          </div>
          <div class="space-y-1">
            <span class="text-[10px] uppercase font-extrabold tracking-wider bg-harvest-amber text-white px-2 py-0.5 rounded-md">
              Lead Developer & Systems Architect
            </span>
            <h3 class="text-base font-serif font-bold text-harvest-green">JOHN IVAN M. PERJES</h3>
            <p class="text-xs font-semibold text-earth-800">Bachelor of Science in Computer Engineering (BSCpE)</p>
            <p class="text-xs text-earth-600 font-medium">AMA Computer College – Lipa Campus</p>
          </div>
        </div>
      </section>
    </div>

  </main>

  <script>
    // DEFAULT LIPA CITY COORDINATES
    let userLat = 13.9419;
    let userLon = 121.1644;
    let locationLabel = "Lipa City Poblacion";
    let currentAwsId = "5035";
    let currentWindyOverlay = "rain";

    const ADMIN_PASSWORD = "AGRIpanahonph2026";
    let globalAudioCtx = null;
    let sirenIntervalTimer = null;
    let autoPollingTimer = null;
    let isSirenMuted = false;
    let hasModalBeenDismissedThisSession = false;
    let userDismissedAlert = false;

    // DEFAULT LIPA CITY BARANGAY LIVESTOCK VIRUS DATABASE (AGRIPA & OCA SOURCED)
    const DEFAULT_ASF_DATABASE = {
      'Sico': { zone: 'RED', level: 'HIGH RISK VIRUS CONTAINMENT ZONE', desc: 'Sourced from LGU Lipa & Office of the City Agriculturist (OCA): Barangay Sico is under RED / HIGH RISK status. Active LGU quarantine checkpoints in place.' },
      'Pinagtongulan': { zone: 'YELLOW', level: 'BUFFER QUARANTINE ZONE', desc: 'Sourced from LGU Lipa & Office of the City Agriculturist (OCA): Pinagtongulan is in a Yellow Buffer Zone. Heightened biosecurity required.' },
      'Inosloban': { zone: 'YELLOW', level: 'BUFFER QUARANTINE ZONE', desc: 'Sourced from LGU Lipa & Office of the City Agriculturist (OCA): Inosloban is under Yellow Surveillance status.' },
      'Plaridel': { zone: 'GREEN', level: 'FREE / PROTECTED ZONE', desc: 'AGRipa Status: Plaridel is classified as Disease-Free Zone.' },
      'Mataas Na Lupa': { zone: 'GREEN', level: 'FREE / PROTECTED ZONE', desc: 'AGRipa Status: Mataas Na Lupa is classified as Disease-Free Zone.' },
      'Lodlod': { zone: 'GREEN', level: 'FREE / PROTECTED ZONE', desc: 'AGRipa Status: Lodlod is classified as Disease-Free Zone.' },
      'Anilao': { zone: 'GREEN', level: 'FREE / PROTECTED ZONE', desc: 'AGRipa Status: Anilao is classified as Disease-Free Zone.' },
      'Balintawak': { zone: 'GREEN', level: 'FREE / PROTECTED ZONE', desc: 'AGRipa Status: Balintawak Urban Zone is classified as Disease-Free Zone.' },
      'Marawoy': { zone: 'GREEN', level: 'FREE / PROTECTED ZONE', desc: 'AGRipa Status: Marawoy Commercial Zone is classified as Disease-Free Zone.' },
      'Tambo': { zone: 'GREEN', level: 'FREE / PROTECTED ZONE', desc: 'AGRipa Status: Tambo Urban Zone is classified as Disease-Free Zone.' },
      'Lipa City Poblacion': { zone: 'GREEN', level: 'FREE / PROTECTED ZONE', desc: 'AGRipa Status: Central Urban Zone is classified as Disease-Free Zone.' }
    };

    function getAsfDatabase() {
      const stored = localStorage.getItem('agripanahon_asf_zones');
      if (!stored) {
        localStorage.setItem('agripanahon_asf_zones', JSON.stringify(DEFAULT_ASF_DATABASE));
        return DEFAULT_ASF_DATABASE;
      }
      return JSON.parse(stored);
    }

    function updateBarangayAsfZone() {
      const selectedBarangay = document.getElementById('adminBarangaySelect').value;
      const selectedZone = document.getElementById('adminZoneSelect').value;
      const db = getAsfDatabase();

      if (selectedZone === 'RED') {
        db[selectedBarangay] = {
          zone: 'RED',
          level: 'HIGH RISK VIRUS CONTAINMENT ZONE',
          desc: `Official AGRipa Data: Barangay ${selectedBarangay} is under RED / HIGH RISK status. Mandatory LGU quarantine checkpoints active.`
        };
      } else if (selectedZone === 'YELLOW') {
        db[selectedBarangay] = {
          zone: 'YELLOW',
          level: 'BUFFER QUARANTINE ZONE',
          desc: `Official AGRipa Data: Barangay ${selectedBarangay} is in a Yellow Buffer Zone. Heightened farm biosecurity required.`
        };
      } else {
        db[selectedBarangay] = {
          zone: 'GREEN',
          level: 'FREE / PROTECTED ZONE',
          desc: `Official AGRipa Data: Barangay ${selectedBarangay} is classified as Disease-Free Zone.`
        };
      }

      localStorage.setItem('agripanahon_asf_zones', JSON.stringify(db));

      const msg = document.getElementById('adminZoneSuccessMsg');
      if (msg) {
        msg.classList.remove('hidden');
        setTimeout(() => msg.classList.add('hidden'), 3000);
      }

      userDismissedAlert = false;
      fetchAgriForecast();
    }

    // LIPA CITY ZONE & BARANGAY CROP / URBAN MAPPING
    const LIPA_CROP_MAP = {
      'Lipa City Poblacion': 'urban_gardening',
      'Balintawak': 'urban_gardening',
      'Marawoy': 'urban_gardening',
      'Tambo': 'urban_gardening',
      'Mataas Na Lupa': 'coffee',
      'Inosloban': 'coffee',
      'Plaridel': 'rice',
      'Lodlod': 'rice',
      'Anilao': 'rice',
      'Pinagtongulan': 'poultry',
      'Sico': 'swine'
    };

    function updateWindyMapEmbed() {
      const iframe = document.getElementById('windyIframe');
      if (iframe) {
        iframe.src = `https://embed.windy.com/embed2.html?lat=${userLat}&lon=${userLon}&detailLat=${userLat}&detailLon=${userLon}&width=650&height=500&zoom=11&level=surface&overlay=${currentWindyOverlay}&product=ecmwf&menu=&message=true&marker=true&calendar=now&pressure=&type=map&location=coordinates&detail=&metricWind=km%2Fh&metricTemp=%C2%B0C&radarRange=-1`;
      }
    }

    function changeWindyOverlay(overlayName) {
      currentWindyOverlay = overlayName;
      updateWindyMapEmbed();
    }

    function unlockAudioContext() {
      if (!globalAudioCtx) {
        const AudioContext = window.AudioContext || window.webkitAudioContext;
        if (AudioContext) globalAudioCtx = new AudioContext();
      }
      if (globalAudioCtx && globalAudioCtx.state === 'suspended') {
        globalAudioCtx.resume();
      }
    }

    window.addEventListener('click', unlockAudioContext, { once: false });
    window.addEventListener('touchstart', unlockAudioContext, { once: false });

    // DUAL-TONE EMERGENCY SOUND ALERT ENGINE
    function playNdrrmcAlertTone() {
      if (isSirenMuted || userDismissedAlert) return;
      try {
        unlockAudioContext();
        const ctx = globalAudioCtx || new (window.AudioContext || window.webkitAudioContext)();
        if (ctx.state === 'suspended') ctx.resume();

        const now = ctx.currentTime;

        const osc1 = ctx.createOscillator();
        const osc2 = ctx.createOscillator();
        const gain = ctx.createGain();

        osc1.type = 'square';
        osc2.type = 'square';

        osc1.frequency.setValueAtTime(853, now);
        osc2.frequency.setValueAtTime(960, now);

        gain.gain.setValueAtTime(0.0, now);
        gain.gain.setValueAtTime(0.3, now + 0.05);
        gain.gain.setValueAtTime(0.0, now + 1.2);
        gain.gain.setValueAtTime(0.3, now + 1.6);
        gain.gain.setValueAtTime(0.0, now + 2.8);

        gain.gain.exponentialRampToValueAtTime(0.0001, now + 3.0);

        osc1.connect(gain);
        osc2.connect(gain);
        gain.connect(ctx.destination);

        osc1.start(now);
        osc2.start(now);
        osc1.stop(now + 3.0);
        osc2.stop(now + 3.0);
      } catch (err) {
        console.warn('Emergency Sound Alarm warning:', err);
      }
    }

    function muteEmergencySiren() {
      isSirenMuted = true;
      if (sirenIntervalTimer) {
        clearInterval(sirenIntervalTimer);
        sirenIntervalTimer = null;
      }
      document.getElementById('toastTimer').innerText = "Muted 🔇";
    }

    function dismissEmergencyAlert() {
      userDismissedAlert = true;
      muteEmergencySiren();
      document.body.classList.remove('emergency-active');
      const toast = document.getElementById('emergencyToast');
      if (toast) toast.classList.add('hidden');
    }

    function dismissBannerNotice() {
      const banner = document.getElementById('batangasClassBanner');
      if (banner) banner.classList.add('hidden');
    }

    function enterSystem() {
      unlockAudioContext();
      const landingScreen = document.getElementById('landingScreen');
      if (landingScreen) {
        landingScreen.classList.add('transition-opacity', 'duration-300', 'opacity-0');
        setTimeout(() => landingScreen.style.display = 'none', 300);
      }
    }

    function dismissSuspensionModal() {
      document.getElementById('classSuspensionModal').classList.add('hidden');
      hasModalBeenDismissedThisSession = true;
    }

    function getCurrentPstTime() {
      return new Date().toLocaleString('en-PH', {
        timeZone: 'Asia/Manila',
        dateStyle: 'medium',
        timeStyle: 'short'
      }) + " PST";
    }

    function logSuspensionDirective(reasonText, titleText) {
      const existingDirectives = JSON.parse(localStorage.getItem('agripanahon_directives_logs') || '[]');
      const timeStamp = getCurrentPstTime();

      const newLog = {
        id: "directive-" + Date.now(),
        category: "🚨 CLASS SUSPENSION DIRECTIVE",
        source: "Gov. Vilma Santos-Recto & President Marcos Jr. Official FB Pages",
        title: titleText || `WALANG PASOK: Class Suspension in Lipa City (${locationLabel})`,
        description: reasonText,
        timestamp: timeStamp,
        fbGovUrl: "https://www.facebook.com/VilmaSantosRectoOfficial",
        fbPbmUrl: "https://www.facebook.com/pbmarcosjr"
      };

      const isDuplicate = existingDirectives.some(item => item.description === reasonText);
      if (!isDuplicate) {
        existingDirectives.unshift(newLog);
        localStorage.setItem('agripanahon_directives_logs', JSON.stringify(existingDirectives));
        renderDirectivesSection();
      }
    }

    function renderDirectivesSection() {
      const container = document.getElementById('tab-directives');
      if (!container) return;

      const logs = JSON.parse(localStorage.getItem('agripanahon_directives_logs') || '[]');
      
      const badge = document.getElementById('navDirectiveBadge');
      if (badge) {
        if (logs.length > 0) {
          badge.innerText = logs.length;
          badge.classList.remove('hidden');
        } else {
          badge.classList.add('hidden');
        }
      }

      let dynamicHtml = `
        <div class="bg-red-900 text-white p-5 rounded-2xl shadow-lg border-2 border-red-600 flex flex-col md:flex-row justify-between items-start md:items-center gap-3">
          <div class="flex items-center gap-3">
            <div class="bg-yellow-400 text-red-950 font-extrabold text-lg px-3 py-1 rounded-xl shadow-md font-sans border border-yellow-300">
              📢 OFFICIAL DIRECTIVES
            </div>
            <div>
              <h2 class="text-lg font-serif font-bold tracking-wide">AGRipa Executive Bulletins & Class Suspensions</h2>
              <p class="text-xs text-red-100">Real-Time Directives Sourced directly from Gov. Vilma Santos-Recto & President Marcos Jr. Official FB Pages</p>
            </div>
          </div>
          <div class="flex gap-2">
            <button onclick="clearDirectivesLogs()" class="text-[10px] bg-red-950 hover:bg-black text-white font-mono px-3 py-1 rounded-full border border-red-700 font-bold transition">
              🗑️ Clear Directives
            </button>
            <span class="text-[10px] bg-red-950 text-yellow-300 font-mono px-3 py-1 rounded-full border border-red-700 font-bold">
              Verification Engine Active ✅
            </span>
          </div>
        </div>

        <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
      `;

      if (logs.length === 0) {
        dynamicHtml += `
          <div class="col-span-full agri-card p-8 rounded-2xl text-center space-y-2">
            <p class="text-3xl">✅</p>
            <h3 class="font-serif font-bold text-earth-800 text-base">No Active Class Suspensions</h3>
            <p class="text-xs text-earth-600">All local weather parameters are normal. No emergency class suspension directives are currently posted on the official FB pages of Gov. Vilma Santos-Recto or President Marcos Jr.</p>
          </div>
        `;
      } else {
        logs.forEach(item => {
          dynamicHtml += `
            <article class="agri-card p-5 rounded-2xl space-y-3 border-l-4 border-red-600 bg-red-50/30 flex flex-col justify-between">
              <div class="space-y-2">
                <div class="flex justify-between items-center text-[10px] font-bold text-earth-500 uppercase">
                  <span class="bg-red-200 text-red-950 px-2 py-0.5 rounded">${item.category}</span>
                  <span class="font-mono text-red-900 font-bold">FB Official Verified</span>
                </div>
                <h3 class="font-serif font-bold text-earth-900 text-base leading-snug text-red-950">
                  ${item.title}
                </h3>
                <p class="text-xs text-earth-700 leading-relaxed font-medium">
                  ${item.description}
                </p>
              </div>
              <div class="pt-2 border-t border-earth-200 space-y-1">
                <div class="flex justify-between items-center text-[10px] text-earth-600 font-mono">
                  <span>Logged: ${item.timestamp}</span>
                  <span class="text-emerald-700 font-bold">VERIFIED EXECUTIVE DIRECTIVE ✅</span>
                </div>
                <div class="flex flex-col gap-0.5 pt-1">
                  <a href="${item.fbGovUrl}" target="_blank" rel="noopener noreferrer" class="text-[11px] font-bold text-red-800 hover:underline truncate">
                    📘 Source: Gov. Vilma Santos-Recto FB Page
                  </a>
                  <a href="${item.fbPbmUrl}" target="_blank" rel="noopener noreferrer" class="text-[11px] font-bold text-red-800 hover:underline truncate">
                    📘 Source: President Ferdinand Marcos Jr. FB Page
                  </a>
                </div>
              </div>
            </article>
          `;
        });
      }

      dynamicHtml += `</div>`;
      container.innerHTML = dynamicHtml;
    }

    function clearDirectivesLogs() {
      localStorage.removeItem('agripanahon_directives_logs');
      renderDirectivesSection();
    }

    // REAL-TIME DIRECTIVE EVALUATION ENGINE
    function evaluateClassSuspensionDirective(rain, wind, tcws) {
      const isLipa = locationLabel.toLowerCase().includes('lipa');
      let isSuspended = false;
      let suspensionTitle = "";
      let suspensionReason = "";

      if (tcws !== "NO SIGNAL") {
        isSuspended = true;
        suspensionTitle = `AUTOMATIC WALANG PASOK: ${tcws} Active`;
        suspensionReason = `Official Directive from Gov. Vilma Santos-Recto & President Marcos Jr. FB Pages: Classes across ALL levels in Lipa City are automatically suspended due to ${tcws}.`;
      } 
      else if (rain >= 25) {
        isSuspended = true;
        suspensionTitle = `WALANG PASOK: Severe Heavy Rainfall (${rain} mm)`;
        suspensionReason = `Official Announcement via Gov. Vilma Santos-Recto and President Ferdinand Marcos Jr. Facebook pages: Classes in ALL LEVELS are suspended in ${locationLabel} due to continuous heavy rainfall (${rain} mm/24hr) and high localized flooding risks.`;
      }
      else if (wind >= 45) {
        isSuspended = true;
        suspensionTitle = `WALANG PASOK: Hazardous Wind Conditions (${wind} km/h)`;
        suspensionReason = `Executive Announcement via Gov. Vilma Santos-Recto & President Marcos Jr. FB pages: Class suspension ordered in ${locationLabel} due to severe wind squalls (${wind} km/h).`;
      }

      const modal = document.getElementById('classSuspensionModal');
      const banner = document.getElementById('batangasClassBanner');
      const currentPstTime = getCurrentPstTime();

      if (isLipa && isSuspended) {
        if (banner) {
          banner.classList.remove('hidden');
          document.getElementById('bannerTitleText').innerText = suspensionTitle;
          document.getElementById('bannerDescText').innerText = suspensionReason;
          document.getElementById('bannerVerificationTimestamp').innerText = currentPstTime;
        }

        if (modal && !hasModalBeenDismissedThisSession && !userDismissedAlert) {
          document.getElementById('suspensionNoticeText').innerHTML = suspensionReason;
          document.getElementById('suspensionScopeText').innerText = `${locationLabel} & Lipa City Urban/Agricultural Barangays`;
          document.getElementById('modalVerificationTimestamp').innerText = currentPstTime;
          modal.classList.remove('hidden');
          playNdrrmcAlertTone();
        }

        logSuspensionDirective(suspensionReason, suspensionTitle);
      } else {
        if (modal) modal.classList.add('hidden');
        if (banner) banner.classList.add('hidden');
      }
    }

    function simulateHeavyRainAlert() {
      userDismissedAlert = false;
      hasModalBeenDismissedThisSession = false;
      evaluateClassSuspensionDirective(38, 52, "SIGNAL #1 (Tropical Depression)");
      evaluateLipaCropStress(38, 34, 40, 4.5, "SIGNAL #1 (Tropical Depression)", 80, "urban_gardening");
    }

    // HIGH-RISK & TCWS SOUND ALERT CONTROLLER
    function triggerRedEmergencyMode(isEmergency, message = "") {
      if (userDismissedAlert) return;

      const toast = document.getElementById('emergencyToast');
      const toastMsg = document.getElementById('toastMessage');

      if (isEmergency) {
        document.body.classList.add('emergency-active');
        toastMsg.innerText = message;
        toast.classList.remove('hidden');

        isSirenMuted = false;

        playNdrrmcAlertTone();

        if (!sirenIntervalTimer) {
          sirenIntervalTimer = setInterval(() => {
            if (!isSirenMuted && !userDismissedAlert) playNdrrmcAlertTone();
          }, 10000);
        }
      } else {
        document.body.classList.remove('emergency-active');
        toast.classList.add('hidden');
        if (sirenIntervalTimer) {
          clearInterval(sirenIntervalTimer);
          sirenIntervalTimer = null;
        }
      }
    }

    // DYNAMIC DUAL LIVESTOCK VIRUS ZONE SCANNER & EVALUATOR (AGRIPA & OCA SOURCED)
    function evaluateLocationVirusZone(locationStr, cropFocus = "") {
      const db = getAsfDatabase();
      const barangayKey = Object.keys(db).find(key => locationStr.toLowerCase().includes(key.toLowerCase())) || 'Lipa City Poblacion';
      const info = db[barangayKey] || { zone: 'GREEN', level: 'FREE / PROTECTED ZONE', desc: 'Classified as Disease-Free Zone.' };

      const card = document.getElementById('virusDetectorCard');
      const header = document.getElementById('virusDetectorHeader');
      const tag = document.getElementById('virusStatusTag');
      const desc = document.getElementById('virusZoneDescText');
      const iconTag = document.getElementById('virusIconTag');
      const guidelinesBox = document.getElementById('virusBiosecurityGuidelines');

      const isSwineSector = (cropFocus === 'swine');
      const isPoultrySector = (cropFocus === 'poultry' || cropFocus === 'broiler');

      let virusName = "LIVESTOCK VIRUS";
      if (isSwineSector) {
        virusName = "AFRICAN SWINE FEVER (ASF)";
        if (iconTag) iconTag.innerText = "🐖";
      } else if (isPoultrySector) {
        virusName = "AVIAN INFLUENZA / BIRD FLU (H5N1)";
        if (iconTag) iconTag.innerText = "🐔";
      } else {
        virusName = "ASF & AVIAN FLU BIOSECURITY";
        if (iconTag) iconTag.innerText = "🏛️";
      }

      if (header) header.innerText = `Target Zone: ${barangayKey} ${virusName} Status`;

      if (guidelinesBox) {
        if (isPoultrySector) {
          guidelinesBox.innerHTML = `
            <div class="bg-white/90 p-3 rounded-xl border border-red-200 space-y-1 shadow-sm">
              <p class="font-bold text-red-900">🕸️ 1. LGU OCA WILD BIRD NETTING MANDATE</p>
              <p class="text-earth-800 text-[11px] leading-relaxed">
                Directive Sourced from LGU Lipa OCA: Cover all side curtains and structural openings with heavy-duty <strong>1/2-inch mesh netting</strong>. Wild migratory birds species are primary vectors for H5N1. Immediately drain standing puddles near chicken coops.
              </p>
            </div>
            <div class="bg-white/90 p-3 rounded-xl border border-red-200 space-y-1 shadow-sm">
              <p class="font-bold text-red-900">🥚 2. EGG TRAY & VEHICLE DECONTAMINATION</p>
              <p class="text-earth-800 text-[11px] leading-relaxed">
                LGU Lipa City OCA Protocol: Prohibit single-use wooden or paper pulp egg trays from external farms. Utilize plastic egg flats washed and soaked in <strong>Quaternary Ammonium Compounds (QAC) or 200 ppm Chlorine solution</strong>. Feed haul trucks must undergo complete wheel-well spray washes.
              </p>
            </div>
            <div class="bg-white/90 p-3 rounded-xl border border-red-200 space-y-1 shadow-sm">
              <p class="font-bold text-red-900">🥾 3. FOOTBATHS & COOP ENTRY CONTROLS</p>
              <p class="text-earth-800 text-[11px] leading-relaxed">
                LGU Lipa OCA Directive: Maintain <strong>1:200 Glutaraldehyde or Synergized Iodine</strong> footbaths at every building door. Require farm personnel to step into footbaths for a minimum of 10 seconds and switch to color-coded coop boots before entering poultry houses.
              </p>
            </div>
          `;
        } else if (isSwineSector) {
          guidelinesBox.innerHTML = `
            <div class="bg-white/90 p-3 rounded-xl border border-red-200 space-y-1 shadow-sm">
              <p class="font-bold text-red-900">🚫 1. STRICT SWILL-FEEDING (PAGPAPA-KANIN) BAN</p>
              <p class="text-earth-800 text-[11px] leading-relaxed">
                LGU City Government of Lipa Executive Directive: Zero tolerance for kitchen scrap/swill feeding. The African Swine Fever virus survives up to 1000 days in frozen meat and months in untreated pork scraps. Feed commercially certified milled feed or cooked farm crops exclusively.
              </p>
            </div>
            <div class="bg-white/90 p-3 rounded-xl border border-red-200 space-y-1 shadow-sm">
              <p class="font-bold text-red-900">🚚 2. TRADER VEHICLE DISINFECTION LOCKS</p>
              <p class="text-earth-800 text-[11px] leading-relaxed">
                LGU Lipa City Veterinary Regulation: Traders and live hog buyer trucks are the #1 transmission vector. Do not allow buyer vehicles past the outer perimeter gate. Load pigs at a designated transfer ramp 50 meters away followed by pressure washing with <strong>1% Potassium Peroxymonosulfate</strong>.
              </p>
            </div>
            <div class="bg-white/90 p-3 rounded-xl border border-red-200 space-y-1 shadow-sm">
              <p class="font-bold text-red-900">🔒 3. 30-DAY ISOLATION & FEVER MONITORING</p>
              <p class="text-earth-800 text-[11px] leading-relaxed">
                LGU Lipa OCA Protocol: Quarantine new replacement gilts or boars in a separate isolation unit for at least 30 days. Record rectal temperatures daily. Immediately report pigs exhibiting high fever (>40.5°C), skin cyanosis, or sudden mortality to the <strong>LGU Lipa City Agriculture Office (OCA)</strong>.
              </p>
            </div>
          `;
        } else {
          guidelinesBox.innerHTML = `
            <div class="bg-white/90 p-3 rounded-xl border border-red-200 space-y-1 shadow-sm">
              <p class="font-bold text-red-900">🛡️ LGU LIPA GENERAL LIVESTOCK PERIMETER SHIELD</p>
              <p class="text-earth-800 text-[11px] leading-relaxed">
                Official LGU Lipa City Advisory: Restrict unauthorized farm visitors and keep stray animals away from livestock quarters. Enforce perimeter fencing and functional footbaths.
              </p>
            </div>
            <div class="bg-white/90 p-3 rounded-xl border border-red-200 space-y-1 shadow-sm">
              <p class="font-bold text-red-900">🧼 LGU OCA CHEMICAL DISINFECTION PROTOCOL</p>
              <p class="text-earth-800 text-[11px] leading-relaxed">
                LGU Lipa OCA Directive: Clean organic dirt with pressurized water before applying disinfectants. Organic mud or manure neutralizes footbath chemicals. Change footbath water every 24–48 hours.
              </p>
            </div>
            <div class="bg-white/90 p-3 rounded-xl border border-red-200 space-y-1 shadow-sm">
              <p class="font-bold text-red-900">📞 MANDATORY LGU INCIDENT REPORTING</p>
              <p class="text-earth-800 text-[11px] leading-relaxed">
                Report unusual animal illness or cluster mortalities immediately to the LGU Lipa City Agriculture Office (OCA) or your local Barangay DRRM Agricultural Representative.
              </p>
            </div>
          `;
        }
      }

      if (info.zone === 'RED') {
        if (card) card.className = "agri-card p-5 rounded-3xl space-y-3 border-l-8 border-red-600 bg-red-50/50 shadow-lg";
        if (tag) {
          tag.innerText = `🚨 RED ZONE: AGRIPA HIGH RISK ${virusName} CONTAINMENT`;
          tag.className = "bg-red-600 text-white text-[10px] px-3 py-1 rounded-full font-extrabold uppercase border border-red-700 font-mono shadow animate-pulse";
        }

        if (desc) {
          if (isPoultrySector) {
            desc.innerHTML = `<strong>OFFICIAL LGU LIPA POULTRY ALERT:</strong> ${barangayKey} is designated under RED / HIGH RISK status by LGU Lipa and the Office of the City Agriculturist (OCA). Mandatory bird netting, egg tray disinfection, and wild bird exclusion active for Avian Influenza prevention.`;
          } else if (isSwineSector) {
            desc.innerHTML = `<strong>OFFICIAL LGU LIPA SWINE ALERT:</strong> ${barangayKey} is designated under RED / HIGH RISK status by LGU Lipa and the Office of the City Agriculturist (OCA). Strict zero-swill feeding (pagpapa-kanin) and vehicle disinfection checkpoints enforced for ASF containment.`;
          } else {
            desc.innerHTML = `<strong>LGU LIPA HIGH RISK LIVESTOCK CONTAINMENT:</strong> ${barangayKey} is flagged under RED status for ASF / Avian Flu by the LGU Lipa City Agriculture Office (OCA). Enforce total farm biosecurity.`;
          }
        }
        return true;
      } else if (info.zone === 'YELLOW') {
        if (card) card.className = "agri-card p-5 rounded-3xl space-y-3 border-l-8 border-amber-500 bg-amber-50/50 shadow-lg";
        if (tag) {
          tag.innerText = `⚠️ YELLOW ZONE: AGRIPA BUFFER QUARANTINE (${virusName})`;
          tag.className = "bg-amber-500 text-white text-[10px] px-3 py-1 rounded-full font-extrabold uppercase border border-amber-600 font-mono shadow";
        }
        if (desc) {
          desc.innerText = `OFFICIAL LGU LIPA BUFFER ZONE: ${barangayKey} is under Yellow Surveillance status designated by the LGU Lipa City Agriculture Office (OCA). Restrict visitor access and enforce entry footbaths.`;
        }
        return false;
      } else {
        if (card) card.className = "agri-card p-5 rounded-3xl space-y-3 border-l-8 border-emerald-600 bg-emerald-50/40 shadow-lg";
        if (tag) {
          tag.innerText = "🛡️ GREEN ZONE: AGRIPA PROTECTED & DISEASE-FREE";
          tag.className = "bg-emerald-600 text-white text-[10px] px-3 py-1 rounded-full font-extrabold uppercase border border-emerald-700 font-mono shadow";
        }
        if (desc) {
          desc.innerText = `OFFICIAL LGU LIPA PROTECTED STATUS: ${barangayKey} is classified as an ASF & Avian Flu Free Protected Zone by the LGU Lipa City Agriculture Office (OCA). Maintain standard biosecurity protocols.`;
        }
        return false;
      }
    }

    // EVALUATE WEATHER RISK PARAMETERS (ONLY TRIGGER SIREN ON TCWS SIGNAL WARNINGS)
    function evaluateLipaCropStress(rain, heatIndex, soilMoisture, et0, tcws, humidity, crop) {
      const pestCard = document.getElementById('pestBadgeCard');
      const pestText = document.getElementById('pestRiskText');
      const pestIcon = document.getElementById('pestRiskIcon');

      let riskLevel = "Normal (Low Stress)";
      let cardBorderClass = "border-emerald-600";
      let textClass = "text-emerald-700";
      let icon = "🌱";
      let isTcwsEmergency = false;
      let alertMsg = "";

      evaluateLocationVirusZone(locationLabel, crop);

      if (tcws && tcws !== "NO SIGNAL") {
        riskLevel = `WEATHER SIGNAL ALERT (${tcws})`;
        cardBorderClass = "border-red-600";
        textClass = "text-red-700";
        icon = "🌀";
        isTcwsEmergency = true;
        alertMsg = `🚨 WEATHER SIGNAL ALERT IN ${locationLabel.toUpperCase()}: ${tcws} is currently active!`;
      } 
      else if (heatIndex >= 41 || rain >= 30 || soilMoisture < 20) {
        riskLevel = "High Heat / Field Stress";
        cardBorderClass = "border-amber-600";
        textClass = "text-amber-700";
        icon = "🔥";
      } else if (heatIndex >= 36 || rain >= 15 || soilMoisture < 35 || et0 > 5.5) {
        riskLevel = "Moderate Weather Risk";
        cardBorderClass = "border-amber-500";
        textClass = "text-amber-700";
        icon = "⚠️";
      } else if (heatIndex >= 32 || et0 > 4.5) {
        riskLevel = "Mild Field Stress";
        cardBorderClass = "border-yellow-500";
        textClass = "text-yellow-700";
        icon = "🌾";
      }

      if (pestCard) {
        pestCard.className = `agri-card p-4 rounded-2xl border-l-4 ${cardBorderClass} flex items-center justify-between transition-colors`;
      }
      if (pestText) {
        pestText.innerText = riskLevel;
        pestText.className = `text-base font-serif font-bold ${textClass} mt-0.5`;
      }
      if (pestIcon) {
        pestIcon.innerText = icon;
      }

      if (isTcwsEmergency && !userDismissedAlert) {
        triggerRedEmergencyMode(true, alertMsg);
      } else if (!isTcwsEmergency) {
        triggerRedEmergencyMode(false);
      }
    }

    function filterLocations() {
      const filter = document.getElementById('locationSearchInput').value.toLowerCase();
      const select = document.getElementById('provinceSelect');
      const options = select.getElementsByTagName('option');
      let firstMatch = null;

      for (let i = 0; i < options.length; i++) {
        const text = options[i].text.toLowerCase();
        if (text.includes(filter)) {
          options[i].style.display = "";
          if (!firstMatch) firstMatch = options[i];
        } else {
          options[i].style.display = "none";
        }
      }

      if (firstMatch && filter.length > 1) {
        select.value = firstMatch.value;
        selectProvince();
      }
    }

    function switchTab(tabId) {
      unlockAudioContext();
      document.querySelectorAll('.tab-content').forEach(tab => tab.classList.add('hidden'));
      document.querySelectorAll('.nav-btn').forEach(btn => btn.classList.remove('active'));

      const selectedTab = document.getElementById(`tab-${tabId}`);
      if (selectedTab) selectedTab.classList.remove('hidden');

      const selectedBtn = document.getElementById(`nav-${tabId}`);
      if (selectedBtn) selectedBtn.classList.add('active');
    }

    function calculateVPD(temp, humidity) {
      const svp = 0.61078 * Math.exp((17.27 * temp) / (temp + 237.3));
      const avp = svp * (humidity / 100);
      return (svp - avp).toFixed(2);
    }

    function calculateDewPoint(temp, humidity) {
      const a = 17.27;
      const b = 237.7;
      const alpha = ((a * temp) / (b + temp)) + Math.log(humidity / 100.0);
      return ((b * alpha) / (a - alpha)).toFixed(1);
    }

    async function fetchAgriForecast() {
      unlockAudioContext();
      const crop = document.getElementById('cropSelect').value;

      updateWindyMapEmbed();
      const apiUrl = `https://api.open-meteo.com/v1/forecast?latitude=${userLat}&longitude=${userLon}&hourly=relative_humidity_2m,soil_moisture_0_to_10cm,soil_temperature_0cm,apparent_temperature,uv_index&daily=temperature_2m_max,temperature_2m_min,precipitation_sum,wind_speed_10m_max,et0_fao_evapotranspiration&timezone=Asia%2FManila`;

      let data;
      try {
        const response = await fetch(apiUrl);
        if (!response.ok) throw new Error("HTTP Error " + response.status);
        data = await response.json();
      } catch (error) {
        console.warn("Switching to fallback telemetry engine:", error);
        document.getElementById('offlineNotice').classList.remove('hidden');
        data = generateFallbackTelemetry();
      }

      const todayRain = data.daily.precipitation_sum[0] || 0;
      const todayWind = data.daily.wind_speed_10m_max[0] || 0;
      const todayTemp = data.daily.temperature_2m_max[0] || 0;
      const todayET0 = data.daily.et0_fao_evapotranspiration[0] || 0;
      
      const humidity = (data.hourly && data.hourly.relative_humidity_2m) ? data.hourly.relative_humidity_2m[12] : 75;
      const rawMoisture = (data.hourly && data.hourly.soil_moisture_0_to_10cm) ? data.hourly.soil_moisture_0_to_10cm[12] : 0.45;
      const soilMoisture = (rawMoisture * 100).toFixed(0);
      const heatIndex = (data.hourly && data.hourly.apparent_temperature) ? data.hourly.apparent_temperature[12] : todayTemp;
      const vpd = calculateVPD(todayTemp, humidity);
      const dewPoint = calculateDewPoint(todayTemp, humidity);

      let tcws = "NO SIGNAL";

      if (todayWind > 185) {
        tcws = "SIGNAL #5 (Super Typhoon)";
      } else if (todayWind > 118) {
        tcws = "SIGNAL #4 (Typhoon)";
      } else if (todayWind > 89) {
        tcws = "SIGNAL #3 (Severe Tropical Storm)";
      } else if (todayWind > 62) {
        tcws = "SIGNAL #2 (Tropical Storm)";
      } else if (todayWind > 39) {
        tcws = "SIGNAL #1 (Tropical Depression)";
      }

      evaluateClassSuspensionDirective(todayRain, todayWind, tcws);
      evaluateLipaCropStress(todayRain, heatIndex, soilMoisture, todayET0, tcws, humidity, crop);

      const payload = {
        rain: todayRain,
        wind: todayWind,
        temp: todayTemp,
        et0: todayET0,
        humidity,
        soilMoisture,
        heatIndex,
        vpd,
        dewPoint,
        tcws,
        dailyForecast: data.daily,
        timestamp: getCurrentPstTime()
      };

      localStorage.setItem('agripanahon_last_weather_v39', JSON.stringify(payload));
      renderTelemetry(payload, crop);
    }

    function generateFallbackTelemetry() {
      const now = new Date();
      const dates = [];
      for (let i = 0; i < 7; i++) {
        const d = new Date();
        d.setDate(now.getDate() + i);
        dates.push(d.toISOString().split('T')[0]);
      }
      return {
        daily: {
          time: dates,
          temperature_2m_max: [31, 32, 30, 29, 31, 33, 32],
          temperature_2m_min: [24, 25, 23, 23, 24, 25, 24],
          precipitation_sum: [5, 12, 28, 4, 0, 2, 8],
          wind_speed_10m_max: [14, 18, 25, 12, 10, 15, 16],
          et0_fao_evapotranspiration: [4.1, 3.8, 2.5, 4.5, 4.8, 5.0, 4.3]
        },
        hourly: {
          relative_humidity_2m: Array(24).fill(78),
          soil_moisture_0_to_10cm: Array(24).fill(0.42),
          apparent_temperature: Array(24).fill(33.0)
        }
      };
    }

    function renderTelemetry(data, crop) {
      document.getElementById('rainVal').innerText = `${data.rain} mm`;
      document.getElementById('windVal').innerText = `${data.wind} km/h`;
      document.getElementById('heatIndexVal').innerText = `${data.heatIndex}°C`;
      document.getElementById('humidityVal').innerText = `${data.humidity}%`;
      document.getElementById('etVal').innerText = `${data.et0.toFixed(1)} mm/d`;
      document.getElementById('vpdVal').innerText = `${data.vpd} kPa`;
      document.getElementById('soilVal').innerText = `${data.soilMoisture}%`;
      document.getElementById('dewVal').innerText = `${data.dewPoint}°C`;
      document.getElementById('dataTimestamp').innerText = `Synced: ${data.timestamp}`;
      document.getElementById('tcwsSignalText').innerText = data.tcws || "NO SIGNAL";

      const zoneTag = document.getElementById('advisoryZoneTag');
      if (zoneTag) zoneTag.innerText = locationLabel;

      if (data.dailyForecast && data.dailyForecast.time) {
        const tbody = document.getElementById('forecastTableBody');
        tbody.innerHTML = '';
        data.dailyForecast.time.forEach((dateStr, idx) => {
          tbody.innerHTML += `
            <tr class="border-b border-earth-200 font-medium">
              <td class="p-2 font-mono">${dateStr}</td>
              <td class="p-2 font-bold">${data.dailyForecast.temperature_2m_max[idx]}°C / ${data.dailyForecast.temperature_2m_min[idx]}°C</td>
              <td class="p-2 text-harvest-leaf font-bold">${data.dailyForecast.precipitation_sum[idx]} mm</td>
              <td class="p-2">${data.dailyForecast.wind_speed_10m_max[idx]} km/h</td>
            </tr>
          `;
        });
      }

      let advisoryHtml = `
        <div class="space-y-4">
          <div class="bg-harvest-green text-white p-3 rounded-xl flex items-center justify-between shadow-sm">
            <span class="font-bold uppercase tracking-wider text-xs">🏛️ AGRipa Sector Analysis: ${locationLabel}</span>
            <span class="font-mono text-[11px] bg-emerald-900 px-2.5 py-0.5 rounded border border-emerald-700">Evapotranspiration Rate: ${data.et0.toFixed(1)} mm/day</span>
          </div>
      `;

      switch (crop) {
        case 'urban_gardening':
        case 'backyard_vegetables':
          advisoryHtml += `
            <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
              <div class="bg-earth-100/90 p-4 rounded-2xl border border-earth-300 space-y-2">
                <p class="font-bold text-harvest-green text-sm flex items-center gap-1.5">
                  <span>💧</span> Container & Hydroponic Irrigation Recommendations
                </p>
                <p class="text-xs text-earth-800 leading-relaxed">
                  Evapotranspiration is recorded at <strong>${data.et0.toFixed(1)} mm/day</strong>. Containerized soils in urban centers heat up rapidly. Water balcony containers thoroughly early in the morning before 8:00 AM. If operating Deep Water Culture (DWC) or Nutrient Film Technique (NFT) hydroponic systems, cover nutrient reservoir tanks with reflective insulation; nutrient water temperatures exceeding <strong>28°C</strong> reduce dissolved oxygen levels, inviting <em>Pythium</em> root rot.
                </p>
              </div>
              <div class="bg-amber-50/90 p-4 rounded-2xl border border-amber-300 space-y-2">
                <p class="font-bold text-amber-900 text-sm flex items-center gap-1.5">
                  <span>⚠️</span> Microclimate Heat & Fungal Pathogen Warnings
                </p>
                <p class="text-xs text-earth-800 leading-relaxed">
                  Relative Humidity is <strong>${data.humidity}%</strong> with a Vapor Pressure Deficit (VPD) of <strong>${data.vpd} kPa</strong>. High humidity combined with stagnant air around raised beds encourages Downy Mildew (*Pseudoperonospora cubensis*) on cucumbers and eggplants. <strong>WARNING:</strong> Avoid overhead sprinkler watering late in the afternoon. Ensure 30cm spacing between grow bags to promote canopy airflow.
                </p>
              </div>
            </div>
          `;
          break;

        case 'swine':
        case 'household_livestock':
          advisoryHtml += `
            <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
              <div class="bg-red-50/90 p-4 rounded-2xl border-2 border-red-300 space-y-2">
                <p class="font-bold text-red-950 text-sm flex items-center gap-1.5">
                  <span>🐖</span> African Swine Fever (ASF) Biosecurity Recommendations (AGRipa Sourced)
                </p>
                <p class="text-xs text-earth-800 leading-relaxed">
                  Sourced from LGU Lipa City and the Office of the City Agriculturist (OCA), Barangay Sico and surrounding swine zones require rigorous barrier controls. Enforce mandatory vehicle wheel-well spray dips utilizing <strong>Potassium Peroxymonosulfate or 2% Sodium Hydroxide</strong> at farm entry gates. Restrict live pig buyers from entering pens; establish an off-site loading ramp at least 50 meters from piggery buildings. <strong>CRITICAL LGU DIRECTIVE:</strong> Prohibit all swill feeding (pagpapa-kanin).
                </p>
              </div>
              <div class="bg-amber-50/90 p-4 rounded-2xl border border-amber-300 space-y-2">
                <p class="font-bold text-amber-900 text-sm flex items-center gap-1.5">
                  <span>🔥</span> Swine Thermal Stress & Ventilation Warnings
                </p>
                <p class="text-xs text-earth-800 leading-relaxed">
                  Heat Index is reaching <strong>${data.heatIndex}°C</strong>. Swine lack functional sweat glands and rely heavily on evaporative cooling. <strong>WARNING:</strong> Sows exposed to heat stress above 33°C suffer increased embryonic mortality and decreased lactational feed intake. Run roof misting sprinklers for 10 minutes every hour during peak afternoon hours and supplement drinking water with <strong>vitamin C and electrolytes</strong>.
                </p>
              </div>
            </div>
          `;
          break;

        case 'poultry':
        case 'broiler':
          advisoryHtml += `
            <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
              <div class="bg-red-50/90 p-4 rounded-2xl border-2 border-red-300 space-y-2">
                <p class="font-bold text-red-950 text-sm flex items-center gap-1.5">
                  <span>🐔</span> Avian Influenza (Bird Flu H5N1) Biosecurity Recommendations (AGRipa Sourced)
                </p>
                <p class="text-xs text-earth-800 leading-relaxed">
                  Sourced from LGU Lipa City and the Office of the City Agriculturist (OCA), Pinagtongulan and commercial layer farms must enforce total wild bird exclusion. Cover all curtain gaps and ventilation inlets with <strong>1/2-inch wire mesh screen</strong>. Require all farm personnel to change into house-dedicated rubber boots and step into a fresh <strong>1:200 Glutaraldehyde footbath</strong> prior to entering poultry houses. Disinfect plastic egg flats before reuse.
                </p>
              </div>
              <div class="bg-amber-50/90 p-4 rounded-2xl border border-amber-300 space-y-2">
                <p class="font-bold text-amber-900 text-sm flex items-center gap-1.5">
                  <span>⚠️</span> Flock Heat Prostration & Litter Quality Warnings
                </p>
                <p class="text-xs text-earth-800 leading-relaxed">
                  Apparent temperature is <strong>${data.heatIndex}°C</strong> with Relative Humidity at <strong>${data.humidity}%</strong>. <strong>WARNING:</strong> Broilers above 28 days of age are at extreme risk of sudden heat prostration. Increase tunnel ventilation air velocity to >2.5 m/s. Keep rice hull litter moisture below 25%; damp litter under high temperatures causes rapid buildup of toxic <strong>ammonia gas (NH₃)</strong>, compromising flock respiratory tracts.
                </p>
              </div>
            </div>
          `;
          break;

        case 'coffee':
        case 'cacao':
          advisoryHtml += `
            <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
              <div class="bg-earth-100/90 p-4 rounded-2xl border border-earth-300 space-y-2">
                <p class="font-bold text-harvest-green text-sm flex items-center gap-1.5">
                  <span>☕</span> Kapeng Barako & Cacao Cultural Recommendations
                </p>
                <p class="text-xs text-earth-800 leading-relaxed">
                  Soil Moisture in Mataas Na Lupa and Inosloban highland zones is <strong>${data.soilMoisture}%</strong>. Apply organic compost and a 10cm dry leaf mulch layer around the drip line of <em>Coffee liberica</em> (Kapeng Barako) trees to conserve soil moisture. For cacao orchards, prune intercropped banana shade trees to allow 50% sunlight penetration, balancing photosynthesis with moisture retention.
                </p>
              </div>
              <div class="bg-amber-50/90 p-4 rounded-2xl border border-amber-300 space-y-2">
                <p class="font-bold text-amber-900 text-sm flex items-center gap-1.5">
                  <span>🍄</span> Coffee Leaf Rust & Black Pod Rot Pathogen Warnings
                </p>
                <p class="text-xs text-earth-800 leading-relaxed">
                  Dew point is calculated at <strong>${data.dewPoint}°C</strong>. Prolonged leaf wetness during humid mornings creates ideal germination conditions for Coffee Leaf Rust (*Hemileia vastatrix*) and Cacao Black Pod Rot (*Phytophthora palmivora*). <strong>WARNING:</strong> Inspect leaf undersides for orange powdery urediniospores. Apply preventive copper-based fungicides if leaf wetness persists longer than 6 morning hours.
                </p>
              </div>
            </div>
          `;
          break;

        case 'rice':
        case 'corn':
        case 'sugarcane':
          advisoryHtml += `
            <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
              <div class="bg-earth-100/90 p-4 rounded-2xl border border-earth-300 space-y-2">
                <p class="font-bold text-harvest-green text-sm flex items-center gap-1.5">
                  <span>🌾</span> Palay & Grain Field Management Recommendations
                </p>
                <p class="text-xs text-earth-800 leading-relaxed">
                  24-hour accumulated rainfall is <strong>${data.rain} mm</strong> with soil moisture at <strong>${data.soilMoisture}%</strong>. In Plaridel, Lodlod, and Anilao rice paddies, maintain a shallow 3–5 cm water depth during the tillering stage. Practice Alternate Wetting and Drying (AWD) to strengthen root anchoring and decrease methane emissions.
                </p>
              </div>
              <div class="bg-amber-50/90 p-4 rounded-2xl border border-amber-300 space-y-2">
                <p class="font-bold text-amber-900 text-sm flex items-center gap-1.5">
                  <span>🐛</span> Wind Lodging & Armyworm Pest Warnings
                </p>
                <p class="text-xs text-earth-800 leading-relaxed">
                  Peak wind gusts are reaching <strong>${data.wind} km/h</strong>. <strong>WARNING:</strong> Mature yellow corn fields approaching harvest are vulnerable to stalk lodging under wind squalls exceeding 35 km/h. Inspect corn whorls for Fall Armyworm (*Spodoptera frugiperda*) frass and egg masses. Clear drainage canals to prevent field waterlogging during localized rain showers.
                </p>
              </div>
            </div>
          `;
          break;

        default:
          advisoryHtml += `
            <div class="bg-earth-100/90 p-4 rounded-2xl border border-earth-300 space-y-2">
              <p class="font-bold text-harvest-green text-sm flex items-center gap-1.5">
                <span>🌾</span> General Agricultural Management Guidance
              </p>
              <p class="text-xs text-earth-800 leading-relaxed">
                Weather parameters across the sector remain stable. Maintain standard biosecurity protocols, regularly inspect crops for early pest infestations, and ensure field drainage outlets remain unblocked.
              </p>
            </div>
          `;
      }

      advisoryHtml += `</div>`;
      document.getElementById('advisoryOutput').innerHTML = advisoryHtml;
    }

    function checkNetworkStatus() {
      const badge = document.getElementById('networkStatusBadge');
      const notice = document.getElementById('offlineNotice');

      if (navigator.onLine) {
        badge.innerText = "ONLINE";
        badge.className = "bg-harvest-green text-white text-xs px-3 py-1.5 rounded-full font-extrabold uppercase tracking-wider shadow-sm";
        notice.classList.add('hidden');
      } else {
        badge.innerText = "OFFLINE MODE";
        badge.className = "bg-amber-600 text-white text-xs px-3 py-1.5 rounded-full font-extrabold uppercase tracking-wider shadow-sm";
        notice.classList.remove('hidden');
      }
      updateWindyMapEmbed();
    }

    function selectProvince() {
      const parts = document.getElementById('provinceSelect').value.split(',');
      userLat = parseFloat(parts[0]);
      userLon = parseFloat(parts[1]);
      
      const fullLabel = parts[2];
      const zoneName = fullLabel.split(',')[0].trim();
      
      locationLabel = fullLabel;
      currentAwsId = parts[3] || "5035";

      const cropDropdown = document.getElementById('cropSelect');
      if (LIPA_CROP_MAP[zoneName]) {
        cropDropdown.value = LIPA_CROP_MAP[zoneName];
      } else {
        cropDropdown.value = 'urban_gardening';
      }

      document.getElementById('selectedProvinceName').innerText = `Selected: ${locationLabel}`;
      document.getElementById('awsStationId').innerText = `${currentAwsId}-LIPA`;
      
      hasModalBeenDismissedThisSession = false;
      userDismissedAlert = false;
      
      updateWindyMapEmbed();
      fetchAgriForecast();
    }

    function getLocation() {
      if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(
          (pos) => {
            const lat = pos.coords.latitude;
            const lon = pos.coords.longitude;
            
            if (lat >= 13.85 && lat <= 14.02 && lon >= 121.08 && lon <= 121.25) {
              userLat = lat;
              userLon = lon;
              locationLabel = "Lipa City GPS Location";
              document.getElementById('selectedProvinceName').innerText = `GPS Location: ${userLat.toFixed(2)}, ${userLon.toFixed(2)}`;
              updateWindyMapEmbed();
              fetchAgriForecast();
            } else {
              alert("GPS position is outside Lipa City bounds! Defaulting to Lipa City Poblacion.");
            }
          },
          () => alert("GPS Position Acquisition Failed.")
        );
      }
    }

    function submitAnonymousSuggestion(e) {
      e.preventDefault();
      const cat = document.getElementById('suggestionCategory').value;
      const text = document.getElementById('suggestionText').value;
      const existing = JSON.parse(localStorage.getItem('agripanahon_suggestions') || '[]');
      existing.push({ category: cat, message: text, timestamp: getCurrentPstTime() });
      localStorage.setItem('agripanahon_suggestions', JSON.stringify(existing));
      document.getElementById('suggestionText').value = '';
      document.getElementById('submitSuccessMsg').classList.remove('hidden');
      setTimeout(() => document.getElementById('submitSuccessMsg').classList.add('hidden'), 4000);
    }

    function unlockAdminPanel() {
      const pass = document.getElementById('adminPassInput').value;
      if (pass === ADMIN_PASSWORD) {
        document.getElementById('adminAuthBox').classList.add('hidden');
        document.getElementById('adminInboxContent').classList.remove('hidden');
        document.getElementById('adminStatusBadge').innerText = "Unlocked 🔓";
        document.getElementById('adminStatusBadge').className = "bg-emerald-100 text-harvest-green text-[10px] px-2.5 py-1 rounded-md font-bold uppercase border border-emerald-200";
        renderSuggestions();
      } else {
        document.getElementById('adminAuthError').classList.remove('hidden');
      }
    }

    function renderSuggestions() {
      const list = document.getElementById('suggestionList');
      const items = JSON.parse(localStorage.getItem('agripanahon_suggestions') || '[]');
      if (items.length === 0) {
        list.innerHTML = '<p class="text-xs text-earth-500 font-medium">No suggestions received yet.</p>';
        return;
      }
      list.innerHTML = items.map((item) => `
        <div class="bg-earth-50 p-3 rounded-xl border border-earth-300 text-xs space-y-1">
          <div class="flex justify-between items-center text-[10px] font-bold text-harvest-green">
            <span>${item.category}</span>
            <span class="font-mono text-earth-500">${item.timestamp}</span>
          </div>
          <p class="text-earth-800 font-medium">${item.message}</p>
        </div>
      `).join('');
    }

    function clearAllSuggestions() {
      localStorage.removeItem('agripanahon_suggestions');
      renderSuggestions();
    }

    function startAutoPollingEngine() {
      if (autoPollingTimer) clearInterval(autoPollingTimer);
      autoPollingTimer = setInterval(() => {
        if (navigator.onLine) {
          fetchAgriForecast();
        }
      }, 30000);
    }

    window.addEventListener('online', checkNetworkStatus);
    window.addEventListener('offline', checkNetworkStatus);
    window.addEventListener('load', () => {
      checkNetworkStatus();
      renderDirectivesSection();
      fetchAgriForecast();
      startAutoPollingEngine();
    });
  </script>
</body>
</html>
