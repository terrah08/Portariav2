<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>Painel Portaria</title>
<script src="https://cdn.tailwindcss.com"></script>
</head>

<body class="bg-gray-100 p-4">

<div class="max-w-4xl mx-auto bg-white p-5 rounded-xl shadow">

    <h1 class="text-2xl font-bold mb-4 text-center">Painel da Portaria</h1>

    <!-- Data -->
    <div class="mb-4 flex gap-3 items-center">
        <label class="font-semibold">Data:</label>
        <input type="date" id="currentDate" class="border p-2 rounded" />
    </div>

    <!-- Anotação -->
    <textarea id="note" placeholder="Observação (opcional)" class="w-full border p-2 rounded mb-4"></textarea>

    <!-- Botões -->
    <div id="buttonsContainer" class="grid grid-cols-2 sm:grid-cols-3 md:grid-cols-4 lg:grid-cols-5 gap-3 mb-6"></div>

    <!-- Ações -->
    <div class="flex gap-3 mb-4">
        <button id="saveDay" class="px-4 py-2 bg-blue-600 text-white rounded">Salvar</button>
        <button id="exportCSV" class="px-4 py-2 bg-green-600 text-white rounded">Exportar CSV</button>
        <button id="resetDay" class="px-4 py-2 bg-red-600 text-white rounded">Resetar Dia</button>
    </div>

    <!-- Totais -->
    <div class="mb-4">
        <p>Total de Pessoas: <strong id="totalPeople">0</strong></p>
        <p>Total Arrecadado: <strong id="totalCollected">R$ 0,00</strong></p>
    </div>

    <!-- Tabela -->
    <div class="overflow-x-auto">
        <table class="w-full border">
            <thead class="bg-gray-200">
            <tr>
                <th class="p-2">Hora</th>
                <th class="p-2">Tipo</th>
                <th class="p-2">Valor</th>
                <th class="p-2">Pessoas</th>
                <th class="p-2">Observação</th>
                <th class="p-2">Ações</th>
            </tr>
            </thead>
            <tbody id="entriesBody"></tbody>
        </table>
    </div>
</div>

<script>
/* ----------------------------------------------
   CONFIGURAÇÃO DOS TIPOS DE ENTRADAS
---------------------------------------------- */
const PRICE_TYPES = [
    { id: "20", label: "Dinheiro - R$20 💸", price: 20, people: 1, color: "bg-yellow-400 text-gray-900" },
    { id: "30", label: "Dinheiro - R$30 💸", price: 30, people: 1, color: "bg-yellow-400 text-gray-900" },
    { id: "50", label: "Dinheiro - R$50 💸", price: 50, people: 2, color: "bg-yellow-400 text-gray-900" },

    { id: "20_credito", label: "Crédito - R$20 💳", price: 20, people: 1, color: "bg-green-500 text-white" },
    { id: "30_credito", label: "Crédito - R$30 💳", price: 30, people: 1, color: "bg-green-500 text-white" },
    { id: "50_credito", label: "Crédito - R$50 💳", price: 50, people: 2, color: "bg-green-500 text-white" },

    { id: "20_debito", label: "Débito - R$20 💳", price: 20, people: 1, color: "bg-blue-500 text-white" },
    { id: "30_debito", label: "Débito - R$30 💳", price: 30, people: 1, color: "bg-blue-500 text-white" },
    { id: "50_debito", label: "Débito - R$50 💳", price: 50, people: 2, color: "bg-blue-500 text-white" },

    { id: "20_pix", label: "Pix - R$20 ❖", price: 20, people: 1, color: "bg-gray-600 text-white" },
    { id: "30_pix", label: "Pix - R$30 ❖", price: 30, people: 1, color: "bg-gray-600 text-white" },
    { id: "50_pix", label: "Pix - R$50 ❖", price: 50, people: 2, color: "bg-gray-600 text-white" },

    { id: "free", label: "Lista (Free)", price: 0, people: 1, color: "bg-gray-200 text-gray-800" },
    { id: "militar", label: "Militar", price: 0, people: 1, color: "bg-gray-200 text-gray-800" },
    { id: "aniversario", label: "Aniversário", price: 0, people: 1, color: "bg-gray-200 text-gray-800" }
];

const todayKey = () => new Date().toISOString().slice(0, 10);
const storageKey = (d) => `portaria_${d}`;

let entries = [];
let currentDate = todayKey();

/* ELEMENTOS */
const currentDateEl = document.getElementById("currentDate");
const entriesBody = document.getElementById("entriesBody");
const totalPeopleEl = document.getElementById("totalPeople");
const totalCollectedEl = document.getElementById("totalCollected");
const noteEl = document.getElementById("note");
const buttonsContainer = document.getElementById("buttonsContainer");

/* ----------------------------------------------
   FUNÇÕES PRINCIPAIS
---------------------------------------------- */
function loadEntries() {
    const raw = localStorage.getItem(storageKey(currentDate));
    entries = raw ? JSON.parse(raw) : [];
    renderEntries();
}

function saveEntries() {
    localStorage.setItem(storageKey(currentDate), JSON.stringify(entries));
}

function addEntry(id) {
    const t = PRICE_TYPES.find(p => p.id === id);
    if (!t) return;

    entries.unshift({
        id: Date.now(),
        timestamp: new Date().toISOString(),
        type: t.label,
        price: t.price,
        people: t.people,
        note: noteEl.value.trim(),
    });

    noteEl.value = "";
    saveEntries();
    renderEntries();
}

function deleteEntry(id) {
    if (!confirm("Deseja realmente excluir este registro?")) return;
    entries = entries.filter(e => e.id !== id);
    saveEntries();
    renderEntries();
}

function editEntry(id) {
    const e = entries.find(x => x.id === id);
    if (!e) return;

    const newNote = prompt("Editar anotação:", e.note || "");
    if (newNote === null) return;

    e.note = newNote.trim();
    saveEntries();
    renderEntries();
}

function renderEntries() {
    entriesBody.innerHTML = entries.length
        ? entries.map(e => `
            <tr class="border-t">
                <td class="p-2">${new Date(e.timestamp).toLocaleTimeString()}</td>
                <td class="p-2">${e.type}</td>
                <td class="p-2">R$ ${e.price.toFixed(2)}</td>
                <td class="p-2">${e.people}</td>
                <td class="p-2">${e.note || ""}</td>
                <td class="p-2 flex gap-2">
                    <button onclick="editEntry(${e.id})" class="px-2 py-1 bg-blue-500 text-white rounded text-xs">Editar</button>
                    <button onclick="deleteEntry(${e.id})" class="px-2 py-1 bg-red-500 text-white rounded text-xs">Excluir</button>
                </td>
            </tr>
        `).join("")
        : `<tr><td colspan="6" class="text-center py-4 text-gray-400">Nenhum registro</td></tr>`;

    const totals = entries.reduce((a, e) => {
        a.people += e.people;
        a.collected += e.price;
        return a;
    }, { people: 0, collected: 0 });

    totalPeopleEl.textContent = totals.people;
    totalCollectedEl.textContent = "R$ " + totals.collected.toFixed(2);
}

/* ----------------------------------------------
    GERAR BOTÕES
---------------------------------------------- */
PRICE_TYPES.forEach(p => {
    const btn = document.createElement("button");
    btn.className = `p-3 rounded-xl border shadow ${p.color} text-center text-xs font-semibold hover:scale-105 transition`;
    btn.innerHTML = `${p.label}`;
    btn.onclick = () => addEntry(p.id);
    buttonsContainer.appendChild(btn);
});

/* ----------------------------------------------
   AÇÕES
---------------------------------------------- */
document.getElementById("saveDay").onclick = saveEntries;
document.getElementById("resetDay").onclick = () => {
    if (confirm("Deseja realmente limpar o dia?")) {
        entries = [];
        saveEntries();
        renderEntries();
    }
};

document.getElementById("exportCSV").onclick = () => {
    const headers = ["timestamp", "type", "price", "people", "note"];
    const rows = entries.map(e => [e.timestamp, e.type, e.price, e.people, e.note || ""]);

    const csv = [headers, ...rows]
        .map(r => r.map(v => `"${String(v).replace(/"/g, '""')}"`).join(","))
        .join("\n");

    const blob = new Blob([csv], { type: "text/csv;charset=utf-8;" });
    const url = URL.createObjectURL(blob);

    const a = document.createElement("a");
    a.href = url;
    a.download = `portaria_${currentDate}.csv`;
    a.click();
    URL.revokeObjectURL(url);
};

currentDateEl.value = currentDate;
currentDateEl.onchange = e => {
    currentDate = e.target.value;
    loadEntries();
};

loadEntries();
</script>

</body>
</html>
