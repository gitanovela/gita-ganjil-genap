<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Latihan Ganjil Genap</title>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        body { font-family: Arial, sans-serif; text-align: center; }
        .quiz-container { margin: 20px auto; width: 50%; }
        .question { font-size: 20px; margin-bottom: 10px; }
        .options button { margin: 5px; padding: 10px; cursor: pointer; }
        #timer { font-size: 18px; color: red; }
        #chartContainer { width: 80%; margin: auto; }
    </style>
</head>
<body>
    <h1>Latihan Soal Ganjil Genap</h1>
    <div id="timer">Waktu: 60 detik</div>
    <div class="quiz-container">
        <div id="question" class="question"></div>
        <div class="options" id="options"></div>
    </div>
    <canvas id="scoreChart" width="400" height="200"></canvas>
    <script>
        let questions = Array.from({length: 100}, (_, i) => ({ 
            question: `Apakah ${i + 1} adalah bilangan ganjil atau genap?`,
            answer: (i + 1) % 2 === 0 ? 'Genap' : 'Ganjil'
        }));
        let score = 0;
        let questionIndex = 0;
        let timer = 60;
        let interval;

        function startQuiz() {
            questionIndex = 0;
            score = 0;
            timer = 60;
            nextQuestion();
            interval = setInterval(updateTimer, 1000);
        }
        
        function updateTimer() {
            document.getElementById("timer").innerText = `Waktu: ${timer} detik`;
            if (timer === 0) {
                clearInterval(interval);
                showScore();
            }
            timer--;
        }
        
        function nextQuestion() {
            if (questionIndex < 100) {
                let q = questions[Math.floor(Math.random() * questions.length)];
                document.getElementById("question").innerText = q.question;
                document.getElementById("options").innerHTML = `
                    <button onclick="checkAnswer('Ganjil', '${q.answer}')">Ganjil</button>
                    <button onclick="checkAnswer('Genap', '${q.answer}')">Genap</button>
                `;
                questionIndex++;
            } else {
                clearInterval(interval);
                showScore();
            }
        }

        function checkAnswer(selected, correct) {
            if (selected === correct) {
                score++;
            }
            nextQuestion();
        }

        function showScore() {
            document.getElementById("quiz-container").innerHTML = `<h2>Skor: ${score}/100</h2>`;
            drawChart();
        }

        function drawChart() {
            let ctx = document.getElementById('scoreChart').getContext('2d');
            new Chart(ctx, {
                type: 'bar',
                data: {
                    labels: ['Benar', 'Salah'],
                    datasets: [{
                        label: 'Hasil',
                        data: [score, 100 - score],
                        backgroundColor: ['green', 'red']
                    }]
                }
            });
        }

        startQuiz();
    </script>
</body>
</html>

