<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Portaria - Segunda Sem Leite (Relatório Profissional)</title>

<script src="https://cdn.tailwindcss.com"></script>
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<script src="https://cdn.jsdelivr.net/npm/chartjs-plugin-datalabels@2.2.0/dist/chartjs-plugin-datalabels.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/jspdf@2.5.1/dist/jspdf.umd.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/html2canvas@1.4.1/dist/html2canvas.min.js"></script>

<style>
.small { font-size: .85rem; }
</style>
</head>

<body class="bg-gray-50 min-h-screen p-4 md:p-8">
<div class="max-w-6xl mx-auto">

<header class="flex flex-col md:flex-row md:items-center md:justify-between gap-4 mb-6">
  <div>
    <h1 class="text-2xl md:text-3xl font-extrabold">Portaria - Segunda Sem Leite 🥛🚫</h1>
    <div class="text-sm text-gray-500">Controle de entradas, arrecadação e relatórios profissionais</div>
  </div>

  <div class="flex gap-3 items-center">
    <label class="small text-gray-600">Dia:</label>
    <input id="currentDate" type="date" class="border rounded px-2 py-1" />
    <button id="saveDay" class="px-3 py-2 rounded-xl bg-green-600 text-white small">Salvar</button>
    <button id="resetDay" class="px-3 py-2 rounded-xl bg-red-600 text-white small">Resetar</button>
  </div>
</header>

<main class="grid grid-cols-1 lg:grid-cols-3 gap-6">

<section class="bg-white p-4 rounded-xl shadow">
  <h2 class="font-semibold mb-2">Registrar entrada</h2>
  <div id="buttonsContainer" class="grid grid-cols-2 sm:grid-cols-3 gap-2 mb-3"></div>
</section>

<section class="bg-white p-4 rounded-xl shadow col-span-2">
  <div class="flex gap-4">
    <div class="bg-gray-100 p-3 rounded-lg text-center">
      <div class="text-xs text-gray-500">Pessoas</div>
      <div id="totalPeople" class="text-2xl font-bold">0</div>
    </div>

    <div class="bg-gray-100 p-3 rounded-lg text-center">
      <div class="text-xs text-gray-500 flex items-center justify-center gap-1">
        Valor Total
        <button id="toggleValue" class="cursor-pointer">👁️</button>
      </div>
      <div id="totalCollected" class="text-2xl font-bold">R$ 0,00</div>
    </div>
  </div>

  <div id="tableWrapper" class="mt-4 overflow-x-auto">
    <table class="min-w-full text-sm">
      <thead>
        <tr class="bg-gray-50">
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

</main>
</div>

<script>
const PRICE_TYPES = [
  { id:"20", label:"Dinheiro - R$20", price:20, people:1, kind:"Dinheiro" },
  { id:"30", label:"Dinheiro - R$30", price:30, people:1, kind:"Dinheiro" },
  { id:"50", label:"Dinheiro - R$50", price:50, people:2, kind:"Dinheiro" }
];

PRICE_TYPES.forEach(p => {
  const b = document.createElement('button');
  b.textContent = p.label;
  b.className = "px-3 py-2 rounded-xl bg-green-600 text-white text-sm";
  b.onclick = () => addEntry(p.id);
  buttonsContainer.appendChild(b);
});

let entries = [];
let valueHidden = false;
let realTotalValue = "R$ 0,00";

const entriesBody = document.getElementById('entriesBody');
const totalPeopleEl = document.getElementById('totalPeople');
const totalCollectedEl = document.getElementById('totalCollected');

function renderEntries() {
  entriesBody.innerHTML = entries.map(e => `
    <tr class="border-t">
      <td class="p-2">${new Date(e.timestamp).toLocaleTimeString()}</td>
      <td class="p-2">${e.type}</td>
      <td class="p-2 text-right">R$ ${e.price.toFixed(2)}</td>
      <td class="p-2 text-center">${e.people}</td>
      <td class="p-2">
        <button onclick="deleteEntry(${e.id})" class="text-red-600">Excluir</button>
      </td>
    </tr>
  `).join('');

  const totals = entries.reduce((a,e)=>{
    a.people+=e.people;
    a.collected+=e.price;
    return a;
  },{people:0,collected:0});

  totalPeopleEl.textContent = totals.people;
  realTotalValue = `R$ ${totals.collected.toFixed(2)}`;
  totalCollectedEl.textContent = valueHidden ? "••••••" : realTotalValue;
}

function addEntry(id) {
  const t = PRICE_TYPES.find(p => p.id === id);
  entries.unshift({
    id: Date.now(),
    timestamp: new Date().toISOString(),
    type: t.label,
    price: t.price,
    people: t.people
  });
  renderEntries();
}

function deleteEntry(id) {
  entries = entries.filter(e => e.id !== id);
  renderEntries();
}

document.getElementById('toggleValue').addEventListener('click', ()=>{
  valueHidden = !valueHidden;
  totalCollectedEl.textContent = valueHidden ? "••••••" : realTotalValue;
});

renderEntries();
</script>

</body>
</html>
