<!DOCTYPE html>
<html lang="pl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>LOCK IN - Dziennik & Nawykometr</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Chakra+Petch:wght@500;700;900&family=Inter:wght@400;600;800&display=swap');
        
        body {
            font-family: 'Inter', sans-serif;
            background-color: #090a0f;
            color: #e2e8f0;
            background-image: 
                radial-gradient(circle at 50% 0%, rgba(20, 184, 166, 0.08) 0%, transparent 70%),
                linear-gradient(rgba(255, 255, 255, 0.02) 1px, transparent 1px),
                linear-gradient(90deg, rgba(255, 255, 255, 0.02) 1px, transparent 1px);
            background-size: 100% 100%, 30px 30px, 30px 30px;
        }

        .font-tech {
            font-family: 'Chakra Petch', sans-serif;
        }

        .glow-green {
            box-shadow: 0 0 20px rgba(16, 185, 129, 0.25);
        }

        .glow-red {
            box-shadow: 0 0 30px rgba(239, 68, 68, 0.5);
            animation: pulse-red 2s infinite;
        }

        @keyframes pulse-red {
            0%, 100% { box-shadow: 0 0 20px rgba(239, 68, 68, 0.4); transform: scale(1); }
            50% { box-shadow: 0 0 40px rgba(239, 68, 68, 0.8); transform: scale(1.02); }
        }

        .glass-panel {
            background: rgba(15, 23, 42, 0.75);
            backdrop-filter: blur(12px);
            border: 1px solid rgba(255, 255, 255, 0.08);
        }

        .modal-blur {
            backdrop-filter: blur(16px);
        }

        /* Custom Scrollbar */
        ::-webkit-scrollbar {
            width: 6px;
        }
        ::-webkit-scrollbar-track {
            background: #090a0f;
        }
        ::-webkit-scrollbar-thumb {
            background: #334155;
            border-radius: 3px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #10b981;
        }
    </style>
</head>
<body class="min-h-screen pb-12 flex flex-col justify-between">
    <div>
        <!-- TOP BAR -->
        <header class="border-b border-slate-800 glass-panel sticky top-0 z-30">
            <div class="max-w-6xl mx-auto px-4 py-3 flex flex-wrap justify-between items-center gap-4">
                <div class="flex items-center space-x-3">
                    <div class="w-3 h-3 rounded-full bg-emerald-500 animate-ping"></div>
                    <h1 class="text-2xl font-black font-tech tracking-wider text-transparent bg-clip-text bg-gradient-to-r from-emerald-400 to-teal-200">
                        LOCK IN <span class="text-xs px-2 py-0.5 bg-emerald-950 text-emerald-400 border border-emerald-800 rounded font-mono">PRO</span>
                    </h1>
                </div>

                <!-- STREAK & TIMER -->
                <div class="flex items-center gap-6">
                    <div class="text-center">
                        <div class="text-xs uppercase tracking-widest text-slate-500 font-mono">Seria Lock In</div>
                        <div class="text-xl font-tech font-bold text-emerald-400 flex items-center justify-center gap-1">
                            <i class="fa-solid fa-fire text-amber-500"></i> <span id="streak-count">0</span> DNI
                        </div>
                    </div>

                    <div class="text-center border-l border-slate-800 pl-6">
                        <div class="text-xs uppercase tracking-widest text-slate-500 font-mono">Reset za (00:00)</div>
                        <div class="text-xl font-tech font-bold text-slate-200 font-mono" id="midnight-timer">00:00:00</div>
                    </div>
                </div>
            </div>
        </header>

        <!-- MAIN CONTAINER -->
        <main class="max-w-6xl mx-auto px-4 mt-8 grid grid-cols-1 lg:grid-cols-12 gap-8">
            
            <!-- EMERGENCY LOCK IN BUTTON (FULL WIDTH TOP BANNER) -->
            <div class="lg:col-span-12">
                <div class="p-6 rounded-2xl bg-gradient-to-r from-red-950 via-slate-900 to-slate-950 border-2 border-red-600/50 flex flex-col md:flex-row items-center justify-between gap-6 glow-red">
                    <div class="space-y-1 text-center md:text-left">
                        <div class="flex items-center gap-2 text-red-500 font-mono text-xs uppercase tracking-widest justify-center md:justify-start">
                            <i class="fa-solid fa-triangle-exclamation animate-bounce"></i> Protokół Awaryjny Impulsu
                        </div>
                        <h2 class="text-2xl font-tech font-black text-white uppercase tracking-wide">
                            Czujesz chęć na epizod / objadanie?
                        </h2>
                        <p class="text-slate-400 text-sm max-w-xl">
                            Kliknij zanim otworzysz lodówkę. Daj sobie 15 minut i zobacz twardą rzeczywistość. Nie daj się złamać.
                        </p>
                    </div>

                    <button id="btn-emergency" onclick="openEmergencyModal()" class="w-full md:w-auto px-8 py-4 bg-red-600 hover:bg-red-500 text-white font-tech font-black text-lg tracking-wider uppercase rounded-xl transition duration-200 shadow-lg flex items-center justify-center gap-3 active:scale-95">
                        <i class="fa-solid fa-skull text-xl"></i> POMOC / IMPULS!
                    </button>
                </div>
            </div>

            <!-- LEFT COLUMN: TODAY'S TARGETS & CHECKLIST -->
            <section class="lg:col-span-7 space-y-6">
                <div class="glass-panel p-6 rounded-2xl border border-slate-800">
                    <div class="flex justify-between items-center mb-6">
                        <div>
                            <span class="text-xs text-emerald-400 font-mono uppercase tracking-widest">Dzisiejsza Misja</span>
                            <h2 class="text-xl font-tech font-bold text-white flex items-center gap-2">
                                <i class="fa-solid fa-list-check text-emerald-500"></i> Cele na Dziś
                            </h2>
                        </div>
                        <span id="display-date" class="text-xs font-mono bg-slate-800 text-slate-300 px-3 py-1 rounded-full border border-slate-700">
                            --.--.----
                        </span>
                    </div>

                    <!-- Add Goal Input -->
                    <form onsubmit="addCustomTarget(event)" class="flex gap-2 mb-6">
                        <input type="text" id="new-target-input" placeholder="Dodaj własny cel (np. Trening FBW, 3L wody)..." 
                               class="flex-1 bg-slate-900 border border-slate-700 rounded-xl px-4 py-2.5 text-sm text-slate-200 focus:outline-none focus:border-emerald-500 transition">
                        <button type="submit" class="px-4 py-2.5 bg-emerald-600 hover:bg-emerald-500 text-slate-950 font-bold rounded-xl text-sm font-tech uppercase tracking-wider transition">
                            <i class="fa-solid fa-plus"></i> Dodaj
                        </button>
                    </form>

                    <!-- Checklist -->
                    <div id="targets-list" class="space-y-3">
                        <!-- Rendered via JS -->
                    </div>

                    <!-- Progress Bar -->
                    <div class="mt-6 pt-4 border-t border-slate-800">
                        <div class="flex justify-between text-xs font-mono text-slate-400 mb-2">
                            <span>Postęp celów</span>
                            <span id="progress-percent">0%</span>
                        </div>
                        <div class="w-full bg-slate-900 h-2.5 rounded-full overflow-hidden border border-slate-800">
                            <div id="progress-bar" class="bg-gradient-to-r from-emerald-500 to-teal-300 h-full w-0 transition-all duration-500"></div>
                        </div>
                    </div>
                </div>

                <!-- DAILY JOURNAL / REFLECTION -->
                <div class="glass-panel p-6 rounded-2xl border border-slate-800">
                    <div class="flex justify-between items-center mb-4">
                        <div>
                            <span class="text-xs text-emerald-400 font-mono uppercase tracking-widest">Asekuracja Umysłu</span>
                            <h2 class="text-xl font-tech font-bold text-white flex items-center gap-2">
                                <i class="fa-solid fa-book-bookmark text-emerald-500"></i> Dziennik & Notatki Posiłkowe
                            </h2>
                        </div>
                        <span class="text-xs text-slate-500 font-mono flex items-center gap-1">
                            <i class="fa-solid fa-cloud-arrow-up text-emerald-500"></i> Auto-zapis
                        </span>
                    </div>

                    <textarea id="daily-journal" oninput="saveJournal()" rows="6" 
                              placeholder="Zapisz jak się dzisiaj czujesz, co jadłeś/aś, co wywołało stres lub jakie pokusy zwalczyłeś/aś..." 
                              class="w-full bg-slate-900/90 border border-slate-800 rounded-xl p-4 text-sm text-slate-200 focus:outline-none focus:border-emerald-500 focus:ring-1 focus:ring-emerald-500 transition resize-none font-sans leading-relaxed"></textarea>
                </div>
            </section>

            <!-- RIGHT COLUMN: CALENDAR & HISTORY LOG -->
            <section class="lg:col-span-5 space-y-6">
                <!-- CALENDAR CARD -->
                <div class="glass-panel p-6 rounded-2xl border border-slate-800">
                    <div class="flex justify-between items-center mb-4">
                        <h2 class="text-lg font-tech font-bold text-white flex items-center gap-2">
                            <i class="fa-regular fa-calendar-days text-emerald-500"></i> Kalendarz Historii
                        </h2>
                        <button onclick="viewToday()" class="text-xs bg-slate-800 hover:bg-slate-700 text-emerald-400 border border-slate-700 px-3 py-1 rounded-lg font-mono transition">
                            Dzisiaj
                        </button>
                    </div>

                    <div class="flex justify-between items-center mb-4 text-sm font-tech">
                        <button onclick="changeMonth(-1)" class="p-2 hover:bg-slate-800 rounded-lg text-slate-400 hover:text-white transition"><i class="fa-solid fa-chevron-left"></i></button>
                        <span id="calendar-month-year" class="font-bold text-slate-200">Październik 2026</span>
                        <button onclick="changeMonth(1)" class="p-2 hover:bg-slate-800 rounded-lg text-slate-400 hover:text-white transition"><i class="fa-solid fa-chevron-right"></i></button>
                    </div>

                    <!-- Calendar Grid -->
                    <div class="grid grid-cols-7 gap-1 text-center font-mono text-xs text-slate-500 mb-2">
                        <div>Pn</div><div>Wt</div><div>Śr</div><div>Cz</div><div>Pt</div><div>So</div><div>Nd</div>
                    </div>
                    <div id="calendar-grid" class="grid grid-cols-7 gap-1">
                        <!-- JS Dynamic Calendar -->
                    </div>
                </div>

                <!-- SELECTED DAY PREVIEW ARCHIVE -->
                <div class="glass-panel p-6 rounded-2xl border border-slate-800">
                    <h3 id="archive-title" class="text-md font-tech font-bold text-slate-300 mb-3 flex items-center justify-between">
                        <span><i class="fa-solid fa-clock-rotate-left text-slate-500 mr-2"></i> Podgląd Dnia:</span>
                        <span id="archive-date-tag" class="text-xs font-mono text-emerald-400">Dzisiaj</span>
                    </h3>

                    <div id="archive-content" class="space-y-4 text-xs text-slate-400">
                        <!-- JS Content Preview -->
                    </div>
                </div>
            </section>

        </main>
    </div>

    <!-- FOOTER -->
    <footer class="mt-12 text-center text-xs text-slate-600 font-mono">
        LOCK IN SYSTEM &bull; DYSCEPLINA > EMOCJE &bull; WSKAZÓWKA: DANE ZAPISUJĄ SIĘ AUTOMATYCZNIE W PRZEGLĄDARCE
    </footer>

    <!-- MODAL: TOXIC MOTIVATION / EMERGENCY IMPULSE -->
    <div id="emergency-modal" class="fixed inset-0 bg-black/85 modal-blur z-50 hidden flex items-center justify-center p-4">
        <div class="max-w-2xl w-full bg-slate-950 border-2 border-red-600 rounded-3xl p-6 md:p-8 space-y-6 relative shadow-2xl overflow-hidden">
            <!-- Background Alert Pattern -->
            <div class="absolute -right-10 -top-10 opacity-10 text-red-600 text-9xl font-black pointer-events-none font-tech">STOP</div>

            <!-- Modal Header -->
            <div class="flex justify-between items-start border-b border-red-900/50 pb-4">
                <div class="flex items-center gap-3">
                    <div class="w-10 h-10 rounded-xl bg-red-600/20 border border-red-500 flex items-center justify-center text-red-500 text-xl font-bold">
                        <i class="fa-solid fa-ban"></i>
                    </div>
                    <div>
                        <h3 class="text-xl font-tech font-black text-red-500 uppercase tracking-wider">ZATRZYMAJ SIĘ! WALCZ ZE SŁABOŚCIĄ</h3>
                        <p class="text-xs font-mono text-slate-400">Ochłoń. Przeczytaj to zanim cokolwiek zrobisz.</p>
                    </div>
                </div>
                <button onclick="closeEmergencyModal()" class="text-slate-500 hover:text-white text-2xl font-bold transition">&times;</button>
            </div>

            <!-- TOXIC QUOTE BOX -->
            <div class="bg-red-950/30 border border-red-800/60 p-6 rounded-2xl relative">
                <i class="fa-solid fa-quote-left text-red-700 text-3xl absolute top-3 left-3 opacity-30"></i>
                <p id="toxic-quote-text" class="text-lg md:text-xl font-tech font-bold text-red-200 text-center relative z-10 my-4 leading-relaxed">
                    "Na prawdę chcesz przehandlować 30 dni ciężkiej pracy dla 5 minut cukrowego haju? Zastanów się, kim będziesz za godzinę: zwycięzcą czy kimś pełnym poczucia winy?"
                </p>
            </div>

            <!-- URGE SURFING TIMER (15 MIN) -->
            <div class="bg-slate-900 border border-slate-800 p-4 rounded-2xl text-center space-y-2">
                <div class="text-xs uppercase font-mono text-slate-400">Stoper Pokonania Impulsu (Zasada 15 Minut)</div>
                <div id="urge-timer" class="text-3xl font-tech font-black text-emerald-400 font-mono tracking-widest">15:00</div>
                <p class="text-xs text-slate-500">Impuls to tylko fala chemiczna w mózgu. Zniknie po 10-15 minutach. Przetrzyj oczy, wypij szklankę wody i czekaj.</p>
                <div class="flex justify-center gap-2 pt-2">
                    <button id="btn-urge-start" onclick="startUrgeTimer()" class="px-4 py-1.5 bg-emerald-600 hover:bg-emerald-500 text-slate-950 font-bold rounded-lg text-xs font-tech uppercase">Start Stopera</button>
                    <button onclick="resetUrgeTimer()" class="px-4 py-1.5 bg-slate-800 hover:bg-slate-700 text-slate-300 font-bold rounded-lg text-xs font-tech uppercase">Reset</button>
                </div>
            </div>

            <!-- ACTIONS -->
            <div class="flex flex-col sm:flex-row gap-3 pt-2">
                <button onclick="nextToxicQuote()" class="flex-1 py-3 bg-red-900/40 hover:bg-red-900/70 border border-red-700 text-red-200 font-tech font-bold rounded-xl text-sm uppercase transition flex items-center justify-center gap-2">
                    <i class="fa-solid fa-rotate"></i> Daj mocniejszy cytat!
                </button>
                <button onclick="closeEmergencyModal()" class="flex-1 py-3 bg-emerald-600 hover:bg-emerald-500 text-slate-950 font-tech font-black rounded-xl text-sm uppercase tracking-wider transition shadow-lg">
                    <i class="fa-solid fa-shield-halved"></i> Wytrzymam. Wracam do LOCK IN
                </button>
            </div>
        </div>
    </div>

    <script>
        // --- TOXIC MOTIVATION QUOTES ---
        const TOXIC_QUOTES = [
            "Naprawdę chcesz przehandlować tygodnie ciężkiej pracy dla 5 minut smakowej iluzji? Przestań się ze sobą cackać!",
            "Nie jesteś głodny/a. Jesteś po prostu zestresowany/a albo ci się nudzi. Wypij szklankę wody i ogarnij umysł!",
            "Za godzinę będziesz na siebie wściekły/a i znowu poczujesz satysfakcję wymieszaną z poczuciem wstydu. Warto? Zastanów się!",
            "Jedzenie nie rozwiąże Twoich problemów, doda tylko nowe. Bądź wreszcie dorosły/a i staw czoła emocjom!",
            "Sukces buduje się w momentach, gdy masz ochotę się poddać, ale tego NIE ROBISZ. To jest właśnie Twój test!",
            "Kompulsywne obżeranie się to oszukiwanie samego siebie. Przestań grać ofiarę i przejmij kontrolę nad własnymi dłońmi!",
            "Masz dwa wybory: ból dyscypliny dzisiaj albo ból żalu jutro. Co wybierasz?",
            "Każdy potrafi mieć 'Lock In' gdy jest łatwo. Prawdziwa siła tworzy się TERAZ, w tym kryzysowym momencie!"
        ];

        // --- DEFAULT DAILY TARGETS ---
        const DEFAULT_TARGETS = [
            { id: '1', text: 'Zjedz 3 zbilansowane, pożywne posiłki', completed: false },
            { id: '2', text: 'Zero napadów / kompulsywnego jedzenia', completed: false },
            { id: '3', text: 'Nawodnienie min. 2.5 litra wody', completed: false },
            { id: '4', text: 'Trening / Aktywność fizyczna / Spacer', completed: false },
            { id: '5', text: 'Brak przekąsek prosto z paczki/lodówki', completed: false }
        ];

        // --- APP STATE ---
        let currentViewDate = getTodayKey();
        let appData = loadAppData();
        let currentMonth = new Date().getMonth();
        let currentYear = new Date().getFullYear();
        let urgeInterval = null;
        let urgeSecondsLeft = 15 * 60;

        window.onload = function() {
            checkAndPerformDailyReset();
            renderTodayData();
            renderCalendar();
            startMidnightTimer();
            // Refresh auto-check every 30 seconds
            setInterval(checkAndPerformDailyReset, 30000);
        };

        // Date Format YYYY-MM-DD
        function getTodayKey() {
            const today = new Date();
            const year = today.getFullYear();
            const month = String(today.getMonth() + 1).padStart(2, '0');
            const day = String(today.getDate()).padStart(2, '0');
            return `${year}-${month}-${day}`;
        }

        // Load or Init App Data
        function loadAppData() {
            const saved = localStorage.getItem('lockin_app_data');
            if (saved) {
                try { return JSON.parse(saved); } catch(e) { console.error(e); }
            }
            return {
                lastActiveDate: getTodayKey(),
                streak: 0,
                days: {}
            };
        }

        function saveAppData() {
            localStorage.setItem('lockin_app_data', JSON.stringify(appData));
        }

        function checkAndPerformDailyReset() {
            const today = getTodayKey();
            
            // If new day detected
            if (appData.lastActiveDate !== today) {
                // Calculate if yesterday was completed to update streak
                const yesterdayKey = getYesterdayKey(today);
                const yesterdayData = appData.days[yesterdayKey];

                if (yesterdayData) {
                    const total = yesterdayData.targets ? yesterdayData.targets.length : 0;
                    const done = yesterdayData.targets ? yesterdayData.targets.filter(t => t.completed).length : 0;
                    // If achieved at least 80% of targets, streak incremented
                    if (total > 0 && (done / total) >= 0.7) {
                        appData.streak += 1;
                    } else {
                        appData.streak = 0;
                    }
                }

                appData.lastActiveDate = today;
                currentViewDate = today;
                saveAppData();
                renderTodayData();
                renderCalendar();
            }
        }

        function getYesterdayKey(todayKey) {
            const d = new Date(todayKey);
            d.setDate(d.getDate() - 1);
            return `${d.getFullYear()}-${String(d.getMonth() + 1).padStart(2, '0')}-${String(d.getDate()).padStart(2, '0')}`;
        }

        // Get or Create Day Object
        function getDayData(dateKey) {
            if (!appData.days[dateKey]) {
                appData.days[dateKey] = {
                    targets: JSON.parse(JSON.stringify(DEFAULT_TARGETS)),
                    journal: ''
                };
            }
            return appData.days[dateKey];
        }

        function renderTodayData() {
            const todayKey = getTodayKey();
            const todayData = getDayData(todayKey);

            document.getElementById('display-date').innerText = todayKey;
            document.getElementById('streak-count').innerText = appData.streak;

            // Render Targets Checklist
            const container = document.getElementById('targets-list');
            container.innerHTML = '';

            let completedCount = 0;

            todayData.targets.forEach((target) => {
                if (target.completed) completedCount++;

                const div = document.createElement('div');
                div.className = `flex items-center justify-between p-3.5 rounded-xl border transition ${
                    target.completed 
                    ? 'bg-emerald-950/30 border-emerald-800/60 text-emerald-200' 
                    : 'bg-slate-900/80 border-slate-800 text-slate-300'
                }`;

                div.innerHTML = `
                    <label class="flex items-center space-x-3 cursor-pointer flex-1 mr-2">
                        <input type="checkbox" ${target.completed ? 'checked' : ''} onchange="toggleTarget('${target.id}')"
                               class="w-5 h-5 rounded border-slate-700 text-emerald-500 focus:ring-emerald-500 focus:ring-offset-slate-900 bg-slate-800">
                        <span class="text-sm font-medium ${target.completed ? 'line-through text-slate-500' : ''}">${escapeHtml(target.text)}</span>
                    </label>
                    <button onclick="deleteTarget('${target.id}')" class="text-slate-600 hover:text-red-400 p-1 transition text-xs">
                        <i class="fa-solid fa-trash"></i>
                    </button>
                `;
                container.appendChild(div);
            });

            // Progress calculation
            const total = todayData.targets.length;
            const percent = total > 0 ? Math.round((completedCount / total) * 100) : 0;
            document.getElementById('progress-bar').style.width = `${percent}%`;
            document.getElementById('progress-percent').innerText = `${percent}%`;

            // Journal text
            document.getElementById('daily-journal').value = todayData.journal || '';

            // Update history archive preview
            renderArchivePreview(currentViewDate);
        }

        function toggleTarget(id) {
            const todayData = getDayData(getTodayKey());
            const target = todayData.targets.find(t => t.id === id);
            if (target) {
                target.completed = !target.completed;
                saveAppData();
                renderTodayData();
                renderCalendar();
            }
        }

        function addCustomTarget(e) {
            e.preventDefault();
            const input = document.getElementById('new-target-input');
            const text = input.value.trim();
            if (!text) return;

            const todayData = getDayData(getTodayKey());
            todayData.targets.push({
                id: Date.now().toString(),
                text: text,
                completed: false
            });

            input.value = '';
            saveAppData();
            renderTodayData();
            renderCalendar();
        }

        function deleteTarget(id) {
            const todayData = getDayData(getTodayKey());
            todayData.targets = todayData.targets.filter(t => t.id !== id);
            saveAppData();
            renderTodayData();
            renderCalendar();
        }

        function saveJournal() {
            const text = document.getElementById('daily-journal').value;
            const todayData = getDayData(getTodayKey());
            todayData.journal = text;
            saveAppData();
            if (currentViewDate === getTodayKey()) {
                renderArchivePreview(getTodayKey());
            }
        }

        function renderCalendar() {
            const grid = document.getElementById('calendar-grid');
            grid.innerHTML = '';

            const firstDayOfMonth = new Date(currentYear, currentMonth, 1);
            const lastDayOfMonth = new Date(currentYear, currentMonth + 1, 0);

            // Set Title Month
            const monthNames = ["Styczeń", "Luty", "Marzec", "Kwiecień", "Maj", "Czerwiec", "Lipiec", "Sierpień", "Wrzesień", "Październik", "Listopad", "Grudzień"];
            document.getElementById('calendar-month-year').innerText = `${monthNames[currentMonth]} ${currentYear}`;

            // Adjust starting day of week (Monday = 0)
            let startingDay = firstDayOfMonth.getDay() - 1;
            if (startingDay === -1) startingDay = 6; // Sunday

            // Empty slots before 1st
            for (let i = 0; i < startingDay; i++) {
                const empty = document.createElement('div');
                empty.className = 'h-9';
                grid.appendChild(empty);
            }

            const todayKey = getTodayKey();

            // Days of the month
            for (let day = 1; day <= lastDayOfMonth.getDate(); day++) {
                const dayString = String(day).padStart(2, '0');
                const monthString = String(currentMonth + 1).padStart(2, '0');
                const dateKey = `${currentYear}-${monthString}-${dayString}`;

                const dayData = appData.days[dateKey];
                let bgClass = 'bg-slate-900/50 text-slate-400 hover:bg-slate-800';

                // Check completion status if data exists
                if (dayData && dayData.targets && dayData.targets.length > 0) {
                    const done = dayData.targets.filter(t => t.completed).length;
                    const ratio = done / dayData.targets.length;
                    if (ratio >= 0.8) {
                        bgClass = 'bg-emerald-950 border border-emerald-600 text-emerald-300 font-bold';
                    } else if (ratio > 0) {
                        bgClass = 'bg-amber-950/60 border border-amber-800/50 text-amber-300';
                    }
                }

                if (dateKey === todayKey) {
                    bgClass += ' ring-2 ring-emerald-400 font-bold';
                }

                if (dateKey === currentViewDate) {
                    bgClass += ' border-2 border-white';
                }

                const button = document.createElement('button');
                button.className = `h-9 rounded-lg text-xs font-mono flex items-center justify-center transition ${bgClass}`;
                button.innerText = day;
                button.onclick = () => selectCalendarDate(dateKey);
                grid.appendChild(button);
            }
        }

        function changeMonth(delta) {
            currentMonth += delta;
            if (currentMonth < 0) {
                currentMonth = 11;
                currentYear--;
            } else if (currentMonth > 11) {
                currentMonth = 0;
                currentYear++;
            }
            renderCalendar();
        }

        function selectCalendarDate(dateKey) {
            currentViewDate = dateKey;
            renderCalendar();
            renderArchivePreview(dateKey);
        }

        function viewToday() {
            currentViewDate = getTodayKey();
            currentMonth = new Date().getMonth();
            currentYear = new Date().getFullYear();
            renderCalendar();
            renderTodayData();
        }

        function renderArchivePreview(dateKey) {
            document.getElementById('archive-date-tag').innerText = dateKey;
            const container = document.getElementById('archive-content');
            const dayData = appData.days[dateKey];

            if (!dayData) {
                container.innerHTML = `<p class="text-slate-600 italic">Brak zapisanych aktywności dla tego dnia.</p>`;
                return;
            }

            const targetsCount = dayData.targets ? dayData.targets.length : 0;
            const completedCount = dayData.targets ? dayData.targets.filter(t => t.completed).length : 0;

            let targetsHtml = '';
            if (dayData.targets && dayData.targets.length > 0) {
                targetsHtml = `<ul class="space-y-1">` + dayData.targets.map(t => 
                    `<li class="flex items-center gap-2 ${t.completed ? 'text-emerald-400' : 'text-slate-600 line-through'}">
                        <i class="fa-solid ${t.completed ? 'fa-check' : 'fa-xmark'} text-xs"></i> ${escapeHtml(t.text)}
                    </li>`
                ).join('') + `</ul>`;
            } else {
                targetsHtml = `<p class="text-slate-600">Brak celów.</p>`;
            }

            container.innerHTML = `
                <div>
                    <div class="font-bold text-slate-300 mb-1">Wykonane cele (${completedCount}/${targetsCount}):</div>
                    ${targetsHtml}
                </div>
                <div class="pt-2 border-t border-slate-800">
                    <div class="font-bold text-slate-300 mb-1">Dziennik / Notatki:</div>
                    <p class="text-slate-400 whitespace-pre-wrap bg-slate-900/50 p-3 rounded-lg border border-slate-800 font-sans">${escapeHtml(dayData.journal || 'Brak wpisu.')}</p>
                </div>
            `;
        }

        // Countdown to 00:00 Reset
        function startMidnightTimer() {
            function updateTimer() {
                const now = new Date();
                const midnight = new Date();
                midnight.setHours(24, 0, 0, 0);

                const diff = midnight - now;
                const hours = Math.floor(diff / (1000 * 60 * 60));
                const minutes = Math.floor((diff % (1000 * 60 * 60)) / (1000 * 60));
                const seconds = Math.floor((diff % (1000 * 60)) / 1000);

                document.getElementById('midnight-timer').innerText = 
                    `${String(hours).padStart(2, '0')}:${String(minutes).padStart(2, '0')}:${String(seconds).padStart(2, '0')}`;
            }
            updateTimer();
            setInterval(updateTimer, 1000);
        }

        // --- TOXIC MOTIVATION MODAL LOGIC ---
        function openEmergencyModal() {
            document.getElementById('emergency-modal').classList.remove('hidden');
            nextToxicQuote();
            resetUrgeTimer();
        }

        function closeEmergencyModal() {
            document.getElementById('emergency-modal').classList.add('hidden');
            clearInterval(urgeInterval);
            urgeInterval = null;
        }

        function nextToxicQuote() {
            const randomIndex = Math.floor(Math.random() * TOXIC_QUOTES.length);
            document.getElementById('toxic-quote-text').innerText = `"${TOXIC_QUOTES[randomIndex]}"`;
        }

        // URGE SURFING TIMER (15 MIN)
        function startUrgeTimer() {
            if (urgeInterval) return;
            const timerDisplay = document.getElementById('urge-timer');

            urgeInterval = setInterval(() => {
                if (urgeSecondsLeft <= 0) {
                    clearInterval(urgeInterval);
                    urgeInterval = null;
                    timerDisplay.innerText = "GRATULACJE! FALA PRZESZŁA!";
                    return;
                }
                urgeSecondsLeft--;
                const mins = Math.floor(urgeSecondsLeft / 60);
                const secs = urgeSecondsLeft % 60;
                timerDisplay.innerText = `${String(mins).padStart(2, '0')}:${String(secs).padStart(2, '0')}`;
            }, 1000);
        }

        function resetUrgeTimer() {
            clearInterval(urgeInterval);
            urgeInterval = null;
            urgeSecondsLeft = 15 * 60;
            document.getElementById('urge-timer').innerText = "15:00";
        }

        // Helper to escape HTML and prevent XSS
        function escapeHtml(str) {
            return str.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;").replace(/"/g, "&quot;").replace(/'/g, "&#039;");
        }
    </script>
</body>
</html>
