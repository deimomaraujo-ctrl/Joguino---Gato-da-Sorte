<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>RocaMap Offline - Medição Rural</title>
    <!-- Vincula o Manifesto para permitir a instalação no celular -->
    <link rel="manifest" href="manifest.json">
    <meta name="theme-color" content="#2c5e3b">
    <style>
        :root {
            --primary-color: #2c5e3b;
            --secondary-color: #f4f4f9;
            --accent-color: #d97706;
            --text-color: #333;
        }
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            background-color: var(--secondary-color);
            color: var(--text-color);
        }
        header {
            background-color: var(--primary-color);
            color: white;
            padding: 15px;
            text-align: center;
        }
        .status-offline {
            background-color: #4b5563;
            color: white;
            font-size: 0.85rem;
            padding: 5px;
            text-align: center;
        }
        .container {
            padding: 20px;
            max-width: 600px;
            margin: auto;
        }
        .card {
            background: white;
            padding: 15px;
            border-radius: 8px;
            box-shadow: 0 2px 4px rgba(0,0,0,0.1);
            margin-bottom: 15px;
        }
        button {
            width: 100%;
            padding: 12px;
            margin-top: 10px;
            background-color: var(--primary-color);
            color: white;
            border: none;
            border-radius: 5px;
            font-size: 1rem;
            cursor: pointer;
        }
        button.secondary {
            background-color: var(--accent-color);
        }
        button:active {
            opacity: 0.8;
        }
        .metrics {
            font-weight: bold;
            color: var(--primary-color);
        }
    </style>
</head>
<body>

    <header>
        <h2>RocaMap Offline</h2>
    </header>
    <div class="status-offline">Pronto para uso sem internet (PWA)</div>

    <div class="container">
        <!-- Secção de Marcos e Pontos Fixos -->
        <div class="card">
            <h3>Demarcação de Cantos (Marcos)</h3>
            <p>Posicione o celular no marco e aguarde a estabilização para garantir precisão sob a mata.</p>
            <button onclick="salvarPontoFixo()">Salvar Marco Atual (15s)</button>
            <p id="status-ponto">Estado: Aguardando captura...</p>
        </div>

        <!-- Secção de Trilhas -->
        <div class="card">
            <h3>Rastreamento de Trilha</h3>
            <p>Ative ao caminhar por estradas internas ou picadas na mata.</p>
            <button class="secondary" onclick="alternarTrilha()">Iniciar Modo Trilha</button>
            <p id="status-trilha">Trilha: Desativada</p>
        </div>

        <!-- Secção de Dados e Área -->
        <div class="card">
            <h3>Dados da Propriedade</h3>
            <p>Marcos salvos: <span id="qtd-marcos" class="metrics">0</span></p>
            <p>Área estimada: <span id="area-calculada" class="metrics">0.00 Alqueires / Hectares</span></p>
            <button onclick="gerarPerimetro()">Gerar Perímetro e Fechar Área</button>
        </div>
    </div>

    <script>
        // Funções de exemplo de interface
        function salvarPontoFixo() {
            const statusPonto = document.getElementById("status-ponto");
            statusPonto.innerText = "Estado: Coletando satélites (Estabilizando)...";
            setTimeout(() => {
                statusPonto.innerText = "Estado: Marco salvo com sucesso no armazenamento local!";
            }, 3000);
        }

        function alternarTrilha() {
            const statusTrilha = document.getElementById("status-trilha");
            if (statusTrilha.innerText.includes("Desativada")) {
                statusTrilha.innerText = "Trilha: Gravando percurso localmente...";
            } else {
                statusTrilha.innerText = "Trilha: Desativada";
            }
        }

        function gerarPerimetro() {
            alert("Perímetro gerado e salvo no banco de dados interno do celular!");
        }

        // Registro do Service Worker para permitir funcionamento offline
        if ('serviceWorker' in navigator) {
            window.addEventListener('load', () => {
                navigator.serviceWorker.register('./sw.js')
                    .then((reg) => console.log('Service Worker registrado com sucesso:', reg.scope))
                    .catch((err) => console.log('Falha ao registrar Service Worker:', err));
            });
        }
    </script>

</body>
</html>
