<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no" />
  <title>Portaria - Segunda Sem Leite 🥛🚫</title>
  <link href="https://cdn.jsdelivr.net/npm/tailwindcss@2.2.19/dist/tailwind.min.css" rel="stylesheet">

  <meta name="theme-color" content="#10B981" />
  <link rel="manifest" href="manifest.json">

  <style>
    body { -webkit-tap-highlight-color: transparent; font-family: system-ui, sans-serif; }
    button:active { transform: scale(0.96); }
    table { width: 100%; }
    .edit-btn { cursor: pointer; color: #2563eb; font-weight: bold; }
    .delete-btn { cursor: pointer; color: #dc2626; font-weight: bold; }
  </style>

  <script>
    if ('serviceWorker' in navigator) {
      window.addEventListener('load', () => navigator.serviceWorker.register('service-worker.js'));
    }
  </script>
</head>

<body class="min-h-screen bg-gray-100 p-3 sm:p-6">
  <div class="max-w-6xl mx-auto">

    <!-- Header -->
    <header class="flex flex-col sm:flex-row sm:items-center justify-between mb-6 gap-4">
      <div class="flex items-center gap-3">
        <img src="data:image/jpeg;base64,/9j/4AAQSkZJRgABAQAAAQABAAD/2wBDAAYEBQYFBAYGBQYHBwYIChAKCgkJChQ...
        ...REMOVED FOR SIZE IN PART 1...
        ...BASE64 WILL CONTINUE IN PART 2..." 
        alt="Logo" class="w-14 h-14 rounded-xl shadow">

        <div>
          <h1 class="text-2xl sm:text-3xl font-extrabold text-gray-900">
            Portaria - Segunda Sem Leite 🥛🚫
          </h1>

          <div class="flex flex-wrap items-center justify-center sm:justify-start gap-2 mt-2">
            <label class="text-sm text-gray-500">Dia:</label>
            <input id="currentDate" type="date" class="border rounded-md px-2 py-1 text-sm" />
            <button id="saveDay" class="px-3 py-2 rounded-lg bg-green-600 text-white shadow text-sm">Salvar</button>
          </div>
        </div>
      </div>

      <!-- Totals -->
      <div class="flex flex-wrap justify-center sm:justify-end gap-3">
        <div class="bg-white shadow rounded-lg px-4 py-3 text-center">
          <div class="text-xs text-gray-500">Pessoas</div>
          <div id="totalPeople" class="text-xl font-bold">0</div>
        </div>

        <div class="bg-white shadow rounded-lg px-4 py-3 text-center">
          <div class="text-xs text-gray-500">Total</div>
          <div id="totalCollected" class="text-xl font-bold">R$ 0,00</div>
        </div>

        <button id="exportCSV" class="px-3 py-2 rounded-lg bg-indigo-600 text-white shadow text-sm">Exportar CSV</button>
        <button id="resetDay" class="px-3 py-2 rounded-lg bg-red-600 text-white shadow text-sm">Resetar</button>
      </div>
    </header>

    <!-- Main content -->
    <main class="grid grid-cols-1 lg:grid-cols-3 gap-6">
      <!-- Register entry -->
      <section class="bg-white p-5 rounded-2xl shadow col-span-1">
        <h2 class="text-lg font-semibold mb-3">Registrar entrada</h2>
        <p class="text-sm text-gray-500 mb-4">Escolha o tipo de entrada:</p>

        <div id="buttonsContainer" class="grid grid-cols-2 sm:grid-cols-3 gap-3 mb-4"></div>

        <textarea id="note" placeholder="Nota (ex: nome, grupo, observação)"
          class="w-full rounded-md border p-2 mb-3 h-20 text-sm"></textarea>

        <div class="text-xs text-gray-400">Entradas grátis contam como 1 pessoa.</div>
      </section>

      <!-- Entries table -->
      <section class="bg-white p-5 rounded-2xl shadow col-span-2 overflow-x-auto">
        <h2 class="text-lg font-semibold mb-3">Entradas do dia</h2>

        <table class="min-w-full text-left text-sm">
          <thead>
            <tr class="text-gray-500 border-b">
              <th class="py-2">Hora</th>
              <th class="py-2">Tipo</th>
              <th class="py-2">Preço</th>
              <th class="py-2">Pessoas</th>
              <th class="py-2">Nota</th>
              <th class="py-2">Editar</th>
              <th class="py-2">Excluir</th>
            </tr>
          </thead>
          <tbody id="entriesBody"></tbody>
        </table>
      </section>
    </main>
      <div class="totals">
        <div class="total-box green">
          <span>Dinheiro</span>
          <strong id="totalDinheiro">R$ 0,00</strong>
        </div>
        <div class="total-box blue">
          <span>Crédito</span>
          <strong id="totalCredito">R$ 0,00</strong>
        </div>
        <div class="total-box blue">
          <span>Débito</span>
          <strong id="totalDebito">R$ 0,00</strong>
        </div>
        <div class="total-box gray">
          <span>PIX</span>
          <strong id="totalPix">R$ 0,00</strong>
        </div>
      </div>
    </div>

    <!-- LOGO BASE64 -->
    <img id="logo"
      src="data:image/jpeg;base64,{{BASE64_AQUI}}"
      class="logo"
      alt="Logo" />

    <script>
      let totals = {
        dinheiro: 0,
        credito: 0,
        debito: 0,
        pix: 0
      };

      function format(v) {
        return v.toLocaleString("pt-BR", { style: "currency", currency: "BRL" });
      }

      function adicionar(tipo) {
        const valor = parseFloat(
          prompt(`Digite o valor para ${tipo}:`).replace(",", ".")
        );

        if (isNaN(valor) || valor <= 0) {
          alert("Valor inválido!");
          return;
        }

        totals[tipo] += valor;

        document.getElementById("totalDinheiro").innerText = format(totals.dinheiro);
        document.getElementById("totalCredito").innerText = format(totals.credito);
        document.getElementById("totalDebito").innerText = format(totals.debito);
        document.getElementById("totalPix").innerText = format(totals.pix);
      }
    </script>
  </body>
</html>
