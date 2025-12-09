<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>Rifa - Pagando minha Faculdade</title>

<style>
    body {
        font-family: Arial, sans-serif;
        background: #0a0a0a;
        color: #fff;
        margin: 0;
        padding: 20px;
    }

    h1, h2 {
        text-align: center;
        color: #00aaff;
    }

    .premio h2 {
        color: #00ff88;
        text-shadow: 0 0 10px #00ff88, 0 0 20px #00ff88, 0 0 30px #00ff88;
        animation: brilho 1.8s infinite alternate ease-in-out;
    }

    @keyframes brilho {
        0% { text-shadow: 0 0 8px #00ff88; }
        100% { text-shadow: 0 0 18px #00ff88; }
    }

    .premio {
        background: #111;
        padding: 20px;
        border-radius: 12px;
        margin-bottom: 25px;
        text-align: center;
        border: 2px solid #00ff88;
        box-shadow: 0px 0px 15px #00ff88;
    }

    .pix-box {
        background: #111;
        padding: 20px;
        border-radius: 12px;
        margin-bottom: 25px;
        border: 2px solid #0f0;
        box-shadow: 0px 0px 15px #0f0;
        user-select: none;
        pointer-events: none;
    }

    .form {
        background: #111;
        padding: 20px;
        border-radius: 12px;
        border: 2px solid #555;
        margin-bottom: 25px;
    }

    .numeros {
        display: grid;
        grid-template-columns: repeat(auto-fill, minmax(60px, 1fr));
        gap: 8px;
        margin-top: 20px;
    }

    .numero {
        padding: 10px;
        background: #222;
        border-radius: 6px;
        text-align: center;
        cursor: pointer;
        border: 1px solid #444;
    }

    .numero:hover {
        background: #00aaff;
    }

    .selecionado {
        background: #ffaa00 !important;
        color: #000;
        font-weight: bold;
    }

    .vendido {
        background: red !important;
        color: #fff;
        pointer-events: none;
        font-weight: bold;
    }

    button {
        width: 100%;
        padding: 15px;
        background: #00aaff;
        border: none;
        color: #000;
        font-size: 18px;
        border-radius: 10px;
        cursor: pointer;
        margin-top: 20px;
        font-weight: bold;
    }

    button:hover {
        background: #0088cc;
    }
</style>
</head>
<body>

<h1>🎉 Rifa – Pagando minha Faculdade 🎓</h1>

<div class="premio">
    <h2>💰 Prêmio: R$ 1.000,00</h2>
    <p>Participando, você me ajuda diretamente a pagar minha faculdade. Obrigado! 🙏</p>
    <h3>📅 Sorteio no Instagram: <strong>@jobsondrums</strong></h3>
</div>

<div class="pix-box">
    <h2>💸 Pagamento via PIX</h2>
    <p><strong>Chave PIX (CPF):</strong></p>
    <p style="font-size: 26px; font-weight: bold;">114.669.621-35</p>
    <p>Valor por número: <strong>R$ 10,00</strong></p>
    <p>Essa área é bloqueada para garantir autenticidade.</p>
</div>

<div class="form">
    <h2>🧍 Dados do Participante</h2>

    <label>Nome completo:</label><br>
    <input type="text" id="nome" style="width:100%; padding:10px; margin-bottom:10px;"><br>

    <label>Cidade / Estado:</label><br>
    <input type="text" id="cidade" style="width:100%; padding:10px; margin-bottom:10px;"><br>

    <label>Telefone:</label><br>
    <input type="text" id="telefone" style="width:100%; padding:10px; margin-bottom:10px;"><br>

    <h2>🔢 Escolha seus números</h2>
    <div class="numeros" id="lista"></div>

    <button onclick="finalizar()">Finalizar Compra</button>
</div>

<script>
let selecionados = [];

// CARREGAR NÚMEROS VENDIDOS DO LOCALSTORAGE
let vendidos = JSON.parse(localStorage.getItem("numerosVendidos")) || [];

// GERAR 300 NÚMEROS
const lista = document.getElementById("lista");
for (let i = 1; i <= 300; i++) {
    const div = document.createElement("div");
    div.className = "numero";
    div.innerText = i;

    // MARCAR VENDIDOS
    if (vendidos.includes(i)) {
        div.classList.add("vendido");
    }

    div.onclick = function () {
        if (div.classList.contains("vendido")) return;

        if (div.classList.contains("selecionado")) {
            div.classList.remove("selecionado");
            selecionados = selecionados.filter(n => n !== i);
        } else {
            div.classList.add("selecionado");
            selecionados.push(i);
        }
    };

    lista.appendChild(div);
}

function finalizar() {
    const nome = document.getElementById("nome").value;
    const cidade = document.getElementById("cidade").value;
    const telefone = document.getElementById("telefone").value;

    if (!nome || !cidade || !telefone) {
        alert("Preencha seus dados antes de finalizar.");
        return;
    }

    if (selecionados.length === 0) {
        alert("Selecione ao menos 1 número.");
        return;
    }

    // MARCAR COMO VENDIDOS E SALVAR
    selecionados.forEach(num => {
        vendidos.push(num);
    });

    localStorage.setItem("numerosVendidos", JSON.stringify(vendidos));

    const msg = encodeURIComponent(
        `Olá! Quero confirmar minha participação na rifa.\n\n` +
        `👤 Nome: ${nome}\n` +
        `📍 Cidade: ${cidade}\n` +
        `📞 Telefone: ${telefone}\n\n` +
        `🎟 Números escolhidos: ${selecionados.join(", ")}\n` +
        `💰 Total: R$ ${selecionados.length * 10},00`
    );

    window.location.href = `https://wa.me/5563999467072?text=${msg}`;

    selecionados = [];
    location.reload();
}
</script>

</body>
</html>
