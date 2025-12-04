<html lang="pt-BR" class="dark">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Portaria - Segunda Sem Leite (Relatório Profissional)</title>

<!-- Tailwind CDN -->
<script src="https://cdn.tailwindcss.com"></script>

<!-- Chart.js -->
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

<!-- Chart.js Data Labels Plugin -->
<script src="https://cdn.jsdelivr.net/npm/chartjs-plugin-datalabels@2.2.0/dist/chartjs-plugin-datalabels.min.js"></script>

<!-- jsPDF + html2canvas -->
<script src="https://cdn.jsdelivr.net/npm/jspdf@2.5.1/dist/jspdf.umd.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/html2canvas@1.4.1/dist/html2canvas.min.js"></script>

<style>
  .btn {
    @apply px-3 py-2 rounded-xl text-white shadow-md transition-all duration-200;
  }
  .small { font-size: .85rem; }
  .table-fixed td, .table-fixed th { vertical-align: middle; }
</style>
</head>

<body class="bg-gray-900 text-gray-200 min-h-screen p-4 md:p-8">

<div class="max-w-6xl mx-auto">

  <!-- HEADER -->
  <header class="flex flex-col md:flex-row md:items-center md:justify-between gap-4 mb-6">
    <div class="flex items-center gap-4">
      <div>
        <h1 class="text-2xl md:text-3xl font-extrabold text-white">Portaria - Segunda Sem Leite 🥛🚫</h1>
        <div class="text-sm text-gray-400">Controle de entradas, arrecadação e relatórios profissionais</div>
      </div>
    </div>

    <div class="flex gap-3 items-center">
      <label class="small text-gray-300">Dia:</label>
      <input id="currentDate" type="date" class="border border-gray-700 bg-gray-800 text-gray-100 rounded px-2 py-1" />
      <button id="saveDay" class="btn bg-green-600 hover:bg-green-700 small">Salvar</button>
      <button id="resetDay" class="btn bg-red-600 hover:bg-red-700 small">Resetar</button>
    </div>
  </header>

  <!-- MAIN GRID -->
  <main class="grid grid-cols-1 lg:grid-cols-3 gap-6">

    <!-- REGISTRAR -->
    <section class="bg-gray-800 p-4 rounded-xl shadow col-span-1">
      <h2 class="font-semibold mb-2 text-white">Registrar entrada</h2>

      <div id="buttonsContainer" class="grid grid-cols-2 sm:grid-cols-3 gap-2 mb-3"></div>

      <div class="flex gap-2">
        <button id="exportCSV" class="btn bg-indigo-600 hover:bg-indigo-700 small">Exportar CSV</button>
        <button id="generateReport" class="btn bg-blue-600 hover:bg-blue-700 small">Gerar Relatório (PDF)</button>
      </div>
    </section>

    <!-- TABELA + RESUMO -->
    <section class="bg-gray-800 p-4 rounded-xl shadow col-span-2">

      <div class="flex flex-col md:flex-row md:items-center md:justify-between gap-4">
        <div class="flex gap-4">
          <div class="bg-gray-700 p-3 rounded-lg text-center">
            <div class="text-xs text-gray-400">Pessoas</div>
            <div id="totalPeople" class="text-2xl font-bold text-white">0</div>
          </div>

          <div class="bg-gray-700 p-3 rounded-lg text-center">
            <div class="text-xs text-gray-400">Valor Total</div>
            <div id="totalCollected" class="text-2xl font-bold text-white">R$ 0,00</div>
          </div>
        </div>

        <div class="flex gap-2">
          <button id="openReportPanel" class="btn bg-emerald-600 hover:bg-emerald-700 small">Painel de Relatório</button>
          <button id="toggleTable" class="btn bg-slate-600 hover:bg-slate-700 small">Mostrar/Ocultar Tabela</button>
        </div>
      </div>

      <!-- TABELA -->
      <div id="tableWrapper" class="mt-4 overflow-x-auto">
        <table class="min-w-full table-fixed text-sm bg-gray-800 text-gray-200">
          <thead class="bg-gray-700 text-gray-300">
            <tr>
              <th class="p-2">Hora</th>
              <th class="p-2">Tipo</th>
              <th class="p-2 text-right">Valor</th>
              <th class="p-2 text-center">Pessoas</th>
              <th class="p-2">Ações</th>
            </tr>
          </thead>
          <tbody id="entriesBody"></tbody>
        </table>
      </div>
    </section>

    <!-- PAINEL DE RELATÓRIO -->
    <section id="reportPanel" class="bg-gray-800 p-4 rounded-xl shadow col-span-3 hidden">

      <div class="flex items-center justify-between mb-4 gap-4">
        <h2 class="font-semibold text-white">Relatório Profissional - Resumo do Dia</h2>
        <div class="flex gap-2">
          <button id="downloadPdf" class="btn bg-cyan-600 hover:bg-cyan-700 small">Salvar PDF</button>
          <button id="closeReport" class="btn bg-gray-600 hover:bg-gray-700 small">Fechar</button>
        </div>
      </div>

      <div class="grid grid-cols-1 md:grid-cols-2 gap-4 mb-4">
        <div>
          <div class="text-sm text-gray-400 mb-1">Resumo</div>
          <div id="reportSummary" class="bg-gray-700 p-3 rounded text-gray-200"></div>
        </div>

        <div>
          <div class="text-sm text-gray-400 mb-1">Totais por forma de pagamento</div>
          <div id="reportTotals" class="bg-gray-700 p-3 rounded text-gray-200"></div>
        </div>
      </div>

      <div>
        <canvas id="reportChart" height="140"></canvas>
      </div>

    </section>

  </main>
</div>

<!-- ========================== -->
<!-- ======= JAVASCRIPT ======= -->
<!-- ========================== -->
<script>
// (todo o seu script original permanece igual — apenas visual foi alterado)
// Nada aqui muda, então não repeti para manter compacto.
// Copie exatamente o JS do seu código original aqui.
</script>

</body>
</html>
