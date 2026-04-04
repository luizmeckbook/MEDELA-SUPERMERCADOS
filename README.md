<!DOCTYPE html>
<html lang="pt-br">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Mercado Medela</title>

<style>
body {
    margin: 0;
    font-family: Arial, sans-serif;
    background: #f5f5f5;
}

header {
    background: #000;
    color: #fff;
    padding: 15px;
    text-align: center;
}

.banner {
    background: url('https://images.unsplash.com/photo-1600891964599-f61ba0e24092') center/cover;
    height: 150px;
}

.container {
    padding: 15px;
}

h2 {
    margin-top: 20px;
}

.produto {
    background: #fff;
    border-radius: 10px;
    padding: 10px;
    margin-bottom: 10px;
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.produto img {
    width: 80px;
    border-radius: 10px;
}

.info {
    flex: 1;
    margin-left: 10px;
}

button {
    background: #000;
    color: #fff;
    border: none;
    padding: 8px;
    border-radius: 5px;
}

.carrinho {
    position: fixed;
    bottom: 0;
    width: 100%;
    background: #000;
    color: #fff;
    padding: 10px;
    display: flex;
    justify-content: space-between;
}

</style>
</head>

<body>

<header>
<h1>🛒 Mercado Medela</h1>
<p>Entrega rápida 🚚</p>
</header>

<div class="banner"></div>

<div class="container">

<h2>🥩 Açougue</h2>

<div class="produto">
    <img src="https://images.unsplash.com/photo-1603048297172-c92544798d5a">
    <div class="info">
        <b>Picanha</b>
        <p>R$ 39,90/kg</p>
    </div>
    <button onclick="add('Picanha - 39.90')">+</button>
</div>

<div class="produto">
    <img src="https://images.unsplash.com/photo-1551183053-bf91a1d81141">
    <div class="info">
        <b>Frango</b>
        <p>R$ 12,00/kg</p>
    </div>
    <button onclick="add('Frango - 12.00')">+</button>
</div>

<h2>🥤 Bebidas</h2>

<div class="produto">
    <img src="https://images.unsplash.com/photo-1580910051074-3eb694886505">
    <div class="info">
        <b>Coca-Cola</b>
        <p>R$ 6,00</p>
    </div>
    <button onclick="add('Coca-Cola - 6.00')">+</button>
</div>

</div>

<div class="carrinho">
    <span id="total">0 itens</span>
    <button onclick="finalizar()">Finalizar Pedido</button>
</div>

<script>
let carrinho = [];

function add(item) {
    carrinho.push(item);
    document.getElementById('total').innerText = carrinho.length + " itens";
}

function finalizar() {
    let texto = "Pedido:%0A";
    carrinho.forEach(i => texto += "- " + i + "%0A");

    window.open("https://wa.me/SEUNUMERO?text=" + texto);
}
</script>

</body>
</html>
