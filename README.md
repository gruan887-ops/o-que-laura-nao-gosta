<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Descobrindo os gostos de Laura</title>
    <style>
        * { box-sizing: border-box; margin: 0; padding: 0; }
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #0f0c29, #302b63, #24243e);
            color: #fff;
            min-height: 100vh;
            display: flex;
            align-items: center;
            justify-content: center;
            padding: 15px;
        }
        .card {
            background: rgba(20, 20, 35, 0.85);
            backdrop-filter: blur(10px);
            border: 2px solid rgba(0, 243, 255, 0.3);
            border-radius: 20px;
            width: 100%;
            max-width: 500px;
            padding: 30px 20px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.5);
            text-align: center;
        }
        h1 {
            font-size: 1.8rem;
            margin-bottom: 10px;
            color: #00f3ff;
            text-transform: uppercase;
            letter-spacing: 1px;
        }
        .subtitle {
            font-size: 0.9rem;
            color: #b0b0b5;
            margin-bottom: 25px;
        }
        .progress-container {
            background: rgba(255,255,255,0.1);
            border-radius: 10px;
            height: 12px;
            width: 100%;
            margin-bottom: 30px;
            overflow: hidden;
        }
        .progress-bar {
            background: linear-gradient(90deg, #004dff, #00f3ff);
            height: 100%;
            width: 0%;
            transition: width 0.3s ease;
        }
        .food-box {
            min-height: 120px;
            display: flex;
            align-items: center;
            justify-content: center;
            margin-bottom: 30px;
            background: rgba(0,0,0,0.3);
            border-radius: 15px;
            border: 1px solid rgba(255,255,255,0.08);
            padding: 20px;
        }
        .food-name {
            font-size: 2.2rem;
            font-weight: 900;
            text-transform: uppercase;
            letter-spacing: 1px;
            color: #ffffff;
            word-break: break-word;
        }
        .buttons {
            display: flex;
            gap: 15px;
        }
        button {
            flex: 1;
            padding: 18px 10px;
            border-radius: 12px;
            font-size: 1.2rem;
            font-weight: bold;
            border: none;
            cursor: pointer;
            color: #fff;
            transition: transform 0.1s ease, filter 0.2s ease;
            text-transform: uppercase;
        }
        button:active {
            transform: scale(0.95);
        }
        .btn-dislike {
            background: linear-gradient(135deg, #ff416c, #ff4b2b);
            box-shadow: 0 5px 15px rgba(255, 65, 108, 0.4);
        }
        .btn-like {
            background: linear-gradient(135deg, #00b09b, #96c93d);
            box-shadow: 0 5px 15px rgba(0, 176, 155, 0.4);
        }
        .results {
            display: none;
            text-align: left;
        }
        .results.active {
            display: block;
        }
        .results h2 {
            text-align: center;
            color: #00f3ff;
            margin-bottom: 20px;
            font-size: 1.6rem;
        }
        .result-columns {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 15px;
            margin-bottom: 25px;
        }
        .result-box {
            background: rgba(0,0,0,0.4);
            border-radius: 10px;
            padding: 15px;
            max-height: 250px;
            overflow-y: auto;
            border: 1px solid rgba(255,255,255,0.1);
        }
        .result-box h3 {
            font-size: 1rem;
            margin-bottom: 10px;
            padding-bottom: 5px;
            border-bottom: 1px solid rgba(255,255,255,0.1);
        }
        .result-box.likes h3 { color: #96c93d; }
        .result-box.dislikes h3 { color: #ff416c; }
        ul {
            list-style: none;
            padding-left: 0;
            font-size: 0.9rem;
            line-height: 1.6;
        }
        ul li {
            padding: 3px 0;
            border-bottom: 1px solid rgba(255,255,255,0.03);
        }
        .btn-restart {
            width: 100%;
            background: linear-gradient(135deg, #004dff, #00f3ff);
            margin-top: 10px;
            box-shadow: 0 5px 15px rgba(0, 77, 255, 0.4);
        }
        .hidden { display: none !important; }
    </style>
</head>
<body>

    <div class="card">
        <div id="game-screen">
            <h1>Descobrindo os gostos de Laura</h1>
            <div class="subtitle" id="progress-text">Comida 1 de 100</div>
            
            <div class="progress-container">
                <div class="progress-bar" id="progress-bar"></div>
            </div>

            <div class="food-box">
                <div class="food-name" id="food-name">Carregando...</div>
            </div>

            <div class="buttons">
                <button class="btn-dislike" id="btn-dislike">👎 Não Gosta</button>
                <button class="btn-like" id="btn-like">👍 Gosta</button>
            </div>
        </div>

        <div class="results" id="results-screen">
            <h2>Resultado Final!</h2>
            <div class="result-columns">
                <div class="result-box likes">
                    <h3>👍 Gosta:</h3>
                    <ul id="likes-list"></ul>
                </div>
                <div class="result-box dislikes">
                    <h3>👎 Não Gosta:</h3>
                    <ul id="dislikes-list"></ul>
                </div>
            </div>
            <button class="btn-restart" id="btn-restart">Jogar de Novo</button>
        </div>
    </div>

    <script>
        const foods = [
            "Pizza", "Hambúrguer", "Cheeseburger", "Lasanha", "Macarrão",
            "Nhoque", "Cuscuz", "Tapioca", "Pastel", "Coxinha",
            "Empada", "Batata frita", "Frango", "Churrasco", "Costela",
            "Fígado", "Peixe", "Salmão", "Sushi", "Temaki",
            "Yakisoba", "Camarão", "Polvo", "Lula", "Feijoada",
            "Farofa", "Arroz", "Feijão", "Brócolis", "Couve-flor",
            "Berinjela", "Quiabo", "Jiló", "Pepino", "Tomate",
            "Alface", "Manga", "Morango", "Banana", "Maçã",
            "Uva", "Abacaxi", "Chocolate", "Brigadeiro", "Sorvete",
            "Açaí", "Pudim", "Azeitona", "Mostarda", "Ketchup",
            "Pão de Queijo", "Acarajé", "Moqueca", "Vatapá", "Pamonha",
            "Canjica", "Churros", "Torta de Limão", "Bolo de Cenoura", "Mousse de Maracujá",
            "Quibe", "Esfiha", "Beirute", "Kebab", "Taco",
            "Burrito", "Nachos", "Guacamole", "Quesadilla", "Ceviche",
            "Paella", "Risoto", "Polenta", "Bife à Parmegiana", "Estrogonofe",
            "Batata Rosti", "Cebola Empanada", "Tiramisu", "Waffle", "Panqueca",
            "Crepe", "Fondue", "Omelete", "Sopa de Cebola", "Caldo Verde",
            "Bobó de Camarão", "Bacalhau", "Sardinha", "Atum", "Espetinho",
            "Coração de Frango", "Linguiça", "Picanha", "Maminha", "Alcatra",
            "Cupim", "Costelinha de Porco", "Torresmo", "Mandioca Frita", "Batata Doce"
        ];

        let shuffled = [];
        let index = 0;
        let likes = [];
        let dislikes = [];

        const foodNameEl = document.getElementById('food-name');
        const progressText = document.getElementById('progress-text');
        const progressBar = document.getElementById('progress-bar');
        const btnLike = document.getElementById('btn-like');
        const btnDislike = document.getElementById('btn-dislike');
        const gameScreen = document.getElementById('game-screen');
        const resultsScreen = document.getElementById('results-screen');
        const likesList = document.getElementById('likes-list');
        const dislikesList = document.getElementById('dislikes-list');
        const btnRestart = document.getElementById('btn-restart');

        function shuffle(array) {
            let arr = [...array];
            for (let i = arr.length - 1; i > 0; i--) {
                let j = Math.floor(Math.random() * (i + 1));
                [arr[i], arr[j]] = [arr[j], arr[i]];
            }
            return arr;
        }

        function startGame() {
            shuffled = shuffle(foods);
            index = 0;
            likes = [];
            dislikes = [];
            gameScreen.classList.remove('hidden');
            resultsScreen.classList.remove('active');
            updateCard();
        }

        function updateCard() {
            if (index >= shuffled.length) {
                showResults();
                return;
            }
            foodNameEl.textContent = shuffled[index];
            progressText.textContent = `Comida ${index + 1} de ${shuffled.length}`;
            progressBar.style.width = `${((index) / shuffled.length) * 100}%`;
        }

        function handleChoice(liked) {
            if (liked) {
                likes.push(shuffled[index]);
            } else {
                dislikes.push(shuffled[index]);
            }
            index++;
            updateCard();
        }

        function showResults() {
            gameScreen.classList.add('hidden');
            resultsScreen.classList.add('active');
            
            likesList.innerHTML = likes.map(item => `<li>• ${item}</li>`).join('') || '<li>Nenhum</li>';
            dislikesList.innerHTML = dislikes.map(item => `<li>• ${item}</li>`).join('') || '<li>Nenhum</li>';
        }

        btnLike.addEventListener('click', () => handleChoice(true));
        btnDislike.addEventListener('click', () => handleChoice(false));
        btnRestart.addEventListener('click', startGame);

        window.onload = startGame;
    </script>
</body>
</html>
```
