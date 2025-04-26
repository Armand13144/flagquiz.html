# flagquiz.html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>World Flag Challenge</title>
    <style>
        :root {
            --primary: #3498db;
            --secondary: #2ecc71;
            --danger: #e74c3c;
            --dark: #2c3e50;
            --light: #ecf0f1;
        }
        
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        
        body {
            background-color: #f5f5f5;
            color: var(--dark);
            line-height: 1.6;
        }
        
        .container {
            max-width: 800px;
            margin: 0 auto;
            padding: 20px;
        }
        
        header {
            text-align: center;
            margin-bottom: 30px;
            padding: 20px 0;
            background: linear-gradient(135deg, var(--primary), var(--secondary));
            color: white;
            border-radius: 10px;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
        }
        
        h1 {
            font-size: 2.5rem;
            margin-bottom: 10px;
        }
        
        .game-area {
            background-color: white;
            border-radius: 10px;
            padding: 30px;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
            text-align: center;
        }
        
        .flag-container {
            height: 180px;
            margin: 20px auto;
            display: flex;
            justify-content: center;
            align-items: center;
            background-color: var(--light);
            border-radius: 10px;
            overflow: hidden;
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
        }
        
        .flag-img {
            max-height: 100%;
            max-width: 100%;
            object-fit: contain;
        }
        
        .options {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 15px;
            margin: 30px 0;
        }
        
        .option-btn {
            padding: 15px;
            border: none;
            border-radius: 8px;
            background-color: var(--primary);
            color: white;
            font-size: 1.1rem;
            cursor: pointer;
            transition: all 0.3s ease;
        }
        
        .option-btn:hover {
            transform: translateY(-3px);
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
        }
        
        .score-container {
            display: flex;
            justify-content: space-between;
            margin-bottom: 20px;
            font-size: 1.2rem;
            font-weight: bold;
        }
        
        .progress-container {
            height: 10px;
            background-color: var(--light);
            border-radius: 5px;
            margin-bottom: 20px;
            overflow: hidden;
        }
        
        .progress-bar {
            height: 100%;
            background: linear-gradient(90deg, var(--primary), var(--secondary));
            width: 0%;
            transition: width 0.3s ease;
        }
        
        .feedback {
            font-size: 1.2rem;
            font-weight: bold;
            margin: 20px 0;
            min-height: 30px;
        }
        
        .correct {
            color: var(--secondary);
        }
        
        .incorrect {
            color: var(--danger);
        }
        
        .next-btn {
            padding: 12px 30px;
            background-color: var(--secondary);
            color: white;
            border: none;
            border-radius: 8px;
            font-size: 1.1rem;
            cursor: pointer;
            transition: all 0.3s ease;
            display: none;
        }
        
        .next-btn:hover {
            background-color: #27ae60;
            transform: translateY(-3px);
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
        }
        
        .game-over {
            text-align: center;
            display: none;
        }
        
        .restart-btn {
            padding: 12px 30px;
            background-color: var(--primary);
            color: white;
            border: none;
            border-radius: 8px;
            font-size: 1.1rem;
            cursor: pointer;
            transition: all 0.3s ease;
            margin-top: 20px;
        }
        
        .restart-btn:hover {
            background-color: #2980b9;
            transform: translateY(-3px);
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
        }
        
        @media (max-width: 600px) {
            .options {
                grid-template-columns: 1fr;
            }
            
            h1 {
                font-size: 2rem;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <h1>World Flag Challenge</h1>
            <p>Test your knowledge of country flags, including Kurdistan!</p>
        </header>
        
        <div class="game-area">
            <div class="score-container">
                <div>Score: <span id="score">0</span></div>
                <div>Question: <span id="question-count">1</span>/10</div>
            </div>
            
            <div class="progress-container">
                <div class="progress-bar" id="progress-bar"></div>
            </div>
            
            <div class="flag-container">
                <img src="" alt="Flag" class="flag-img" id="flag-img">
            </div>
            
            <div class="options" id="options">
                <!-- Options will be inserted here by JavaScript -->
            </div>
            
            <div class="feedback" id="feedback"></div>
            
            <button class="next-btn" id="next-btn">Next Question</button>
            
            <div class="game-over" id="game-over">
                <h2>Game Over!</h2>
                <p>Your final score is: <span id="final-score">0</span>/10</p>
                <button class="restart-btn" id="restart-btn">Play Again</button>
            </div>
        </div>
    </div>

    <script>
        // Game data with working flag URLs
        const flagQuestions = [
            {
                flagUrl: "https://upload.wikimedia.org/wikipedia/commons/b/ba/Flag_of_Kurdistan.svg",
                correctAnswer: "Kurdistan",
                options: ["Kurdistan", "Iraq", "Iran", "Turkey"]
            },
            {
                flagUrl: "https://upload.wikimedia.org/wikipedia/commons/f/f6/Flag_of_Iraq.svg",
                correctAnswer: "Iraq",
                options: ["Syria", "Iraq", "Jordan", "Kuwait"]
            },
            {
                flagUrl: "https://upload.wikimedia.org/wikipedia/commons/9/9a/Flag_of_Spain.svg",
                correctAnswer: "Spain",
                options: ["Portugal", "Spain", "Mexico", "Argentina"]
            },
            {
                flagUrl: "https://upload.wikimedia.org/wikipedia/commons/f/fa/Flag_of_the_People%27s_Republic_of_China.svg",
                correctAnswer: "China",
                options: ["Japan", "South Korea", "China", "Vietnam"]
            },
            {
                flagUrl: "https://upload.wikimedia.org/wikipedia/commons/0/05/Flag_of_Brazil.svg",
                correctAnswer: "Brazil",
                options: ["Brazil", "Argentina", "Colombia", "Venezuela"]
            },
            {
                flagUrl: "https://upload.wikimedia.org/wikipedia/commons/4/41/Flag_of_Austria.svg",
                correctAnswer: "Austria",
                options: ["Austria", "Switzerland", "Poland", "Hungary"]
            },
            {
                flagUrl: "https://upload.wikimedia.org/wikipedia/commons/0/03/Flag_of_Italy.svg",
                correctAnswer: "Italy",
                options: ["France", "Italy", "Ireland", "Romania"]
            },
            {
                flagUrl: "https://upload.wikimedia.org/wikipedia/commons/f/f3/Flag_of_Russia.svg",
                correctAnswer: "Russia",
                options: ["Russia", "Belarus", "Ukraine", "Serbia"]
            },
            {
                flagUrl: "https://upload.wikimedia.org/wikipedia/commons/a/a4/Flag_of_the_United_States.svg",
                correctAnswer: "United States",
                options: ["United States", "United Kingdom", "Australia", "Canada"]
            },
            {
                flagUrl: "https://upload.wikimedia.org/wikipedia/commons/c/cf/Flag_of_Canada.svg",
                correctAnswer: "Canada",
                options: ["Canada", "Norway", "Finland", "Sweden"]
            }
        ];

        // Game variables
        let currentQuestion = 0;
        let score = 0;
        let shuffledQuestions = [];
        
        // DOM elements
        const flagImg = document.getElementById('flag-img');
        const optionsContainer = document.getElementById('options');
        const scoreElement = document.getElementById('score');
        const questionCountElement = document.getElementById('question-count');
        const feedbackElement = document.getElementById('feedback');
        const nextButton = document.getElementById('next-btn');
        const progressBar = document.getElementById('progress-bar');
        const gameOverElement = document.getElementById('game-over');
        const finalScoreElement = document.getElementById('final-score');
        const restartButton = document.getElementById('restart-btn');
        
        // Initialize the game
        function initGame() {
            // Shuffle questions
            shuffledQuestions = [...flagQuestions].sort(() => Math.random() - 0.5);
            
            currentQuestion = 0;
            score = 0;
            scoreElement.textContent = score;
            questionCountElement.textContent = `${currentQuestion + 1}/${shuffledQuestions.length}`;
            progressBar.style.width = '0%';
            
            gameOverElement.style.display = 'none';
            loadQuestion();
        }
        
        // Load a question
        function loadQuestion() {
            if (currentQuestion >= shuffledQuestions.length) {
                endGame();
                return;
            }
            
            const question = shuffledQuestions[currentQuestion];
            flagImg.src = question.flagUrl;
            flagImg.alt = `Flag of ${question.correctAnswer}`;
            
            // Clear previous options and feedback
            optionsContainer.innerHTML = '';
            feedbackElement.textContent = '';
            feedbackElement.className = 'feedback';
            nextButton.style.display = 'none';
            
            // Shuffle options
            const shuffledOptions = [...question.options].sort(() => Math.random() - 0.5);
            
            // Create option buttons
            shuffledOptions.forEach(option => {
                const button = document.createElement('button');
                button.className = 'option-btn';
                button.textContent = option;
                button.addEventListener('click', () => checkAnswer(option, question.correctAnswer));
                optionsContainer.appendChild(button);
            });
            
            // Update progress
            progressBar.style.width = `${(currentQuestion / shuffledQuestions.length) * 100}%`;
            questionCountElement.textContent = `${currentQuestion + 1}/${shuffledQuestions.length}`;
        }
        
        // Check the selected answer
        function checkAnswer(selectedOption, correctAnswer) {
            // Disable all option buttons
            const buttons = document.querySelectorAll('.option-btn');
            buttons.forEach(button => {
                button.disabled = true;
                if (button.textContent === correctAnswer) {
                    button.style.backgroundColor = 'var(--secondary)';
                } else if (button.textContent === selectedOption && selectedOption !== correctAnswer) {
                    button.style.backgroundColor = 'var(--danger)';
                }
            });
            
            if (selectedOption === correctAnswer) {
                score++;
                scoreElement.textContent = score;
                feedbackElement.textContent = 'Correct!';
                feedbackElement.className = 'feedback correct';
            } else {
                feedbackElement.textContent = `Incorrect! The correct answer is ${correctAnswer}.`;
                feedbackElement.className = 'feedback incorrect';
            }
            
            nextButton.style.display = 'inline-block';
        }
        
        // Move to the next question
        function nextQuestion() {
            currentQuestion++;
            loadQuestion();
        }
        
        // End the game
        function endGame() {
            gameOverElement.style.display = 'block';
            document.querySelector('.game-area').style.display = 'none';
            finalScoreElement.textContent = score;
            progressBar.style.width = '100%';
        }
        
        // Event listeners
        nextButton.addEventListener('click', nextQuestion);
        restartButton.addEventListener('click', () => {
            document.querySelector('.game-area').style.display = 'block';
            initGame();
        });
        
        // Start the game
        initGame();
    </script>
</body>
</html>
