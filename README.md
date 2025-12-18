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
  .btn {
    @apply px-3 py-2 rounded-xl text-white shadow-md transition-all duration-200;
  }
  .small { font-size: .85rem; }
</style>
</head>

<body class="bg-gray-50 min-h-screen p-4 md:p-8">
<div class="max-w-6xl mx-auto">

<header class="flex flex-col md:flex-row md:justify-between gap-4 mb-6">
  <div>
    <h1 class="text-3xl font-extrabold">Portaria - Segunda Sem Leite 🥛🚫</h1>
    <div class="text-sm text-gray-500">Controle de entradas e relatórios</div>
  </div>

  <div class="flex gap-2 items-center">
    <input id="currentDate" type="date" class="border rounded px-2 py-1" />
    <button id="saveDay" class="btn bg-green-600 small">Salvar</button>
    <button id="resetDay" class="btn bg-red-600 small">Resetar</button>
  </div>
</header>

<main class="grid grid-cols-1 lg:grid-cols-3 gap-6">

<section class="bg-white p-4 rounded-xl shadow">
  <h2 class="font-semibold mb-2">Registrar entrada</h2>
  <div id="buttonsContainer" class="grid grid-cols-2 sm:grid-cols-3 gap-2 mb-3"></div>
  <div class="flex gap-2">
    <button id="exportCSV" class="btn bg-indigo-600 small">Exportar CSV</button>
    <button id="openReportPanel" class="btn bg-emerald-600 small">Painel de Relatório</button>
  </div>
</section>

<section class="bg-white p-4 rounded-xl shadow col-span-2">
  <div class="flex justify-between mb-4">
    <div class="flex gap-4">
      <div class="bg-gray-100 p-3 rounded text-center">
        <div class="text-xs text-gray-500">Pessoas</div>
        <div id="totalPeople" class="text-2xl font-bold">0</div>
      </div>

      <!-- AQUI FOI O ÚNICO ACRÉSCIMO -->
      <div class="bg-gray-100 p-3 rounded text-center">
        <div class="text-xs text-gray-500 flex items-center justify-center gap-1">
          Valor Total
          <button id="toggleTotalEye">👁</button>
        </div>
        <div id="totalCollected" class="text-2xl font-bold">R$ 0,00</div>
      </div>
    </div>

    <button id="toggleTable" class="btn bg-slate-600 small">Tabela</button>
  </div>

  <div id="tableWrapper" class="overflow-x-auto">
    <table class="min-w-full text-sm">
      <thead>
        <tr class="bg-gray-100">
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

<section id="reportPanel" class="bg-white p-4 rounded-xl shadow col-span-3 hidden">
  <div class="flex justify-between mb-4">
    <h2 class="font-semibold">Relatório do Dia</h2>
    <div class="flex gap-2">
      <button id="downloadPdf" class="btn bg-cyan-600 small">PDF</button>
      <button id="closeReport" class="btn bg-gray-500 small">Fechar</button>
    </div>
  </div>

  <div class="grid md:grid-cols-2 gap-4 mb-4">
    <div id="reportSummary" class="bg-gray-50 p-3 rounded"></div>
    <div id="reportTotals" class="bg-gray-50 p-3 rounded"></div>
  </div>

  <canvas id="reportChart"></canvas>
</section>

</main>
</div>

<script>
/* ==== TIPOS ==== */
const PRICE_TYPES = [
  { id:"20", label:"Dinheiro - R$20", price:20, people:1, kind:"Dinheiro" },
  { id:"30", label:"Dinheiro - R$30", price:30, people:1, kind:"Dinheiro" },
  { id:"50", label:"Dinheiro - R$50", price:50, people:2, kind:"Dinheiro" },
  { id:"20_credito", label:"Crédito - R$20", price:20, people:1, kind:"Crédito" },
  { id:"30_credito", label:"Crédito - R$30", price:30, people:1, kind:"Crédito" },
  { id:"50_credito", label:"Crédito - R$50", price:50, people:2, kind:"Crédito" },
  { id:"20_debito", label:"Débito - R$20", price:20, people:1, kind:"Débito" },
  { id:"30_debito", label:"Débito - R$30", price:30, people:1, kind:"Débito" },
  { id:"50_debito", label:"Débito - R$50", price:50, people:2, kind:"Débito" },
  { id:"20_pix", label:"Pix - R$20", price:20, people:1, kind:"Pix" },
  { id:"30_pix", label:"Pix - R$30", price:30, people:1, kind:"Pix" },
  { id:"50_pix", label:"Pix - R$50", price:50, people:2, kind:"Pix" },
  { id:"free50", label:"Free 50 Pessoas", price:0, people:50, kind:"Gratuidade" },
  { id:"free", label:"Lista Free", price:0, people:1, kind:"Gratuidade" },
  { id:"militar", label:"Militar", price:0, people:1, kind:"Gratuidade" },
  { id:"aniversario", label:"Aniversário", price:0, people:1, kind:"Gratuidade" }
];

const buttonsContainer = document.getElementById("buttonsContainer");
PRICE_TYPES.forEach(p=>{
  const b=document.createElement("button");
  b.textContent=p.label;
  b.className="btn bg-blue-600 small";
  b.onclick=()=>addEntry(p.id);
  buttonsContainer.appendChild(b);
});

/* ==== DADOS ==== */
let entries=[];
let showTotal=true;
let lastTotal=0;

function render(){
  const body=document.getElementById("entriesBody");
  body.innerHTML="";
  let people=0,total=0;

  entries.forEach(e=>{
    people+=e.people;
    total+=e.price;
    body.innerHTML+=`
      <tr>
        <td class="p-2">${new Date(e.ts).toLocaleTimeString()}</td>
        <td class="p-2">${e.type}</td>
        <td class="p-2 text-right">R$ ${e.price.toFixed(2)}</td>
        <td class="p-2 text-center">${e.people}</td>
        <td class="p-2"><button onclick="del(${e.id})">❌</button></td>
      </tr>`;
  });

  document.getElementById("totalPeople").textContent=people;
  lastTotal=total;
  updateTotal();
}

function updateTotal(){
  document.getElementById("totalCollected").textContent =
    showTotal ? `R$ ${lastTotal.toFixed(2)}` : "R$ ••••";
}

document.getElementById("toggleTotalEye").onclick=()=>{
  showTotal=!showTotal;
  updateTotal();
};

function addEntry(id){
  const p=PRICE_TYPES.find(x=>x.id===id);
  entries.unshift({id:Date.now(),ts:new Date(),...p});
  render();
}

function del(id){
  entries=entries.filter(e=>e.id!==id);
  render();
}

render();
</script>
</body>
</html>
