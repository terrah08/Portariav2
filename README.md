<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Portaria - Segunda Sem Leite (Relatório Profissional)</title>

<script src="https://cdn.tailwindcss.com"></script>
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<script src="https://cdn.jsdelivr.net/npm/chartjs-plugin-datalabels@2.2.0"></script>
<script src="https://cdn.jsdelivr.net/npm/jspdf@2.5.1/dist/jspdf.umd.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/html2canvas@1.4.1/dist/html2canvas.min.js"></script>

<style>
.btn { @apply px-3 py-2 rounded-xl text-white shadow-md transition-all duration-200; }
.small { font-size: .85rem; }
</style>
</head>

<body class="bg-gray-50 min-h-screen p-4 md:p-8">
<div class="max-w-6xl mx-auto">

<header class="mb-6">
  <h1 class="text-3xl font-extrabold">Portaria - Segunda Sem Leite 🥛🚫</h1>
</header>

<section class="bg-white p-4 rounded-xl shadow mb-6">
  <div class="flex gap-4">

    <div class="bg-gray-100 p-3 rounded-lg text-center">
      <div class="text-xs text-gray-500">Pessoas</div>
      <div id="totalPeople" class="text-2xl font-bold">0</div>
    </div>

    <!-- 🔴 ALTERAÇÃO AQUI -->
    <div class="bg-gray-100 p-3 rounded-lg text-center relative">
      <div class="text-xs text-gray-500 flex items-center justify-center gap-1">
        Valor Total
        <button id="toggleEye" class="text-gray-600 hover:text-black text-sm">👁</button>
      </div>
      <div id="totalCollected" class="text-2xl font-bold">R$ 0,00</div>
    </div>

  </div>
</section>

</div>

<script>
/* =========================
   CONTROLE DO OLHO 👁
========================= */
let showTotal = true;
let lastTotalValue = 0;

document.getElementById('toggleEye').onclick = () => {
  showTotal = !showTotal;
  updateTotalDisplay();
};

function updateTotalDisplay() {
  const el = document.getElementById('totalCollected');
  el.textContent = showTotal
    ? `R$ ${lastTotalValue.toFixed(2)}`
    : 'R$ ••••';
}

/* =========================
   SIMULAÇÃO DO TOTAL
   (SEU CÓDIGO ORIGINAL CONTINUA IGUAL)
========================= */
function setTotal(valor) {
  lastTotalValue = valor;
  updateTotalDisplay();
}

/* Exemplo de atualização */
setTotal(1234.56);
</script>

</body>
</html>
