<!doctype html>
<html lang="id" class="scroll-smooth">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=5.0" />
    <title>QurbanKu - Perencana & Tracker Tabungan Qurban</title>

    <!-- Google Fonts: Plus Jakarta Sans (Modern UX) & Amiri (Kaligrafi Islami) -->
    <link rel="preconnect" href="https://fonts.googleapis.com" />
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
    <link
      href="https://fonts.googleapis.com/css2?family=Amiri:ital,wght@0,400;0,700;1,400&family=Plus+Jakarta+Sans:wght@400;500;600;700;800&display=swap"
      rel="stylesheet"
    />

    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>

    <!-- Font Awesome Icons 6.5.1 -->
    <link
      rel="stylesheet"
      href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.1/css/all.min.css"
    />

    <!-- Chart.js CDN untuk Grafik Perkembangan Tabungan -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

    <!-- Konfigurasi Tailwind Custom Colors & Fonts -->
    <script>
      tailwind.config = {
        theme: {
          extend: {
            fontFamily: {
              sans: ['"Plus Jakarta Sans"', "sans-serif"],
              arabic: ['"Amiri"', "serif"],
            },
            colors: {
              emerald: {
                50: "#ecfdf5",
                100: "#d1fae5",
                200: "#a7f3d0",
                300: "#6ee7b7",
                400: "#34d399",
                500: "#10b981",
                600: "#059669",
                700: "#047857",
                800: "#065f46",
                900: "#064e3b",
                950: "#022c22",
              },
              gold: {
                50: "#fffbeb",
                100: "#fef3c7",
                200: "#fde68a",
                300: "#fcd34d",
                400: "#fbbf24",
                500: "#f59e0b",
                600: "#d97706",
                700: "#b45309",
              },
            },
          },
        },
      };
    </script>

    <style>
      /* Geometri Islami Latar Belakang Hero */
      .islamic-pattern {
        background-color: #064e3b;
        background-image:
          radial-gradient(#10b981 0.75px, transparent 0.75px),
          radial-gradient(#d97706 0.75px, #064e3b 0.75px);
        background-size: 24px 24px;
        background-position: 0 0, 12px 12px;
      }

      /* Horizontal Scroll Snap Optimization untuk Mobile */
      .no-scrollbar::-webkit-scrollbar {
        display: none;
      }
      .no-scrollbar {
        -ms-overflow-style: none;
        scrollbar-width: none;
      }

      /* Native Touch Optimizations */
      button, a, input, select {
        touch-action: manipulation;
      }

      /* Custom Smooth Scrollbar */
      ::-webkit-scrollbar {
        width: 6px;
        height: 6px;
      }
      ::-webkit-scrollbar-track {
        background: #f1f5f9;
      }
      ::-webkit-scrollbar-thumb {
        background: #059669;
        border-radius: 4px;
      }

      /* Toast Slide Animation */
      @keyframes slideDown {
        from { transform: translateY(-20px); opacity: 0; }
        to { transform: translateY(0); opacity: 1; }
      }
      .toast-animate {
        animation: slideDown 0.25s ease-out forwards;
      }

      /* Print CSS Rules */
      @media print {
        body { background: white !important; color: black !important; font-size: 11pt; }
        .no-print { display: none !important; }
        .print-only { display: block !important; }
        .shadow-lg, .shadow-md, .shadow-xl, .shadow-2xl { box-shadow: none !important; border: 1px solid #cbd5e1 !important; }
        header, footer, nav { display: none !important; }
      }

      /* Penyesuaian visual spesifik jenis & merek perangkat */
      body[data-device-type="HP"] .device-card-pad {
        padding: 0.9rem;
      }
      body[data-device-type="Tablet"] .device-card-pad {
        padding: 1.25rem;
      }
      body[data-device-type="Laptop"] .device-card-pad,
      body[data-device-type="PC"] .device-card-pad {
        padding: 1.5rem;
      }
      body[data-device-brand="Apple"] .device-accent-ring {
        box-shadow: 0 0 0 1px rgba(148, 163, 184, 0.35);
      }
      body[data-device-brand="Samsung"] .device-accent-ring {
        box-shadow: 0 0 0 1px rgba(16, 185, 129, 0.28);
      }
    </style>
  </head>
  <body data-device-type="PC" data-device-brand="Umum" class="bg-slate-50 text-slate-800 antialiased selection:bg-emerald-600 selection:text-white min-h-screen flex flex-col">

    <!-- ==================== NOTIFIKASI TOAST CONTAINER ==================== -->
    <div id="toast-container" class="fixed top-4 left-4 right-4 sm:left-auto sm:right-5 z-50 flex flex-col gap-2 pointer-events-none sm:max-w-md w-auto"></div>

    <!-- ==================== MODAL KONFIRMASI KUSTOM ==================== -->
    <div id="confirm-modal" class="fixed inset-0 z-50 hidden items-center justify-center bg-slate-900/60 backdrop-blur-sm p-4 no-print" role="dialog" aria-modal="true">
      <div id="confirm-modal-box" class="bg-white rounded-3xl max-w-sm sm:max-w-md w-full p-5 sm:p-6 shadow-2xl border border-slate-100 transform transition-all scale-95 opacity-0 my-auto">
        <div class="flex items-center gap-3 text-amber-600 mb-3">
          <div class="w-10 h-10 rounded-2xl bg-amber-100 flex items-center justify-center text-lg shrink-0">
            <i class="fa-solid fa-triangle-exclamation"></i>
          </div>
          <h3 class="text-base sm:text-lg font-bold text-slate-800" id="confirm-title">Konfirmasi Tindakan</h3>
        </div>
        <p class="text-slate-600 text-xs sm:text-sm mb-6 leading-relaxed" id="confirm-message">Apakah Anda yakin ingin melanjutkan tindakan ini?</p>
        <div class="flex items-center justify-end gap-2.5">
          <button type="button" id="confirm-cancel-btn" class="min-h-[44px] px-4 py-2.5 rounded-xl text-xs sm:text-sm font-semibold text-slate-600 hover:bg-slate-100 transition">Batal</button>
          <button type="button" id="confirm-ok-btn" class="min-h-[44px] px-4 py-2.5 rounded-xl text-xs sm:text-sm font-semibold bg-rose-600 text-white hover:bg-rose-700 shadow-sm transition">Ya, Lanjutkan</button>
        </div>
      </div>
    </div>

    <!-- ==================== MODAL EDIT SETORAN ==================== -->
    <div id="edit-deposit-modal" class="fixed inset-0 z-50 hidden items-center justify-center bg-slate-900/60 backdrop-blur-sm p-4 no-print" role="dialog" aria-modal="true">
      <div class="bg-white rounded-3xl max-w-md w-full p-5 sm:p-6 shadow-2xl border border-slate-100 my-auto">
        <div class="flex items-center justify-between mb-4">
          <div class="flex items-center gap-2.5">
            <div class="w-9 h-9 rounded-xl bg-emerald-100 text-emerald-700 flex items-center justify-center">
              <i class="fa-solid fa-pen-to-square"></i>
            </div>
            <h3 class="text-base sm:text-lg font-bold text-slate-900">Edit Setoran Tabungan</h3>
          </div>
          <button type="button" onclick="closeEditDepositModal()" class="w-9 h-9 flex items-center justify-center rounded-xl text-slate-400 hover:text-slate-600 hover:bg-slate-100">
            <i class="fa-solid fa-xmark text-lg"></i>
          </button>
        </div>
        <form id="edit-deposit-form" class="space-y-4">
          <input type="hidden" id="edit-deposit-id" />
          <div>
            <label class="block text-xs font-bold text-slate-700 mb-1">Nominal Setoran (Rp)</label>
            <input type="number" id="edit-deposit-amount" min="1000" step="1000" required class="w-full min-h-[44px] px-3.5 py-2.5 rounded-xl border border-slate-300 text-sm font-bold text-slate-900 focus:outline-none focus:ring-2 focus:ring-emerald-500" />
          </div>
          <div>
            <label class="block text-xs font-bold text-slate-700 mb-1">Tanggal Setoran</label>
            <input type="date" id="edit-deposit-date" required class="w-full min-h-[44px] px-3.5 py-2.5 rounded-xl border border-slate-300 text-sm text-slate-900 focus:outline-none focus:ring-2 focus:ring-emerald-500" />
          </div>
          <div>
            <label class="block text-xs font-bold text-slate-700 mb-1">Metode Setoran</label>
            <select id="edit-deposit-method" class="w-full min-h-[44px] px-3.5 py-2.5 rounded-xl border border-slate-300 text-sm text-slate-900 focus:outline-none focus:ring-2 focus:ring-emerald-500 bg-white">
              <option value="Transfer Bank (BSI)">Transfer Bank (BSI)</option>
              <option value="E-Wallet / QRIS">E-Wallet / QRIS</option>
              <option value="Setoran Tunai">Setoran Tunai</option>
            </select>
          </div>
          <div>
            <label class="block text-xs font-bold text-slate-700 mb-1">Nama Orang / Penyetor *</label>
            <input type="text" id="edit-deposit-person" required placeholder="Nama penyetor" class="w-full min-h-[44px] px-3.5 py-2.5 rounded-xl border border-slate-300 text-sm font-semibold text-slate-900 focus:outline-none focus:ring-2 focus:ring-emerald-500" />
          </div>
          <div>
            <label class="block text-xs font-bold text-slate-700 mb-1">Catatan</label>
            <input type="text" id="edit-deposit-note" placeholder="Contoh: Tabungan pekan ke-4" class="w-full min-h-[44px] px-3.5 py-2.5 rounded-xl border border-slate-300 text-sm text-slate-900 focus:outline-none focus:ring-2 focus:ring-emerald-500" />
          </div>
          <div class="flex items-center justify-end gap-2.5 pt-2">
            <button type="button" onclick="closeEditDepositModal()" class="min-h-[44px] px-4 py-2 rounded-xl text-xs sm:text-sm font-semibold text-slate-600 hover:bg-slate-100 transition">Batal</button>
            <button type="submit" class="min-h-[44px] px-5 py-2 rounded-xl text-xs sm:text-sm font-bold bg-emerald-700 hover:bg-emerald-800 text-white shadow-sm transition">Simpan Perubahan</button>
          </div>
        </form>
      </div>
    </div>

    <!-- ==================== HEADER RESPONSIF ==================== -->
    <header class="sticky top-0 z-40 bg-white/95 backdrop-blur-md border-b border-emerald-100 shadow-sm no-print">
      <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
        <div class="flex justify-between items-center h-16">
          
          <!-- Brand Logo -->
          <div class="flex items-center gap-3">
            <div class="w-10 h-10 rounded-2xl bg-gradient-to-br from-emerald-700 to-emerald-900 flex items-center justify-center text-gold-400 shadow-md shrink-0">
              <i class="fa-solid fa-kaaba text-lg"></i>
            </div>
            <div>
              <span class="font-extrabold text-lg sm:text-xl tracking-tight text-emerald-900">
                Qurban<span class="text-gold-600">Ku</span>
              </span>
              <span class="inline-block text-[10px] sm:text-[11px] font-semibold bg-emerald-50 text-emerald-700 px-2 py-0.5 rounded-full ml-1 sm:ml-2 border border-emerald-200">
                1448 H / 2027
              </span>
              <span id="device-profile-badge" class="hidden xl:inline-flex items-center gap-1.5 text-[10px] font-bold bg-slate-100 text-slate-700 px-2.5 py-0.5 rounded-full ml-1.5 border border-slate-200" title="Perangkat terdeteksi otomatis">
                <i id="device-profile-icon" class="fa-solid fa-desktop text-emerald-600"></i>
                <span id="device-profile-text">PC • Umum</span>
              </span>
            </div>
          </div>

          <!-- Navigasi Desktop (>= 768px) -->
          <nav class="hidden md:flex items-center gap-5 text-sm font-semibold text-slate-600">
            <a href="#pilih-hewan" class="hover:text-emerald-700 transition py-2">Target Hewan</a>
            <a href="#tracker-tabungan" class="hover:text-emerald-700 transition py-2">Tracker & Grafik</a>
            <a href="#patungan-sapi" class="hover:text-emerald-700 transition py-2">Patungan 1/7</a>
            <a href="#panduan-qurban" class="hover:text-emerald-700 transition py-2">Panduan Qurban</a>
          </nav>

          <!-- Action Buttons -->
          <div class="flex items-center gap-2">
            <button type="button" onclick="exportReport()" title="Cetak Laporan Tabungan" class="min-h-[40px] flex items-center gap-1.5 px-3 py-2 rounded-xl bg-emerald-50 hover:bg-emerald-100 text-emerald-800 text-xs sm:text-sm font-semibold border border-emerald-200 transition">
              <i class="fa-solid fa-print text-emerald-600"></i>
              <span class="hidden sm:inline">Laporan</span>
            </button>
            <button type="button" onclick="confirmResetDemo()" title="Reset Data Demo" class="min-h-[40px] flex items-center gap-1.5 px-3 py-2 rounded-xl bg-slate-100 hover:bg-rose-50 text-slate-600 hover:text-rose-600 text-xs sm:text-sm font-semibold border border-slate-200 transition">
              <i class="fa-solid fa-rotate-left"></i>
              <span class="hidden lg:inline">Reset</span>
            </button>

            <!-- Hamburger Button (< 768px) -->
            <button type="button" id="mobile-menu-btn" onclick="toggleMobileMenu()" class="md:hidden min-h-[40px] w-10 flex items-center justify-center rounded-xl bg-slate-100 text-slate-700 hover:bg-slate-200 transition focus:outline-none" aria-label="Buka Menu">
              <i class="fa-solid fa-bars text-lg" id="mobile-menu-icon"></i>
            </button>
          </div>
        </div>
      </div>

      <!-- Mobile Nav Drawer -->
      <div id="mobile-menu" class="hidden md:hidden bg-white border-b border-slate-200 px-4 pt-2 pb-4 space-y-2 shadow-lg">
        <a href="#pilih-hewan" onclick="toggleMobileMenu()" class="flex items-center gap-3 px-3 py-2.5 rounded-xl font-medium text-slate-700 hover:bg-emerald-50 hover:text-emerald-800 transition">
          <i class="fa-solid fa-bullseye w-5 text-emerald-600"></i> Target Hewan Qurban
        </a>
        <a href="#tracker-tabungan" onclick="toggleMobileMenu()" class="flex items-center gap-3 px-3 py-2.5 rounded-xl font-medium text-slate-700 hover:bg-emerald-50 hover:text-emerald-800 transition">
          <i class="fa-solid fa-chart-line w-5 text-emerald-600"></i> Tracker & Grafik Setoran
        </a>
        <a href="#patungan-sapi" onclick="toggleMobileMenu()" class="flex items-center gap-3 px-3 py-2.5 rounded-xl font-medium text-slate-700 hover:bg-emerald-50 hover:text-emerald-800 transition">
          <i class="fa-solid fa-users-viewfinder w-5 text-emerald-600"></i> Grup Patungan Sapi (1/7)
        </a>
        <a href="#panduan-qurban" onclick="toggleMobileMenu()" class="flex items-center gap-3 px-3 py-2.5 rounded-xl font-medium text-slate-700 hover:bg-emerald-50 hover:text-emerald-800 transition">
          <i class="fa-solid fa-book-open w-5 text-emerald-600"></i> Panduan & Fiqih Qurban
        </a>
      </div>
    </header>

    <!-- ==================== HERO & WIDGET UTAMA BERANDA ==================== -->
    <section class="relative islamic-pattern text-white overflow-hidden shrink-0">
      <div class="absolute inset-0 bg-gradient-to-br from-emerald-950/95 via-emerald-900/90 to-emerald-800/85"></div>
      
      <div class="relative max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-6 sm:py-10">
        <div class="grid grid-cols-1 lg:grid-cols-12 gap-6 items-center">
          
          <!-- Widget Salat & Countdown -->
          <div class="lg:col-span-7 space-y-4">
            
            <!-- Widget Lokasi & Waktu Sholat Card (Gaya App Islami) -->
            <div class="bg-emerald-950/80 backdrop-blur-md border border-emerald-700/60 rounded-3xl p-4 sm:p-5 shadow-xl">
              <div class="flex flex-wrap items-center justify-between gap-2 border-b border-emerald-800/80 pb-3 mb-3">
                <div class="flex items-center gap-2">
                  <i class="fa-solid fa-location-dot text-gold-400"></i>
                  <span class="text-xs sm:text-sm font-bold text-emerald-100">Kabupaten Bogor, ID</span>
                </div>
                <span class="text-[11px] font-semibold text-gold-300 bg-emerald-900/90 px-2.5 py-0.5 rounded-full border border-emerald-700">
                  17 Jumadilakhir 1448 H
                </span>
              </div>

              <!-- Live Waktu Sholat Strip -->
              <div class="grid grid-cols-5 gap-1.5 text-center text-[10px] sm:text-xs">
                <div class="p-2 rounded-xl bg-emerald-900/60 border border-emerald-800">
                  <span class="text-emerald-300 block">Subuh</span>
                  <span class="font-extrabold text-white mt-0.5 block">04:14</span>
                </div>
                <div class="p-2 rounded-xl bg-emerald-900/60 border border-emerald-800">
                  <span class="text-emerald-300 block">Zuhur</span>
                  <span class="font-extrabold text-white mt-0.5 block">11:42</span>
                </div>
                <div class="p-2 rounded-xl bg-emerald-900/60 border border-emerald-800">
                  <span class="text-emerald-300 block">Asar</span>
                  <span class="font-extrabold text-white mt-0.5 block">15:07</span>
                </div>
                <div class="p-2 rounded-xl bg-gold-500/20 border border-gold-400/50">
                  <span class="text-gold-300 block font-bold">Magrib</span>
                  <span class="font-extrabold text-gold-400 mt-0.5 block">17:58</span>
                </div>
                <div class="p-2 rounded-xl bg-emerald-900/60 border border-emerald-800">
                  <span class="text-emerald-300 block">Isya</span>
                  <span class="font-extrabold text-white mt-0.5 block">19:11</span>
                </div>
              </div>

              <!-- Countdown Timer Box -->
              <div class="mt-4 pt-3 border-t border-emerald-800/80">
                <div class="flex items-center justify-between gap-2 mb-2">
                  <span class="text-xs font-bold uppercase tracking-wider text-emerald-200 flex items-center gap-1.5">
                    <i class="fa-regular fa-calendar-check text-gold-400"></i> Estimasi Idul Adha (<span id="target-date-display">17 Mei 2027</span>)
                  </span>
                  <div class="flex items-center gap-1.5 no-print">
                    <label for="target-date-input" class="text-[10px] text-emerald-300">Tgl:</label>
                    <input type="date" id="target-date-input" value="2027-05-17" onchange="updateTargetDate(this.value)" class="bg-emerald-900 text-white text-[11px] rounded-lg px-2 py-0.5 border border-emerald-700 focus:outline-none focus:border-gold-400" />
                  </div>
                </div>

                <div class="grid grid-cols-4 gap-2 text-center">
                  <div class="bg-emerald-900/90 border border-emerald-700/70 rounded-xl py-2">
                    <span id="cd-days" class="block text-lg sm:text-2xl font-extrabold text-gold-400">000</span>
                    <span class="text-[10px] text-emerald-200 font-medium">Hari</span>
                  </div>
                  <div class="bg-emerald-900/90 border border-emerald-700/70 rounded-xl py-2">
                    <span id="cd-hours" class="block text-lg sm:text-2xl font-extrabold text-white">00</span>
                    <span class="text-[10px] text-emerald-200 font-medium">Jam</span>
                  </div>
                  <div class="bg-emerald-900/90 border border-emerald-700/70 rounded-xl py-2">
                    <span id="cd-minutes" class="block text-lg sm:text-2xl font-extrabold text-white">00</span>
                    <span class="text-[10px] text-emerald-200 font-medium">Menit</span>
                  </div>
                  <div class="bg-emerald-900/90 border border-emerald-700/70 rounded-xl py-2">
                    <span id="cd-seconds" class="block text-lg sm:text-2xl font-extrabold text-emerald-300">00</span>
                    <span class="text-[10px] text-emerald-200 font-medium">Detik</span>
                  </div>
                </div>
              </div>

            </div>
          </div>

          <!-- Ringkasan Tabungan Aktif -->
          <div class="lg:col-span-5">
            <div class="bg-white text-slate-800 rounded-3xl p-5 sm:p-6 shadow-2xl border-2 border-gold-400/50 relative overflow-hidden">
              <div class="flex items-center justify-between mb-3">
                <div>
                  <span class="text-[10px] font-extrabold uppercase tracking-wider text-emerald-700 bg-emerald-50 px-2.5 py-1 rounded-lg">
                    Ringkasan Target Aktif
                  </span>
                  <h2 id="hero-animal-name" class="text-base sm:text-lg font-bold text-slate-900 mt-1.5">
                    Sapi 1/7 (Patungan)
                  </h2>
                </div>
                <div class="w-11 h-11 rounded-2xl bg-gold-100 text-gold-600 flex items-center justify-center text-xl shrink-0 shadow-inner">
                  <i id="hero-animal-icon" class="fa-solid fa-cow"></i>
                </div>
              </div>

              <div class="my-4">
                <p class="text-xs text-slate-500 font-medium">Total Dana Terkumpul</p>
                <div class="flex items-baseline gap-2 mt-0.5">
                  <span id="hero-total-saved" class="text-2xl sm:text-3xl font-extrabold text-emerald-800 tracking-tight">
                    Rp 0
                  </span>
                </div>
                <div class="flex flex-wrap items-center gap-2 mt-1">
                  <p class="text-xs text-slate-500">
                    dari target <span id="hero-target-price" class="font-bold text-slate-700">Rp 3.000.000</span>
                  </p>
                  <button type="button" onclick="openQuickTargetEdit()" class="text-[11px] font-bold text-emerald-700 hover:text-emerald-900 bg-emerald-50 hover:bg-emerald-100 px-2 py-0.5 rounded-md border border-emerald-200 transition no-print">
                    <i class="fa-solid fa-pen-to-square mr-1"></i>Ubah
                  </button>
                </div>
              </div>

              <!-- Progress Bar -->
              <div class="space-y-1.5 mb-4">
                <div class="flex justify-between text-xs font-bold">
                  <span class="text-emerald-700" id="hero-progress-pct">0% Tercapai</span>
                  <span class="text-amber-600" id="hero-remaining-text">Kurang Rp 3.000.000</span>
                </div>
                <div class="w-full h-3 bg-slate-100 rounded-full overflow-hidden p-0.5 border border-slate-200">
                  <div id="hero-progress-bar" class="h-full bg-gradient-to-r from-emerald-600 via-emerald-500 to-gold-500 rounded-full transition-all duration-700 ease-out" style="width: 0%"></div>
                </div>
              </div>

              <!-- Quick Action Button -->
              <div class="flex gap-2 no-print">
                <a href="#form-setoran-card" onclick="focusDepositInput()" class="flex-1 min-h-[44px] bg-gradient-to-r from-emerald-700 to-emerald-800 hover:from-emerald-800 hover:to-emerald-900 text-white font-bold px-4 rounded-xl text-xs sm:text-sm text-center shadow-md transition flex items-center justify-center gap-2">
                  <i class="fa-solid fa-circle-plus text-gold-400"></i> Catat Setoran
                </a>
                <a href="#kalkulator-section" class="min-h-[44px] w-12 rounded-xl bg-gold-50 hover:bg-gold-100 text-gold-700 font-bold text-sm border border-gold-200 transition flex items-center justify-center shrink-0" title="Strategi Menabung">
                  <i class="fa-solid fa-calculator"></i>
                </a>
              </div>
            </div>
          </div>

        </div>
      </div>
    </section>

    <!-- ==================== MAIN CONTENT CONTAINER ==================== -->
    <main class="max-w-7xl mx-auto w-full px-4 sm:px-6 lg:px-8 py-6 sm:py-10 space-y-8 sm:space-y-12 flex-1">
      
      <!-- ==================== 3. QUICK ACTION BAR GRID ==================== -->
      <section class="no-print">
        <div class="bg-white rounded-3xl p-4 sm:p-5 shadow-sm border border-slate-200/80">
          <p class="text-[11px] font-extrabold uppercase tracking-wider text-slate-400 mb-3">Menu Pintas Aplikasi</p>
          <div class="grid grid-cols-4 sm:grid-cols-8 gap-2.5 text-center">
            <a href="#form-setoran-card" onclick="focusDepositInput()" class="flex flex-col items-center gap-1.5 p-2 rounded-2xl hover:bg-emerald-50 transition">
              <div class="w-11 h-11 rounded-2xl bg-emerald-100 text-emerald-700 flex items-center justify-center text-lg shadow-sm">
                <i class="fa-solid fa-piggy-bank"></i>
              </div>
              <span class="text-[11px] font-bold text-slate-700">Setoran</span>
            </a>
            <a href="#pilih-hewan" class="flex flex-col items-center gap-1.5 p-2 rounded-2xl hover:bg-emerald-50 transition">
              <div class="w-11 h-11 rounded-2xl bg-gold-100 text-gold-700 flex items-center justify-center text-lg shadow-sm">
                <i class="fa-solid fa-cow"></i>
              </div>
              <span class="text-[11px] font-bold text-slate-700">Hewan</span>
            </a>
            <a href="#kalkulator-section" class="flex flex-col items-center gap-1.5 p-2 rounded-2xl hover:bg-emerald-50 transition">
              <div class="w-11 h-11 rounded-2xl bg-emerald-100 text-emerald-700 flex items-center justify-center text-lg shadow-sm">
                <i class="fa-solid fa-calculator"></i>
              </div>
              <span class="text-[11px] font-bold text-slate-700">Rencana</span>
            </a>
            <a href="#patungan-sapi" class="flex flex-col items-center gap-1.5 p-2 rounded-2xl hover:bg-emerald-50 transition">
              <div class="w-11 h-11 rounded-2xl bg-gold-100 text-gold-700 flex items-center justify-center text-lg shadow-sm">
                <i class="fa-solid fa-users-viewfinder"></i>
              </div>
              <span class="text-[11px] font-bold text-slate-700">Patungan</span>
            </a>
            <a href="#savingsChart" class="flex flex-col items-center gap-1.5 p-2 rounded-2xl hover:bg-emerald-50 transition">
              <div class="w-11 h-11 rounded-2xl bg-emerald-100 text-emerald-700 flex items-center justify-center text-lg shadow-sm">
                <i class="fa-solid fa-chart-line"></i>
              </div>
              <span class="text-[11px] font-bold text-slate-700">Grafik</span>
            </a>
            <a href="#riwayat-section" class="flex flex-col items-center gap-1.5 p-2 rounded-2xl hover:bg-emerald-50 transition">
              <div class="w-11 h-11 rounded-2xl bg-gold-100 text-gold-700 flex items-center justify-center text-lg shadow-sm">
                <i class="fa-solid fa-receipt"></i>
              </div>
              <span class="text-[11px] font-bold text-slate-700">Riwayat</span>
            </a>
            <a href="#panduan-qurban" class="flex flex-col items-center gap-1.5 p-2 rounded-2xl hover:bg-emerald-50 transition">
              <div class="w-11 h-11 rounded-2xl bg-emerald-100 text-emerald-700 flex items-center justify-center text-lg shadow-sm">
                <i class="fa-solid fa-book-open"></i>
              </div>
              <span class="text-[11px] font-bold text-slate-700">Panduan</span>
            </a>
            <button type="button" onclick="exportReport()" class="flex flex-col items-center gap-1.5 p-2 rounded-2xl hover:bg-emerald-50 transition">
              <div class="w-11 h-11 rounded-2xl bg-slate-100 text-slate-700 flex items-center justify-center text-lg shadow-sm">
                <i class="fa-solid fa-print"></i>
              </div>
              <span class="text-[11px] font-bold text-slate-700">Cetak</span>
            </button>
          </div>
        </div>
      </section>

      <!-- ==================== 4. KARTU PESAN QURBAN HARI INI ==================== -->
      <section>
        <div class="bg-white rounded-3xl p-5 sm:p-6 shadow-md border border-emerald-100 relative overflow-hidden">
          <div class="flex items-center justify-between border-b border-slate-100 pb-3 mb-3">
            <div class="flex items-center gap-2">
              <span class="w-2.5 h-2.5 rounded-full bg-gold-500 animate-pulse"></span>
              <h3 class="font-extrabold text-xs sm:text-sm uppercase tracking-wider text-emerald-900">Pesan & Motivasi Qurban Hari Ini</h3>
            </div>
            <button type="button" onclick="randomizeQuote()" class="text-[11px] font-bold text-emerald-700 hover:text-emerald-900 bg-emerald-50 px-2.5 py-1 rounded-lg border border-emerald-200 transition no-print">
              <i class="fa-solid fa-shuffle mr-1"></i>Ganti Ayat
            </button>
          </div>

          <div class="space-y-2">
            <p id="quote-arabic" class="font-arabic text-right text-xl sm:text-2xl text-emerald-900 leading-loose">
              وَمَا كَانَ اللَّهُ مُعَذِّبَهُمْ وَهُمْ يَسْتَغْفِرُونَ
            </p>
            <p id="quote-translation" class="text-xs sm:text-sm text-slate-700 font-medium italic">
              "Dan Allah tidak sekali-kali akan mengazab mereka, sedang mereka memohon ampunan."
            </p>
            <div class="flex items-center justify-between pt-2">
              <span id="quote-source" class="text-[11px] font-bold text-gold-700">QS. Al-Anfal ayat 33</span>
              <div class="flex items-center gap-2 no-print">
                <button type="button" onclick="copyQuote()" class="min-h-[34px] px-3 py-1 rounded-xl bg-slate-100 hover:bg-slate-200 text-slate-700 text-xs font-bold transition flex items-center gap-1">
                  <i class="fa-regular fa-copy"></i> Salin
                </button>
              </div>
            </div>
          </div>
        </div>
      </section>

      <!-- ==================== 5. PILIHAN TARGET HEWAN (HORIZONTAL DI HP, GRID DI TABLET/PC) ==================== -->
      <section id="pilih-hewan" class="scroll-mt-20">
        <div class="flex flex-col sm:flex-row sm:items-end justify-between mb-4 gap-2">
          <div>
            <span class="text-xs font-bold uppercase tracking-wider text-gold-600">Langkah 1 • Tentukan Target</span>
            <h2 class="text-xl sm:text-2xl font-extrabold text-slate-900 mt-0.5">Pilih Target Hewan Qurban</h2>
          </div>
          <p class="text-xs text-slate-500">Hanya tersedia Domba / Kambing dan Sapi 1/7. Nominal dapat langsung diedit.</p>
        </div>

        <!-- Horizontal Snap Scroll di HP / Grid 2 Kolom Simetris di Tablet & PC -->
        <div class="flex sm:grid sm:grid-cols-2 gap-4 overflow-x-auto no-scrollbar snap-x snap-mandatory -mx-4 px-4 sm:mx-0 sm:px-0 pb-2" id="animal-cards-container">
          <!-- Di-render via JS -->
        </div>

        <!-- Panel Ubah Nominal Target Aktif -->
        <div id="quick-target-panel" class="mt-4 bg-white border border-emerald-200 rounded-2xl p-4 shadow-sm flex flex-col sm:flex-row items-start sm:items-center justify-between gap-3 no-print">
          <div class="flex items-center gap-3">
            <div class="w-9 h-9 rounded-xl bg-gold-50 text-gold-600 border border-gold-200 flex items-center justify-center text-sm shrink-0">
              <i class="fa-solid fa-pen-ruler"></i>
            </div>
            <div>
              <h3 class="font-bold text-xs sm:text-sm text-slate-900">Ubah Nominal Target (<span id="quick-active-animal-label" class="text-emerald-700">Sapi 1/7</span>)</h3>
              <p class="text-[11px] text-slate-500">Sesuaikan target biaya dengan harga hewan kurban di wilayah Anda.</p>
            </div>
          </div>

          <div class="flex items-center gap-2 w-full sm:w-auto">
            <div class="relative flex-1 sm:w-44">
              <span class="absolute left-3 top-2 text-xs font-bold text-emerald-700">Rp</span>
              <input type="number" id="quick-active-price-input" min="100000" step="50000" placeholder="3000000" class="w-full min-h-[40px] pl-8 pr-3 py-1.5 rounded-xl border border-slate-300 text-xs sm:text-sm font-bold text-slate-900 focus:outline-none focus:ring-2 focus:ring-emerald-500" />
            </div>
            <button type="button" onclick="applyQuickActivePrice()" class="min-h-[40px] px-4 py-1.5 rounded-xl bg-emerald-700 hover:bg-emerald-800 text-white font-bold text-xs sm:text-sm transition shrink-0">
              Simpan Target
            </button>
          </div>
        </div>
      </section>

      <!-- ==================== 6. KALKULATOR RENCANA SETORAN ==================== -->
      <section id="kalkulator-section" class="scroll-mt-20">
        <div class="bg-gradient-to-br from-emerald-900 to-emerald-950 rounded-3xl p-5 sm:p-7 text-white shadow-xl relative overflow-hidden">
          <div class="relative z-10">
            <div class="flex flex-col md:flex-row md:items-center justify-between gap-3 pb-4 border-b border-emerald-800">
              <div>
                <span class="text-xs font-bold uppercase tracking-wider text-gold-400">Langkah 2 • Strategi Menabung</span>
                <h2 class="text-xl sm:text-2xl font-extrabold mt-0.5">Rencana Setoran Otomatis</h2>
                <p class="text-xs text-emerald-200 mt-0.5">
                  Sisa kekurangan <span id="calc-remaining-label" class="font-bold text-white">Rp 0</span> dibagi sisa <span id="calc-days-left-label" class="font-bold text-gold-300">0 hari</span>.
                </p>
              </div>
              <div class="bg-emerald-800/80 border border-emerald-700 px-3 py-1.5 rounded-2xl text-xs w-fit">
                <span class="text-emerald-300 block text-[10px]">Status Target:</span>
                <span id="calc-status-badge" class="font-bold text-xs text-gold-400">Sedang Berjalan</span>
              </div>
            </div>

            <!-- Horizontal Snap Scroll di HP / Grid 3 Kolom di Tablet & PC -->
            <div class="flex md:grid md:grid-cols-3 gap-4 overflow-x-auto no-scrollbar snap-x snap-mandatory -mx-5 px-5 md:mx-0 md:px-0 pt-5">
              
              <!-- Harian -->
              <div class="snap-center shrink-0 w-[82vw] sm:w-[60vw] md:w-auto bg-white/10 backdrop-blur-sm border border-emerald-700/70 rounded-2xl p-4 flex flex-col justify-between">
                <div>
                  <div class="flex items-center justify-between mb-2">
                    <span class="text-xs font-bold uppercase tracking-wider text-emerald-300">Opsi 1 • Harian</span>
                    <span class="w-7 h-7 rounded-lg bg-emerald-800 text-gold-400 flex items-center justify-center text-xs"><i class="fa-solid fa-sun"></i></span>
                  </div>
                  <div class="text-xl font-extrabold text-white" id="plan-daily">Rp 0</div>
                  <p class="text-[11px] text-emerald-200 mt-1">Selama <span id="plan-days-count" class="font-semibold text-white">0</span> hari ke depan</p>
                </div>
                <button type="button" onclick="quickFillDeposit('daily')" class="mt-4 min-h-[40px] w-full py-2 rounded-xl bg-emerald-700/80 hover:bg-emerald-600 text-xs font-semibold text-white border border-emerald-600 transition no-print">Gunakan Nominal Ini</button>
              </div>

              <!-- Mingguan -->
              <div class="snap-center shrink-0 w-[82vw] sm:w-[60vw] md:w-auto bg-white/10 backdrop-blur-sm border-2 border-gold-400/70 rounded-2xl p-4 relative flex flex-col justify-between">
                <span class="absolute -top-2.5 right-4 bg-gold-500 text-emerald-950 text-[9px] font-extrabold uppercase px-2 py-0.5 rounded-full">Populer</span>
                <div>
                  <div class="flex items-center justify-between mb-2">
                    <span class="text-xs font-bold uppercase tracking-wider text-gold-300">Opsi 2 • Mingguan</span>
                    <span class="w-7 h-7 rounded-lg bg-gold-500/20 text-gold-400 flex items-center justify-center text-xs"><i class="fa-solid fa-calendar-week"></i></span>
                  </div>
                  <div class="text-xl font-extrabold text-gold-400" id="plan-weekly">Rp 0</div>
                  <p class="text-[11px] text-emerald-200 mt-1">Selama <span id="plan-weeks-count" class="font-semibold text-white">0</span> pekan (tiap Jumat)</p>
                </div>
                <button type="button" onclick="quickFillDeposit('weekly')" class="mt-4 min-h-[40px] w-full py-2 rounded-xl bg-gold-500 hover:bg-gold-400 text-xs font-bold text-emerald-950 transition no-print">Gunakan Nominal Ini</button>
              </div>

              <!-- Bulanan -->
              <div class="snap-center shrink-0 w-[82vw] sm:w-[60vw] md:w-auto bg-white/10 backdrop-blur-sm border border-emerald-700/70 rounded-2xl p-4 flex flex-col justify-between">
                <div>
                  <div class="flex items-center justify-between mb-2">
                    <span class="text-xs font-bold uppercase tracking-wider text-emerald-300">Opsi 3 • Bulanan</span>
                    <span class="w-7 h-7 rounded-lg bg-emerald-800 text-gold-400 flex items-center justify-center text-xs"><i class="fa-solid fa-calendar-days"></i></span>
                  </div>
                  <div class="text-xl font-extrabold text-white" id="plan-monthly">Rp 0</div>
                  <p class="text-[11px] text-emerald-200 mt-1">Selama <span id="plan-months-count" class="font-semibold text-white">0</span> bulan bertahap</p>
                </div>
                <button type="button" onclick="quickFillDeposit('monthly')" class="mt-4 min-h-[40px] w-full py-2 rounded-xl bg-emerald-700/80 hover:bg-emerald-600 text-xs font-semibold text-white border border-emerald-600 transition no-print">Gunakan Nominal Ini</button>
              </div>

            </div>
          </div>
        </div>
      </section>

      <!-- ==================== 7. TRACKER & GRAFIK TABUNGAN ==================== -->
      <section id="tracker-tabungan" class="scroll-mt-20 space-y-5">
        <div>
          <span class="text-xs font-bold uppercase tracking-wider text-gold-600">Langkah 3 • Catat & Pantau</span>
          <h2 class="text-xl sm:text-2xl font-extrabold text-slate-900 mt-0.5">Tracker Progres & Riwayat Setoran</h2>
        </div>

        <div class="grid grid-cols-1 lg:grid-cols-12 gap-5">
          
          <!-- Form Setoran (5 Kolom Desktop) -->
          <div class="lg:col-span-5 space-y-4 no-print" id="form-setoran-card">
            <div class="bg-white rounded-3xl p-5 shadow-md border border-slate-200/80">
              <div class="flex items-center gap-3 mb-4">
                <div class="w-10 h-10 rounded-xl bg-emerald-100 text-emerald-700 flex items-center justify-center text-lg shrink-0">
                  <i class="fa-solid fa-piggy-bank"></i>
                </div>
                <div>
                  <h3 class="font-bold text-base text-slate-900">Tambah Setoran Baru</h3>
                  <p class="text-xs text-slate-500">Catat penyetor manual & otomatis perbarui riwayat</p>
                </div>
              </div>

              <!-- Rekening Transfer -->
              <div class="mb-4 p-3.5 rounded-2xl bg-gradient-to-br from-emerald-50 to-gold-50/60 border border-emerald-200/90">
                <div class="flex items-center justify-between gap-2 mb-1.5">
                  <span class="text-[10px] font-extrabold uppercase tracking-wider text-emerald-800 flex items-center gap-1.5">
                    <i class="fa-solid fa-building-columns text-gold-600"></i> Rekening Transfer
                  </span>
                  <span class="text-[10px] font-bold px-2 py-0.5 rounded-full bg-white text-emerald-700 border border-emerald-200">BSI (Kode: 451)</span>
                </div>
                <div class="flex items-center justify-between gap-2 bg-white px-3 py-2 rounded-xl border border-emerald-100">
                  <div>
                    <p id="bank-account-number" class="font-mono font-extrabold text-sm sm:text-base text-slate-900">7123456789</p>
                    <p class="text-[11px] text-slate-600">a.n. <strong class="text-emerald-900">Panitia QurbanKu</strong></p>
                  </div>
                  <button type="button" onclick="copyBankAccount()" class="min-h-[36px] px-3 py-1 rounded-xl bg-emerald-700 hover:bg-emerald-800 text-white text-xs font-bold transition flex items-center gap-1 shadow-sm shrink-0">
                    <i class="fa-regular fa-copy"></i> <span id="copy-rekening-text">Salin</span>
                  </button>
                </div>
              </div>

              <!-- Form Input -->
              <form id="deposit-form" class="space-y-3">
                <div>
                  <label for="deposit-person" class="block text-xs font-bold text-slate-700 mb-1">Nama Orang / Penyetor *</label>
                  <input type="text" id="deposit-person" required placeholder="Ketik nama orang / penyetor..." class="w-full min-h-[42px] px-3 py-2 rounded-xl border border-slate-300 text-xs sm:text-sm font-semibold text-slate-900 focus:outline-none focus:ring-2 focus:ring-emerald-500" />
                </div>

                <div>
                  <label for="deposit-amount" class="block text-xs font-bold text-slate-700 mb-1">Nominal Setoran (Rp) *</label>
                  <div class="relative">
                    <span class="absolute left-3 top-2.5 text-xs font-extrabold text-emerald-700">Rp</span>
                    <input type="number" id="deposit-amount" min="1000" step="1000" required placeholder="100000" class="w-full min-h-[42px] pl-9 pr-3 py-2 rounded-xl border border-slate-300 text-sm font-bold text-slate-900 focus:outline-none focus:ring-2 focus:ring-emerald-500" />
                  </div>
                  <div class="flex flex-wrap gap-1.5 mt-2">
                    <button type="button" onclick="setPresetAmount(50000)" class="px-2.5 py-1 rounded-lg bg-slate-100 hover:bg-emerald-50 text-[11px] font-bold text-slate-600 transition">+50rb</button>
                    <button type="button" onclick="setPresetAmount(100000)" class="px-2.5 py-1 rounded-lg bg-slate-100 hover:bg-emerald-50 text-[11px] font-bold text-slate-600 transition">+100rb</button>
                    <button type="button" onclick="setPresetAmount(250000)" class="px-2.5 py-1 rounded-lg bg-slate-100 hover:bg-emerald-50 text-[11px] font-bold text-slate-600 transition">+250rb</button>
                    <button type="button" onclick="setPresetAmount(500000)" class="px-2.5 py-1 rounded-lg bg-slate-100 hover:bg-emerald-50 text-[11px] font-bold text-slate-600 transition">+500rb</button>
                  </div>
                </div>

                <div class="grid grid-cols-1 sm:grid-cols-2 gap-3">
                  <div>
                    <label for="deposit-date" class="block text-xs font-bold text-slate-700 mb-1">Tanggal *</label>
                    <input type="date" id="deposit-date" required class="w-full min-h-[42px] px-3 py-2 rounded-xl border border-slate-300 text-xs text-slate-900 focus:outline-none focus:ring-2 focus:ring-emerald-500" />
                  </div>
                  <div>
                    <label for="deposit-method" class="block text-xs font-bold text-slate-700 mb-1">Metode</label>
                    <select id="deposit-method" class="w-full min-h-[42px] px-3 py-2 rounded-xl border border-slate-300 text-xs text-slate-900 focus:outline-none focus:ring-2 focus:ring-emerald-500 bg-white">
                      <option value="Transfer Bank (BSI)">Transfer Bank (BSI)</option>
                      <option value="E-Wallet / QRIS">E-Wallet / QRIS</option>
                      <option value="Setoran Tunai">Setoran Tunai</option>
                    </select>
                  </div>
                </div>

                <div>
                  <label for="deposit-note" class="block text-xs font-bold text-slate-700 mb-1">Catatan</label>
                  <input type="text" id="deposit-note" placeholder="Contoh: Tabungan pekan ke-1" class="w-full min-h-[42px] px-3 py-2 rounded-xl border border-slate-300 text-xs text-slate-900 focus:outline-none focus:ring-2 focus:ring-emerald-500" />
                </div>

                <button type="submit" class="w-full min-h-[44px] py-2.5 px-4 rounded-xl bg-gradient-to-r from-emerald-700 to-emerald-800 hover:from-emerald-800 hover:to-emerald-900 text-white font-bold text-xs sm:text-sm shadow-md transition flex items-center justify-center gap-2">
                  <i class="fa-solid fa-check-circle text-gold-400"></i> Simpan Setoran
                </button>
              </form>
            </div>
          </div>

          <!-- Grafik & Tabel Riwayat (7 Kolom Desktop) -->
          <div class="lg:col-span-7 space-y-4">
            
            <!-- Grafik -->
            <div class="bg-white rounded-3xl p-4 sm:p-5 shadow-md border border-slate-200/80">
              <div class="flex items-center justify-between gap-2 mb-3">
                <div>
                  <h3 class="font-bold text-base text-slate-900">Grafik Akumulasi Tabungan</h3>
                  <p class="text-xs text-slate-500">Perkembangan setoran menuju target qurban</p>
                </div>
                <span id="chart-summary-badge" class="text-[10px] font-bold px-2 py-0.5 rounded-full bg-emerald-50 text-emerald-700 border border-emerald-200">0 Transaksi</span>
              </div>
              <div class="relative h-52 sm:h-60 w-full">
                <canvas id="savingsChart"></canvas>
              </div>
            </div>

            <!-- Tabel Riwayat Global -->
            <div id="riwayat-section" class="bg-white rounded-3xl p-4 sm:p-5 shadow-md border border-slate-200/80 scroll-mt-20">
              <div class="flex flex-col sm:flex-row sm:items-center justify-between gap-3 mb-3">
                <div>
                  <h3 class="font-bold text-base text-slate-900">Riwayat Transaksi Semua Penyetor</h3>
                  <p class="text-xs text-slate-500">Daftar transaksi seluruh orang yang sudah dicatat</p>
                </div>
                <div class="flex items-center gap-2 no-print">
                  <select id="deposit-person-filter" onchange="renderDeposits()" class="px-2.5 py-1.5 rounded-xl border border-slate-200 text-xs font-semibold text-slate-700 bg-white focus:outline-none focus:ring-2 focus:ring-emerald-500">
                    <option value="">Semua Orang</option>
                  </select>
                  <input type="text" id="deposit-search" oninput="renderDeposits()" placeholder="Cari nama/catatan..." class="px-3 py-1.5 rounded-xl border border-slate-200 text-xs focus:outline-none focus:ring-2 focus:ring-emerald-500 w-full sm:w-36" />
                  <button type="button" onclick="confirmClearAllDeposits()" title="Hapus Semua" class="min-h-[34px] px-2.5 py-1 rounded-xl bg-rose-50 hover:bg-rose-100 text-rose-600 text-xs font-bold border border-rose-200 transition shrink-0">
                    <i class="fa-solid fa-trash-can"></i>
                  </button>
                </div>
              </div>

              <div class="overflow-x-auto">
                <table class="w-full text-left border-collapse min-w-[460px]">
                  <thead>
                    <tr class="border-b border-slate-200 text-[10px] font-bold uppercase tracking-wider text-slate-400">
                      <th class="py-2 px-2">Tanggal</th>
                      <th class="py-2 px-2">Penyetor</th>
                      <th class="py-2 px-2">Metode & Catatan</th>
                      <th class="py-2 px-2 text-right">Nominal</th>
                      <th class="py-2 px-2 text-center no-print">Aksi</th>
                    </tr>
                  </thead>
                  <tbody id="deposits-table-body" class="divide-y divide-slate-100 text-xs">
                    <!-- Di-render via JS -->
                  </tbody>
                </table>
              </div>
            </div>

            <!-- Riwayat Transaksi per Orang (Otomatis Terbarui) -->
            <div id="riwayat-per-orang-section" class="bg-white rounded-3xl p-4 sm:p-5 shadow-md border border-emerald-200/80 scroll-mt-20">
              <div class="flex flex-col sm:flex-row sm:items-center justify-between gap-2 mb-4">
                <div>
                  <span class="text-[10px] font-extrabold uppercase tracking-wider text-emerald-700 bg-emerald-50 px-2 py-0.5 rounded-md">Rekap & Slot Orang</span>
                  <h3 class="font-bold text-base text-slate-900 mt-1">Riwayat Transaksi per Orang</h3>
                  <p class="text-xs text-slate-500">Tambah atau hapus slot orang langsung di bagiannya; nominal otomatis bertambah saat menyetor di Simpan Setoran.</p>
                </div>
                <div class="flex items-center gap-2 no-print">
                  <span id="person-summary-count" class="text-[11px] font-bold px-2.5 py-1 rounded-full bg-gold-50 text-gold-800 border border-gold-200">0 Orang</span>
                  <button type="button" onclick="addPersonSlot()" class="min-h-[36px] px-3 py-1.5 rounded-xl bg-emerald-700 hover:bg-emerald-800 text-white text-xs font-bold shadow-sm transition flex items-center gap-1.5">
                    <i class="fa-solid fa-user-plus text-gold-400"></i> Tambah Slot Orang
                  </button>
                </div>
              </div>

              <div id="per-person-history-container" class="grid grid-cols-1 md:grid-cols-2 gap-3.5">
                <!-- Di-render via JS -->
              </div>
            </div>

          </div>
        </div>
      </section>

      <!-- ==================== 8. TRACKER PATUNGAN SAPI 1/7 ==================== -->
      <section id="patungan-sapi" class="scroll-mt-20 space-y-4">
        <div class="flex flex-col sm:flex-row sm:items-end justify-between gap-2">
          <div>
            <span class="text-xs font-bold uppercase tracking-wider text-gold-600">Langkah 4 • Kolaborasi Sapi 1/7</span>
            <h2 class="text-xl sm:text-2xl font-extrabold text-slate-900 mt-0.5">Tracker Patungan Sapi (7 Shohibul Qurban)</h2>
          </div>

          <div class="flex items-center gap-2 no-print">
            <button type="button" onclick="addPatunganMember()" class="min-h-[38px] px-3 py-1.5 rounded-xl bg-emerald-700 hover:bg-emerald-800 text-white text-xs font-bold shadow-sm transition flex items-center gap-1.5">
              <i class="fa-solid fa-user-plus text-gold-400"></i> Tambah Slot Patungan
            </button>
            <button type="button" onclick="confirmResetPatungan()" class="min-h-[38px] px-3 py-1.5 rounded-xl bg-slate-100 hover:bg-slate-200 text-slate-700 text-xs font-bold border border-slate-200 transition">
              Reset Grup
            </button>
          </div>
        </div>

        <div class="bg-white rounded-3xl p-4 sm:p-5 shadow-md border border-slate-200/80">
          
          <!-- Summary Cards Patungan -->
          <div class="grid grid-cols-2 lg:grid-cols-4 gap-3 mb-4">
            <div class="p-3 rounded-2xl bg-emerald-50/80 border border-emerald-100">
              <span class="text-[10px] font-semibold text-emerald-700">Target Sapi (<span id="patungan-cow-count-label">1 Ekor</span>)</span>
              <div id="patungan-full-cow-target" class="text-sm sm:text-base font-extrabold text-slate-900 mt-0.5">Rp 21.000.000</div>
              <span class="text-[10px] text-slate-500"><span id="patungan-total-slots-label">7 Slot</span> × <span id="patungan-per-share-target">Rp 3.000.000</span></span>
            </div>

            <div class="p-3 rounded-2xl bg-gold-50/80 border border-gold-200/80">
              <span class="text-[10px] font-semibold text-gold-700">Total Terkumpul</span>
              <div id="patungan-total-collected" class="text-sm sm:text-base font-extrabold text-emerald-800 mt-0.5">Rp 0</div>
              <span id="patungan-pct-label" class="text-[10px] text-slate-600">0% dari target</span>
            </div>

            <div class="p-3 rounded-2xl bg-slate-50 border border-slate-200">
              <span class="text-[10px] font-semibold text-slate-600">Peserta Lunas</span>
              <div id="patungan-slot-summary" class="text-sm sm:text-base font-extrabold text-slate-900 mt-0.5">0 / 7 Slot</div>
              <span id="patungan-paid-count" class="text-[10px] text-emerald-700 font-semibold">0 lunas</span>
            </div>

            <div class="p-3 rounded-2xl bg-slate-50 border border-slate-200">
              <span class="text-[10px] font-semibold text-slate-600">Sisa Kekurangan</span>
              <div id="patungan-remaining" class="text-sm sm:text-base font-extrabold text-amber-600 mt-0.5">Rp 21.000.000</div>
              <span class="text-[10px] text-slate-500">Total grup</span>
            </div>
          </div>

          <!-- Progress Bar -->
          <div class="mb-5">
            <div class="flex justify-between text-xs font-bold mb-1">
              <span class="text-emerald-800">Progres Pendanaan Patungan Sapi</span>
              <span id="patungan-bar-pct" class="text-gold-600">0%</span>
            </div>
            <div class="w-full h-2.5 bg-slate-100 rounded-full overflow-hidden border border-slate-200">
              <div id="patungan-progress-bar" class="h-full bg-gradient-to-r from-emerald-600 to-gold-500 rounded-full transition-all duration-500" style="width: 0%"></div>
            </div>
          </div>

          <!-- Horizontal Snap Scroll di HP / Grid 4 Kolom di PC -->
          <div id="patungan-slots-grid" class="flex sm:grid sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-3.5 overflow-x-auto no-scrollbar snap-x snap-mandatory -mx-4 px-4 sm:mx-0 sm:px-0 pb-2">
            <!-- Di-render via JS -->
          </div>

        </div>
      </section>

      <!-- ==================== 9. PANDUAN & FIQIH QURBAN ==================== -->
      <section id="panduan-qurban" class="scroll-mt-20 space-y-4">
        <div>
          <span class="text-xs font-bold uppercase tracking-wider text-gold-600">Bekal Ilmu & Syariat</span>
          <h2 class="text-xl sm:text-2xl font-extrabold text-slate-900 mt-0.5">Panduan & Ketentuan Qurban</h2>
        </div>

        <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
          <div class="bg-white rounded-3xl p-5 shadow-md border border-slate-200/80 space-y-2.5">
            <div class="flex items-center gap-2.5">
              <div class="w-9 h-9 rounded-xl bg-emerald-100 text-emerald-700 flex items-center justify-center text-base shrink-0">
                <i class="fa-solid fa-scale-balanced"></i>
              </div>
              <h3 class="font-bold text-base text-slate-900">Ketentuan Syariat Hewan</h3>
            </div>
            <ul class="space-y-1.5 text-xs text-slate-600 leading-relaxed">
              <li class="flex items-start gap-2">
                <i class="fa-solid fa-check text-emerald-600 mt-0.5 shrink-0"></i>
                <span><strong>Kambing/Domba:</strong> Umur minimal 1 tahun untuk 1 orang.</span>
              </li>
              <li class="flex items-start gap-2">
                <i class="fa-solid fa-check text-emerald-600 mt-0.5 shrink-0"></i>
                <span><strong>Sapi/Kerbau:</strong> Umur minimal 2 tahun untuk maksimal 7 orang.</span>
              </li>
              <li class="flex items-start gap-2">
                <i class="fa-solid fa-check text-emerald-600 mt-0.5 shrink-0"></i>
                <span><strong>Kondisi Fisik:</strong> Sehat, bebas cacat, tidak buta, dan tidak pincang.</span>
              </li>
            </ul>
          </div>

          <div class="bg-white rounded-3xl p-5 shadow-md border border-slate-200/80 space-y-2.5">
            <div class="flex items-center gap-2.5">
              <div class="w-9 h-9 rounded-xl bg-gold-100 text-gold-600 flex items-center justify-center text-base shrink-0">
                <i class="fa-solid fa-hand-holding-heart"></i>
              </div>
              <h3 class="font-bold text-base text-slate-900">Pembagian Daging Qurban</h3>
            </div>
            <p class="text-xs text-slate-600">Anjuran porsi pembagian hasil sembelihan qurban:</p>
            <div class="grid grid-cols-3 gap-2 pt-1 text-center">
              <div class="p-2 rounded-2xl bg-emerald-50 border border-emerald-100">
                <span class="block text-base font-extrabold text-emerald-800">1/3</span>
                <span class="text-[10px] font-semibold text-slate-600">Shohibul Qurban</span>
              </div>
              <div class="p-2 rounded-2xl bg-gold-50 border border-gold-200">
                <span class="block text-base font-extrabold text-gold-700">1/3</span>
                <span class="text-[10px] font-semibold text-slate-600">Kerabat/Tetangga</span>
              </div>
              <div class="p-2 rounded-2xl bg-slate-50 border border-slate-200">
                <span class="block text-base font-extrabold text-slate-800">1/3</span>
                <span class="text-[10px] font-semibold text-slate-600">Faqir Miskin</span>
              </div>
            </div>
          </div>
        </div>
      </section>

    </main>

    <!-- ==================== FOOTER ==================== -->
    <footer class="bg-emerald-950 text-emerald-100 border-t border-emerald-900 mt-10 no-print shrink-0">
      <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-6">
        <div class="flex flex-col sm:flex-row items-center justify-between gap-3 text-center sm:text-left">
          <div class="flex items-center gap-2.5">
            <div class="w-8 h-8 rounded-xl bg-emerald-900 border border-emerald-700 flex items-center justify-center text-gold-400 shrink-0">
              <i class="fa-solid fa-kaaba"></i>
            </div>
            <span class="font-extrabold text-sm text-white">Qurban<span class="text-gold-400">Ku</span></span>
          </div>
          <p class="text-xs text-emerald-400">Data Anda tersimpan secara aman di browser lokal.</p>
          <p class="text-xs text-emerald-500">&copy; 1448 H / 2027 • QurbanKu</p>
        </div>
      </div>
    </footer>

    <!-- ==================== JAVASCRIPT ==================== -->
    <script>
      const STORAGE_KEY = "qurbanku_app_v4";

      const QUOTES_LIST = [
        {
          ar: "وَمَا كَانَ اللَّهُ مُعَذِّبَهُمْ وَهُمْ يَسْتَغْفِرُونَ",
          id: '"Dan Allah tidak sekali-kali akan mengazab mereka, sedang mereka memohon ampunan."',
          src: "QS. Al-Anfal ayat 33",
        },
        {
          ar: "لَنْ يَنَالَ اللَّهَ لُحُومُهَا وَلَا دِمَاؤُهَا وَلَٰكِنْ يَنَالُهُ التَّقْوَىٰ مِنْكُمْ",
          id: '"Daging-daging qurban dan darahnya itu sekali-kali tidak dapat mencapai (keridhaan) Allah, tetapi ketakwaan dari kamulah yang dapat mecapainya."',
          src: "QS. Al-Hajj ayat 37",
        },
        {
          ar: "فَصَلِّ لِرَبِّكَ وَانْحَرْ",
          id: '"Maka laksanakanlah shalat karena Tuhanmu, dan berkurbanlah."',
          src: "QS. Al-Kautsar ayat 2",
        },
      ];

      const DEFAULT_ANIMALS = [
        {
          id: "domba-kambing",
          name: "Domba / Kambing",
          subtitle: "Qurban mandiri 1 orang • Umur ≥ 1 tahun & sehat",
          icon: "fa-horse-head",
          badge: "Mandiri 1 Orang",
          targetPrice: 2500000,
        },
        {
          id: "sapi-1-7",
          name: "Sapi 1/7",
          subtitle: "1 bagian patungan dari 7 orang • Sapi umur ≥ 2 tahun",
          icon: "fa-cow",
          badge: "Patungan 1/7",
          targetPrice: 3000000,
        },
      ];

      function createDefaultPatungan(sharePrice = 3000000) {
        return [
          { id: 1, name: "Ahmad Fauzi", paidAmount: 500000, targetAmount: sharePrice, note: "Setoran awal" },
          { id: 2, name: "Siti Rahmawati", paidAmount: 350000, targetAmount: sharePrice, note: "Setoran manual" },
          { id: 3, name: "Budi Santoso", paidAmount: 450000, targetAmount: sharePrice, note: "Setoran tengah bulan" },
          { id: 4, name: "Hendra Wijaya", paidAmount: 400000, targetAmount: sharePrice, note: "Setoran pekan ini" },
          { id: 5, name: "Nurul Hidayah", paidAmount: 0, targetAmount: sharePrice, note: "Belum menyetor" },
          { id: 6, name: "Rizky Pratama", paidAmount: 0, targetAmount: sharePrice, note: "Belum menyetor" },
          { id: 7, name: "Slot Terbuka #7", paidAmount: 0, targetAmount: sharePrice, note: "Menunggu peserta" },
        ];
      }

      function createDemoState() {
        const today = new Date();
        const fmt = (daysAgo) => new Date(today.getTime() - daysAgo * 86400000).toISOString().split("T")[0];

        return {
          targetDate: "2027-05-17",
          selectedAnimalId: "sapi-1-7",
          currentQuoteIndex: 0,
          animals: DEFAULT_ANIMALS.map((a) => ({ ...a })),
          personSlots: [
            { id: "slot-1", name: "Ahmad Fauzi" },
            { id: "slot-2", name: "Siti Rahmawati" },
            { id: "slot-3", name: "Budi Santoso" },
            { id: "slot-4", name: "Hendra Wijaya" },
          ],
          deposits: [
            { id: "dep-1", person: "Ahmad Fauzi", amount: 500000, date: fmt(45), method: "Transfer Bank (BSI)", note: "Setoran awal niat qurban" },
            { id: "dep-2", person: "Siti Rahmawati", amount: 350000, date: fmt(30), method: "Setoran Tunai", note: "Tabungan bulanan" },
            { id: "dep-3", person: "Budi Santoso", amount: 450000, date: fmt(15), method: "E-Wallet / QRIS", note: "Setoran tengah bulan" },
            { id: "dep-4", person: "Hendra Wijaya", amount: 400000, date: fmt(3), method: "Transfer Bank (BSI)", note: "Setoran pekan ini" },
          ],
          patungan: createDefaultPatungan(3000000),
        };
      }

      let appState = loadState();
      let savingsChartInstance = null;
      let countdownInterval = null;

      function normalizeAnimals(rawAnimals) {
        const map = new Map();
        if (Array.isArray(rawAnimals)) {
          rawAnimals.forEach((item) => {
            if (!item || !item.id) return;
            if (item.id === "domba-kambing" || item.id === "kambing-std") {
              map.set("domba-kambing", {
                id: "domba-kambing",
                name: "Domba / Kambing",
                subtitle: "Qurban mandiri 1 orang • Umur ≥ 1 tahun & sehat",
                icon: "fa-horse-head",
                badge: "Mandiri 1 Orang",
                targetPrice: Math.max(100000, Math.round(Number(item.targetPrice) || 2500000)),
              });
            } else if (item.id === "sapi-1-7") {
              map.set("sapi-1-7", {
                id: "sapi-1-7",
                name: "Sapi 1/7",
                subtitle: "1 bagian patungan dari 7 orang • Sapi umur ≥ 2 tahun",
                icon: "fa-cow",
                badge: "Patungan 1/7",
                targetPrice: Math.max(100000, Math.round(Number(item.targetPrice) || 3000000)),
              });
            }
          });
        }
        return DEFAULT_ANIMALS.map((def) => map.get(def.id) || { ...def });
      }

      function ensurePersonSlotsSynced() {
        if (!Array.isArray(appState.personSlots)) {
          appState.personSlots = [];
        }
        const seen = new Set(appState.personSlots.map((s) => (s.name || "").trim().toLowerCase()).filter(Boolean));
        appState.deposits.forEach((d) => {
          const pName = (d.person || "").trim();
          if (!pName) return;
          const key = pName.toLowerCase();
          if (!seen.has(key)) {
            seen.add(key);
            appState.personSlots.push({
              id: "slot-" + Date.now() + "-" + Math.random().toString(36).slice(2, 6),
              name: pName,
            });
          }
        });
      }

      function syncPatunganWithDeposits() {
        const sapiAnimal = appState.animals.find((a) => a.id === "sapi-1-7");
        const sharePrice = Math.max(100000, Number(sapiAnimal?.targetPrice) || 3000000);

        const totalsByPerson = new Map();
        appState.deposits.forEach((d) => {
          const pName = (d.person || "").trim();
          if (!pName) return;
          const key = pName.toLowerCase();
          totalsByPerson.set(key, {
            name: pName,
            total: (totalsByPerson.get(key)?.total || 0) + (Number(d.amount) || 0),
          });
        });

        if (!Array.isArray(appState.patungan) || appState.patungan.length === 0) {
          appState.patungan = createDefaultPatungan(sharePrice);
        }

        const matchedKeys = new Set();
        appState.patungan.forEach((slot) => {
          slot.targetAmount = sharePrice;
          const key = (slot.name || "").trim().toLowerCase();
          if (totalsByPerson.has(key)) {
            const pData = totalsByPerson.get(key);
            slot.name = pData.name;
            slot.paidAmount = pData.total;
            slot.note = pData.total >= sharePrice ? "Lunas (otomatis dari setoran)" : "Nominal bertambah dari setoran";
            matchedKeys.add(key);
          } else {
            slot.paidAmount = 0;
            if (!slot.note || slot.note.includes("setoran") || slot.note.includes("Lunas")) {
              slot.note = "Belum ada setoran";
            }
          }
        });

        totalsByPerson.forEach((pData, key) => {
          if (!matchedKeys.has(key)) {
            const openSlot = appState.patungan.find((s) => (s.name || "").startsWith("Slot Terbuka") && (Number(s.paidAmount) || 0) === 0);
            if (openSlot) {
              openSlot.name = pData.name;
              openSlot.paidAmount = pData.total;
              openSlot.note = pData.total >= sharePrice ? "Lunas (otomatis dari setoran)" : "Nominal bertambah dari setoran";
            }
          }
        });
      }

      function loadState() {
        try {
          const raw = localStorage.getItem(STORAGE_KEY);
          if (!raw) return createDemoState();
          const parsed = JSON.parse(raw);
          if (!parsed || !Array.isArray(parsed.deposits)) return createDemoState();

          const animals = normalizeAnimals(parsed.animals);
          const selectedAnimalId = animals.some((a) => a.id === parsed.selectedAnimalId) ? parsed.selectedAnimalId : "sapi-1-7";
          const deposits = parsed.deposits.map((d, idx) => ({
            id: d.id || `dep-${idx + 1}`,
            person: (d.person || "Peserta Qurban").trim(),
            amount: Math.max(0, Number(d.amount) || 0),
            date: d.date || new Date().toISOString().split("T")[0],
            method: d.method === "Autodebet / Gajian" ? "Transfer Bank (BSI)" : d.method || "Transfer Bank (BSI)",
            note: d.note || "Setoran tabungan",
          }));

          const personSlots = Array.isArray(parsed.personSlots)
            ? parsed.personSlots.map((s, idx) => ({
                id: s.id || `slot-${idx + 1}`,
                name: (s.name || `Peserta #${idx + 1}`).trim(),
              }))
            : [];

          return {
            targetDate: parsed.targetDate || "2027-05-17",
            selectedAnimalId,
            currentQuoteIndex: Number(parsed.currentQuoteIndex) || 0,
            animals,
            personSlots,
            deposits,
            patungan: Array.isArray(parsed.patungan) && parsed.patungan.length > 0 ? parsed.patungan : createDefaultPatungan(3000000),
          };
        } catch (err) {
          return createDemoState();
        }
      }

      function saveState() {
        try {
          localStorage.setItem(STORAGE_KEY, JSON.stringify(appState));
        } catch (err) {
          showToast("Gagal menyimpan data lokal.", "error");
        }
      }

      function formatRupiah(num) {
        const val = Number.isFinite(Number(num)) ? Math.round(Number(num)) : 0;
        return "Rp " + val.toLocaleString("id-ID");
      }

      function formatDateID(dateStr) {
        if (!dateStr) return "-";
        const parts = dateStr.split("-");
        if (parts.length !== 3) return dateStr;
        const d = new Date(Number(parts[0]), Number(parts[1]) - 1, Number(parts[2]));
        if (Number.isNaN(d.getTime())) return dateStr;
        return d.toLocaleDateString("id-ID", { day: "numeric", month: "short", year: "numeric" });
      }

      function escapeHTML(str) {
        return String(str || "").replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;").replace(/"/g, "&quot;").replace(/'/g, "&#39;");
      }

      function toggleMobileMenu() {
        const menu = document.getElementById("mobile-menu");
        const icon = document.getElementById("mobile-menu-icon");
        if (!menu) return;
        const isHidden = menu.classList.contains("hidden");
        if (isHidden) {
          menu.classList.remove("hidden");
          if (icon) icon.className = "fa-solid fa-xmark text-lg";
        } else {
          menu.classList.add("hidden");
          if (icon) icon.className = "fa-solid fa-bars text-lg";
        }
      }

      function showToast(message, type = "success") {
        const container = document.getElementById("toast-container");
        if (!container) return;

        const styles = {
          success: "bg-emerald-900 text-white border-emerald-600",
          error: "bg-rose-800 text-white border-rose-600",
          info: "bg-slate-900 text-white border-slate-700",
        };
        const icons = {
          success: "fa-circle-check text-gold-400",
          error: "fa-circle-xmark text-rose-300",
          info: "fa-circle-info text-emerald-300",
        };

        const toast = document.createElement("div");
        toast.className = `toast-animate pointer-events-auto flex items-center gap-3 px-4 py-3 rounded-2xl border shadow-xl text-xs sm:text-sm font-semibold ${styles[type] || styles.success}`;
        toast.innerHTML = `<i class="fa-solid ${icons[type] || icons.success} text-base shrink-0"></i><span class="flex-1">${escapeHTML(message)}</span>`;

        container.appendChild(toast);
        setTimeout(() => {
          toast.style.opacity = "0";
          toast.style.transition = "opacity 0.2s ease";
          setTimeout(() => toast.remove(), 210);
        }, 2800);
      }

      function showConfirmModal({ title = "Konfirmasi", message = "Yakin melanjutkan?", okText = "Ya", onConfirm = () => {} }) {
        const modal = document.getElementById("confirm-modal");
        const box = document.getElementById("confirm-modal-box");
        document.getElementById("confirm-title").textContent = title;
        document.getElementById("confirm-message").textContent = message;
        const okBtn = document.getElementById("confirm-ok-btn");
        const cancelBtn = document.getElementById("confirm-cancel-btn");

        okBtn.textContent = okText;
        modal.classList.remove("hidden");
        modal.classList.add("flex");
        requestAnimationFrame(() => {
          box.classList.remove("scale-95", "opacity-0");
          box.classList.add("scale-100", "opacity-100");
        });

        const cleanup = () => {
          box.classList.remove("scale-100", "opacity-100");
          box.classList.add("scale-95", "opacity-0");
          setTimeout(() => { modal.classList.add("hidden"); modal.classList.remove("flex"); }, 150);
          okBtn.removeEventListener("click", handleOk);
          cancelBtn.removeEventListener("click", handleCancel);
        };

        const handleOk = () => { cleanup(); onConfirm(); };
        const handleCancel = () => cleanup();

        okBtn.addEventListener("click", handleOk);
        cancelBtn.addEventListener("click", handleCancel);
      }

      function randomizeQuote() {
        appState.currentQuoteIndex = ((appState.currentQuoteIndex || 0) + 1) % QUOTES_LIST.length;
        saveState();
        renderQuote();
        showToast("Ayat/Pesan diperbarui", "info");
      }

      function renderQuote() {
        const q = QUOTES_LIST[appState.currentQuoteIndex || 0] || QUOTES_LIST[0];
        document.getElementById("quote-arabic").textContent = q.ar;
        document.getElementById("quote-translation").textContent = q.id;
        document.getElementById("quote-source").textContent = q.src;
      }

      function copyQuote() {
        const q = QUOTES_LIST[appState.currentQuoteIndex || 0] || QUOTES_LIST[0];
        const text = `${q.ar}\n${q.id}\n(${q.src})`;
        if (navigator.clipboard && window.isSecureContext) {
          navigator.clipboard.writeText(text).then(() => showToast("Pesan berhasil disalin!", "success"));
        } else {
          showToast("Disalin ke memori browser.", "info");
        }
      }

      function getActiveAnimal() {
        return appState.animals.find((a) => a.id === appState.selectedAnimalId) || appState.animals[0];
      }

      function getTotalSaved() {
        return appState.deposits.reduce((sum, d) => sum + (Number(d.amount) || 0), 0);
      }

      function getDaysRemaining() {
        const target = new Date(appState.targetDate + "T00:00:00");
        const diffMs = target.getTime() - Date.now();
        if (Number.isNaN(diffMs) || diffMs <= 0) return 1;
        return Math.max(1, Math.ceil(diffMs / (1000 * 60 * 60 * 24)));
      }

      function updateCountdown() {
        const inputEl = document.getElementById("target-date-input");
        const displayEl = document.getElementById("target-date-display");
        if (inputEl && inputEl.value !== appState.targetDate) inputEl.value = appState.targetDate;
        if (displayEl) displayEl.textContent = formatDateID(appState.targetDate);

        const target = new Date(appState.targetDate + "T00:00:00").getTime();
        const dist = Math.max(0, target - Date.now());

        const days = Math.floor(dist / (1000 * 60 * 60 * 24));
        const hours = Math.floor((dist % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
        const minutes = Math.floor((dist % (1000 * 60 * 60)) / (1000 * 60));
        const seconds = Math.floor((dist % (1000 * 60)) / 1000);

        const setT = (id, val) => { const el = document.getElementById(id); if (el) el.textContent = val; };
        setT("cd-days", String(days).padStart(3, "0"));
        setT("cd-hours", String(hours).padStart(2, "0"));
        setT("cd-minutes", String(minutes).padStart(2, "0"));
        setT("cd-seconds", String(seconds).padStart(2, "0"));
      }

      function updateTargetDate(val) {
        if (!val) return;
        appState.targetDate = val;
        saveState();
        updateCountdown();
        renderCalculator();
        showToast(`Target Idul Adha diubah ke ${formatDateID(val)}`, "info");
      }

      function renderHeroAndSummary() {
        const animal = getActiveAnimal();
        const totalSaved = getTotalSaved();
        const targetPrice = Math.max(1, Number(animal.targetPrice) || 1);
        const pct = Math.min(100, Math.round((totalSaved / targetPrice) * 100));
        const remaining = Math.max(0, targetPrice - totalSaved);

        document.getElementById("hero-animal-name").textContent = animal.name;
        const iconEl = document.getElementById("hero-animal-icon");
        if (iconEl) iconEl.className = `fa-solid ${animal.icon}`;

        document.getElementById("hero-total-saved").textContent = formatRupiah(totalSaved);
        document.getElementById("hero-target-price").textContent = formatRupiah(targetPrice);
        document.getElementById("hero-progress-pct").textContent = `${pct}% Tercapai`;
        document.getElementById("hero-remaining-text").textContent = remaining === 0 ? "Lunas!" : `Kurang ${formatRupiah(remaining)}`;
        document.getElementById("hero-progress-bar").style.width = `${pct}%`;

        document.getElementById("quick-active-animal-label").textContent = animal.name;
        const quickInput = document.getElementById("quick-active-price-input");
        if (quickInput && document.activeElement !== quickInput) quickInput.value = animal.targetPrice;
      }

      function renderAnimalCards() {
        const container = document.getElementById("animal-cards-container");
        if (!container) return;

        container.innerHTML = appState.animals
          .map((animal) => {
            const isSelected = animal.id === appState.selectedAnimalId;
            return `
              <div
                onclick="selectAnimal('${animal.id}')"
                class="snap-center shrink-0 w-[85vw] sm:w-auto cursor-pointer rounded-2xl p-4 sm:p-5 transition-all border-2 ${
                  isSelected ? "bg-white border-emerald-600 shadow-md ring-2 ring-emerald-500/20" : "bg-white border-slate-200 hover:border-emerald-300"
                }"
              >
                <div class="flex items-start justify-between gap-2 mb-3">
                  <div class="flex items-center gap-2.5">
                    <div class="w-10 h-10 rounded-xl flex items-center justify-center text-lg ${
                      isSelected ? "bg-emerald-700 text-gold-400" : "bg-emerald-50 text-emerald-700"
                    }">
                      <i class="fa-solid ${animal.icon}"></i>
                    </div>
                    <div>
                      <span class="text-[10px] font-bold uppercase tracking-wider px-2 py-0.5 rounded-full ${
                        isSelected ? "bg-emerald-100 text-emerald-800" : "bg-slate-100 text-slate-600"
                      }">${escapeHTML(animal.badge)}</span>
                      <h3 class="font-bold text-sm sm:text-base text-slate-900 mt-0.5">${escapeHTML(animal.name)}</h3>
                    </div>
                  </div>
                  ${isSelected ? `<span class="w-6 h-6 rounded-full bg-emerald-600 text-white flex items-center justify-center text-xs shrink-0"><i class="fa-solid fa-check"></i></span>` : `<span class="w-6 h-6 rounded-full border border-slate-300 shrink-0"></span>`}
                </div>

                <p class="text-xs text-slate-500 mb-3 leading-snug">${escapeHTML(animal.subtitle)}</p>

                <div class="pt-3 border-t border-slate-100 flex flex-wrap items-center justify-between gap-2" onclick="event.stopPropagation()">
                  <label class="flex items-center gap-1.5 text-xs font-medium text-slate-600">
                    <span>Nominal (Rp):</span>
                    <input
                      type="number"
                      min="100000"
                      step="50000"
                      value="${animal.targetPrice}"
                      onchange="updateAnimalNominal('${animal.id}', this.value)"
                      class="w-28 sm:w-32 px-2.5 py-1 rounded-lg border border-slate-300 text-xs sm:text-sm font-extrabold text-emerald-800 bg-emerald-50/40 focus:outline-none focus:ring-2 focus:ring-emerald-500"
                    />
                  </label>
                  <span class="text-xs sm:text-sm font-extrabold text-emerald-800">${formatRupiah(animal.targetPrice)}</span>
                </div>
              </div>
            `;
          })
          .join("");
      }

      function updateAnimalNominal(id, newValue) {
        const parsed = Number(newValue);
        if (!parsed || parsed < 100000) {
          showToast("Nominal minimal Rp 100.000.", "error");
          renderAnimalCards();
          return;
        }
        const targetAnimal = appState.animals.find((a) => a.id === id);
        if (!targetAnimal) return;
        targetAnimal.targetPrice = Math.round(parsed);
        syncPatunganWithDeposits();
        saveState();
        renderAll();
        showToast(`Nominal ${targetAnimal.name} diubah menjadi ${formatRupiah(targetAnimal.targetPrice)}`, "success");
      }

      function selectAnimal(id) {
        if (!appState.animals.some((a) => a.id === id)) return;
        appState.selectedAnimalId = id;
        saveState();
        renderAll();
        showToast(`Target diubah ke: ${getActiveAnimal().name}`, "info");
      }

      function openQuickTargetEdit() {
        const panel = document.getElementById("quick-target-panel");
        const input = document.getElementById("quick-active-price-input");
        if (panel) panel.scrollIntoView({ behavior: "smooth", block: "center" });
        if (input) setTimeout(() => { input.focus(); input.select(); }, 250);
      }

      function applyQuickActivePrice() {
        const input = document.getElementById("quick-active-price-input");
        const newPrice = Number(input.value);
        if (!newPrice || newPrice < 100000) {
          showToast("Nominal minimal Rp 100.000.", "error");
          return;
        }
        const animal = getActiveAnimal();
        animal.targetPrice = Math.round(newPrice);
        syncPatunganWithDeposits();
        saveState();
        renderAll();
        showToast(`Target ${animal.name} diperbarui ke ${formatRupiah(animal.targetPrice)}`, "success");
      }

      function renderCalculator() {
        const animal = getActiveAnimal();
        const totalSaved = getTotalSaved();
        const remaining = Math.max(0, (Number(animal.targetPrice) || 0) - totalSaved);
        const daysLeft = getDaysRemaining();
        const weeksLeft = Math.max(1, Math.ceil(daysLeft / 7));
        const monthsLeft = Math.max(1, Math.ceil(daysLeft / 30));

        const daily = remaining > 0 ? Math.ceil(remaining / daysLeft) : 0;
        const weekly = remaining > 0 ? Math.ceil(remaining / weeksLeft) : 0;
        const monthly = remaining > 0 ? Math.ceil(remaining / monthsLeft) : 0;

        document.getElementById("calc-remaining-label").textContent = formatRupiah(remaining);
        document.getElementById("calc-days-left-label").textContent = `${daysLeft} hari`;

        const badge = document.getElementById("calc-status-badge");
        if (remaining === 0) {
          badge.textContent = "Target Lunas (100%)";
          badge.className = "font-bold text-xs text-emerald-300";
        } else {
          badge.textContent = `Kurang ${formatRupiah(remaining)}`;
          badge.className = "font-bold text-xs text-gold-400";
        }

        document.getElementById("plan-daily").textContent = formatRupiah(daily);
        document.getElementById("plan-weekly").textContent = formatRupiah(weekly);
        document.getElementById("plan-monthly").textContent = formatRupiah(monthly);

        document.getElementById("plan-days-count").textContent = daysLeft;
        document.getElementById("plan-weeks-count").textContent = weeksLeft;
        document.getElementById("plan-months-count").textContent = monthsLeft;

        window.__qurbanPlanAmounts = { daily, weekly, monthly };
      }

      function quickFillDeposit(type) {
        const plans = window.__qurbanPlanAmounts || { daily: 10000, weekly: 70000, monthly: 300000 };
        const amt = plans[type] || 50000;
        const input = document.getElementById("deposit-amount");
        if (input) {
          input.value = amt > 0 ? amt : 50000;
          input.scrollIntoView({ behavior: "smooth", block: "center" });
          input.focus();
          showToast(`Nominal ${formatRupiah(input.value)} diisikan.`, "info");
        }
      }

      function focusDepositInput() {
        const personInput = document.getElementById("deposit-person");
        if (personInput) setTimeout(() => personInput.focus(), 250);
      }

      function setPresetAmount(val) {
        const input = document.getElementById("deposit-amount");
        if (!input) return;
        const current = Number(input.value) || 0;
        input.value = current + val;
        input.focus();
      }

      function copyBankAccount() {
        const numEl = document.getElementById("bank-account-number");
        if (!numEl) return;
        const accountNo = numEl.textContent.trim();
        if (navigator.clipboard && window.isSecureContext) {
          navigator.clipboard.writeText(accountNo).then(() => showToast(`Nomor Rekening ${accountNo} Disalin!`, "success"));
        } else {
          showToast(`Nomor Rekening: ${accountNo}`, "info");
        }
      }

      function getUniquePeopleNames() {
        const map = new Map();
        (appState.personSlots || []).forEach((s) => {
          const name = (s.name || "").trim();
          if (name && !map.has(name.toLowerCase())) map.set(name.toLowerCase(), name);
        });
        appState.deposits.forEach((d) => {
          const name = (d.person || "").trim();
          if (name && !map.has(name.toLowerCase())) map.set(name.toLowerCase(), name);
        });
        return Array.from(map.values());
      }

      function renderPersonSelectors() {
        const names = getUniquePeopleNames();
        const filterSelect = document.getElementById("deposit-person-filter");
        if (filterSelect) {
          const prev = filterSelect.value;
          filterSelect.innerHTML = `<option value="">Semua Orang</option>` + names.map((n) => `<option value="${escapeHTML(n)}">${escapeHTML(n)}</option>`).join("");
          if (names.includes(prev)) filterSelect.value = prev;
        }
      }

      function renderDeposits() {
        const tbody = document.getElementById("deposits-table-body");
        if (!tbody) return;

        const filterPerson = (document.getElementById("deposit-person-filter")?.value || "").trim().toLowerCase();
        const searchQuery = (document.getElementById("deposit-search")?.value || "").trim().toLowerCase();

        let filtered = [...appState.deposits].sort((a, b) => b.date.localeCompare(a.date));

        if (filterPerson) {
          filtered = filtered.filter((d) => (d.person || "").trim().toLowerCase() === filterPerson);
        }

        if (searchQuery) {
          filtered = filtered.filter(
            (d) =>
              (d.person || "").toLowerCase().includes(searchQuery) ||
              (d.note || "").toLowerCase().includes(searchQuery) ||
              (d.method || "").toLowerCase().includes(searchQuery)
          );
        }

        const summaryBadge = document.getElementById("chart-summary-badge");
        if (summaryBadge) {
          summaryBadge.textContent = `${filtered.length} Transaksi`;
        }

        if (filtered.length === 0) {
          tbody.innerHTML = `
            <tr>
              <td colspan="5" class="py-6 text-center text-slate-400 font-medium text-xs">
                Belum ada transaksi setoran yang dicatat.
              </td>
            </tr>
          `;
          return;
        }

        tbody.innerHTML = filtered
          .map((dep) => `
            <tr class="hover:bg-slate-50/80 transition">
              <td class="py-2.5 px-2 font-semibold text-slate-700 whitespace-nowrap">${formatDateID(dep.date)}</td>
              <td class="py-2.5 px-2 font-bold text-slate-900 whitespace-nowrap">
                <span class="inline-flex items-center gap-1.5">
                  <span class="w-6 h-6 rounded-full bg-emerald-100 text-emerald-800 text-[10px] font-extrabold flex items-center justify-center shrink-0">
                    ${escapeHTML((dep.person || "P").charAt(0).toUpperCase())}
                  </span>
                  ${escapeHTML(dep.person)}
                </span>
              </td>
              <td class="py-2.5 px-2 text-slate-600">
                <span class="font-medium text-slate-800 block">${escapeHTML(dep.note || "-")}</span>
                <span class="text-[10px] text-slate-400 block">${escapeHTML(dep.method || "Transfer BSI")}</span>
              </td>
              <td class="py-2.5 px-2 text-right font-extrabold text-emerald-700 whitespace-nowrap">+${formatRupiah(dep.amount)}</td>
              <td class="py-2.5 px-2 text-center whitespace-nowrap no-print">
                <div class="flex items-center justify-center gap-1">
                  <button type="button" onclick="openEditDepositModal('${dep.id}')" class="p-1 rounded-lg text-slate-400 hover:text-emerald-700 hover:bg-emerald-50 transition" title="Edit Transaksi">
                    <i class="fa-solid fa-pen-to-square"></i>
                  </button>
                  <button type="button" onclick="confirmDeleteDeposit('${dep.id}')" class="p-1 rounded-lg text-slate-400 hover:text-rose-600 hover:bg-rose-50 transition" title="Hapus Transaksi">
                    <i class="fa-solid fa-trash-can"></i>
                  </button>
                </div>
              </td>
            </tr>
          `)
          .join("");
      }

      function addPersonSlot() {
        const idx = (appState.personSlots || []).length + 1;
        const newName = `Peserta Baru #${idx}`;
        appState.personSlots.push({
          id: "slot-" + Date.now() + "-" + Math.random().toString(36).slice(2, 5),
          name: newName,
        });
        saveState();
        renderPersonSelectors();
        renderPerPersonHistory();
        showToast(`Slot orang "${newName}" ditambahkan. Ubah nama atau langsung isi nominal setoran.`, "info");
      }

      function updatePersonSlotName(slotId, newName) {
        const slot = (appState.personSlots || []).find((s) => s.id === slotId);
        if (!slot) return;
        const oldName = (slot.name || "").trim();
        const cleaned = newName.trim() || "Peserta Qurban";
        slot.name = cleaned;

        if (oldName && oldName.toLowerCase() !== cleaned.toLowerCase()) {
          appState.deposits.forEach((d) => {
            if ((d.person || "").trim().toLowerCase() === oldName.toLowerCase()) {
              d.person = cleaned;
            }
          });
        }

        syncPatunganWithDeposits();
        saveState();
        renderAll();
        showToast(`Nama slot diperbarui menjadi ${cleaned}`, "success");
      }

      function deletePersonSlot(slotId) {
        const slot = (appState.personSlots || []).find((s) => s.id === slotId);
        if (!slot) return;
        const targetName = (slot.name || "").trim();

        showConfirmModal({
          title: "Hapus Slot Orang",
          message: `Hapus slot "${targetName}" beserta seluruh riwayat transaksinya?`,
          okText: "Hapus Slot",
          onConfirm: () => {
            appState.personSlots = (appState.personSlots || []).filter((s) => s.id !== slotId);
            if (targetName) {
              appState.deposits = appState.deposits.filter((d) => (d.person || "").trim().toLowerCase() !== targetName.toLowerCase());
            }
            syncPatunganWithDeposits();
            saveState();
            renderAll();
            showToast(`Slot "${targetName}" dihapus.`, "info");
          },
        });
      }

      function addQuickPersonDeposit(slotId) {
        const slot = (appState.personSlots || []).find((s) => s.id === slotId);
        if (!slot) return;
        const amtInput = document.getElementById(`quick-person-amt-${slotId}`);
        const noteInput = document.getElementById(`quick-person-note-${slotId}`);
        const amount = Number(amtInput ? amtInput.value : 0);
        const note = noteInput ? noteInput.value.trim() : "";

        if (!amount || amount < 1000) {
          showToast("Masukkan nominal setoran minimal Rp 1.000.", "error");
          if (amtInput) amtInput.focus();
          return;
        }

        const todayStr = new Date().toISOString().split("T")[0];
        appState.deposits.push({
          id: "dep-" + Date.now() + "-" + Math.random().toString(36).slice(2, 5),
          person: slot.name,
          amount: Math.round(amount),
          date: todayStr,
          method: "Transfer Bank (BSI)",
          note: note || `Setoran ${slot.name}`,
        });

        syncPatunganWithDeposits();
        saveState();
        renderAll();
        showToast(`Nominal ${slot.name} bertambah +${formatRupiah(amount)}!`, "success");
      }

      function renderPerPersonHistory() {
        const container = document.getElementById("per-person-history-container");
        const countBadge = document.getElementById("person-summary-count");
        if (!container) return;

        ensurePersonSlotsSynced();

        const depositsByPerson = new Map();
        const sortedDeposits = [...appState.deposits].sort((a, b) => b.date.localeCompare(a.date));

        sortedDeposits.forEach((dep) => {
          const name = (dep.person || "Peserta Qurban").trim();
          const key = name.toLowerCase();
          if (!depositsByPerson.has(key)) {
            depositsByPerson.set(key, { total: 0, items: [] });
          }
          const bucket = depositsByPerson.get(key);
          bucket.total += Number(dep.amount) || 0;
          bucket.items.push(dep);
        });

        const slots = appState.personSlots || [];
        if (countBadge) countBadge.textContent = `${slots.length} Orang`;

        if (slots.length === 0) {
          container.innerHTML = `<div class="col-span-full py-6 text-center text-xs text-slate-400">Belum ada slot orang. Klik "Tambah Slot Orang" atau isi setoran baru.</div>`;
          return;
        }

        const activeTarget = Math.max(1, Number(getActiveAnimal()?.targetPrice) || 3000000);

        container.innerHTML = slots
          .map((slot) => {
            const key = (slot.name || "").trim().toLowerCase();
            const bucket = depositsByPerson.get(key) || { total: 0, items: [] };
            const pct = Math.min(100, Math.round((bucket.total / activeTarget) * 100));

            const txRows =
              bucket.items.length > 0
                ? bucket.items
                    .map(
                      (tx) => `
                        <div class="flex items-center justify-between gap-2 text-[11px] py-1.5 border-b border-slate-100 last:border-b-0">
                          <div class="min-w-0">
                            <p class="font-semibold text-slate-700 truncate">${escapeHTML(tx.note)}</p>
                            <span class="text-[10px] text-slate-400">${formatDateID(tx.date)} • ${escapeHTML(tx.method)}</span>
                          </div>
                          <div class="flex items-center gap-1.5 shrink-0">
                            <span class="font-extrabold text-emerald-700">+${formatRupiah(tx.amount)}</span>
                            <button type="button" onclick="confirmDeleteDeposit('${tx.id}')" class="text-slate-400 hover:text-rose-600 p-0.5 no-print" title="Hapus transaksi">
                              <i class="fa-solid fa-xmark"></i>
                            </button>
                          </div>
                        </div>
                      `
                    )
                    .join("")
                : `<p class="text-[11px] text-slate-400 py-2 text-center">Belum ada transaksi setoran untuk slot ini.</p>`;

            return `
              <div class="rounded-2xl border border-slate-200 bg-slate-50/60 p-3.5 flex flex-col justify-between gap-2.5">
                <div>
                  <div class="flex items-start justify-between gap-2 mb-2">
                    <div class="flex items-center gap-2 min-w-0 flex-1">
                      <div class="w-8 h-8 rounded-xl bg-emerald-700 text-gold-400 flex items-center justify-center text-xs font-extrabold shrink-0">
                        ${escapeHTML((slot.name || "P").charAt(0).toUpperCase())}
                      </div>
                      <div class="min-w-0 flex-1">
                        <input
                          type="text"
                          value="${escapeHTML(slot.name)}"
                          onchange="updatePersonSlotName('${slot.id}', this.value)"
                          aria-label="Nama slot orang"
                          class="w-full font-bold text-xs sm:text-sm text-slate-900 bg-white/80 hover:bg-white focus:bg-white border border-transparent hover:border-slate-200 focus:border-emerald-500 rounded-lg px-1.5 py-0.5 focus:outline-none truncate"
                        />
                        <span class="text-[10px] font-semibold text-slate-500 px-1.5">${bucket.items.length} transaksi</span>
                      </div>
                    </div>
                    <div class="flex items-start gap-2 shrink-0">
                      <div class="text-right">
                        <span class="block text-xs sm:text-sm font-extrabold text-emerald-800">${formatRupiah(bucket.total)}</span>
                        <span class="text-[10px] font-bold text-gold-700">${pct}% target</span>
                      </div>
                      <button type="button" onclick="deletePersonSlot('${slot.id}')" class="min-h-[30px] px-2 py-1 rounded-lg bg-rose-50 hover:bg-rose-100 text-rose-600 border border-rose-200 text-[10px] font-bold transition no-print" title="Hapus Slot Orang">
                        <i class="fa-solid fa-trash-can"></i> Hapus
                      </button>
                    </div>
                  </div>

                  <div class="w-full h-1.5 bg-slate-200 rounded-full overflow-hidden">
                    <div class="h-full bg-gradient-to-r from-emerald-600 to-gold-500 rounded-full" style="width: ${pct}%"></div>
                  </div>
                </div>

                <!-- Form Setor Langsung di Slot Orang -->
                <div class="flex items-center gap-1.5 no-print">
                  <input type="number" id="quick-person-amt-${slot.id}" min="1000" step="1000" placeholder="Nominal (Rp)" class="w-28 px-2 py-1.5 rounded-lg border border-slate-300 bg-white text-[11px] font-bold text-slate-800 focus:outline-none focus:ring-2 focus:ring-emerald-500" />
                  <input type="text" id="quick-person-note-${slot.id}" placeholder="Catatan..." class="flex-1 min-w-0 px-2 py-1.5 rounded-lg border border-slate-300 bg-white text-[11px] text-slate-800 focus:outline-none focus:ring-2 focus:ring-emerald-500" />
                  <button type="button" onclick="addQuickPersonDeposit('${slot.id}')" class="px-2.5 py-1.5 rounded-lg bg-emerald-700 hover:bg-emerald-800 text-white text-[11px] font-bold transition shrink-0">
                    + Setor
                  </button>
                </div>

                <div class="bg-white rounded-xl p-2.5 border border-slate-100 max-h-36 overflow-y-auto">
                  ${txRows}
                </div>
              </div>
            `;
          })
          .join("");
      }

      function openEditDepositModal(id) {
        const dep = appState.deposits.find((d) => d.id === id);
        if (!dep) return;
        document.getElementById("edit-deposit-id").value = dep.id;
        document.getElementById("edit-deposit-person").value = dep.person || "";
        document.getElementById("edit-deposit-amount").value = dep.amount;
        document.getElementById("edit-deposit-date").value = dep.date;
        document.getElementById("edit-deposit-method").value = dep.method || "Transfer Bank (BSI)";
        document.getElementById("edit-deposit-note").value = dep.note || "";

        const modal = document.getElementById("edit-deposit-modal");
        modal.classList.remove("hidden");
        modal.classList.add("flex");
      }

      function closeEditDepositModal() {
        const modal = document.getElementById("edit-deposit-modal");
        modal.classList.add("hidden");
        modal.classList.remove("flex");
      }

      document.getElementById("edit-deposit-form").addEventListener("submit", function (e) {
        e.preventDefault();
        const id = document.getElementById("edit-deposit-id").value;
        const person = document.getElementById("edit-deposit-person").value.trim();
        const amount = Number(document.getElementById("edit-deposit-amount").value);
        const date = document.getElementById("edit-deposit-date").value;
        const method = document.getElementById("edit-deposit-method").value;
        const note = document.getElementById("edit-deposit-note").value.trim();

        const idx = appState.deposits.findIndex((d) => d.id === id);
        if (idx !== -1 && person && amount >= 1000 && date) {
          appState.deposits[idx] = { id, person, amount: Math.round(amount), date, method, note: note || method };
          syncPatunganWithDeposits();
          saveState();
          closeEditDepositModal();
          renderAll();
          showToast(`Transaksi ${person} berhasil diperbarui.`, "success");
        } else {
          showToast("Nama penyetor, nominal, dan tanggal wajib diisi.", "error");
        }
      });

      document.getElementById("deposit-form").addEventListener("submit", function (e) {
        e.preventDefault();
        const personInput = document.getElementById("deposit-person");
        const amountInput = document.getElementById("deposit-amount");
        const dateInput = document.getElementById("deposit-date");
        const methodInput = document.getElementById("deposit-method");
        const noteInput = document.getElementById("deposit-note");

        const person = (personInput?.value || "").trim();
        const amount = Number(amountInput?.value);
        const date = dateInput?.value || new Date().toISOString().split("T")[0];
        const method = methodInput?.value || "Transfer Bank (BSI)";
        const note = (noteInput?.value || "").trim();

        if (!person) {
          showToast("Nama orang / penyetor wajib diisi.", "error");
          if (personInput) personInput.focus();
          return;
        }

        if (!amount || amount < 1000) {
          showToast("Nominal setoran minimal Rp 1.000.", "error");
          if (amountInput) amountInput.focus();
          return;
        }

        appState.deposits.push({
          id: "dep-" + Date.now() + "-" + Math.random().toString(36).slice(2, 6),
          person,
          amount: Math.round(amount),
          date,
          method,
          note: note || `Setoran ${person}`,
        });

        ensurePersonSlotsSynced();
        syncPatunganWithDeposits();
        saveState();
        renderAll();

        if (amountInput) amountInput.value = "";
        if (noteInput) noteInput.value = "";
        showToast(`Setoran ${formatRupiah(amount)} atas nama ${person} berhasil disimpan!`, "success");
      });

      function confirmDeleteDeposit(id) {
        const dep = appState.deposits.find((d) => d.id === id);
        if (!dep) return;
        showConfirmModal({
          title: "Hapus Setoran",
          message: `Hapus setoran ${formatRupiah(dep.amount)} atas nama ${dep.person}?`,
          okText: "Hapus",
          onConfirm: () => {
            appState.deposits = appState.deposits.filter((d) => d.id !== id);
            syncPatunganWithDeposits();
            saveState();
            renderAll();
            showToast("Setoran dihapus & riwayat orang diperbarui.", "info");
          },
        });
      }

      function confirmClearAllDeposits() {
        if (appState.deposits.length === 0) return;
        showConfirmModal({
          title: "Reset Semua Setoran",
          message: "Hapus seluruh riwayat transaksi semua orang?",
          okText: "Hapus Semua",
          onConfirm: () => {
            appState.deposits = [];
            syncPatunganWithDeposits();
            saveState();
            renderAll();
            showToast("Riwayat setoran dibersihkan.", "info");
          },
        });
      }

      function renderChart() {
        const canvas = document.getElementById("savingsChart");
        if (!canvas || typeof Chart === "undefined") return;

        const animal = getActiveAnimal();
        const targetVal = Number(animal.targetPrice) || 0;
        const sorted = [...appState.deposits].sort((a, b) => a.date.localeCompare(b.date));

        let runningTotal = 0;
        const labels = ["Awal"];
        const cumulativeData = [0];

        sorted.forEach((dep) => {
          runningTotal += Number(dep.amount) || 0;
          labels.push(`${formatDateID(dep.date)} (${dep.person || "Peserta"})`);
          cumulativeData.push(runningTotal);
        });

        const targetLine = labels.map(() => targetVal);

        if (savingsChartInstance) savingsChartInstance.destroy();

        const ctx = canvas.getContext("2d");
        savingsChartInstance = new Chart(ctx, {
          type: "line",
          data: {
            labels,
            datasets: [
              {
                label: "Terkumpul (Rp)",
                data: cumulativeData,
                borderColor: "#059669",
                backgroundColor: "rgba(16, 185, 129, 0.12)",
                borderWidth: 2.5,
                fill: true,
                tension: 0.3,
              },
              {
                label: "Target (Rp)",
                data: targetLine,
                borderColor: "#d97706",
                borderDash: [5, 5],
                borderWidth: 2,
                pointRadius: 0,
                fill: false,
              },
            ],
          },
          options: {
            responsive: true,
            maintainAspectRatio: false,
            plugins: { legend: { position: "bottom", labels: { font: { size: 10 } } } },
            scales: {
              y: { beginAtZero: true, ticks: { font: { size: 9 } } },
              x: { ticks: { font: { size: 9 } } },
            },
          },
        });
      }

      function renderPatungan() {
        const grid = document.getElementById("patungan-slots-grid");
        if (!grid) return;

        const sapiAnimal = appState.animals.find((a) => a.id === "sapi-1-7");
        const sharePrice = Math.max(100000, Number(sapiAnimal?.targetPrice) || 3000000);
        const members = appState.patungan || [];
        const totalCollected = members.reduce((sum, m) => sum + (Number(m.paidAmount) || 0), 0);
        const totalSlots = members.length;
        const cowCount = Math.max(1, Math.ceil(totalSlots / 7));
        const totalTarget = sharePrice * totalSlots;
        const remaining = Math.max(0, totalTarget - totalCollected);
        const pct = totalTarget > 0 ? Math.min(100, Math.round((totalCollected / totalTarget) * 100)) : 0;
        const paidFullCount = members.filter((m) => (Number(m.paidAmount) || 0) >= sharePrice).length;

        const cowCountLabel = document.getElementById("patungan-cow-count-label");
        const totalSlotsLabel = document.getElementById("patungan-total-slots-label");
        if (cowCountLabel) cowCountLabel.textContent = `${cowCount} Ekor`;
        if (totalSlotsLabel) totalSlotsLabel.textContent = `${totalSlots} Slot`;

        document.getElementById("patungan-full-cow-target").textContent = formatRupiah(totalTarget);
        document.getElementById("patungan-per-share-target").textContent = formatRupiah(sharePrice);
        document.getElementById("patungan-total-collected").textContent = formatRupiah(totalCollected);
        document.getElementById("patungan-pct-label").textContent = `${pct}% dari target`;
        document.getElementById("patungan-slot-summary").textContent = `${paidFullCount} / ${totalSlots} Slot`;
        document.getElementById("patungan-paid-count").textContent = `${paidFullCount} lunas`;
        document.getElementById("patungan-remaining").textContent = formatRupiah(remaining);
        document.getElementById("patungan-bar-pct").textContent = `${pct}%`;
        document.getElementById("patungan-progress-bar").style.width = `${pct}%`;

        grid.innerHTML = members
          .map((m, idx) => {
            const paid = Math.max(0, Number(m.paidAmount) || 0);
            const target = sharePrice;
            const mPct = Math.min(100, Math.round((paid / target) * 100));
            const isLunas = paid >= target;
            const cowNumber = Math.floor(idx / 7) + 1;
            const bagianNumber = (idx % 7) + 1;

            return `
              <div class="snap-center shrink-0 w-[75vw] sm:w-auto rounded-2xl border p-3.5 flex flex-col justify-between ${
                isLunas ? "bg-emerald-50/70 border-emerald-300" : "bg-white border-slate-200"
              }">
                <div>
                  <div class="flex items-center justify-between gap-1 mb-1">
                    <span class="text-[9px] font-bold text-slate-500 uppercase">Sapi #${cowNumber} • Bagian ${bagianNumber}/7</span>
                    <div class="flex items-center gap-1">
                      <span class="text-[9px] font-bold px-1.5 py-0.5 rounded ${isLunas ? "bg-emerald-600 text-white" : "bg-gold-100 text-gold-700"}">${isLunas ? "LUNAS" : "MENYETOR"}</span>
                      <button type="button" onclick="deletePatunganSlot(${m.id})" class="text-slate-400 hover:text-rose-600 p-0.5 no-print" title="Hapus Slot Patungan">
                        <i class="fa-solid fa-trash-can text-[10px]"></i>
                      </button>
                    </div>
                  </div>
                  <input
                    type="text"
                    value="${escapeHTML(m.name)}"
                    onchange="updatePatunganMemberName(${m.id}, this.value)"
                    class="w-full font-bold text-xs text-slate-900 bg-transparent border-b border-transparent hover:border-slate-300 focus:border-emerald-600 focus:outline-none py-0.5"
                  />
                  <p class="text-[10px] text-slate-500 truncate mt-0.5">${escapeHTML(m.note || "")}</p>
                </div>
                
                <div class="my-2 space-y-1">
                  <div class="flex justify-between text-[11px] font-bold">
                    <span class="text-emerald-800">${formatRupiah(paid)}</span>
                    <span class="text-slate-500">${mPct}%</span>
                  </div>
                  <div class="w-full h-1.5 bg-slate-100 rounded-full overflow-hidden">
                    <div class="h-full bg-emerald-600 rounded-full" style="width: ${mPct}%"></div>
                  </div>
                </div>

                <div class="flex items-center gap-1 mt-1 no-print">
                  <button type="button" onclick="recordPatunganDeposit(${m.id}, 100000)" class="flex-1 py-1 rounded bg-slate-100 hover:bg-emerald-50 text-[10px] font-bold text-slate-700 hover:text-emerald-800 border border-slate-200 transition">
                    +100rb
                  </button>
                  <button type="button" onclick="recordPatunganPelunasan(${m.id})" class="flex-1 py-1 rounded bg-emerald-700 hover:bg-emerald-800 text-[10px] font-bold text-white transition">
                    Pelunasan
                  </button>
                </div>
              </div>
            `;
          })
          .join("");
      }

      function deletePatunganSlot(id) {
        const member = (appState.patungan || []).find((m) => m.id === id);
        if (!member) return;
        showConfirmModal({
          title: "Hapus Slot Patungan",
          message: `Hapus slot patungan "${member.name}" dari daftar Sapi 1/7?`,
          okText: "Hapus Slot",
          onConfirm: () => {
            appState.patungan = (appState.patungan || []).filter((m) => m.id !== id);
            syncPatunganWithDeposits();
            saveState();
            renderAll();
            showToast(`Slot patungan "${member.name}" dihapus.`, "info");
          },
        });
      }

      function updatePatunganMemberName(id, value) {
        const member = appState.patungan.find((m) => m.id === id);
        if (!member) return;
        const cleanName = value.trim() || `Peserta #${id}`;
        member.name = cleanName;
        syncPatunganWithDeposits();
        saveState();
        renderAll();
      }

      function recordPatunganDeposit(id, amount) {
        const member = appState.patungan.find((m) => m.id === id);
        if (!member) return;
        const todayStr = new Date().toISOString().split("T")[0];
        appState.deposits.push({
          id: "dep-" + Date.now(),
          person: member.name,
          amount: Math.round(amount),
          date: todayStr,
          method: "Transfer Bank (BSI)",
          note: `Setoran patungan Sapi 1/7 (${member.name})`,
        });
        syncPatunganWithDeposits();
        saveState();
        renderAll();
        showToast(`Setoran ${formatRupiah(amount)} untuk ${member.name} tercatat & riwayat diperbarui!`, "success");
      }

      function recordPatunganPelunasan(id) {
        const member = appState.patungan.find((m) => m.id === id);
        if (!member) return;
        const sapiAnimal = appState.animals.find((a) => a.id === "sapi-1-7");
        const sharePrice = Math.max(100000, Number(sapiAnimal?.targetPrice) || 3000000);
        const sisa = Math.max(0, sharePrice - (Number(member.paidAmount) || 0));
        if (sisa <= 0) {
          showToast(`${member.name} sudah lunas.`, "info");
          return;
        }
        const todayStr = new Date().toISOString().split("T")[0];
        appState.deposits.push({
          id: "dep-" + Date.now(),
          person: member.name,
          amount: sisa,
          date: todayStr,
          method: "Transfer Bank (BSI)",
          note: `Pelunasan patungan Sapi 1/7 (${member.name})`,
        });
        syncPatunganWithDeposits();
        saveState();
        renderAll();
        showToast(`${member.name} LUNAS (+${formatRupiah(sisa)}) & riwayat diperbarui!`, "success");
      }

      function addPatunganMember() {
        const sapiAnimal = appState.animals.find((a) => a.id === "sapi-1-7");
        const sharePrice = Math.max(100000, Number(sapiAnimal?.targetPrice) || 3000000);
        const nextIdx = appState.patungan.length;
        const cowNum = Math.floor(nextIdx / 7) + 1;
        const partNum = (nextIdx % 7) + 1;
        appState.patungan.push({
          id: Date.now() + Math.floor(Math.random() * 1000),
          name: `Peserta Sapi #${cowNum} (${partNum}/7)`,
          paidAmount: 0,
          targetAmount: sharePrice,
          note: `Slot Sapi #${cowNum} bagian ke-${partNum}`,
        });
        syncPatunganWithDeposits();
        saveState();
        renderPatungan();
        renderPersonSelectors();
        showToast(`Slot baru ditambahkan (Sapi #${cowNum} • Bagian ${partNum}/7).`, "success");
      }

      function confirmResetPatungan() {
        showConfirmModal({
          title: "Reset Grup Patungan",
          message: "Kembalikan slot grup patungan ke data awal?",
          okText: "Reset",
          onConfirm: () => {
            const sapiAnimal = appState.animals.find((a) => a.id === "sapi-1-7");
            const sharePrice = Math.max(100000, Number(sapiAnimal?.targetPrice) || 3000000);
            appState.patungan = createDefaultPatungan(sharePrice);
            syncPatunganWithDeposits();
            saveState();
            renderAll();
            showToast("Grup patungan direset.", "info");
          },
        });
      }

      function exportReport() {
        showToast("Menyiapkan dokumen...", "info");
        setTimeout(() => window.print(), 300);
      }

      /**
       * Mendeteksi jenis perangkat (HP, Tablet, Laptop, PC) serta mereknya
       * berdasarkan User-Agent, Client Hints, dan karakteristik layar/pointer.
       */
      async function detectDeviceProfile() {
        const ua = navigator.userAgent || "";
        const platform = navigator.platform || "";
        const width = window.innerWidth || screen.width || 1280;
        const maxTouch = navigator.maxTouchPoints || 0;
        const isCoarsePointer = typeof window.matchMedia === "function" && window.matchMedia("(pointer: coarse)").matches;
        const isTouchDevice = maxTouch > 0 || isCoarsePointer || "ontouchstart" in window;

        let brand = "Umum";
        if (/iPhone/i.test(ua)) {
          brand = "Apple iPhone";
        } else if (/iPad/i.test(ua) || (/Mac/i.test(ua) && maxTouch > 1)) {
          brand = "Apple iPad";
        } else if (/Macintosh|Mac OS X/i.test(ua)) {
          brand = "Apple Mac";
        } else if (/SM-|GT-|SCH-|SGH-|Galaxy/i.test(ua)) {
          brand = "Samsung";
        } else if (/POCO/i.test(ua)) {
          brand = "Xiaomi POCO";
        } else if (/Redmi/i.test(ua)) {
          brand = "Xiaomi Redmi";
        } else if (/Mi\s|Xiaomi/i.test(ua)) {
          brand = "Xiaomi";
        } else if (/CPH|OPPO/i.test(ua)) {
          brand = "OPPO";
        } else if (/vivo|V2\d{3}|PD\d{4}/i.test(ua)) {
          brand = "Vivo";
        } else if (/RMX|realme/i.test(ua)) {
          brand = "Realme";
        } else if (/HUAWEI|HONOR|ANE-|LIO-|TAS-|NOH-/i.test(ua)) {
          brand = "Huawei";
        } else if (/ASUS|ROG|ZenBook|VivoBook/i.test(ua)) {
          brand = "ASUS";
        } else if (/Lenovo|ThinkPad|IdeaPad|Legion/i.test(ua)) {
          brand = "Lenovo";
        } else if (/HP\s|Pavilion|EliteBook|ProBook|Victus|OMEN/i.test(ua)) {
          brand = "HP";
        } else if (/Dell|Latitude|Inspiron|XPS/i.test(ua)) {
          brand = "Dell";
        } else if (/Acer|Aspire|Predator|Swift/i.test(ua)) {
          brand = "Acer";
        } else if (/MSI/i.test(ua)) {
          brand = "MSI";
        } else if (/Android/i.test(ua)) {
          brand = "Android";
        } else if (/Windows/i.test(ua)) {
          brand = "Windows";
        } else if (/Linux/i.test(ua)) {
          brand = "Linux";
        }

        if (navigator.userAgentData && typeof navigator.userAgentData.getHighEntropyValues === "function") {
          try {
            const hints = await navigator.userAgentData.getHighEntropyValues(["model", "platform"]);
            const model = (hints.model || "").trim();
            if (model) {
              if (/^SM-|^Galaxy/i.test(model)) brand = `Samsung (${model})`;
              else if (/POCO/i.test(model)) brand = `Xiaomi POCO (${model})`;
              else if (/Redmi/i.test(model)) brand = `Xiaomi Redmi (${model})`;
              else if (/CPH/i.test(model)) brand = `OPPO (${model})`;
              else if (/RMX/i.test(model)) brand = `Realme (${model})`;
              else brand = model;
            }
          } catch (_) {
            // Abaikan jika browser membatasi high-entropy hints
          }
        }

        let deviceType = "PC";
        let iconClass = "fa-desktop";

        if (/iPhone|Mobile|Android.*Mobile|Windows Phone/i.test(ua) || (isTouchDevice && width < 640)) {
          deviceType = "HP";
          iconClass = "fa-mobile-screen-button";
        } else if (/iPad|Tablet|PlayBook|Silk/i.test(ua) || (/Android/i.test(ua) && !/Mobile/i.test(ua)) || (isTouchDevice && width >= 640 && width <= 1024)) {
          deviceType = "Tablet";
          iconClass = "fa-tablet-screen-button";
        } else if (/MacBook|Laptop|Chromebook/i.test(ua) || (!isTouchDevice && width >= 1024 && width <= 1440) || (/Windows|Macintosh|Linux/i.test(ua || platform) && width < 1440)) {
          deviceType = "Laptop";
          iconClass = "fa-laptop";
        } else {
          deviceType = "PC";
          iconClass = "fa-desktop";
        }

        document.body.setAttribute("data-device-type", deviceType);
        document.body.setAttribute("data-device-brand", brand);

        const badgeText = document.getElementById("device-profile-text");
        const badgeIcon = document.getElementById("device-profile-icon");
        if (badgeText) badgeText.textContent = `${deviceType} • ${brand}`;
        if (badgeIcon) badgeIcon.className = `fa-solid ${iconClass} text-emerald-600`;
      }

      function confirmResetDemo() {
        showConfirmModal({
          title: "Reset Demo",
          message: "Semua data akan dikembalikan ke setelan pabrik demo. Lanjutkan?",
          okText: "Reset Demo",
          onConfirm: () => {
            appState = createDemoState();
            syncPatunganWithDeposits();
            saveState();
            renderAll();
            updateCountdown();
            showToast("Aplikasi dikembalikan ke data demo.", "info");
          },
        });
      }

      function renderAll() {
        ensurePersonSlotsSynced();
        syncPatunganWithDeposits();
        renderQuote();
        renderHeroAndSummary();
        renderAnimalCards();
        renderCalculator();
        renderPersonSelectors();
        renderDeposits();
        renderPerPersonHistory();
        renderPatungan();
        renderChart();
      }

      document.addEventListener("DOMContentLoaded", function () {
        const dateInput = document.getElementById("deposit-date");
        if (dateInput && !dateInput.value) {
          dateInput.value = new Date().toISOString().split("T")[0];
        }

        detectDeviceProfile();
        window.addEventListener("resize", detectDeviceProfile);

        renderAll();
        updateCountdown();

        if (countdownInterval) clearInterval(countdownInterval);
        countdownInterval = setInterval(updateCountdown, 1000);
      });
    </script>
  </body>
</html>
