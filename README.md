<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Quiz Bangun Datar</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }

        .container {
            background: white;
            border-radius: 20px;
            box-shadow: 0 20px 60px rgba(0,0,0,0.3);
            max-width: 800px;
            width: 100%;
            padding: 40px;
            animation: slideIn 0.5s ease-out;
        }

        @keyframes slideIn {
            from {
                opacity: 0;
                transform: translateY(-30px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        h1 {
            text-align: center;
            color: #667eea;
            margin-bottom: 10px;
            font-size: 2.5em;
        }

        .subtitle {
            text-align: center;
            color: #666;
            margin-bottom: 30px;
            font-size: 1.1em;
        }

        .score-board {
            display: flex;
            justify-content: space-between;
            margin-bottom: 30px;
            padding: 15px;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            border-radius: 10px;
            color: white;
            font-weight: bold;
        }

        .question-container {
            margin-bottom: 30px;
        }

        .question-number {
            color: #667eea;
            font-weight: bold;
            margin-bottom: 10px;
            font-size: 1.1em;
        }

        .question-text {
            font-size: 1.3em;
            margin-bottom: 20px;
            color: #333;
            line-height: 1.6;
        }

        .shape-visual {
            text-align: center;
            margin: 20px 0;
            padding: 20px;
            background: #f8f9fa;
            border-radius: 10px;
        }

        .options {
            display: grid;
            gap: 15px;
        }

        .option {
            padding: 15px 20px;
            border: 2px solid #e0e0e0;
            border-radius: 10px;
            cursor: pointer;
            transition: all 0.3s ease;
            background: white;
            font-size: 1.1em;
        }

        .option:hover {
            border-color: #667eea;
            background: #f0f4ff;
            transform: translateX(5px);
        }

        .option.selected {
            border-color: #667eea;
            background: #667eea;
            color: white;
        }

        .option.correct {
            border-color: #4caf50;
            background: #4caf50;
            color: white;
            animation: pulse 0.5s ease;
        }

        .option.wrong {
            border-color: #f44336;
            background: #f44336;
            color: white;
            animation: shake 0.5s ease;
        }

        @keyframes pulse {
            0%, 100% { transform: scale(1); }
            50% { transform: scale(1.05); }
        }

        @keyframes shake {
            0%, 100% { transform: translateX(0); }
            25% { transform: translateX(-10px); }
            75% { transform: translateX(10px); }
        }

        .feedback {
            margin-top: 20px;
            padding: 15px;
            border-radius: 10px;
            font-weight: bold;
            display: none;
        }

        .feedback.correct {
            background: #d4edda;
            color: #155724;
            border: 2px solid #4caf50;
        }

        .feedback.wrong {
            background: #f8d7da;
            color: #721c24;
            border: 2px solid #f44336;
        }

        .btn {
            padding: 15px 30px;
            border: none;
            border-radius: 10px;
            cursor: pointer;
            font-size: 1.1em;
            font-weight: bold;
            transition: all 0.3s ease;
            margin-top: 20px;
        }

        .btn-next {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            width: 100%;
        }

        .btn-next:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(102, 126, 234, 0.4);
        }

        .btn-next:disabled {
            background: #ccc;
            cursor: not-allowed;
            transform: none;
        }

        .result-container {
            text-align: center;
            display: none;
        }

        .result-score {
            font-size: 4em;
            color: #667eea;
            margin: 20px 0;
            font-weight: bold;
        }

        .result-message {
            font-size: 1.5em;
            margin: 20px 0;
            color: #333;
        }

        .btn-restart {
            background: linear-gradient(135deg, #f093fb 0%, #f5576c 100%);
            color: white;
        }

        .progress-bar {
            width: 100%;
            height: 10px;
            background: #e0e0e0;
            border-radius: 5px;
            margin-bottom: 20px;
            overflow: hidden;
        }

        .progress-fill {
            height: 100%;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            transition: width 0.3s ease;
        }

        svg {
            max-width: 200px;
            height: auto;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>🎯 Quiz Bangun Datar</h1>
        <p class="subtitle">Uji Pengetahuanmu tentang Bangun Datar!</p>
        
        <div class="progress-bar">
            <div class="progress-fill" id="progressBar"></div>
        </div>

        <div class="score-board">
            <span>Soal: <span id="currentQuestion">1</span>/<span id="totalQuestions">15</span></span>
            <span>Skor: <span id="score">0</span></span>
        </div>

        <div id="quizContainer">
            <div class="question-container">
                <div class="question-number" id="questionNumber"></div>
                <div class="question-text" id="questionText"></div>
                <div class="shape-visual" id="shapeVisual"></div>
                <div class="options" id="options"></div>
                <div class="feedback" id="feedback"></div>
            </div>
            <button class="btn btn-next" id="nextBtn" disabled>Lanjut →</button>
        </div>

        <div class="result-container" id="resultContainer">
            <h2>🎉 Quiz Selesai!</h2>
            <div class="result-score" id="finalScore"></div>
            <div class="result-message" id="resultMessage"></div>
            <button class="btn btn-restart" onclick="restartQuiz()">🔄 Main Lagi</button>
        </div>
    </div>

    <script>
        const questions = [
            {
                question: "Apa yang dimaksud dengan bangun datar?",
                visual: "",
                options: [
                    "Bangun yang memiliki volume",
                    "Bangun dua dimensi yang memiliki panjang dan lebar",
                    "Bangun tiga dimensi",
                    "Bangun yang hanya memiliki tinggi"
                ],
                correct: 1,
                explanation: "Bangun datar adalah bangun dua dimensi yang hanya memiliki panjang dan lebar, tidak memiliki ketebalan atau volume."
            },
            {
                question: "Persegi memiliki sifat khusus. Manakah pernyataan yang benar tentang persegi?",
                visual: '<svg width="150" height="150"><rect x="25" y="25" width="100" height="100" fill="#667eea" stroke="#333" stroke-width="2"/></svg>',
                options: [
                    "Memiliki 4 sisi yang berbeda panjang",
                    "Memiliki 4 sisi sama panjang dan 4 sudut siku-siku",
                    "Memiliki 2 sisi sejajar",
                    "Tidak memiliki sudut"
                ],
                correct: 1,
                explanation: "Persegi memiliki 4 sisi yang sama panjang dan 4 sudut siku-siku (90°)."
            },
            {
                question: "Jika sisi persegi adalah 8 cm, berapakah luasnya?",
                visual: '<svg width="150" height="150"><rect x="25" y="25" width="100" height="100" fill="#f093fb" stroke="#333" stroke-width="2"/><text x="75" y="80" text-anchor="middle" font-size="16" fill="white">8 cm</text></svg>',
                options: [
                    "16 cm²",
                    "32 cm²",
                    "64 cm²",
                    "24 cm²"
                ],
                correct: 2,
                explanation: "Luas persegi = sisi × sisi = 8 × 8 = 64 cm²"
            },
            {
                question: "Rumus keliling persegi panjang adalah...",
                visual: '<svg width="200" height="120"><rect x="25" y="25" width="150" height="70" fill="#764ba2" stroke="#333" stroke-width="2"/></svg>',
                options: [
                    "2 × (panjang + lebar)",
                    "panjang × lebar",
                    "4 × sisi",
                    "1/2 × alas × tinggi"
                ],
                correct: 0,
                explanation: "Keliling persegi panjang = 2 × (panjang + lebar)"
            },
            {
                question: "Segitiga dengan ketiga sisinya sama panjang disebut...",
                visual: '<svg width="150" height="150"><polygon points="75,25 25,125 125,125" fill="#4caf50" stroke="#333" stroke-width="2"/></svg>',
                options: [
                    "Segitiga siku-siku",
                    "Segitiga sama kaki",
                    "Segitiga sama sisi",
                    "Segitiga sembarang"
                ],
                correct: 2,
                explanation: "Segitiga sama sisi memiliki ketiga sisi yang sama panjang dan ketiga sudutnya sama besar (60°)."
            },
            {
                question: "Jika alas segitiga 10 cm dan tinggi 6 cm, berapakah luasnya?",
                visual: '<svg width="150" height="150"><polygon points="75,25 25,125 125,125" fill="#ff9800" stroke="#333" stroke-width="2"/><line x1="75" y1="25" x2="75" y2="125" stroke="red" stroke-width="2" stroke-dasharray="5,5"/><text x="80" y="75" font-size="12" fill="red">6 cm</text><text x="75" y="140" text-anchor="middle" font-size="12">10 cm</text></svg>',
                options: [
                    "60 cm²",
                    "30 cm²",
                    "16 cm²",
                    "20 cm²"
                ],
                correct: 1,
                explanation: "Luas segitiga = 1/2 × alas × tinggi = 1/2 × 10 × 6 = 30 cm²"
            },
            {
                question: "Lingkaran memiliki sifat khusus. Apa yang dimaksud dengan diameter?",
                visual: '<svg width="150" height="150"><circle cx="75" cy="75" r="50" fill="#2196f3" stroke="#333" stroke-width="2"/><line x1="25" y1="75" x2="125" y2="75" stroke="red" stroke-width="2"/></svg>',
                options: [
                    "Jarak dari pusat ke tepi lingkaran",
                    "Garis yang menghubungkan dua titik pada lingkaran melalui pusat",
                    "Jarak mengelilingi lingkaran",
                    "Setengah dari keliling lingkaran"
                ],
                correct: 1,
                explanation: "Diameter adalah garis yang menghubungkan dua titik pada lingkaran dan melalui pusat. Diameter = 2 × jari-jari."
            },
            {
                question: "Jika jari-jari lingkaran 7 cm, berapakah kelilingnya? (gunakan π = 22/7)",
                visual: '<svg width="150" height="150"><circle cx="75" cy="75" r="50" fill="#e91e63" stroke="#333" stroke-width="2"/><line x1="75" y1="75" x2="125" y2="75" stroke="white" stroke-width="2"/><text x="100" y="70" font-size="12" fill="white">r=7</text></svg>',
                options: [
                    "22 cm",
                    "44 cm",
                    "154 cm",
                    "14 cm"
                ],
                correct: 1,
                explanation: "Keliling lingkaran = 2 × π × r = 2 × 22/7 × 7 = 44 cm"
            },
            {
                question: "Trapesium adalah bangun datar yang memiliki...",
                visual: '<svg width="180" height="120"><polygon points="40,25 140,25 160,95 20,95" fill="#9c27b0" stroke="#333" stroke-width="2"/></svg>',
                options: [
                    "4 sisi sama panjang",
                    "Sepasang sisi yang sejajar",
                    "Tidak ada sisi yang sejajar",
                    "3 sisi sama panjang"
                ],
                correct: 1,
                explanation: "Trapesium memiliki sepasang sisi yang sejajar (sisi atas dan bawah)."
            },
            {
                question: "Jajargenjang memiliki sifat...",
                visual: '<svg width="180" height="120"><polygon points="30,25 150,25 120,95 0,95" fill="#00bcd4" stroke="#333" stroke-width="2"/></svg>',
                options: [
                    "Sudut-sudut berhadapan sama besar",
                    "Semua sudut adalah sudut siku-siku",
                    "Hanya memiliki 2 sisi",
                    "Tidak memiliki sisi sejajar"
                ],
                correct: 0,
                explanation: "Jajargenjang memiliki sisi-sisi yang berhadapan sama panjang dan sejajar, serta sudut-sudut yang berhadapan sama besar."
            },
            {
                question: "Belah ketupat memiliki berapa simetri lipat?",
                visual: '<svg width="150" height="150"><polygon points="75,25 125,75 75,125 25,75" fill="#ff5722" stroke="#333" stroke-width="2"/></svg>',
                options: [
                    "1",
                    "2",
                    "4",
                    "Tidak ada"
                ],
                correct: 1,
                explanation: "Belah ketupat memiliki 2 simetri lipat, yaitu diagonal-diagonalnya."
            },
            {
                question: "Jika panjang persegi panjang 12 cm dan lebar 5 cm, berapakah luasnya?",
                visual: '<svg width="180" height="120"><rect x="20" y="30" width="140" height="60" fill="#3f51b5" stroke="#333" stroke-width="2"/><text x="90" y="65" text-anchor="middle" font-size="12" fill="white">12 cm × 5 cm</text></svg>',
                options: [
                    "17 cm²",
                    "34 cm²",
                    "60 cm²",
                    "24 cm²"
                ],
                correct: 2,
                explanation: "Luas persegi panjang = panjang × lebar = 12 × 5 = 60 cm²"
            },
            {
                question: "Layang-layang memiliki karakteristik...",
                visual: '<svg width="150" height="150"><polygon points="75,25 110,75 75,140 40,75" fill="#cddc39" stroke="#333" stroke-width="2"/></svg>',
                options: [
                    "Semua sisi sama panjang",
                    "Dua pasang sisi yang berdekatan sama panjang",
                    "Tidak ada sisi yang sama panjang",
                    "Tiga sisi sama panjang"
                ],
                correct: 1,
                explanation: "Layang-layang memiliki dua pasang sisi yang berdekatan sama panjang."
            },
            {
                question: "Jika sisi persegi 10 cm, berapakah kelilingnya?",
                visual: '<svg width="150" height="150"><rect x="25" y="25" width="100" height="100" fill="#8bc34a" stroke="#333" stroke-width="2"/><text x="75" y="80" text-anchor="middle" font-size="14" fill="white">10 cm</text></svg>',
                options: [
                    "20 cm",
                    "40 cm",
                    "100 cm",
                    "30 cm"
                ],
                correct: 1,
                explanation: "Keliling persegi = 4 × sisi = 4 × 10 = 40 cm"
            },
            {
                question: "Rumus luas lingkaran adalah...",
                visual: '<svg width="150" height="150"><circle cx="75" cy="75" r="50" fill="#ff9800" stroke="#333" stroke-width="2"/><text x="75" y="80" text-anchor="middle" font-size="14" fill="white">r</text></svg>',
                options: [
                    "π × r",
                    "2 × π × r",
                    "π × r²",
                    "π × d"
                ],
                correct: 2,
                explanation: "Luas lingkaran = π × r² (pi kali jari-jari kuadrat)"
            }
        ];

        let currentQuestionIndex = 0;
        let score = 0;
        let answered = false;

        // Audio contexts for sound effects
        function playSound(type) {
            const audioContext = new (window.AudioContext || window.webkitAudioContext)();
            const oscillator = audioContext.createOscillator();
            const gainNode = audioContext.createGain();
            
            oscillator.connect(gainNode);
            gainNode.connect(audioContext.destination);
            
            if (type === 'click') {
                oscillator.frequency.value = 800;
                gainNode.gain.setValueAtTime(0.1, audioContext.currentTime);
                gainNode.gain.exponentialRampToValueAtTime(0.01, audioContext.currentTime + 0.1);
                oscillator.start(audioContext.currentTime);
                oscillator.stop(audioContext.currentTime + 0.1);
            } else if (type === 'correct') {
                oscillator.frequency.value = 523.25;
                gainNode.gain.setValueAtTime(0.2, audioContext.currentTime);
                oscillator.start(audioContext.currentTime);
                oscillator.frequency.exponentialRampToValueAtTime(783.99, audioContext.currentTime + 0.3);
                gainNode.gain.exponentialRampToValueAtTime(0.01, audioContext.currentTime + 0.3);
                oscillator.stop(audioContext.currentTime + 0.3);
            } else if (type === 'wrong') {
                oscillator.frequency.value = 200;
                gainNode.gain.setValueAtTime(0.2, audioContext.currentTime);
                oscillator.start(audioContext.currentTime);
                oscillator.frequency.exponentialRampToValueAtTime(100, audioContext.currentTime + 0.3);
                gainNode.gain.exponentialRampToValueAtTime(0.01, audioContext.currentTime + 0.3);
                oscillator.stop(audioContext.currentTime + 0.3);
            }
        }

        function loadQuestion() {
            answered = false;
            const question = questions[currentQuestionIndex];
            
            document.getElementById('questionNumber').textContent = `Pertanyaan ${currentQuestionIndex + 1}`;
            document.getElementById('questionText').textContent = question.question;
            document.getElementById('shapeVisual').innerHTML = question.visual;
            document.getElementById('currentQuestion').textContent = currentQuestionIndex + 1;
            document.getElementById('totalQuestions').textContent = questions.length;
            document.getElementById('feedback').style.display = 'none';
            document.getElementById('nextBtn').disabled = true;
            
            const progressPercent = ((currentQuestionIndex + 1) / questions.length) * 100;
            document.getElementById('progressBar').style.width = progressPercent + '%';
            
            const optionsContainer = document.getElementById('options');
            optionsContainer.innerHTML = '';
            
            question.options.forEach((option, index) => {
                const optionDiv = document.createElement('div');
                optionDiv.className = 'option';
                optionDiv.textContent = option;
                optionDiv.onclick = () => selectOption(index);
                optionsContainer.appendChild(optionDiv);
            });
        }

        function selectOption(selectedIndex) {
            if (answered) return;
            
            playSound('click');
            answered = true;
            
            const question = questions[currentQuestionIndex];
            const options = document.querySelectorAll('.option');
            const feedback = document.getElementById('feedback');
            
            options.forEach((option, index) => {
                if (index === question.correct) {
                    option.classList.add('correct');
                }
                if (index === selectedIndex && selectedIndex !== question.correct) {
                    option.classList.add('wrong');
                }
                option.style.pointerEvents = 'none';
            });
            
            if (selectedIndex === question.correct) {
                playSound('correct');
                score += 10;
                document.getElementById('score').textContent = score;
                feedback.className = 'feedback correct';
                feedback.innerHTML = `<strong>✓ Benar!</strong><br>${question.explanation}`;
            } else {
                playSound('wrong');
                feedback.className = 'feedback wrong';
                feedback.innerHTML = `<strong>✗ Salah!</strong><br>${question.explanation}`;
            }
            
            feedback.style.display = 'block';
            document.getElementById('nextBtn').disabled = false;
        }

        document.getElementById('nextBtn').onclick = function() {
            playSound('click');
            currentQuestionIndex++;
            
            if (currentQuestionIndex < questions.length) {
                loadQuestion();
            } else {
                showResults();
            }
        };

        function showResults() {
            document.getElementById('quizContainer').style.display = 'none';
            document.getElementById('resultContainer').style.display = 'block';
            
            const percentage = (score / (questions.length * 10)) * 100;
            document.getElementById('finalScore').textContent = score + ' / ' + (questions.length * 10);
            
            let message = '';
            if (percentage === 100) {
                message = '🏆 Sempurna! Kamu Master Bangun Datar!';
            } else if (percentage >= 80) {
                message = '⭐ Hebat! Pengetahuanmu sangat baik!';
            } else if (percentage >= 60) {
                message = '👍 Bagus! Terus belajar ya!';
            } else {
                message = '💪 Jangan menyerah! Coba lagi!';
            }
            
            document.getElementById('resultMessage').textContent = message;
        }

        function restartQuiz() {
            playSound('click');
            currentQuestionIndex = 0;
            score = 0;
            document.getElementById('score').textContent = score;
            document.getElementById('quizContainer').style.display = 'block';
            document.getElementById('resultContainer').style.display = 'none';
            loadQuestion();
        }

        loadQuestion();
    </script>
</body>
</html>
