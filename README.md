<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Portaria - Segunda Sem Leite</title>

<script src="https://cdn.tailwindcss.com"></script>

<style>
.small { font-size: .85rem; }
</style>
</head>

<body class="bg-gray-50 min-h-screen p-4 md:p-8">
<div class="max-w-6xl mx-auto">

<header class="mb-6">
  <h1 class="text-3xl font-extrabold">Portaria - Segunda Sem Leite 🥛🚫</h1>
  <p class="text-gray-500">Controle de entradas e arrecadação</p>
</header>

<main class="grid grid-cols-1 lg:grid-cols-3 gap-6">

<section class="bg-white p-4 rounded-xl shadow col-span-1">
  <h2 class="font-semibold mb-3">Registrar entrada</h2>
  <div id="buttonsContainer" class="grid grid-cols-2 sm:grid-cols-3 gap-2"></div>
</section>

<section class="bg-white p-4 rounded-xl shadow col-span-2">
  <div class="flex gap-4 mb-4">

    <div class="bg-gray-100 p-3 rounded-lg text-center">
      <div class="text-xs text-gray-500">Pessoas</div>
      <div id="totalPeople" class="text-2xl font-bold">0</div>
    </div>

    <!-- VALOR TOTAL COM OLHO -->
    <div class="bg-gray-100 p-3 rounded-lg text-center">
      <div class="text-xs text-gray-500 flex items-center justify-center gap-1">
        Valor Total
        <button onclick="toggleTotalValue()" title="Mostrar / Ocultar">👁️</button>
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
    <thead class="bg-gray-100">
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
</section>

</main>
</div>

<script>
/* ===== TIPOS ORIGINAIS (SEM REMOVER NADA) ===== */
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
  { id:"free", label:"Lista (Free)", price:0, people:1, kind:"Gratuidade" },
  { id:"militar", label:"Militar", price:0, people:1, kind:"Gratuidade" },
  { id:"aniversario", label:"Aniversário", price:0, people:1, kind:"Gratuidade" }
];

/* ===== BOTÕES ===== */
PRICE_TYPES.forEach(p => {
  const b = document.createElement('button');
  b.className = 'px-3 py-2 rounded-xl text-white text-sm shadow';

  if (p.kind === "Dinheiro") b.classList.add("bg-green-600");
  else if (p.kind === "Crédito") b.classList.add("bg-yellow-400","text-black");
  else if (p.kind === "Débito") b.classList.add("bg-blue-600");
  else if (p.kind === "Pix") b.classList.add("bg-gray-600");
  else b.classList.add("bg-slate-400");

  b.textContent = p.label;
  b.onclick = () => addEntry(p.id);
  buttonsContainer.appendChild(b);
});

/* ===== DADOS ===== */
let entries = [];
const entriesBody = document.getElementById("entriesBody");
const totalPeopleEl = document.getElementById("totalPeople");
const totalCollectedEl = document.getElementById("totalCollected");

/* ===== OLHO ===== */
function toggleTotalValue() {
  const hidden = totalCollectedEl.dataset.hidden === "true";
  totalCollectedEl.dataset.hidden = (!hidden).toString();
  totalCollectedEl.textContent = hidden
    ? totalCollectedEl.dataset.real
    : "••••••";
}

/* ===== RENDER ===== */
function render() {
  entriesBody.innerHTML = entries.map(e => `
    <tr class="border-t">
      <td class="p-2">${new Date(e.time).toLocaleTimeString()}</td>
      <td class="p-2">${e.label}</td>
      <td class="p-2 text-right">R$ ${e.price.toFixed(2)}</td>
      <td class="p-2 text-center">${e.people}</td>
      <td class="p-2">
        <button onclick="remove(${e.id})" class="text-red-600">Excluir</button>
      </td>
    </tr>
  `).join("");

  const totals = entries.reduce((a,e)=>{
    a.people += e.people;
    a.value += e.price;
    return a;
  },{people:0,value:0});

  totalPeopleEl.textContent = totals.people;

  const formatted = `R$ ${totals.value.toFixed(2)}`;
  totalCollectedEl.dataset.real = formatted;
  totalCollectedEl.textContent =
    totalCollectedEl.dataset.hidden === "true" ? "••••••" : formatted;
}

/* ===== ADD ===== */
function addEntry(id) {
  const t = PRICE_TYPES.find(p => p.id === id);
  entries.unshift({
    id: Date.now(),
    time: new Date(),
    label: t.label,
    price: t.price,
    people: t.people
  });
  render();
}

/* ===== REMOVE ===== */
function remove(id) {
  entries = entries.filter(e => e.id !== id);
  render();
}
</script>

</body>
</html>
