# UPSC-Mock-Interviewer
It is a responsive website where anyone can give mock interview and get result score according to your performance in the test.
just like chatgpt by using html, css and javascript only.
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>UPSC Mock Interview</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f4f4;
            margin: 0;
            padding: 0;
        }
        header {
            background-color: #0073e6;
            color: white;
            text-align: center;
            padding: 40px 0;
        }
        .container {
            width: 80%;
            margin: 0 auto;
            padding: 20px;
        }
        .chat-box {
            background: white;
            padding: 20px;
            margin-bottom: 20px;
            border-radius: 8px;
            box-shadow: 0 2px 6px rgba(0,0,0,0.1);
        }
        .chat-box h2 {
            color: #0073e6;
            margin-bottom: 10px;
        }
        .chat-log {
            border: 1px solid #ccc;
            border-radius: 8px;
            padding: 10px;
            height: 300px;
            overflow-y: auto;
            margin-bottom: 10px;
        }
        .input-section {
            display: flex;
            gap: 10px;
        }
        .input-section textarea {
            flex: 1;
            padding: 10px;
            border-radius: 4px;
            border: 1px solid #ddd;
        }
        .input-section button {
            padding: 10px 20px;
            border: none;
            border-radius: 4px;
            background-color: #0073e6;
            color: white;
            cursor: pointer;
        }
        .questions-list {
            background: white;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 2px 6px rgba(0,0,0,0.1);
            margin-bottom: 20px;
        }
        .questions-list h3 {
            color: #0073e6;
            margin-bottom: 10px;
        }
        .questions-list ul {
            list-style-type: none;
            padding: 0;
        }
        .questions-list ul li {
            margin: 5px 0;
            cursor: pointer;
        }
        .results {
            background: white;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 2px 6px rgba(0,0,0,0.1);
        }
        .results h3 {
            color: #0073e6;
        }
    </style>
</head>
<body>
    <header>
        <h1>UPSC Mock Interview</h1>
        <p>Simulate your UPSC interview experience and get feedback</p>
    </header>

    <div class="container">
        <div class="chat-box">
            <h2>Interview</h2>
            <div class="chat-log" id="chat-log">
                <!-- Chat messages will appear here -->
            </div>
            <div class="input-section">
                <textarea id="user-input" rows="3" placeholder="Type your response here..."></textarea>
                <button onclick="submitResponse()">Submit Response</button>
            </div>
        </div>

        <div class="questions-list">
            <h3>Available Questions</h3>
            <ul id="questions">
                <!-- Questions will be listed here -->
            </ul>
        </div>

        <div class="results" id="results" style="display:none;">
            <h3>Interview Results</h3>
            <div id="result-summary">
                <!-- Results will be displayed here -->
            </div>
        </div>
    </div>

    <script>
        const chatLog = document.getElementById('chat-log');
const userInput = document.getElementById('user-input');
const questionsList = document.getElementById('questions');
const resultsDiv = document.getElementById('results');
const resultSummary = document.getElementById('result-summary');

const questions = [
    { 
        question: "Tell us about yourself.", 
        answer: "I am a dedicated individual with a strong passion for public service. My background includes a degree in Political Science, and I have been actively involved in community service and development projects." 
    },
    { 
        question: "What motivates you to join the civil services?", 
        answer: "I am motivated by the opportunity to contribute to national development and address societal issues. I believe that through the civil services, I can help implement policies that will bring about positive change." 
    },
    { 
        question: "Discuss a recent current event and your perspective on it.", 
        answer: "The recent climate summit highlighted the urgent need for global cooperation to combat climate change. In my view, it is crucial for nations to work together on sustainable practices and invest in renewable energy to address this global challenge." 
    },
    { 
        question: "What are your views on climate change policies in India?", 
        answer: "India should focus on enhancing its climate change policies by promoting renewable energy sources, implementing strict regulations on emissions, and investing in sustainable infrastructure to mitigate the effects of climate change and promote environmental sustainability." 
    }
];

let userResponses = [];
let currentQuestionIndex = 0;

function appendMessage(role, content) {
    const messageElement = document.createElement('div');
    messageElement.textContent = `${role}: ${content}`;
    chatLog.appendChild(messageElement);
    chatLog.scrollTop = chatLog.scrollHeight;  // Scroll to the bottom
}

function submitResponse() {
    const userResponse = userInput.value.trim();
    if (!userResponse) return;

    appendMessage('User', userResponse);
    userResponses[currentQuestionIndex] = userResponse;
    userInput.value = '';

    // Move to the next question or finish the interview
    currentQuestionIndex++;
    if (currentQuestionIndex < questions.length) {
        setTimeout(() => {
            appendMessage('System', `Question ${currentQuestionIndex + 1}: ${questions[currentQuestionIndex].question}`);
        }, 1000);
    } else {
        calculateResults();
    }
}

function calculateResults() {
    let score = 0;
    let correctCount = 0;
    let totalCount = questions.length;

    userResponses.forEach((response, index) => {
        const correctAnswer = questions[index].answer.toLowerCase();
        const userAnswer = response.toLowerCase();

        // Basic evaluation: check if the user's answer contains the correct answer
        if (userAnswer.includes(correctAnswer)) {
            correctCount++;
        }
    });

    score = (correctCount / totalCount) * 100;

    resultsDiv.style.display = 'block';
    resultSummary.innerHTML = `
        <p>Total Questions: ${totalCount}</p>
        <p>Correctly Attempted: ${correctCount}</p>
        <p>Score: ${score.toFixed(2)} / 100</p>
    `;

    appendMessage('System', 'Interview complete. Check the results below.');
}

function loadQuestions() {
    if (questions.length > 0) {
        appendMessage('System', `Question 1: ${questions[0].question}`);
    }

    questions.forEach((q, index) => {
        const li = document.createElement('li');
        li.textContent = q.question;
        li.onclick = () => {
            userInput.value = q.question;
            submitResponse();
        };
        questionsList.appendChild(li);
    });
}

// Load questions and start the interview on page load
window.onload = loadQuestions;

        
    </script>
</body>
</html>
