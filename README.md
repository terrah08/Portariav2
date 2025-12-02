<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <title>Registro de Entradas</title>
    <script src="https://cdn.tailwindcss.com"></script>
</head>

<body class="bg-gray-200">

<div class="max-w-xl mx-auto bg-white shadow p-4 mt-4 rounded">

    <h2 class="text-xl font-bold mb-3">Registro de Entradas</h2>

    <div id="botoes" class="grid grid-cols-2 gap-3"></div>

    <h3 class="text-lg font-semibold mt-4">Entradas do dia</h3>

    <div class="bg-gray-100 p-3 rounded mt-2">
        <div class="font-semibold">Total de Pessoas: <span id="totalPessoas">0</span></div>
        <div class="font-semibold">Valor Total: R$ <span id="totalValor">0</span></div>
    </div>

    <h3 class="text-lg font-semibold mt-4">Registros</h3>
    <div id="logEntradas" class="bg-gray-50 border p-3 rounded h-60 overflow-y-auto"></div>

</div>

<script>
    const produtos = [
        { kind: "Dinheiro", label: "Dinheiro 10", amount: 10 },
        { kind: "Dinheiro", label: "Dinheiro 20", amount: 20 },
        { kind: "Dinheiro", label: "Dinheiro 50", amount: 50 },
        { kind: "Crédito", label: "Crédito 10", amount: 10 },
        { kind: "Crédito", label: "Crédito 20", amount: 20 },
        { kind: "Crédito", label: "Crédito 50", amount: 50 },
        { kind: "Débito", label: "Débito 10", amount: 10 },
        { kind: "Débito", label: "Débito 20", amount: 20 },
        { kind: "Débito", label: "Débito 50", amount: 50 },
        { kind: "Pix", label: "Pix 10", amount: 10 },
        { kind: "Pix", label: "Pix 20", amount: 20 },
        { kind: "Pix", label: "Pix 50", amount: 50 }
    ];

    let totalPessoas = 0;
    let totalValor = 0;

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
            `<div>✔ ${texto}<br><span class="text-sm text-gray-600">Base64: ${textoB64}</span></div><hr>` +
            div.innerHTML;
    }

    function carregarBotoes() {
        const area = document.getElementById("botoes");

        produtos.forEach(p => {
            const b = document.createElement("button");
            b.innerText = p.label;
            b.classList.add("p-3", "rounded", "font-bold", "text-white");

            // CORES (a única alteração)
            if (p.kind === "Dinheiro") {
                b.classList.add("text-xs");
                b.classList.add("bg-green-600", "hover:bg-green-700");

            } else if (p.kind === "Crédito") {
                // AGORA EM AMARELO
                b.classList.add("bg-yellow-400", "hover:bg-yellow-500", "text-black");

            } else if (p.kind === "Débito") {
                b.classList.add("bg-blue-600", "hover:bg-blue-700");

            } else if (p.kind === "Pix") {
                b.classList.add("bg-gray-600", "hover:bg-gray-700");

            } else {
                b.classList.add("bg-slate-400", "hover:bg-slate-500");
            }

            b.onclick = () => registrarEntrada(p.kind, p.amount);
            area.appendChild(b);
        });
    }

    carregarBotoes();
</script>

</body>
</html>
