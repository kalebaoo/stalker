<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Questionário</title>
    <style>
        /* Reset de margin e padding */
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        /* Corpo da página */
        body {
            font-family: 'Arial', sans-serif;
            background-color: #f4f7fc;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            overflow: hidden;
            background: linear-gradient(135deg, #f4f7fc 0%, #d0e7ff 100%);
        }

        /* Caixa de conteúdo */
        .container {
            background-color: white;
            border-radius: 15px;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
            width: 100%;
            max-width: 400px;
            padding: 20px;
            text-align: center;
            animation: fadeIn 0.5s ease-in-out;
            transform: translateY(0);
            transition: transform 0.3s ease;
        }

        .container:hover {
            transform: translateY(-10px);
        }

        /* Animação de FadeIn */
        @keyframes fadeIn {
            0% { opacity: 0; }
            100% { opacity: 1; }
        }

        /* Títulos */
        h1 {
            color: #4f4f4f;
            margin-bottom: 20px;
            font-size: 24px;
            font-weight: bold;
        }

        h2 {
            color: #555;
            margin-bottom: 10px;
        }

        /* Estilo dos campos de input */
        input[type="text"], input[type="email"] {
            width: 100%;
            padding: 12px;
            margin: 10px 0;
            border-radius: 8px;
            border: 1px solid #ddd;
            font-size: 16px;
            transition: border-color 0.3s ease;
        }

        input[type="text"]:focus, input[type="email"]:focus {
            border-color: #4CAF50;
            outline: none;
        }

        /* Botões */
        button {
            width: 100%;
            padding: 12px;
            background-color: #4CAF50;
            color: white;
            font-size: 16px;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            transition: background-color 0.3s ease;
        }

        button:hover {
            background-color: #45a049;
        }

        /* Estilo de carregamento */
        .loading {
            text-align: center;
        }

        /* Ocultar elementos */
        .hidden {
            display: none;
        }

    </style>
</head>
<body>

    <!-- Caixa de conteúdo -->
    <div class="container" id="loading">
        <div class="loading">
            <p>Carregando...</p>
            <img src="loading-symbol.gif" alt="Carregando..." /> <!-- Imagem de carregamento (opcional) -->
        </div>
    </div>

    <!-- Formulário de Nome -->
    <div class="container hidden" id="nameForm">
        <h1>Bem-vindo!</h1>
        <h2>Qual seu nome?</h2>
        <input type="text" id="name" name="name" required>
        <button onclick="sendName()">Enviar</button>
    </div>

    <!-- Pergunta -->
    <div class="container hidden" id="questionSection">
        <h1>Oq vc quer aq?</h1>
        <input type="text" id="answer" required>
        <button onclick="sendAnswer()">Enviar Resposta</button>
    </div>

    <!-- Nova Pergunta -->
    <div class="container hidden" id="newQuestionSection">
        <h1>Agora, mais uma pergunta!</h1>
        <h2>e vc acha legal me stalkear?</h2>
        <input type="text" id="favoriteColor" required>
        <button onclick="sendFavoriteColor()">Enviar Resposta</button>
    </div>

    <!-- Despedida e Redirecionamento -->
    <div class="container hidden" id="goodbyeSection">
        <h1>Obrigado por participar!</h1>
        <p>Para mais, confira o meu Instagram!</p>
        <button onclick="redirectToInstagram()">Ir para Instagram</button>
    </div>

    <script>
        let userName = "";
        let userDevice = navigator.userAgent;  // Captura o dispositivo do usuário
        let discordWebhookUrl = "https://discord.com/api/webhooks/1366584744657293373/mjIYbtbcDepb8iSLysQqHf0Ijzp_NRZT0G632O_W3vLvlLWCSadRjLz8taOrMlAyUDeo";  // Webhook do Discord

        window.onload = () => {
            // Exibe o carregamento inicial por 2 segundos
            setTimeout(() => {
                document.getElementById("loading").style.display = 'none';
                document.getElementById("nameForm").style.display = 'block';
            }, 2000);
        }

        function sendName() {
            userName = document.getElementById("name").value;

            if (!userName) {
                alert("Por favor, insira seu nome.");
                return;
            }

            // Envia o nome e o dispositivo para o Discord
            let embed = {
                title: "Novo usuário entrou",
                fields: [
                    { name: "Nome", value: userName, inline: true },
                    { name: "Dispositivo", value: userDevice, inline: true }
                ]
            };

            let payload = {
                embeds: [embed]
            };

            // Envia os logs para o Discord
            fetch(discordWebhookUrl, {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify(payload)
            })
            .then(response => response.json())
            .then(data => {
                console.log("Logs enviados para o Discord:", data);
            })
            .catch(error => {
                console.error("Erro ao enviar para o Discord:", error);
            });

            // Exibe a primeira pergunta
            setTimeout(() => {
                document.getElementById("nameForm").style.display = 'none';
                document.getElementById("questionSection").style.display = 'block';
            }, 500);
        }

        function sendAnswer() {
            let answer = document.getElementById("answer").value;

            if (!answer) {
                alert("Por favor, insira sua resposta.");
                return;
            }

            // Envia a resposta para o Discord
            let embed = {
                title: "Resposta do usuário",
                fields: [
                    { name: "Pergunta", value: "O que deseja aqui?", inline: true },
                    { name: "Resposta", value: answer, inline: true }
                ]
            };

            let payload = {
                embeds: [embed]
            };

            fetch(discordWebhookUrl, {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify(payload)
            })
            .then(response => response.json())
            .then(data => {
                console.log("Resposta enviada para o Discord:", data);
            })
            .catch(error => {
                console.error("Erro ao enviar para o Discord:", error);
            });

            // Exibe a segunda pergunta
            setTimeout(() => {
                document.getElementById("questionSection").style.display = 'none';
                document.getElementById("newQuestionSection").style.display = 'block';
            }, 500);
        }

        function sendFavoriteColor() {
            let favoriteColor = document.getElementById("favoriteColor").value;

            if (!favoriteColor) {
                alert("Por favor, insira sua cor favorita.");
                return;
            }

            // Envia a cor favorita para o Discord
            let embed = {
                title: "Resposta do usuário",
                fields: [
                    { name: "Pergunta", value: "Qual sua cor favorita?", inline: true },
                    { name: "Resposta", value: favoriteColor, inline: true }
                ]
            };

            let payload = {
                embeds: [embed]
            };

            fetch(discordWebhookUrl, {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify(payload)
            })
            .then(response => response.json())
            .then(data => {
                console.log("Resposta enviada para o Discord:", data);
            })
            .catch(error => {
                console.error("Erro ao enviar para o Discord:", error);
            });

            // Exibe a despedida e redireciona
            setTimeout(() => {
                document.getElementById("newQuestionSection").style.display = 'none';
                document.getElementById("goodbyeSection").style.display = 'block';
            }, 500);
        }

        function redirectToInstagram() {
            window.location.href = "https://www.instagram.com/kalebn7s/?__pwa=1";
        }
    </script>
</body>
</html>
