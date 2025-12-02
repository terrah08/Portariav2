<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <title>Registro de Entradas</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background: #f2f2f2;
            margin: 0;
            padding: 0;
        }
        .container {
            width: 90%;
            max-width: 900px;
            margin: auto;
            background: white;
            padding: 20px;
            margin-top: 20px;
            border-radius: 10px;
            box-shadow: 0 0 10px #ccc;
        }
        h2 {
            margin-top: 0;
        }
        .btn {
            display: block;
            width: 100%;
            padding: 15px;
            margin: 10px 0;
            border: none;
            border-radius: 8px;
            font-size: 16px; /* Ajustado para caber melhor */
            cursor: pointer;
            font-weight: bold;
        }

        /* Cores solicitadas */
        .dinheiro { background: #4CAF50; color: white; }
        .credito { background: #2196F3; color: white; }
        .debito { background: #1976D2; color: white; }
        .pix { background: #9E9E9E; color: white; }

        #entradasDia {
            margin-top: 20px;
            padding: 10px;
            background: #eee;
            border-radius: 8px;
        }
        .contador {
            font-size: 20px;
            font-weight: bold;
            margin: 8px 0;
        }
        .log {
            padding: 10px;
            background: #fafafa;
            border-radius: 8px;
            max-height: 300px;
            overflow-y: auto;
            border: 1px solid #ddd;
        }
    </style>
</head>
<body>

<div class="container">
    <h2>Registro de Entradas</h2>

    <!-- Botões sem (1P) ou (2P) -->
    <button class="btn dinheiro" onclick="registrarEntrada('Dinheiro', 30)">Dinheiro - R$30</button>
    <button class="btn dinheiro" onclick="registrarEntrada('Dinheiro', 50)">Dinheiro - R$50</button>

    <button class="btn credito" onclick="registrarEntrada('Crédito', 30)">Crédito - R$30</button>
    <button class="btn credito" onclick="registrarEntrada('Crédito', 50)">Crédito - R$50</button>

    <button class="btn debito" onclick="registrarEntrada('Débito', 30)">Débito - R$30</button>
    <button class="btn debito" onclick="registrarEntrada('Débito', 50)">Débito - R$50</button>

    <button class="btn pix" onclick="registrarEntrada('Pix', 30)">Pix - R$30</button>
    <button class="btn pix" onclick="registrarEntrada('Pix', 50)">Pix - R$50</button>

    <h3>Entradas do dia</h3>
    <div id="entradasDia">
        <div class="contador">Total de Pessoas: <span id="totalPessoas">0</span></div>
        <div class="contador">Valor Total R$: <span id="totalValor">0</span></div>
    </div>

    <h3>Registros</h3>
    <div class="log" id="logEntradas"></div>
</div>

<script>
    let totalPessoas = 0;
    let totalValor = 0;

    // Conversor para Base64
    function toBase64(texto) {
        return btoa(unescape(encodeURIComponent(texto)));
    }

    function registrarEntrada(tipo, valor) {
        const data = new Date().toLocaleString("pt-BR");

        totalPessoas++;
        totalValor += valor;

        document.getElementById("totalPessoas").innerText = totalPessoas;
        document.getElementById("totalValor").innerText = totalValor;

        const texto = `${data} - Entrada ${tipo} - R$ ${valor}`;
        const textoB64 = toBase64(texto);

        const div = document.getElementById("logEntradas");
        div.innerHTML =
            `<div>✔ ${texto}<br>🔐 Base64: <span style="color:#555">${textoB64}</span></div><hr>` +
            div.innerHTML;
    }
</script>

</body>
</html>
