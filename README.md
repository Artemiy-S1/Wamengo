```html
<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Wamego - Учи ö'lari</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Nunito:wght@400;600;700;800&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Nunito', sans-serif;
            background-color: #f7f7f7;
            color: #3c3c3c;
            overflow-x: hidden;
            width: 100%;
            margin: 0;
            padding: 0;
            transition: background-color 0.3s, color 0.3s;
        }

        /* Dark Mode Styles */
        body.dark-mode {
            background-color: #131f24;
            color: #ffffff;
        }
        body.dark-mode .bg-white {
            background-color: #202f36;
            border-color: #37464f;
        }
        body.dark-mode .text-gray-700 {
            color: #ffffff;
        }
        body.dark-mode .text-gray-500 {
            color: #aeb5bc;
        }
        body.dark-mode .text-gray-400 {
            color: #7e8992;
        }
        body.dark-mode .border-gray-200 {
            border-color: #37464f;
        }
        body.dark-mode .bg-gray-100 {
            background-color: #37464f;
        }
        body.dark-mode .bg-gray-200 {
            background-color: #37464f;
        }
        body.dark-mode input {
            background-color: #37464f;
            border-color: #4b5c66;
            color: white;
        }
        body.dark-mode .option-card:hover {
            background-color: #37464f;
        }

        /* Duolingo-like Colors */
        .bg-duo-green { background-color: #58cc02; }
        .text-duo-green { color: #58cc02; }
        .border-duo-green { border-color: #58cc02; }
        .bg-duo-green-dark { background-color: #46a302; }
        
        .bg-duo-blue { background-color: #1cb0f6; }
        .text-duo-blue { color: #1cb0f6; }
        .border-duo-blue { border-color: #1cb0f6; }
        .bg-duo-blue-dark { background-color: #1899d6; }

        .bg-duo-red { background-color: #ff4b4b; }
        .text-duo-red { color: #ff4b4b; }
        .border-duo-red { border-color: #ff4b4b; }
        .bg-duo-red-dark { background-color: #ea2b2b; }

        .bg-duo-yellow { background-color: #ffc800; }
        .text-duo-yellow { color: #ffc800; }

        .bg-duo-gray { background-color: #e5e5e5; }
        .text-duo-gray { color: #afafaf; }

        /* 3D Button Effect */
        .btn-3d {
            transition: all 0.1s;
            border-bottom-width: 4px;
        }
        .btn-3d:active {
            transform: translateY(4px);
            border-bottom-width: 0px;
            margin-bottom: 4px;
        }

        /* Custom Scrollbar */
        ::-webkit-scrollbar {
            width: 8px;
        }
        ::-webkit-scrollbar-track {
            background: #f1f1f1; 
        }
        ::-webkit-scrollbar-thumb {
            background: #ccc; 
            border-radius: 4px;
        }
        body.dark-mode ::-webkit-scrollbar-track {
            background: #131f24;
        }
        body.dark-mode ::-webkit-scrollbar-thumb {
            background: #37464f;
        }

        /* Animations */
        @keyframes shake {
            0% { transform: translateX(0); }
            25% { transform: translateX(-5px); }
            50% { transform: translateX(5px); }
            75% { transform: translateX(-5px); }
            100% { transform: translateX(0); }
        }
        .animate-shake {
            animation: shake 0.4s ease-in-out;
        }

        @keyframes pop {
            0% { transform: scale(0.8); opacity: 0; }
            100% { transform: scale(1); opacity: 1; }
        }
        .animate-pop {
            animation: pop 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275) forwards;
        }

        .option-card:hover {
            background-color: #f7f7f7;
            border-color: #e5e5e5;
        }
        .option-card.selected {
            background-color: #ddf4ff;
            border-color: #1cb0f6;
            color: #1899d6;
        }
        .option-card.correct {
            background-color: #d7ffb8;
            border-color: #58cc02;
            color: #46a302;
        }
        .option-card.wrong {
            background-color: #ffdfe0;
            border-color: #ff4b4b;
            color: #ea2b2b;
        }
        body.dark-mode .option-card.selected {
            background-color: #183850;
            border-color: #1cb0f6;
            color: #1cb0f6;
        }

        /* Mascot positioning */
        .mascot-container img {
            max-height: 120px;
            object-fit: contain;
        }
    </style>
</head>
<body class="h-screen flex flex-col w-full">

    <!-- App Container -->
    <div id="app" class="flex-grow flex flex-col h-full bg-white shadow-lg relative overflow-hidden w-full transition-colors duration-300">
        
        <!-- Header (Dynamic) -->
        <header id="header" class="h-16 border-b border-gray-200 flex items-center justify-between px-4 bg-white z-10 hidden transition-colors duration-300">
            <div class="flex items-center gap-2">
                <button id="close-btn" class="text-gray-400 hover:text-gray-600 hidden">
                    <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
                    </svg>
                </button>
                <div class="w-8 h-8 rounded-full bg-duo-gray flex items-center justify-center text-white font-bold text-xs hidden" id="flag-icon">
                    
                </div>
            </div>
            
            <!-- Progress Bar -->
            <div class="flex-grow mx-4 h-4 bg-gray-200 rounded-full overflow-hidden hidden" id="progress-container">
                <div id="progress-bar" class="h-full bg-duo-green rounded-full transition-all duration-500" style="width: 0%"></div>
            </div>

            <!-- Hearts / Gems -->
            <div class="flex items-center gap-4">
                <div class="flex items-center gap-1 text-duo-red font-bold hidden" id="hearts-display">
                    <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" viewBox="0 0 20 20" fill="currentColor">
                        <path fill-rule="evenodd" d="M3.172 5.172a4 4 0 015.656 0L10 6.343l1.172-1.171a4 4 0 115.656 5.656L10 17.657l-6.828-6.829a4 4 0 010-5.656z" clip-rule="evenodd" />
                    </svg>
                    <span id="hearts-count">5</span>
                </div>
            </div>
        </header>

        <!-- Main Content Area -->
        <main id="main-content" class="flex-grow overflow-y-auto relative w-full">
            
            <!-- VIEW: LANDING PAGE -->
            <section id="view-landing" class="h-full flex flex-col items-center justify-center p-8 text-center animate-pop">
                <div class="mb-8 relative">
                    <img src="https://image.qwenlm.ai/public_source/39e8755f-0943-4daa-8aad-ecc21b8a1201/1ebe50898-0541-43f0-9cf5-e9e119e1dd2e.png" alt="Wamego Mascot" class="w-48 mx-auto drop-shadow-xl">
                </div>
                <h1 class="text-3xl font-extrabold text-gray-700 mb-2">Wamego</h1>
                <p class="text-gray-500 mb-8 text-lg" id="landing-desc">Бесплатный, веселый и эффективный способ выучить язык <span class="text-duo-green font-bold">ö'lari</span>.</p>
                
                <div class="w-full space-y-4 max-w-xs">
                    <button onclick="app.goToLogin()" class="w-full btn-3d bg-duo-green text-white font-bold py-3 rounded-2xl text-lg uppercase tracking-wide hover:bg-[#61e002]">
                        Начать
                    </button>
                    <div class="text-sm text-gray-400 font-bold uppercase tracking-wide">
                        Уже есть аккаунт? <button onclick="app.goToLogin()" class="text-duo-green hover:underline ml-1">Войти</button>
                    </div>
                </div>
            </section>

            <!-- VIEW: LOGIN -->
            <section id="view-login" class="h-full flex flex-col items-center justify-center p-6 hidden">
                <div class="w-full max-w-sm">
                    <div class="flex justify-between items-center mb-8">
                        <button onclick="app.goToLanding()" class="text-gray-400 font-bold uppercase tracking-wide hover:bg-gray-100 px-4 py-2 rounded-xl transition-colors">
                            Назад
                        </button>
                        <h2 class="text-2xl font-bold text-gray-700 text-center">Создать/войти в профиль</h2>
                        <div class="w-16"></div> <!-- Spacer for centering -->
                    </div>
                    
                    <div class="space-y-4 mb-8">
                        <input type="text" id="login-name-input" placeholder="Имя" class="w-full p-4 bg-gray-100 border-2 border-gray-200 rounded-2xl focus:border-duo-blue focus:outline-none transition-colors font-bold text-gray-600">
                        <input type="password" placeholder="Пароль" class="w-full p-4 bg-gray-100 border-2 border-gray-200 rounded-2xl focus:border-duo-blue focus:outline-none transition-colors font-bold text-gray-600">
                        <input type="date" class="w-full p-4 bg-gray-100 border-2 border-gray-200 rounded-2xl focus:border-duo-blue focus:outline-none transition-colors font-bold text-gray-600">
                    </div>

                    <div class="space-y-3">
                        <button onclick="app.completeRegistration()" class="w-full btn-3d bg-white border-2 border-gray-200 text-gray-600 font-bold py-3 rounded-2xl flex items-center justify-center gap-3 hover:bg-gray-50">
                            <svg class="w-6 h-6" viewBox="0 0 24 24"><path fill="#4285F4" d="M22.56 12.25c0-.78-.07-1.53-.2-2.25H12v4.26h5.92c-.26 1.37-1.04 2.53-2.21 3.31v2.77h3.57c2.08-1.92 3.28-4.74 3.28-8.09z"/><path fill="#34A853" d="M12 23c2.97 0 5.46-.98 7.28-2.66l-3.57-2.77c-.98.66-2.23 1.06-3.71 1.06-2.86 0-5.29-1.93-6.16-4.53H2.18v2.84C3.99 20.53 7.7 23 12 23z"/><path fill="#FBBC05" d="M5.84 14.09c-.22-.66-.35-1.36-.35-2.09s.13-1.43.35-2.09V7.07H2.18C1.43 8.55 1 10.22 1 12s.43 3.45 1.18 4.93l2.85-2.22.81-.62z"/><path fill="#EA4335" d="M12 5.38c1.62 0 3.06.56 4.21 1.64l3.15-3.15C17.45 2.09 14.97 1 12 1 7.7 1 3.99 3.47 2.18 7.07l3.66 2.84c.87-2.6 3.3-4.53 6.16-4.53z"/></svg>
                            Зарегистрироваться через Google
                        </button>
                        <button onclick="app.completeRegistration()" class="w-full btn-3d bg-white border-2 border-gray-200 text-gray-600 font-bold py-3 rounded-2xl flex items-center justify-center gap-3 hover:bg-gray-50">
                            <div class="w-6 h-6 bg-red-500 rounded-full flex items-center justify-center text-white font-bold text-xs">Я</div>
                            Зарегистрироваться через Яндекс
                        </button>
                        <button onclick="app.completeGuest()" class="w-full btn-3d bg-gray-200 text-gray-600 font-bold py-3 rounded-2xl flex items-center justify-center gap-3 hover:bg-gray-300 mt-4">
                            Войти как Гость
                        </button>
                    </div>
                    
                    <div class="mt-8 text-center">
                        <p class="text-xs text-gray-400">Нажимая "Начать", вы принимаете Условия использования и Политику конфиденциальности.</p>
                    </div>
                </div>
            </section>

            <!-- VIEW: DASHBOARD (PATH) -->
            <section id="view-dashboard" class="hidden pb-20">
                <div class="flex flex-col items-center pt-8 space-y-6">
                    <!-- Unit 1 -->
                    <div class="w-full max-w-xs relative">
                        <!-- Removed Unit Header as requested -->
                        
                        <!-- Path Nodes -->
                        <div class="flex flex-col items-center gap-4 relative pt-4">
                            <!-- Node 1 -->
                            <button onclick="app.startLesson(1)" class="w-20 h-20 rounded-full bg-duo-green border-b-4 border-duo-green-dark flex items-center justify-center text-white relative z-10 hover:brightness-110 transition-all active:translate-y-1 active:border-b-0">
                                <svg xmlns="http://www.w3.org/2000/svg" class="h-10 w-10" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 6.253v13m0-13C10.832 5.477 9.246 5 7.5 5S4.168 5.477 3 6.253v13C4.168 18.477 5.754 18 7.5 18s3.332.477 4.5 1.253m0-13C13.168 5.477 14.754 5 16.5 5c1.747 0 3.332.477 4.5 1.253v13C19.832 18.477 18.247 18 16.5 18c-1.746 0-3.332.477-4.5 1.253" />
                                </svg>
                                <div class="absolute -top-2 -right-2 bg-duo-yellow text-white text-xs font-bold px-2 py-1 rounded-full border-2 border-white">START</div>
                            </button>

                            <!-- Node 2 -->
                            <button onclick="app.startLesson(2)" class="w-20 h-20 rounded-full bg-duo-green border-b-4 border-duo-green-dark flex items-center justify-center text-white relative z-10 hover:brightness-110 transition-all active:translate-y-1 active:border-b-0 mt-4">
                                <svg xmlns="http://www.w3.org/2000/svg" class="h-10 w-10" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 6.253v13m0-13C10.832 5.477 9.246 5 7.5 5S4.168 5.477 3 6.253v13C4.168 18.477 5.754 18 7.5 18s3.332.477 4.5 1.253m0-13C13.168 5.477 14.754 5 16.5 5c1.747 0 3.332.477 4.5 1.253v13C19.832 18.477 18.247 18 16.5 18c-1.746 0-3.332.477-4.5 1.253" />
                                </svg>
                                <div class="absolute -top-2 -right-2 bg-duo-yellow text-white text-xs font-bold px-2 py-1 rounded-full border-2 border-white">START</div>
                            </button>

                             <!-- Node 3 (Rules Module) -->
                             <button onclick="app.startLesson(3)" class="w-20 h-20 rounded-full bg-duo-blue border-b-4 bg-duo-blue-dark flex items-center justify-center text-white relative z-10 hover:brightness-110 transition-all active:translate-y-1 active:border-b-0 mt-4">
                                <svg xmlns="http://www.w3.org/2000/svg" class="h-10 w-10" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 12h6m-6 4h6m2 5H7a2 2 0 01-2-2V5a2 2 0 012-2h5.586a1 1 0 01.707.293l5.414 5.414a1 1 0 01.293.707V19a2 2 0 01-2 2z" />
                                </svg>
                                <div class="absolute -top-2 -right-2 bg-duo-yellow text-white text-xs font-bold px-2 py-1 rounded-full border-2 border-white">NEW</div>
                            </button>

                             <!-- Node 4 (Locked) -->
                             <div class="w-16 h-16 rounded-full bg-gray-200 border-b-4 border-gray-300 flex items-center justify-center text-gray-400 relative z-10 mt-4">
                                <svg xmlns="http://www.w3.org/2000/svg" class="h-8 w-8" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 15v2m-6 4h12a2 2 0 002-2v-6a2 2 0 00-2-2H6a2 2 0 00-2 2v6a2 2 0 002 2zm10-10V7a4 4 0 00-8 0v4h8z" />
                                </svg>
                            </div>
                        </div>
                    </div>
                </div>
            </section>

            <!-- VIEW: LESSON -->
            <section id="view-lesson" class="hidden h-full flex flex-col">
                <div class="flex-grow flex flex-col items-center justify-center p-6 max-w-lg mx-auto w-full">
                    
                    <!-- Question Text -->
                    <h2 id="question-text" class="text-2xl font-bold text-gray-700 mb-8 text-left w-full">
                        <!-- Dynamic Content -->
                    </h2>

                    <!-- Options Grid -->
                    <div id="options-grid" class="grid grid-cols-1 gap-4 w-full mb-8">
                        <!-- Dynamic Buttons -->
                    </div>

                    <!-- Input Area (for translation tasks) -->
                    <div id="input-area" class="hidden w-full mb-8">
                        <div class="border-2 border-gray-200 rounded-2xl p-4 bg-white focus-within:border-duo-blue focus-within:bg-white transition-colors">
                            <input type="text" id="translation-input" class="w-full outline-none text-lg font-bold text-gray-700 placeholder-gray-400" placeholder="Enter translation...">
                        </div>
                    </div>

                </div>

                <!-- Bottom Action Bar -->
                <div class="p-4 border-t border-gray-200 bg-white">
                    <div class="max-w-lg mx-auto flex justify-between items-center">
                        <button id="skip-btn" class="text-gray-400 font-bold uppercase tracking-wide hover:bg-gray-100 px-4 py-3 rounded-xl transition-colors" onclick="app.skipQuestion()">
                            Skip (-20 XP)
                        </button>
                        <button id="check-btn" class="btn-3d bg-duo-green text-white font-bold py-3 px-8 rounded-2xl uppercase tracking-wide w-1/2 text-center" onclick="app.checkAnswer()">
                            Check
                        </button>
                    </div>
                </div>

                <!-- Feedback Sheet (Hidden by default) -->
                <div id="feedback-sheet" class="absolute bottom-0 left-0 right-0 p-6 transform translate-y-full transition-transform duration-300 z-20 border-t-2">
                    <div class="max-w-lg mx-auto flex items-start gap-4">
                        <div id="feedback-icon" class="w-14 h-14 rounded-full flex items-center justify-center flex-shrink-0">
                            <!-- Icon injected via JS -->
                        </div>
                        <div class="flex-grow">
                            <h3 id="feedback-title" class="text-xl font-bold mb-1">Correct!</h3>
                            <p id="feedback-msg" class="text-sm mb-4">Good job.</p>
                        </div>
                        <button id="continue-btn" class="btn-3d font-bold py-3 px-6 rounded-2xl uppercase tracking-wide min-w-[120px]" onclick="app.nextQuestion()">
                            Continue
                        </button>
                    </div>
                </div>
            </section>

            <!-- VIEW: RESULT -->
            <section id="view-result" class="hidden h-full flex flex-col items-center justify-center p-8 text-center animate-pop">
                <div class="mb-6">
                    <div class="w-32 h-32 bg-duo-yellow rounded-full flex items-center justify-center mx-auto mb-4 border-4 border-white shadow-lg">
                        <span class="text-4xl">🏆</span>
                    </div>
                    <h2 class="text-3xl font-extrabold text-duo-yellow mb-2">Lesson Complete!</h2>
                    <p class="text-gray-500 text-lg">You did great with the basics of ö'lari.</p>
                </div>

                <div class="w-full max-w-xs grid grid-cols-2 gap-4 mb-8">
                    <div class="bg-white border-2 border-gray-200 rounded-2xl p-4 flex flex-col items-center">
                        <span class="text-duo-yellow font-bold text-xl" id="result-xp">+10 XP</span>
                        <span class="text-gray-400 text-xs uppercase font-bold">XP</span>
                    </div>
                    <div class="bg-white border-2 border-gray-200 rounded-2xl p-4 flex flex-col items-center">
                        <span class="text-duo-green font-bold text-xl">100%</span>
                        <span class="text-gray-400 text-xs uppercase font-bold">Accuracy</span>
                    </div>
                </div>

                <button onclick="app.goToDashboard()" class="w-full btn-3d bg-duo-green text-white font-bold py-3 rounded-2xl text-lg uppercase tracking-wide hover:bg-[#61e002]">
                    Continue
                </button>
            </section>

            <!-- VIEW: DICTIONARY -->
            <section id="view-dictionary" class="hidden pb-20">
                <div class="p-6">
                    <h2 class="text-2xl font-bold text-gray-700 mb-6">ö'lari Dictionary</h2>

                    <div class="space-y-6">
                        <!-- Category 1 -->
                        <div>
                            <h3 class="text-lg font-bold text-gray-500 uppercase mb-2">People & Pronouns</h3>
                            <div class="bg-white border-2 border-gray-200 rounded-2xl overflow-hidden">
                                <div class="p-4 border-b border-gray-100 flex justify-between items-center">
                                    <span class="font-bold text-gray-700">M'e</span>
                                    <span class="text-gray-500">I / Я</span>
                                </div>
                                <div class="p-4 border-b border-gray-100 flex justify-between items-center">
                                    <span class="font-bold text-gray-700">T'e</span>
                                    <span class="text-gray-500">You / Ты</span>
                                </div>
                                <div class="p-4 border-b border-gray-100 flex justify-between items-center">
                                    <span class="font-bold text-gray-700">S'i</span>
                                    <span class="text-gray-500">He/She / Он/Она</span>
                                </div>
                                <div class="p-4 flex justify-between items-center">
                                    <span class="font-bold text-gray-700">Mort</span>
                                    <span class="text-gray-500">Human / Человек</span>
                                </div>
                            </div>
                        </div>

                        <!-- Category 2 -->
                        <div>
                            <h3 class="text-lg font-bold text-gray-500 uppercase mb-2">Nature</h3>
                            <div class="bg-white border-2 border-gray-200 rounded-2xl overflow-hidden">
                                <div class="p-4 border-b border-gray-100 flex justify-between items-center">
                                    <span class="font-bold text-gray-700">V'a</span>
                                    <span class="text-gray-500">Water / Вода</span>
                                </div>
                                <div class="p-4 border-b border-gray-100 flex justify-between items-center">
                                    <span class="font-bold text-gray-700">G'ort</span>
                                    <span class="text-gray-500">House / Дом</span>
                                </div>
                                <div class="p-4 border-b border-gray-100 flex justify-between items-center">
                                    <span class="font-bold text-gray-700">L'es</span>
                                    <span class="text-gray-500">Forest / Лес</span>
                                </div>
                                <div class="p-4 flex justify-between items-center">
                                    <span class="font-bold text-gray-700">S'olnce</span>
                                    <span class="text-gray-500">Sun / Солнце</span>
                                </div>
                            </div>
                        </div>

                         <!-- Category 3 -->
                        <div>
                            <h3 class="text-lg font-bold text-gray-500 uppercase mb-2">Actions & Adjectives</h3>
                            <div class="bg-white border-2 border-gray-200 rounded-2xl overflow-hidden">
                                <div class="p-4 border-b border-gray-100 flex justify-between items-center">
                                    <span class="font-bold text-gray-700">V'idzu</span>
                                    <span class="text-gray-500">See / Вижу</span>
                                </div>
                                <div class="p-4 border-b border-gray-100 flex justify-between items-center">
                                    <span class="font-bold text-gray-700">Umniy</span>
                                    <span class="text-gray-500">Smart / Умный</span>
                                </div>
                                <div class="p-4 border-b border-gray-100 flex justify-between items-center">
                                    <span class="font-bold text-gray-700">B'uri</span>
                                    <span class="text-gray-500">Good / Хороший</span>
                                </div>
                                <div class="p-4 flex justify-between items-center">
                                    <span class="font-bold text-gray-700">Hudo</span>
                                    <span class="text-gray-500">Bad / Плохой</span>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </section>

            <!-- VIEW: PROFILE -->
            <section id="view-profile" class="hidden pb-20">
                <div class="p-6 flex flex-col items-center">
                    <div class="relative mb-4">
                        <div class="w-24 h-24 bg-duo-blue rounded-full flex items-center justify-center text-white text-3xl font-bold">
                            <span id="profile-avatar-letter">U</span>
                        </div>
                        <button onclick="document.getElementById('profile-name-edit').focus()" class="absolute bottom-0 right-0 bg-white border-2 border-gray-200 rounded-full p-1 shadow-sm hover:bg-gray-50">
                            <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 text-gray-500" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15.232 5.232l3.536 3.536m-2.036-5.036a2.5 2.5 0 113.536 3.536L6.5 21.036H3v-3.572L16.732 3.732z" />
                            </svg>
                        </button>
                    </div>
                    
                    <input type="text" id="profile-name-edit" class="text-2xl font-bold text-gray-700 mb-1 text-center bg-transparent border-b-2 border-transparent focus:border-duo-blue focus:outline-none w-full max-w-[200px]" value="User Name" oninput="app.updateProfileName(this.value)">
                    
                    <!-- Removed Joined Date and Achievements as requested -->

                    <div class="w-full grid grid-cols-1 gap-4 mb-8">
                        <div class="bg-white border-2 border-gray-200 rounded-2xl p-4 flex flex-col items-center">
                            <span class="text-duo-blue font-bold text-xl" id="stat-xp">0</span>
                            <span class="text-gray-400 text-xs uppercase font-bold">Total XP</span>
                        </div>
                    </div>
                </div>
            </section>

            <!-- VIEW: SETTINGS -->
            <section id="view-settings" class="hidden pb-20">
                <div class="p-6">
                    <div class="flex items-center justify-between mb-6">
                        <h2 class="text-2xl font-bold text-gray-700">Settings</h2>
                        <button onclick="app.goToDashboard()" class="text-gray-400 hover:text-gray-600">
                            <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
                            </svg>
                        </button>
                    </div>

                    <div class="space-y-6">
                        <!-- Languages Section -->
                        <div>
                            <h3 class="text-lg font-bold text-gray-500 uppercase mb-2">Languages</h3>
                            <div class="bg-white border-2 border-gray-200 rounded-2xl overflow-hidden">
                                <button onclick="app.setLanguage('ru')" class="w-full p-4 border-b border-gray-100 flex justify-between items-center hover:bg-gray-50">
                                    <div class="flex items-center gap-3">
                                        <span class="text-2xl">🇷</span>
                                        <span class="font-bold text-gray-700">Russian</span>
                                    </div>
                                    <div id="lang-check-ru" class="w-6 h-6 rounded-full border-2 border-duo-green bg-duo-green flex items-center justify-center hidden">
                                        <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 text-white" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="3" d="M5 13l4 4L19 7" />
                                        </svg>
                                    </div>
                                </button>
                                <button onclick="app.setLanguage('en')" class="w-full p-4 flex justify-between items-center hover:bg-gray-50">
                                    <div class="flex items-center gap-3">
                                        <span class="text-2xl">🇬🇧</span>
                                        <span class="font-bold text-gray-700">English</span>
                                    </div>
                                    <div id="lang-check-en" class="w-6 h-6 rounded-full border-2 border-duo-green bg-duo-green flex items-center justify-center hidden">
                                        <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 text-white" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="3" d="M5 13l4 4L19 7" />
                                        </svg>
                                    </div>
                                </button>
                            </div>
                        </div>

                        <!-- Themes Section -->
                        <div>
                            <h3 class="text-lg font-bold text-gray-500 uppercase mb-2">Themes</h3>
                            <div class="bg-white border-2 border-gray-200 rounded-2xl overflow-hidden">
                                <button onclick="app.toggleTheme()" class="w-full p-4 flex justify-between items-center hover:bg-gray-50">
                                    <span class="font-bold text-gray-700">Dark Mode</span>
                                    <div id="theme-toggle-indicator" class="w-12 h-6 bg-gray-300 rounded-full relative transition-colors duration-300">
                                        <div id="theme-toggle-knob" class="w-6 h-6 bg-white rounded-full absolute top-0 left-0 transition-transform duration-300 shadow-sm"></div>
                                    </div>
                                </button>
                            </div>
                        </div>
                    </div>
                </div>
            </section>

        </main>

        <!-- Bottom Navigation (Dashboard Only) -->
        <nav id="bottom-nav" class="h-16 border-t border-gray-200 bg-white flex items-center justify-around hidden z-10 w-full transition-colors duration-300">
            <button onclick="app.goToDashboard()" class="flex flex-col items-center text-gray-400 hover:text-duo-green">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 12l2-2m0 0l7-7 7 7M5 10v10a1 1 0 001 1h3m10-11l2 2m-2-2v10a1 1 0 01-1 1h-3m-6 0a1 1 0 001-1v-4a1 1 0 011-1h2a1 1 0 011 1v4a1 1 0 001 1m-6 0h6" />
                </svg>
                <span class="text-xs font-bold mt-1">Learn</span>
            </button>
            <button onclick="app.goToDictionary()" class="flex flex-col items-center text-gray-400 hover:text-duo-green">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 6.253v13m0-13C10.832 5.477 9.246 5 7.5 5S4.168 5.477 3 6.253v13C4.168 18.477 5.754 18 7.5 18s3.332.477 4.5 1.253m0-13C13.168 5.477 14.754 5 16.5 5c1.747 0 3.332.477 4.5 1.253v13C19.832 18.477 18.247 18 16.5 18c-1.746 0-3.332.477-4.5 1.253" />
                </svg>
                <span class="text-xs font-bold mt-1">Dictionary</span>
            </button>
            <button onclick="app.goToProfile()" class="flex flex-col items-center text-gray-400 hover:text-duo-green">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M16 7a4 4 0 11-8 0 4 4 0 018 0zM12 14a7 7 0 00-7 7h14a7 7 0 00-7-7z" />
                </svg>
                <span class="text-xs font-bold mt-1">Profile</span>
            </button>
            <button onclick="app.goToSettings()" class="flex flex-col items-center text-gray-400 hover:text-duo-green">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10.325 4.317c.426-1.756 2.924-1.756 3.35 0a1.724 1.724 0 002.573 1.066c1.543-.94 3.31.826 2.37 2.37a1.724 1.724 0 001.065 2.572c1.756.426 1.756 2.924 0 3.35a1.724 1.724 0 00-1.066 2.573c.94 1.543-.826 3.31-2.37 2.37a1.724 1.724 0 00-2.572 1.065c-.426 1.756-2.924 1.756-3.35 0a1.724 1.724 0 00-2.573-1.066c-1.543.94-3.31-.826-2.37-2.37a1.724 1.724 0 00-1.065-2.572c-1.756-.426-1.756-2.924 0-3.35a1.724 1.724 0 001.066-2.573c-.94-1.543.826-3.31 2.37-2.37.996.608 2.296.07 2.572-1.065z" />
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 12a3 3 0 11-6 0 3 3 0 016 0z" />
                </svg>
                <span class="text-xs font-bold mt-1">Settings</span>
            </button>
        </nav>

    </div>

    <script>
        // --- DATA FROM TXT FILE ---
        const elariData = {
            alphabet: [
                { letter: "'e", sound: "[Э]", example: "m'e" },
                { letter: "a", sound: "[А]", example: "v'a" },
                { letter: "i", sound: "[И]", example: "k'i" },
                { letter: "u", sound: "[У]", example: "umniy" },
                { letter: "v'a", meaning_ru: "Вода", meaning_en: "Water" },
                { letter: "g'ort", meaning_ru: "Дом", meaning_en: "House" },
                { letter: "m'e", meaning_ru: "Я", meaning_en: "I" },
                { letter: "t'e", meaning_ru: "Ты", meaning_en: "You" },
                { letter: "ch'aj", meaning_ru: "Чай", meaning_en: "Tea" },
                { letter: "umniy", meaning_ru: "Умный", meaning_en: "Smart" }
            ],
            phrases: [
                { elari: "Privet, m'e!", ru: "Привет!", en: "Hello!" },
                { elari: "Kak del'a?", ru: "Как дела?", en: "How are you?" },
                { elari: "M'e l'ubit t'e.", ru: "Я люблю тебя.", en: "I love you." },
                { elari: "Gde g'ort?", ru: "Где дом?", en: "Where is the house?" },
                { elari: "Daj v'a.", ru: "Дай воды.", en: "Give water." },
                { elari: "T'e umniy?", ru: "Ты умный?", en: "Are you smart?" }
            ]
        };

        // --- APP LOGIC ---
        const app = {
            state: {
                currentView: 'landing',
                hearts: 5,
                lessonProgress: 0,
                currentLessonId: null,
                selectedOption: null,
                questions: [],
                lessonAttempts: {}, // Track attempts per lesson ID to shuffle
                user: {
                    name: "Guest",
                    xp: 0,
                    isGuest: true
                },
                language: 'ru', // 'ru' or 'en'
                darkMode: false
            },

            init() {
                this.updateUI();
                this.setLanguage('ru'); // Default to Russian
            },

            toggleTheme() {
                this.state.darkMode = !this.state.darkMode;
                document.body.classList.toggle('dark-mode', this.state.darkMode);
                
                // Update Toggle UI
                const indicator = document.getElementById('theme-toggle-indicator');
                const knob = document.getElementById('theme-toggle-knob');
                
                if (this.state.darkMode) {
                    indicator.classList.remove('bg-gray-300');
                    indicator.classList.add('bg-duo-green');
                    knob.style.transform = 'translateX(100%)';
                } else {
                    indicator.classList.add('bg-gray-300');
                    indicator.classList.remove('bg-duo-green');
                    knob.style.transform = 'translateX(0)';
                }
            },

            setLanguage(lang) {
                this.state.language = lang;
                
                // Update UI checks
                document.getElementById('lang-check-ru').classList.toggle('hidden', lang !== 'ru');
                document.getElementById('lang-check-en').classList.toggle('hidden', lang !== 'en');

                // Update Text Content based on Language
                if (lang === 'ru') {
                    document.getElementById('landing-desc').innerHTML = 'Бесплатный, веселый и эффективный способ выучить язык <span class="text-duo-green font-bold">ö\'lari</span>.';
                    document.querySelector('#view-landing button').innerText = 'Начать';
                    document.querySelector('#view-landing div div button').innerText = 'Войти';
                    document.querySelector('#view-landing div div').innerHTML = 'Уже есть аккаунт? <button onclick="app.goToLogin()" class="text-duo-green hover:underline ml-1">Войти</button>';
                    
                    // Login View
                    document.querySelector('#view-login h2').innerText = 'Создать/войти в профиль';
                    document.querySelector('#view-login button[onclick="app.goToLanding()"]').innerText = 'Назад';
                    document.querySelectorAll('#view-login input')[0].placeholder = 'Имя';
                    document.querySelectorAll('#view-login input')[1].placeholder = 'Пароль';
                    
                    // Dashboard/Nav
                    document.querySelectorAll('#bottom-nav span')[0].innerText = 'Учить';
                    document.querySelectorAll('#bottom-nav span')[1].innerText = 'Словарь';
                    document.querySelectorAll('#bottom-nav span')[2].innerText = 'Профиль';
                    document.querySelectorAll('#bottom-nav span')[3].innerText = 'Настройки';
                    
                    // Settings
                    document.querySelector('#view-settings h2').innerText = 'Настройки';
                    document.querySelector('#view-settings h3:nth-of-type(1)').innerText = 'Языки';
                    document.querySelector('#view-settings h3:nth-of-type(2)').innerText = 'Темы';
                    
                    // Lesson Buttons
                    document.getElementById('skip-btn').innerText = 'Пропустить (-20 XP)';
                    document.getElementById('check-btn').innerText = 'Проверить';
                    document.getElementById('continue-btn').innerText = 'Далее';
                    
                    // Result
                    document.querySelector('#view-result h2').innerText = 'Урок завершен!';
                    document.querySelector('#view-result p').innerText = 'Вы отлично справились с основами ö\'lari.';
                    document.querySelector('#view-result button').innerText = 'Продолжить';
                    
                    // Profile
                    document.querySelector('#view-profile h3').innerText = 'Достижения'; // Hidden anyway but good to have
                    
                    // Dictionary
                    document.querySelector('#view-dictionary h2').innerText = 'Словарь ö\'lari';
                    
                } else {
                    document.getElementById('landing-desc').innerHTML = 'The free, fun, and effective way to learn <span class="text-duo-green font-bold">ö\'lari</span>.';
                    document.querySelector('#view-landing button').innerText = 'Get Started';
                    document.querySelector('#view-landing div div').innerHTML = 'Already have an account? <button onclick="app.goToLogin()" class="text-duo-green hover:underline ml-1">Log in</button>';
                    
                    // Login View
                    document.querySelector('#view-login h2').innerText = 'Create/Log in to profile';
                    document.querySelector('#view-login button[onclick="app.goToLanding()"]').innerText = 'Back';
                    document.querySelectorAll('#view-login input')[0].placeholder = 'Name';
                    document.querySelectorAll('#view-login input')[1].placeholder = 'Password';
                    
                    // Dashboard/Nav
                    document.querySelectorAll('#bottom-nav span')[0].innerText = 'Learn';
                    document.querySelectorAll('#bottom-nav span')[1].innerText = 'Dictionary';
                    document.querySelectorAll('#bottom-nav span')[2].innerText = 'Profile';
                    document.querySelectorAll('#bottom-nav span')[3].innerText = 'Settings';
                    
                    // Settings
                    document.querySelector('#view-settings h2').innerText = 'Settings';
                    document.querySelector('#view-settings h3:nth-of-type(1)').innerText = 'Languages';
                    document.querySelector('#view-settings h3:nth-of-type(2)').innerText = 'Themes';
                    
                    // Lesson Buttons
                    document.getElementById('skip-btn').innerText = 'Skip (-20 XP)';
                    document.getElementById('check-btn').innerText = 'Check';
                    document.getElementById('continue-btn').innerText = 'Continue';
                    
                    // Result
                    document.querySelector('#view-result h2').innerText = 'Lesson Complete!';
                    document.querySelector('#view-result p').innerText = 'You did great with the basics of ö\'lari.';
                    document.querySelector('#view-result button').innerText = 'Continue';
                    
                    // Profile
                    document.querySelector('#view-profile h3').innerText = 'Achievements';
                    
                    // Dictionary
                    document.querySelector('#view-dictionary h2').innerText = 'ö\'lari Dictionary';
                }
            },

            goToLanding() {
                this.switchView('landing');
                document.getElementById('header').classList.add('hidden');
                document.getElementById('bottom-nav').classList.add('hidden');
            },

            goToLogin() {
                this.switchView('login');
                document.getElementById('header').classList.add('hidden');
                document.getElementById('bottom-nav').classList.add('hidden');
            },

            completeRegistration() {
                // Get name from input if provided
                const nameInput = document.getElementById('login-name-input').value.trim();
                if (nameInput) {
                    this.state.user.name = nameInput;
                }
                this.state.user.isGuest = false;
                this.updateProfileUI();
                this.goToDashboard();
            },

            completeGuest() {
                this.state.user.name = "Guest";
                this.state.user.isGuest = true;
                this.state.user.xp = 0;
                this.updateProfileUI();
                this.goToDashboard();
            },

            updateProfileName(newName) {
                if (this.state.user.isGuest) return; // Guests can't change name permanently in this demo
                this.state.user.name = newName || "User";
                this.updateProfileUI();
            },

            updateProfileUI() {
                // Update Profile View Elements
                document.getElementById('profile-name-edit').value = this.state.user.name;
                document.getElementById('profile-name-edit').disabled = this.state.user.isGuest;
                document.getElementById('profile-avatar-letter').innerText = this.state.user.name.charAt(0).toUpperCase();
                
                // Update Stats
                document.getElementById('stat-xp').innerText = this.state.user.xp;
            },

            goToDashboard() {
                this.switchView('dashboard');
                document.getElementById('header').classList.remove('hidden');
                document.getElementById('flag-icon').classList.remove('hidden');
                document.getElementById('hearts-display').classList.remove('hidden');
                document.getElementById('bottom-nav').classList.remove('hidden');
                document.getElementById('close-btn').classList.add('hidden');
                document.getElementById('progress-container').classList.add('hidden');
                
                // Reset nav colors
                this.resetNavColors();
            },

            goToDictionary() {
                this.switchView('dictionary');
                document.getElementById('header').classList.remove('hidden');
                document.getElementById('flag-icon').classList.remove('hidden');
                document.getElementById('hearts-display').classList.remove('hidden');
                document.getElementById('bottom-nav').classList.remove('hidden');
                document.getElementById('close-btn').classList.add('hidden');
                document.getElementById('progress-container').classList.add('hidden');
                
                this.resetNavColors();
                // Highlight dictionary tab
                const navBtns = document.querySelectorAll('#bottom-nav button');
                navBtns[1].classList.remove('text-gray-400');
                navBtns[1].classList.add('text-duo-green');
            },

            goToProfile() {
                this.updateProfileUI();
                this.switchView('profile');
                document.getElementById('header').classList.remove('hidden');
                document.getElementById('flag-icon').classList.remove('hidden');
                document.getElementById('hearts-display').classList.remove('hidden');
                document.getElementById('bottom-nav').classList.remove('hidden');
                document.getElementById('close-btn').classList.add('hidden');
                document.getElementById('progress-container').classList.add('hidden');
                
                this.resetNavColors();
                // Highlight profile tab
                const navBtns = document.querySelectorAll('#bottom-nav button');
                navBtns[2].classList.remove('text-gray-400');
                navBtns[2].classList.add('text-duo-green');
            },

            goToSettings() {
                this.switchView('settings');
                document.getElementById('header').classList.remove('hidden');
                document.getElementById('flag-icon').classList.remove('hidden');
                document.getElementById('hearts-display').classList.remove('hidden');
                document.getElementById('bottom-nav').classList.remove('hidden');
                document.getElementById('close-btn').classList.add('hidden');
                document.getElementById('progress-container').classList.add('hidden');
                
                this.resetNavColors();
                // Highlight settings tab
                const navBtns = document.querySelectorAll('#bottom-nav button');
                navBtns[3].classList.remove('text-gray-400');
                navBtns[3].classList.add('text-duo-green');
            },

            resetNavColors() {
                const navBtns = document.querySelectorAll('#bottom-nav button');
                navBtns.forEach(btn => {
                    btn.classList.remove('text-duo-green');
                    btn.classList.add('text-gray-400');
                });
                // Highlight learn tab by default if on dashboard
                if (this.state.currentView === 'dashboard') {
                    navBtns[0].classList.remove('text-gray-400');
                    navBtns[0].classList.add('text-duo-green');
                }
            },

            startLesson(id) {
                this.state.currentLessonId = id;
                this.state.lessonProgress = 0;
                this.state.hearts = 5;
                
                // Increment attempt counter for this lesson to trigger shuffling
                if (!this.state.lessonAttempts[id]) {
                    this.state.lessonAttempts[id] = 0;
                }
                this.state.lessonAttempts[id]++;

                this.generateQuestions(id);
                
                this.switchView('lesson');
                document.getElementById('header').classList.remove('hidden');
                document.getElementById('bottom-nav').classList.add('hidden');
                document.getElementById('flag-icon').classList.add('hidden');
                document.getElementById('hearts-display').classList.remove('hidden');
                document.getElementById('close-btn').classList.remove('hidden');
                document.getElementById('progress-container').classList.remove('hidden');
                
                this.renderQuestion();
                this.updateHearts();
            },

            generateQuestions(lessonId) {
                const attempts = this.state.lessonAttempts[lessonId];
                const isShuffled = attempts > 1;
                const lang = this.state.language;

                // Base question pool based on Lesson ID
                let baseQuestions = [];

                if (lessonId === 1) {
                    if (lang === 'ru') {
                        baseQuestions = [
                            { type: 'select', prompt: 'Как переводится "Вода"?', options: [{ text: "G'ort", correct: false }, { text: "V'a", correct: true }, { text: "Ch'aj", correct: false }, { text: "L'es", correct: false }] },
                            { type: 'select', prompt: 'Выберите правильный перевод: "Я вижу воду"', options: [{ text: "M'e v'idzet v'a", correct: false }, { text: "M'e v'idzu v'a", correct: true }, { text: "T'e v'idzu v'a", correct: false }] },
                            { type: 'match', prompt: 'Что означает "Umniy"?', options: [{ text: "Плохой", correct: false }, { text: "Умный", correct: true }, { text: "Хороший", correct: false }, { text: "Чай", correct: false }] },
                            { type: 'select', prompt: 'Как сказать "Ты" на ö\'lari?', options: [{ text: "M'e", correct: false }, { text: "S'i", correct: false }, { text: "T'e", correct: true }, { text: "N'i", correct: false }] }
                        ];
                    } else {
                        baseQuestions = [
                            { type: 'select', prompt: 'How do you say "Water"?', options: [{ text: "G'ort", correct: false }, { text: "V'a", correct: true }, { text: "Ch'aj", correct: false }, { text: "L'es", correct: false }] },
                            { type: 'select', prompt: 'Select the correct translation: "I see water"', options: [{ text: "M'e v'idzet v'a", correct: false }, { text: "M'e v'idzu v'a", correct: true }, { text: "T'e v'idzu v'a", correct: false }] },
                            { type: 'match', prompt: 'What does "Umniy" mean?', options: [{ text: "Bad", correct: false }, { text: "Smart", correct: true }, { text: "Good", correct: false }, { text: "Tea", correct: false }] },
                            { type: 'select', prompt: 'How do you say "You" in ö\'lari?', options: [{ text: "M'e", correct: false }, { text: "S'i", correct: false }, { text: "T'e", correct: true }, { text: "N'i", correct: false }] }
                        ];
                    }
                } else if (lessonId === 2) {
                    if (lang === 'ru') {
                        baseQuestions = [
                            { type: 'select', prompt: 'Как переводится "Дом"?', options: [{ text: "V'a", correct: false }, { text: "G'ort", correct: true }, { text: "L'es", correct: false }, { text: "Mort", correct: false }] },
                            { type: 'select', prompt: 'Что означает "B\'uri"?', options: [{ text: "Плохой", correct: false }, { text: "Хороший", correct: true }, { text: "Умный", correct: false }, { text: "Чай", correct: false }] },
                            { type: 'select', prompt: 'Как сказать "Лес" на ö\'lari?', options: [{ text: "G'ort", correct: false }, { text: "L'es", correct: true }, { text: "S'olnce", correct: false }, { text: "V'a", correct: false }] },
                            { type: 'match', prompt: 'Выберите перевод: "Я не понимаю"', options: [{ text: "M'e ne ponimayu", correct: true }, { text: "M'e ne v'idzu", correct: false }, { text: "T'e ne govorit", correct: false }] }
                        ];
                    } else {
                        baseQuestions = [
                            { type: 'select', prompt: 'How do you say "House"?', options: [{ text: "V'a", correct: false }, { text: "G'ort", correct: true }, { text: "L'es", correct: false }, { text: "Mort", correct: false }] },
                            { type: 'select', prompt: 'What does "B\'uri" mean?', options: [{ text: "Bad", correct: false }, { text: "Good", correct: true }, { text: "Smart", correct: false }, { text: "Tea", correct: false }] },
                            { type: 'select', prompt: 'How do you say "Forest" in ö\'lari?', options: [{ text: "G'ort", correct: false }, { text: "L'es", correct: true }, { text: "S'olnce", correct: false }, { text: "V'a", correct: false }] },
                            { type: 'match', prompt: 'Select the translation: "I don\'t understand"', options: [{ text: "M'e ne ponimayu", correct: true }, { text: "M'e ne v'idzu", correct: false }, { text: "T'e ne govorit", correct: false }] }
                        ];
                    }
                } else if (lessonId === 3) {
                    // Rules Module Questions
                    if (lang === 'ru') {
                        baseQuestions = [
                            { type: 'select', prompt: 'Как всегда читается связка \'e?', options: [{ text: "[Э] (как в bet)", correct: true }, { text: "[Е] (мягкое)", correct: false }, { text: "[Ё]", correct: false }] },
                            { type: 'select', prompt: 'Как всегда читается буква a?', options: [{ text: "[А] (открытое, как в father)", correct: true }, { text: "[Э]", correct: false }, { text: "[О]", correct: false }] },
                            { type: 'select', prompt: 'Может ли предложение начинаться с апострофа?', options: [{ text: "Нет, никогда", correct: true }, { text: "Да, иногда", correct: false }, { text: "Только в вопросах", correct: false }] },
                            { type: 'match', prompt: 'Какой звук обозначает буква ö?', options: [{ text: "[Ё]", correct: true }, { text: "[О]", correct: false }, { text: "[У]", correct: false }] },
                            { type: 'select', prompt: 'Если клавиатура не поддерживает ö, что можно использовать?', options: [{ text: "yo или jo", correct: true }, { text: "oe", correct: false }, { text: "o\"", correct: false }] }
                        ];
                    } else {
                        baseQuestions = [
                            { type: 'select', prompt: 'How is the combination \'e always pronounced?', options: [{ text: "[E] (as in bet)", correct: true }, { text: "[YE] (soft)", correct: false }, { text: "[YO]", correct: false }] },
                            { type: 'select', prompt: 'How is the letter a always pronounced?', options: [{ text: "[A] (open, as in father)", correct: true }, { text: "[E]", correct: false }, { text: "[O]", correct: false }] },
                            { type: 'select', prompt: 'Can a sentence start with an apostrophe?', options: [{ text: "No, never", correct: true }, { text: "Yes, sometimes", correct: false }, { text: "Only in questions", correct: false }] },
                            { type: 'match', prompt: 'What sound does the letter ö represent?', options: [{ text: "[YO]", correct: true }, { text: "[O]", correct: false }, { text: "[U]", correct: false }] },
                            { type: 'select', prompt: 'If your keyboard doesn\'t support ö, what can you use?', options: [{ text: "yo or jo", correct: true }, { text: "oe", correct: false }, { text: "o\"", correct: false }] }
                        ];
                    }
                }

                // If second time or more, shuffle options within each question
                if (isShuffled) {
                    baseQuestions.forEach(q => {
                        if (q.options) {
                            // Shuffle options array
                            q.options.sort(() => Math.random() - 0.5);
                        }
                    });
                    // Also shuffle question order slightly for variety
                    baseQuestions.sort(() => Math.random() - 0.5);
                }

                this.state.questions = baseQuestions;
            },

            renderQuestion() {
                const q = this.state.questions[this.state.lessonProgress];
                const container = document.getElementById('options-grid');
                const inputArea = document.getElementById('input-area');
                const questionText = document.getElementById('question-text');
                
                // Reset UI
                container.innerHTML = '';
                container.classList.remove('hidden');
                inputArea.classList.add('hidden');
                questionText.innerText = q.prompt;
                this.state.selectedOption = null;
                this.resetFeedback();

                // Update Progress Bar
                const progressPct = (this.state.lessonProgress / this.state.questions.length) * 100;
                document.getElementById('progress-bar').style.width = `${progressPct}%`;

                if (q.type === 'select' || q.type === 'match') {
                    q.options.forEach((opt, index) => {
                        const btn = document.createElement('div');
                        btn.className = 'option-card border-2 border-gray-200 rounded-2xl p-4 cursor-pointer font-bold text-lg text-gray-700 hover:bg-gray-50 transition-all';
                        btn.innerText = opt.text;
                        btn.onclick = () => this.selectOption(btn, index);
                        container.appendChild(btn);
                    });
                } else if (q.type === 'translate') {
                    container.classList.add('hidden');
                    inputArea.classList.remove('hidden');
                    document.getElementById('translation-input').value = '';
                }
            },

            selectOption(el, index) {
                // Remove selected from others
                const options = document.querySelectorAll('.option-card');
                options.forEach(opt => opt.classList.remove('selected'));
                
                // Add to clicked
                el.classList.add('selected');
                this.state.selectedOption = index;
            },

            checkAnswer() {
                const q = this.state.questions[this.state.lessonProgress];
                let isCorrect = false;

                if (q.type === 'select' || q.type === 'match') {
                    if (this.state.selectedOption === null) return; // Nothing selected
                    isCorrect = q.options[this.state.selectedOption].correct;
                    
                    // Visual feedback on cards
                    const options = document.querySelectorAll('.option-card');
                    if (isCorrect) {
                        options[this.state.selectedOption].classList.add('correct');
                    } else {
                        options[this.state.selectedOption].classList.add('wrong');
                        options[this.state.selectedOption].classList.add('animate-shake');
                        // Highlight correct one
                        q.options.forEach((opt, idx) => {
                            if(opt.correct) options[idx].classList.add('correct');
                        });
                    }
                } else if (q.type === 'translate') {
                    // Simple mock logic for translation
                    const val = document.getElementById('translation-input').value.toLowerCase().trim();
                    // Mock answer for demo
                    isCorrect = val.includes('water') || val.includes('вода'); 
                }

                this.showFeedback(isCorrect);
            },

            showFeedback(isCorrect) {
                const sheet = document.getElementById('feedback-sheet');
                const title = document.getElementById('feedback-title');
                const msg = document.getElementById('feedback-msg');
                const icon = document.getElementById('feedback-icon');
                const btn = document.getElementById('continue-btn');
                const lang = this.state.language;

                sheet.classList.remove('translate-y-full');
                
                if (isCorrect) {
                    sheet.classList.remove('bg-red-100', 'border-red-200');
                    sheet.classList.add('bg-green-100', 'border-green-200');
                    title.innerText = lang === 'ru' ? "Верно!" : "Correct!";
                    title.className = "text-xl font-bold mb-1 text-green-700";
                    msg.innerText = lang === 'ru' ? "Отличная работа." : "Great job.";
                    msg.className = "text-sm mb-4 text-green-700";
                    icon.className = "w-14 h-14 rounded-full flex items-center justify-center flex-shrink-0 bg-green-500 text-white";
                    icon.innerHTML = `<svg xmlns="http://www.w3.org/2000/svg" class="h-8 w-8" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="3" d="M5 13l4 4L19 7" /></svg>`;
                    btn.className = "btn-3d bg-green-500 text-white font-bold py-3 px-6 rounded-2xl uppercase tracking-wide min-w-[120px]";
                    btn.style.backgroundColor = "#58cc02";
                    btn.style.borderBottomColor = "#46a302";
                } else {
                    this.state.hearts--;
                    this.updateHearts();
                    sheet.classList.remove('bg-green-100', 'border-green-200');
                    sheet.classList.add('bg-red-100', 'border-red-200');
                    title.innerText = lang === 'ru' ? "Неверно" : "Incorrect";
                    title.className = "text-xl font-bold mb-1 text-red-700";
                    msg.innerText = lang === 'ru' ? "Правильный ответ выделен зеленым." : "The correct answer is highlighted in green.";
                    msg.className = "text-sm mb-4 text-red-700";
                    icon.className = "w-14 h-14 rounded-full flex items-center justify-center flex-shrink-0 bg-red-500 text-white";
                    icon.innerHTML = `<svg xmlns="http://www.w3.org/2000/svg" class="h-8 w-8" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="3" d="M6 18L18 6M6 6l12 12" /></svg>`;
                    btn.className = "btn-3d bg-red-500 text-white font-bold py-3 px-6 rounded-2xl uppercase tracking-wide min-w-[120px]";
                    btn.style.backgroundColor = "#ff4b4b";
                    btn.style.borderBottomColor = "#ea2b2b";
                }
            },

            resetFeedback() {
                const sheet = document.getElementById('feedback-sheet');
                sheet.classList.add('translate-y-full');
            },

            nextQuestion() {
                this.state.lessonProgress++;
                if (this.state.lessonProgress >= this.state.questions.length) {
                    // Lesson Finished Logic
                    this.state.user.xp += 10;
                    
                    this.switchView('result');
                    document.getElementById('header').classList.add('hidden');
                    document.getElementById('bottom-nav').classList.add('hidden');
                } else {
                    this.renderQuestion();
                }
            },

            skipQuestion() {
                // Penalty for skipping
                this.state.user.xp = Math.max(0, this.state.user.xp - 20);
                this.nextQuestion();
            },

            updateHearts() {
                document.getElementById('hearts-count').innerText = this.state.hearts;
                if(this.state.hearts <= 0) {
                    alert("Out of hearts! Starting over.");
                    this.goToDashboard();
                }
            },

            switchView(viewName) {
                // Hide all views
                ['landing', 'login', 'dashboard', 'lesson', 'result', 'dictionary', 'profile', 'settings'].forEach(v => {
                    document.getElementById(`view-${v}`).classList.add('hidden');
                });
                // Show target
                document.getElementById(`view-${viewName}`).classList.remove('hidden');
                this.state.currentView = viewName;
            },

            updateUI() {
                // Initial setup if needed
            }
        };

        // Initialize
        document.addEventListener('DOMContentLoaded', () => {
            app.init();
        });

    </script>
</body>
</html>
```
