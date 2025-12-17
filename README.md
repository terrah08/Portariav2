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
.btn { padding: .5rem .75rem; border-radius: .75rem; color: #fff; box-shadow: 0 4px 6px rgba(0,0,0,.1); }
.small { font-size: .85rem; }
</style>
</head>

<body class="bg-gray-50 min-h-screen p-4 md:p-8">
<div class="max-w-6xl mx-auto">

<header class="mb-6">
  <h1 class="text-3xl font-extrabold">Portaria - Segunda Sem Leite 🥛🚫</h1>
  <p class="text-sm text-gray-500">Controle de entradas, arrecadação e relatórios profissionais</p>
</header>

<main class="grid grid-cols-1 lg:grid-cols-3 gap-6">

<section class="bg-white p-4 rounded-xl shadow">
  <h2 class="font-semibold mb-2">Registrar entrada</h2>
  <div id="buttonsContainer" class="grid grid-cols-2 sm:grid-cols-3 gap-2"></div>
</section>

<section class="bg-white p-4 rounded-xl shadow lg:col-span-2">
  <div class="flex gap-4 mb-4">

    <div class="bg-gray-100 p-3 rounded-lg text-center">
      <div class="text-xs text-gray-500">Pessoas</div>
      <div id="totalPeople" class="text-2xl font-bold">0</div>
    </div>

    <div class="bg-gray-100 p-3 rounded-lg text-center">
      <div class="text-xs text-gray-500 flex items-center justify-center gap-1">
        Valor Total
        <button onclick="toggleTotalValue()" title="Mostrar/Ocultar valor">👁️</button>
      </div>
      <div id="totalCollected"
           class="text-2xl font-bold"
           data-hidden="false"
           data-real="R$ 0,00">
        R$ 0,00
      </div>
    </div>

  </div>

  <table class="w-full text-sm">
    <thead>
      <tr class="border-b">
        <th>Hora</th>
        <th>Tipo</th>
        <th class="text-right">Valor</th>
        <th class="text-center">Pessoas</th>
      </tr>
    </thead>
    <tbody id="entriesBody"></tbody>
  </table>
</section>

</main>
</div>

<script>
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
const entriesBody = document.getElementById("entriesBody");
const totalPeopleEl = document.getElementById("totalPeople");
const totalCollectedEl = document.getElementById("totalCollected");

let entries = [];

PRICE_TYPES.forEach(p => {
  const b = document.createElement("button");
  b.textContent = p.label;
  b.className = "btn bg-slate-600";
  b.onclick = () => addEntry(p);
  buttonsContainer.appendChild(b);
});

function addEntry(p) {
  entries.unshift({ ...p, timestamp: new Date() });
  renderEntries();
}

function renderEntries() {
  let people = 0, collected = 0;
  entriesBody.innerHTML = "";

  entries.forEach(e => {
    people += e.people;
    collected += e.price;
    entriesBody.innerHTML += `
      <tr class="border-b">
        <td>${e.timestamp.toLocaleTimeString()}</td>
        <td>${e.label}</td>
        <td class="text-right">R$ ${e.price.toFixed(2)}</td>
        <td class="text-center">${e.people}</td>
      </tr>`;
  });

  totalPeopleEl.textContent = people;

  const formatted = `R$ ${collected.toFixed(2)}`;
  totalCollectedEl.dataset.real = formatted;
  totalCollectedEl.textContent =
    totalCollectedEl.dataset.hidden === "true" ? "••••••" : formatted;
}

function toggleTotalValue() {
  const hidden = totalCollectedEl.dataset.hidden === "true";
  totalCollectedEl.dataset.hidden = (!hidden).toString();
  totalCollectedEl.textContent = hidden
    ? totalCollectedEl.dataset.real
    : "••••••";
}
</script>
</body>
</html>
