# Pencocokan
Sistem operasi
<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Belajar Hardware - Game Pencocokan Memori</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600;700&display=swap');
        
        body {
            font-family: 'Poppins', sans-serif;
        }
        
        .card-flip {
            transform-style: preserve-3d;
            transition: transform 0.6s;
        }
        
        .card-flip.flipped {
            transform: rotateY(180deg);
        }
        
        .card-front, .card-back {
            backface-visibility: hidden;
            position: absolute;
            width: 100%;
            height: 100%;
        }
        
        .card-back {
            transform: rotateY(180deg);
        }
        
        .pulse-animation {
            animation: pulse 2s infinite;
        }
        
        @keyframes pulse {
            0%, 100% { transform: scale(1); }
            50% { transform: scale(1.05); }
        }
        
        .bounce-in {
            animation: bounceIn 0.5s ease-out;
        }
        
        @keyframes bounceIn {
            0% { transform: scale(0.3); opacity: 0; }
            50% { transform: scale(1.05); }
            70% { transform: scale(0.9); }
            100% { transform: scale(1); opacity: 1; }
        }
        
        .shake {
            animation: shake 0.5s ease-in-out;
        }
        
        @keyframes shake {
            0%, 100% { transform: translateX(0); }
            25% { transform: translateX(-5px); }
            75% { transform: translateX(5px); }
        }
        
        .floating {
            animation: floating 3s ease-in-out infinite;
        }
        
        @keyframes floating {
            0%, 100% { transform: translateY(0px); }
            50% { transform: translateY(-10px); }
        }
    </style>
</head>
<body class="min-h-screen bg-gradient-to-br from-blue-400 via-green-500 to-teal-500">
    <div class="container mx-auto px-4 py-8">
        <!-- Header -->
        <div class="text-center mb-8">
            <h1 class="text-4xl md:text-5xl font-bold text-white mb-4 drop-shadow-lg floating">🖥️ Belajar Hardware</h1>
            <p class="text-xl text-white/90 mb-6">Game Pencocokan Memori untuk Pembelajaran yang Lebih Seru!</p>
            
            <!-- Game Stats -->
            <div class="flex justify-center gap-4 mb-6 flex-wrap">
                <div class="bg-white/20 backdrop-blur-sm rounded-lg px-4 py-2 text-white">
                    <div class="text-sm opacity-80">Langkah</div>
                    <div class="text-2xl font-bold" id="moves">0</div>
                </div>
                <div class="bg-white/20 backdrop-blur-sm rounded-lg px-4 py-2 text-white">
                    <div class="text-sm opacity-80">Pasangan</div>
                    <div class="text-2xl font-bold" id="matches">0/9</div>
                </div>
                <div class="bg-white/20 backdrop-blur-sm rounded-lg px-4 py-2 text-white">
                    <div class="text-sm opacity-80">Waktu</div>
                    <div class="text-2xl font-bold" id="timer">00:00</div>
                </div>
                <div class="bg-white/20 backdrop-blur-sm rounded-lg px-4 py-2 text-white">
                    <div class="text-sm opacity-80">Skor</div>
                    <div class="text-2xl font-bold" id="score">0</div>
                </div>
            </div>
            
            <button onclick="resetGame()" class="bg-white text-blue-600 px-6 py-3 rounded-full font-semibold hover:bg-blue-50 transition-all duration-300 shadow-lg hover:shadow-xl transform hover:scale-105">
                🔄 Mulai Ulang
            </button>
        </div>

        <!-- Game Board -->
        <div class="max-w-5xl mx-auto mb-8">
            <div class="grid grid-cols-3 md:grid-cols-6 gap-3" id="gameBoard">
                <!-- Cards will be generated here -->
            </div>
        </div>

        <!-- Hardware Questions -->
        <div class="max-w-3xl mx-auto bg-white/95 backdrop-blur-sm rounded-2xl p-6 shadow-2xl" id="questionCard">
            <h3 class="text-2xl font-bold text-gray-800 mb-4 text-center">🧠 Pertanyaan Hardware</h3>
            <div class="text-center">
                <p class="text-lg text-gray-700 mb-4" id="questionText">Cocokkan semua kartu untuk mendapatkan pertanyaan tentang hardware komputer!</p>
                <div class="mb-4" id="answerSection" style="display: none;">
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-3 mb-4" id="answerOptions">
                        <!-- Answer options will be generated here -->
                    </div>
                    <div class="text-sm text-gray-600" id="feedback"></div>
                </div>
                <button onclick="getNewQuestion()" class="bg-gradient-to-r from-blue-500 to-green-500 text-white px-6 py-3 rounded-full font-semibold hover:from-blue-600 hover:to-green-600 transition-all duration-300 shadow-lg hover:shadow-xl transform hover:scale-105" id="newQuestionBtn" style="display: none;">
                    🎲 Pertanyaan Baru
                </button>
            </div>
        </div>

        <!-- Success Modal -->
        <div id="successModal" class="fixed inset-0 bg-black/50 backdrop-blur-sm flex items-center justify-center z-50" style="display: none;">
            <div class="bg-white rounded-2xl p-8 max-w-md mx-4 text-center bounce-in">
                <div class="text-6xl mb-4">🎉</div>
                <h2 class="text-3xl font-bold text-gray-800 mb-4">Hebat!</h2>
                <p class="text-gray-600 mb-6">Anda berhasil mencocokkan semua kartu hardware!</p>
                <div class="text-sm text-gray-500 mb-6">
                    <div>Waktu: <span id="finalTime" class="font-semibold"></span></div>
                    <div>Langkah: <span id="finalMoves" class="font-semibold"></span></div>
                    <div>Skor: <span id="finalScore" class="font-semibold"></span></div>
                </div>
                <button onclick="closeModal()" class="bg-gradient-to-r from-blue-500 to-green-500 text-white px-8 py-3 rounded-full font-semibold hover:from-blue-600 hover:to-green-600 transition-all duration-300">
                    Lanjutkan Belajar
                </button>
            </div>
        </div>
    </div>

    <script>
        // Game state
        let gameState = {
            cards: [],
            flippedCards: [],
            matches: 0,
            moves: 0,
            score: 0,
            gameStarted: false,
            gameCompleted: false,
            startTime: null,
            timerInterval: null
        };

        // Hardware emoji pairs
        const hardwarePairs = [
            '💻', '🖥️', '⌨️', '🖱️', '🖨️', '📱', '💾', '🔌', '🎧'
        ];

        // Hardware questions with multiple choice answers
        const hardwareQuestions = [
            {
                question: "Apa fungsi utama dari CPU (Central Processing Unit)?",
                options: ["Menyimpan data", "Memproses instruksi", "Menampilkan gambar", "Memutar musik"],
                correct: 1,
                explanation: "CPU adalah 'otak' komputer yang memproses semua instruksi dan perhitungan."
            },
            {
                question: "Komponen mana yang berfungsi sebagai memori jangka pendek komputer?",
                options: ["Hard Drive", "RAM", "CPU", "Motherboard"],
                correct: 1,
                explanation: "RAM (Random Access Memory) menyimpan data sementara yang sedang digunakan."
            },
            {
                question: "Apa kepanjangan dari GPU?",
                options: ["General Processing Unit", "Graphics Processing Unit", "Game Processing Unit", "Global Processing Unit"],
                correct: 1,
                explanation: "GPU (Graphics Processing Unit) khusus memproses grafik dan visual."
            },
            {
                question: "Komponen mana yang menghubungkan semua bagian komputer?",
                options: ["RAM", "CPU", "Motherboard", "Power Supply"],
                correct: 2,
                explanation: "Motherboard adalah papan sirkuit utama yang menghubungkan semua komponen."
            },
            {
                question: "Apa fungsi utama dari Power Supply Unit (PSU)?",
                options: ["Mendinginkan komputer", "Menyediakan listrik", "Menyimpan data", "Memproses grafik"],
                correct: 1,
                explanation: "PSU mengubah listrik AC menjadi DC untuk semua komponen komputer."
            },
            {
                question: "Jenis memori mana yang tetap menyimpan data meski komputer dimatikan?",
                options: ["RAM", "Cache", "ROM", "Register"],
                correct: 2,
                explanation: "ROM (Read-Only Memory) menyimpan data permanen seperti BIOS."
            },
            {
                question: "Apa fungsi utama dari heat sink dan cooling fan?",
                options: ["Mempercepat proses", "Mendinginkan komponen", "Menyimpan data", "Menghemat listrik"],
                correct: 1,
                explanation: "Heat sink dan fan mencegah komponen panas berlebihan yang bisa merusak."
            },
            {
                question: "Port mana yang paling umum digunakan untuk menghubungkan perangkat modern?",
                options: ["Serial Port", "Parallel Port", "USB Port", "PS/2 Port"],
                correct: 2,
                explanation: "USB (Universal Serial Bus) adalah standar koneksi paling umum saat ini."
            },
            {
                question: "Apa perbedaan utama antara HDD dan SSD?",
                options: ["Ukuran fisik", "Cara menyimpan data", "Warna", "Harga saja"],
                correct: 1,
                explanation: "HDD menggunakan piringan magnetik, SSD menggunakan chip flash memory."
            },
            {
                question: "Berapa bit yang dapat diproses CPU 64-bit dalam satu waktu?",
                options: ["32 bit", "64 bit", "128 bit", "16 bit"],
                correct: 1,
                explanation: "CPU 64-bit dapat memproses 64 bit data sekaligus, lebih efisien dari 32-bit."
            }
        ];

        let currentQuestionIndex = 0;
        let currentQuestion = null;

        // Initialize game
        function initGame() {
            createCards();
            renderCards();
            resetStats();
        }

        // Create card array with pairs
        function createCards() {
            gameState.cards = [];
            
            // Create pairs
            hardwarePairs.forEach((emoji, index) => {
                gameState.cards.push({
                    id: index * 2,
                    emoji: emoji,
                    isFlipped: false,
                    isMatched: false
                });
                gameState.cards.push({
                    id: index * 2 + 1,
                    emoji: emoji,
                    isFlipped: false,
                    isMatched: false
                });
            });
            
            // Shuffle cards
            shuffleArray(gameState.cards);
        }

        // Shuffle array function
        function shuffleArray(array) {
            for (let i = array.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [array[i], array[j]] = [array[j], array[i]];
            }
        }

        // Render cards to DOM
        function renderCards() {
            const gameBoard = document.getElementById('gameBoard');
            gameBoard.innerHTML = '';
            
            gameState.cards.forEach((card, index) => {
                const cardElement = document.createElement('div');
                cardElement.className = 'relative w-full aspect-square cursor-pointer';
                cardElement.innerHTML = `
                    <div class="card-flip w-full h-full ${card.isFlipped || card.isMatched ? 'flipped' : ''}" data-index="${index}">
                        <div class="card-front bg-gradient-to-br from-indigo-400 to-purple-500 rounded-xl shadow-lg flex items-center justify-center border-4 border-white/30 hover:border-white/50 transition-all duration-300 hover:shadow-xl">
                            <div class="text-3xl">🔧</div>
                        </div>
                        <div class="card-back bg-gradient-to-br from-yellow-300 to-orange-400 rounded-xl shadow-lg flex items-center justify-center border-4 border-white/30">
                            <div class="text-3xl">${card.emoji}</div>
                        </div>
                    </div>
                `;
                
                cardElement.addEventListener('click', () => flipCard(index));
                gameBoard.appendChild(cardElement);
            });
        }

        // Flip card function
        function flipCard(index) {
            const card = gameState.cards[index];
            
            // Don't flip if card is already flipped, matched, or game is completed
            if (card.isFlipped || card.isMatched || gameState.gameCompleted) return;
            
            // Don't flip if two cards are already flipped
            if (gameState.flippedCards.length >= 2) return;
            
            // Start timer on first move
            if (!gameState.gameStarted) {
                startTimer();
                gameState.gameStarted = true;
            }
            
            // Flip the card
            card.isFlipped = true;
            gameState.flippedCards.push(index);
            
            // Update display
            updateCardDisplay(index);
            
            // Check for match if two cards are flipped
            if (gameState.flippedCards.length === 2) {
                gameState.moves++;
                updateStats();
                
                setTimeout(() => {
                    checkMatch();
                }, 1000);
            }
        }

        // Update card display
        function updateCardDisplay(index) {
            const cardElement = document.querySelector(`[data-index="${index}"]`);
            const card = gameState.cards[index];
            
            if (card.isFlipped || card.isMatched) {
                cardElement.classList.add('flipped');
            } else {
                cardElement.classList.remove('flipped');
            }
        }

        // Check for match
        function checkMatch() {
            const [firstIndex, secondIndex] = gameState.flippedCards;
            const firstCard = gameState.cards[firstIndex];
            const secondCard = gameState.cards[secondIndex];
            
            if (firstCard.emoji === secondCard.emoji) {
                // Match found
                firstCard.isMatched = true;
                secondCard.isMatched = true;
                gameState.matches++;
                
                // Calculate score (bonus for fewer moves)
                const timeBonus = Math.max(0, 100 - Math.floor((Date.now() - gameState.startTime) / 1000));
                const moveBonus = Math.max(0, 50 - gameState.moves);
                gameState.score += 100 + timeBonus + moveBonus;
                
                // Add pulse animation to matched cards
                const firstElement = document.querySelector(`[data-index="${firstIndex}"]`);
                const secondElement = document.querySelector(`[data-index="${secondIndex}"]`);
                firstElement.classList.add('pulse-animation');
                secondElement.classList.add('pulse-animation');
                
                setTimeout(() => {
                    firstElement.classList.remove('pulse-animation');
                    secondElement.classList.remove('pulse-animation');
                }, 2000);
                
                // Check if game is completed
                if (gameState.matches === hardwarePairs.length) {
                    gameCompleted();
                }
            } else {
                // No match - flip cards back
                firstCard.isFlipped = false;
                secondCard.isFlipped = false;
                
                // Add shake animation
                const firstElement = document.querySelector(`[data-index="${firstIndex}"]`);
                const secondElement = document.querySelector(`[data-index="${secondIndex}"]`);
                firstElement.classList.add('shake');
                secondElement.classList.add('shake');
                
                setTimeout(() => {
                    updateCardDisplay(firstIndex);
                    updateCardDisplay(secondIndex);
                    firstElement.classList.remove('shake');
                    secondElement.classList.remove('shake');
                }, 500);
            }
            
            // Clear flipped cards
            gameState.flippedCards = [];
            updateStats();
        }

        // Game completed
        function gameCompleted() {
            gameState.gameCompleted = true;
            stopTimer();
            
            // Show success modal
            document.getElementById('finalTime').textContent = document.getElementById('timer').textContent;
            document.getElementById('finalMoves').textContent = gameState.moves;
            document.getElementById('finalScore').textContent = gameState.score;
            document.getElementById('successModal').style.display = 'flex';
            
            // Show hardware question
            showHardwareQuestion();
        }

        // Show hardware question
        function showHardwareQuestion() {
            currentQuestion = hardwareQuestions[currentQuestionIndex];
            const questionText = document.getElementById('questionText');
            const answerSection = document.getElementById('answerSection');
            const answerOptions = document.getElementById('answerOptions');
            const newQuestionBtn = document.getElementById('newQuestionBtn');
            
            questionText.textContent = currentQuestion.question;
            answerSection.style.display = 'block';
            newQuestionBtn.style.display = 'inline-block';
            
            // Create answer options
            answerOptions.innerHTML = '';
            currentQuestion.options.forEach((option, index) => {
                const button = document.createElement('button');
                button.className = 'bg-gray-100 hover:bg-blue-100 text-gray-800 px-4 py-3 rounded-lg transition-all duration-300 text-left';
                button.textContent = `${String.fromCharCode(65 + index)}. ${option}`;
                button.onclick = () => selectAnswer(index);
                answerOptions.appendChild(button);
            });
            
            document.getElementById('feedback').textContent = '';
        }

        // Select answer
        function selectAnswer(selectedIndex) {
            const feedback = document.getElementById('feedback');
            const buttons = document.querySelectorAll('#answerOptions button');
            
            // Disable all buttons
            buttons.forEach(button => button.disabled = true);
            
            if (selectedIndex === currentQuestion.correct) {
                buttons[selectedIndex].className = 'bg-green-500 text-white px-4 py-3 rounded-lg';
                feedback.textContent = `✅ Benar! ${currentQuestion.explanation}`;
                feedback.className = 'text-sm text-green-600 font-medium';
                gameState.score += 50;
            } else {
                buttons[selectedIndex].className = 'bg-red-500 text-white px-4 py-3 rounded-lg';
                buttons[currentQuestion.correct].className = 'bg-green-500 text-white px-4 py-3 rounded-lg';
                feedback.textContent = `❌ Salah. ${currentQuestion.explanation}`;
                feedback.className = 'text-sm text-red-600 font-medium';
            }
            
            updateStats();
        }

        // Get new question
        function getNewQuestion() {
            currentQuestionIndex = (currentQuestionIndex + 1) % hardwareQuestions.length;
            showHardwareQuestion();
        }

        // Close modal
        function closeModal() {
            document.getElementById('successModal').style.display = 'none';
        }

        // Start timer
        function startTimer() {
            gameState.startTime = Date.now();
            gameState.timerInterval = setInterval(updateTimer, 1000);
        }

        // Stop timer
        function stopTimer() {
            if (gameState.timerInterval) {
                clearInterval(gameState.timerInterval);
                gameState.timerInterval = null;
            }
        }

        // Update timer display
        function updateTimer() {
            if (!gameState.startTime) return;
            
            const elapsed = Math.floor((Date.now() - gameState.startTime) / 1000);
            const minutes = Math.floor(elapsed / 60);
            const seconds = elapsed % 60;
            
            document.getElementById('timer').textContent = 
                `${minutes.toString().padStart(2, '0')}:${seconds.toString().padStart(2, '0')}`;
        }

        // Update stats display
        function updateStats() {
            document.getElementById('moves').textContent = gameState.moves;
            document.getElementById('matches').textContent = `${gameState.matches}/${hardwarePairs.length}`;
            document.getElementById('score').textContent = gameState.score;
        }

        // Reset stats
        function resetStats() {
            gameState.moves = 0;
            gameState.matches = 0;
            gameState.score = 0;
            gameState.gameStarted = false;
            gameState.gameCompleted = false;
            gameState.flippedCards = [];
            
            stopTimer();
            gameState.startTime = null;
            
            document.getElementById('timer').textContent = '00:00';
            document.getElementById('questionText').textContent = 'Cocokkan semua kartu untuk mendapatkan pertanyaan tentang hardware komputer!';
            document.getElementById('answerSection').style.display = 'none';
            document.getElementById('newQuestionBtn').style.display = 'none';
            
            updateStats();
        }

        // Reset game
        function resetGame() {
            resetStats();
            initGame();
            closeModal();
        }

        // Initialize game on page load
        initGame();
    </script>
<script>(function(){function c(){var b=a.contentDocument||a.contentWindow.document;if(b){var d=b.createElement('script');d.innerHTML="window.__CF$cv$params={r:'981c884d40c67156',t:'MTc1ODMyMTA3Ny4wMDAwMDA='};var a=document.createElement('script');a.nonce='';a.src='/cdn-cgi/challenge-platform/scripts/jsd/main.js';document.getElementsByTagName('head')[0].appendChild(a);";b.getElementsByTagName('head')[0].appendChild(d)}}if(document.body){var a=document.createElement('iframe');a.height=1;a.width=1;a.style.position='absolute';a.style.top=0;a.style.left=0;a.style.border='none';a.style.visibility='hidden';document.body.appendChild(a);if('loading'!==document.readyState)c();else if(window.addEventListener)document.addEventListener('DOMContentLoaded',c);else{var e=document.onreadystatechange||function(){};document.onreadystatechange=function(b){e(b);'loading'!==document.readyState&&(document.onreadystatechange=e,c())}}}})();</script></body>
</html>
