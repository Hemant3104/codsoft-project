<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Online Quiz Maker</title>
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <style>
        /* Funky animated background */
        body {
            font-family: Arial, sans-serif;
            margin: 0; padding: 0;
            background: linear-gradient(-45deg, #ff9a9e, #fad0c4, #a1c4fd, #c2e9fb, #fbc2eb, #a6c1ee);
            background-size: 400% 400%;
            animation: gradientBG 12s ease-in-out infinite;
        }
        @keyframes gradientBG {
            0% {background-position: 0% 50%;}
            50% {background-position: 100% 50%;}
            100% {background-position: 0% 50%;}
        }

        .container {
            max-width: 600px;
            margin: 30px auto;
            background: rgba(255,255,255,0.95);
            padding: 20px;
            border-radius: 18px;
            box-shadow: 0 8px 32px 0 rgba(31, 38, 135, 0.18);
            animation: popIn 0.7s cubic-bezier(.68,-0.55,.27,1.55);
        }
        @keyframes popIn {
            0% { transform: scale(0.8) rotate(-3deg); opacity: 0; }
            100% { transform: scale(1) rotate(0); opacity: 1; }
        }

        h1, h2 {
            text-align: center;
            letter-spacing: 2px;
            color: #4e54c8;
            text-shadow: 1px 2px 8px #fbc2eb55;
            animation: fadeInDown 1s;
        }
        @keyframes fadeInDown {
            0% { opacity: 0; transform: translateY(-30px);}
            100% { opacity: 1; transform: translateY(0);}
        }

        .btn {
            padding: 10px 22px;
            margin: 5px;
            border: none;
            border-radius: 25px;
            background: linear-gradient(90deg, #43e97b 0%, #38f9d7 100%);
            color: #fff;
            cursor: pointer;
            font-weight: bold;
            font-size: 1rem;
            box-shadow: 0 2px 8px #38f9d755;
            transition: transform 0.18s, box-shadow 0.18s, background 0.3s;
            outline: none;
        }
        .btn:hover, .btn:focus {
            background: linear-gradient(90deg, #fa709a 0%, #fee140 100%);
            color: #222;
            transform: scale(1.07) rotate(-2deg);
            box-shadow: 0 4px 16px #fa709a55;
        }

        .hidden { display: none; }

        input, select, textarea {
            width: 100%;
            padding: 10px;
            margin: 5px 0 15px 0;
            border: 1px solid #ccc;
            border-radius: 8px;
            font-size: 1rem;
            transition: border 0.2s, box-shadow 0.2s;
            background: #f7f7fa;
        }
        input:focus, select:focus, textarea:focus {
            border: 1.5px solid #43e97b;
            box-shadow: 0 0 8px #43e97b55;
            outline: none;
        }

        .quiz-list {
            list-style: none;
            padding: 0;
        }
        .quiz-list li {
            padding: 12px;
            border-bottom: 1px solid #eee;
            background: rgba(255,255,255,0.7);
            border-radius: 8px;
            margin-bottom: 8px;
            transition: background 0.2s, transform 0.2s;
            position: relative;
            overflow: hidden;
        }
        .quiz-list li:hover {
            background: linear-gradient(90deg, #a1c4fd33 0%, #c2e9fb33 100%);
            transform: scale(1.02) rotate(-1deg);
            box-shadow: 0 2px 12px #a1c4fd33;
        }
        .quiz-list li::before {
            content: '';
            position: absolute;
            left: -40px; top: 0; bottom: 0;
            width: 8px;
            background: linear-gradient(#43e97b, #fa709a);
            border-radius: 8px;
            opacity: 0.3;
            transition: left 0.2s;
        }
        .quiz-list li:hover::before {
            left: 0;
            opacity: 0.7;
        }

        .option-label {
            display: block;
            margin-bottom: 10px;
            padding: 8px 12px;
            border-radius: 8px;
            background: #f7f7fa;
            cursor: pointer;
            transition: background 0.2s, color 0.2s, transform 0.15s;
            font-size: 1rem;
            border: 1px solid #eee;
        }
        .option-label:hover, .option-label:has(input:checked) {
            background: linear-gradient(90deg, #43e97b33 0%, #38f9d733 100%);
            color: #222;
            transform: scale(1.03);
            border: 1.5px solid #43e97b;
        }
        .option-label input[type="radio"] {
            accent-color: #43e97b;
            margin-right: 8px;
        }

        #quiz-result-section {
            animation: popIn 0.7s cubic-bezier(.68,-0.55,.27,1.55);
        }
        #quiz-score h3 {
            color: #43e97b;
            text-shadow: 1px 2px 8px #43e97b33;
        }
        #quiz-correct-answers div {
            background: #f7f7fa;
            border-radius: 8px;
            margin-bottom: 10px;
            padding: 10px;
            box-shadow: 0 1px 4px #a1c4fd22;
            animation: fadeInDown 0.7s;
        }

        /* Funky floating shapes */
        .funky-shape {
            position: fixed;
            z-index: 0;
            opacity: 0.18;
            pointer-events: none;
            animation: floatShape 12s infinite alternate ease-in-out;
        }
        .funky-shape1 {
            width: 120px; height: 120px; left: 5vw; top: 10vh;
            background: radial-gradient(circle at 40% 40%, #fa709a 60%, transparent 100%);
            border-radius: 50%;
            animation-delay: 0s;
        }
        .funky-shape2 {
            width: 80px; height: 80px; right: 8vw; top: 30vh;
            background: radial-gradient(circle at 60% 60%, #43e97b 60%, transparent 100%);
            border-radius: 50%;
            animation-delay: 2s;
        }
        .funky-shape3 {
            width: 100px; height: 100px; left: 20vw; bottom: 8vh;
            background: radial-gradient(circle at 60% 60%, #a1c4fd 60%, transparent 100%);
            border-radius: 50%;
            animation-delay: 4s;
        }
        .funky-shape4 {
            width: 60px; height: 60px; right: 18vw; bottom: 18vh;
            background: radial-gradient(circle at 60% 60%, #fee140 60%, transparent 100%);
            border-radius: 50%;
            animation-delay: 6s;
        }
        @keyframes floatShape {
            0% { transform: translateY(0) scale(1);}
            100% { transform: translateY(-40px) scale(1.1);}
        }

        @media (max-width: 700px) {
            .container { width: 95%; }
            .funky-shape { display: none; }
        }
    </style>
</head>
<body>
<!-- Funky floating shapes -->
<div class="funky-shape funky-shape1"></div>
<div class="funky-shape funky-shape2"></div>
<div class="funky-shape funky-shape3"></div>
<div class="funky-shape funky-shape4"></div>

<div class="container">
    <div id="auth-section">
        <h1>Online Quiz Maker</h1>
        <div id="login-form">
            <h2>Login</h2>
            <input type="text" id="login-username" placeholder="Username" required>
            <input type="password" id="login-password" placeholder="Password" required>
            <button class="btn" onclick="login()">Login</button>
            <p>Don't have an account? <a href="#" onclick="showRegister()">Register</a></p>
        </div>
        <div id="register-form" class="hidden">
            <h2>Register</h2>
            <input type="text" id="register-username" placeholder="Username" required>
            <input type="password" id="register-password" placeholder="Password" required>
            <button class="btn" onclick="register()">Register</button>
            <p>Already have an account? <a href="#" onclick="showLogin()">Login</a></p>
        </div>
    </div>

    <div id="main-section" class="hidden"></div>
        <div id="home-page">
            <h1>Welcome, <span id="user-name"></span>!</h1>
            <button class="btn" onclick="showCreateQuiz()">Create Quiz</button>
            <button class="btn" onclick="showQuizList()">Take Quiz</button>
            <button class="btn" onclick="logout()">Logout</button>
        </div>

        <div id="create-quiz-section" class="hidden">
            <h2>Create a New Quiz</h2>
            <input type="text" id="quiz-title" placeholder="Quiz Title" required>
            <div id="questions-container"></div>
            <button class="btn" onclick="addQuestion()">Add Question</button>
            <button class="btn" onclick="saveQuiz()">Save Quiz</button>
            <button class="btn" onclick="showHome()">Cancel</button>
        </div>

        <div id="quiz-list-section" class="hidden">
            <h2>Available Quizzes</h2>
            <ul id="quiz-list" class="quiz-list"></ul>
            <button class="btn" onclick="showHome()">Back</button>
        </div>

        <div id="take-quiz-section" class="hidden">
            <h2 id="take-quiz-title"></h2>
            <div id="take-quiz-question"></div>
            <div id="take-quiz-options"></div>
            <button class="btn" id="next-question-btn" onclick="nextQuestion()">Next</button>
            <button class="btn hidden" id="submit-quiz-btn" onclick="submitQuiz()">Submit</button>
        </div>

        <div id="quiz-result-section" class="hidden">
            <h2>Quiz Results</h2>
            <div id="quiz-score"></div>
            <div id="quiz-correct-answers"></div>
            <button class="btn" onclick="showQuizList()">Back to Quizzes</button>
        </div>
    </div>
</div>

<script>
    // --- User Authentication (localStorage) ---
    function getUsers() {
        return JSON.parse(localStorage.getItem('quiz_users') || '{}');
    }
    function setUsers(users) {
        localStorage.setItem('quiz_users', JSON.stringify(users));
    }
    function getCurrentUser() {
        return localStorage.getItem('quiz_current_user');
    }
    function setCurrentUser(username) {
        localStorage.setItem('quiz_current_user', username);
    }
    function clearCurrentUser() {
        localStorage.removeItem('quiz_current_user');
    }

    function showRegister() {
        document.getElementById('login-form').classList.add('hidden');
        document.getElementById('register-form').classList.remove('hidden');
    }
    function showLogin() {
        document.getElementById('register-form').classList.add('hidden');
        document.getElementById('login-form').classList.remove('hidden');
    }
    function register() {
        const username = document.getElementById('register-username').value.trim();
        const password = document.getElementById('register-password').value;
        if (!username || !password) return alert('Please fill all fields.');
        let users = getUsers();
        if (users[username]) return alert('Username already exists.');
        users[username] = { password };
        setUsers(users);
        alert('Registration successful! Please login.');
        showLogin();
    }
    function login() {
        const username = document.getElementById('login-username').value.trim();
        const password = document.getElementById('login-password').value;
        let users = getUsers();
        if (!users[username] || users[username].password !== password) {
            alert('Invalid credentials.');
            return;
        }
        setCurrentUser(username);
        showMain();
    }
    function logout() {
        clearCurrentUser();
        document.getElementById('main-section').classList.add('hidden');
        document.getElementById('auth-section').classList.remove('hidden');
    }

    // --- Navigation ---
    function showMain() {
        document.getElementById('auth-section').classList.add('hidden');
        document.getElementById('main-section').classList.remove('hidden');
        document.getElementById('user-name').textContent = getCurrentUser();
        showHome();
    }
    function showHome() {
        hideAllSections();
        document.getElementById('home-page').classList.remove('hidden');
    }
    function showCreateQuiz() {
        hideAllSections();
        document.getElementById('create-quiz-section').classList.remove('hidden');
        document.getElementById('quiz-title').value = '';
        document.getElementById('questions-container').innerHTML = '';
        addQuestion();
    }
    function showQuizList() {
        hideAllSections();
        document.getElementById('quiz-list-section').classList.remove('hidden');
        renderQuizList();
    }
    function hideAllSections() {
        ['home-page', 'create-quiz-section', 'quiz-list-section', 'take-quiz-section', 'quiz-result-section']
            .forEach(id => document.getElementById(id).classList.add('hidden'));
    }

    // --- Quiz Creation ---
    function addQuestion() {
        const container = document.getElementById('questions-container');
        const qIndex = container.children.length;
        const qDiv = document.createElement('div');
        qDiv.innerHTML = `
            <h4 style="color:#fa709a;">Question ${qIndex + 1}</h4>
            <input type="text" placeholder="Question" class="question-text" required>
            <input type="text" placeholder="Option A" class="option-input" required>
            <input type="text" placeholder="Option B" class="option-input" required>
            <input type="text" placeholder="Option C" class="option-input" required>
            <input type="text" placeholder="Option D" class="option-input" required>
            <select class="correct-answer-select">
                <option value="">Select Correct Answer</option>
                <option value="0">A</option>
                <option value="1">B</option>
                <option value="2">C</option>
                <option value="3">D</option>
            </select>
            <hr>
        `;
        qDiv.style.animation = "popIn 0.5s";
        container.appendChild(qDiv);
    }
    function saveQuiz() {
        const title = document.getElementById('quiz-title').value.trim();
        if (!title) return alert('Quiz title is required.');
        const questions = [];
        const container = document.getElementById('questions-container');
        for (let qDiv of container.children) {
            const qText = qDiv.querySelector('.question-text').value.trim();
            const options = Array.from(qDiv.querySelectorAll('.option-input')).map(i => i.value.trim());
            const correct = qDiv.querySelector('.correct-answer-select').value;
            if (!qText || options.some(o => !o) || correct === '') {
                return alert('Please fill all question fields and select correct answers.');
            }
            questions.push({ question: qText, options, correct: parseInt(correct) });
        }
        let quizzes = JSON.parse(localStorage.getItem('quiz_list') || '[]');
        quizzes.push({
            id: Date.now(),
            title,
            author: getCurrentUser(),
            questions
        });
        localStorage.setItem('quiz_list', JSON.stringify(quizzes));
        alert('Quiz saved!');
        showHome();
    }

    // --- Quiz Listing & Taking ---
    function renderQuizList() {
        const quizList = document.getElementById('quiz-list');
        quizList.innerHTML = '';
        let quizzes = JSON.parse(localStorage.getItem('quiz_list') || '[]');
        if (quizzes.length === 0) {
            quizList.innerHTML = '<li>No quizzes available.</li>';
            return;
        }
        quizzes.forEach(quiz => {
            const li = document.createElement('li');
            li.innerHTML = `<strong>${quiz.title}</strong> by ${quiz.author} 
                <button class="btn" onclick="startQuiz(${quiz.id})">Take Quiz</button>`;
            quizList.appendChild(li);
        });
    }

    // --- Quiz Taking ---
    let currentQuiz = null, currentQuestionIndex = 0, userAnswers = [];
    function startQuiz(quizId) {
        let quizzes = JSON.parse(localStorage.getItem('quiz_list') || '[]');
        currentQuiz = quizzes.find(q => q.id === quizId);
        currentQuestionIndex = 0;
        userAnswers = [];
        hideAllSections();
        document.getElementById('take-quiz-section').classList.remove('hidden');
        document.getElementById('take-quiz-title').textContent = currentQuiz.title;
        showQuizQuestion();
    }
    function showQuizQuestion() {
        const q = currentQuiz.questions[currentQuestionIndex];
        document.getElementById('take-quiz-question').textContent = `Q${currentQuestionIndex + 1}: ${q.question}`;
        const optionsDiv = document.getElementById('take-quiz-options');
        optionsDiv.innerHTML = '';
        q.options.forEach((opt, i) => {
            const label = document.createElement('label');
            label.className = 'option-label';
            label.innerHTML = `<input type="radio" name="quiz-option" value="${i}"> ${String.fromCharCode(65 + i)}. ${opt}`;
            optionsDiv.appendChild(label);
        });
        document.getElementById('next-question-btn').classList.remove('hidden');
        document.getElementById('submit-quiz-btn').classList.add('hidden');
        if (currentQuestionIndex === currentQuiz.questions.length - 1) {
            document.getElementById('next-question-btn').classList.add('hidden');
            document.getElementById('submit-quiz-btn').classList.remove('hidden');
        }
    }
    function nextQuestion() {
        const selected = document.querySelector('input[name="quiz-option"]:checked');
        if (!selected) return alert('Please select an answer.');
        userAnswers[currentQuestionIndex] = parseInt(selected.value);
        currentQuestionIndex++;
        showQuizQuestion();
    }
    function submitQuiz() {
        const selected = document.querySelector('input[name="quiz-option"]:checked');
        if (!selected) return alert('Please select an answer.');
        userAnswers[currentQuestionIndex] = parseInt(selected.value);
        showQuizResult();
    }

    // --- Quiz Results ---
    function showQuizResult() {
        hideAllSections();
        document.getElementById('quiz-result-section').classList.remove('hidden');
        let score = 0;
        let resultHtml = '';
        currentQuiz.questions.forEach((q, i) => {
            const userAns = userAnswers[i];
            const correct = q.correct;
            if (userAns === correct) score++;
            resultHtml += `<div>
                <strong>Q${i + 1}: ${q.question}</strong><br>
                Your answer: <span style="color:${userAns === correct ? 'green' : 'red'}">
                    ${userAns !== undefined ? String.fromCharCode(65 + userAns) + '. ' + q.options[userAns] : 'No answer'}
                </span><br>
                Correct answer: <span style="color:green">
                    ${String.fromCharCode(65 + correct)}. ${q.options[correct]}
                </span>
                <hr>
            </div>`;
        });
        document.getElementById('quiz-score').innerHTML = `<h3>Your Score: ${score} / ${currentQuiz.questions.length}</h3>`;
        document.getElementById('quiz-correct-answers').innerHTML = resultHtml;
    }

    // --- On Load ---
    window.onload = function() {
        if (getCurrentUser()) showMain();
    };
</script>
</body>
</html>
