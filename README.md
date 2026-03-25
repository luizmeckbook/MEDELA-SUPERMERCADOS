<!PLANETA AGUA>
<html>
<head>
  <title>Água e Gelo do Jorge</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">

  <style>
    body {
      margin: 0;
      font-family: Arial;
      background: linear-gradient(to bottom, #0a0a0a, #001f2f);
      color: white;
      text-align: center;
    }

    header {
      padding: 30px;
      animation: fadeDown 1s ease;
    }

    h1 {
      font-size: 28px;
    }

    img {
      width: 90%;
      max-width: 300px;
      border-radius: 15px;
      margin: 20px 0;
      animation: fadeUp 1.5s ease;
    }

    .card {
      background: #111;
      margin: 15px;
      padding: 20px;
      border-radius: 15px;
      animation: fadeUp 2s ease;
    }

    button {
      padding: 15px 25px;
      background: #00bfff;
      color: black;
      border: none;
      border-radius: 10px;
      font-size: 16px;
      margin-top: 20px;
      transition: 0.3s;
    }

    button:hover {
      transform: scale(1.1);
      background: #00ffff;
    }

    footer {
      margin-top: 30px;
      padding: 20px;
      font-size: 14px;
      opacity: 0.7;
      animation: fadeUp 2.5s ease;
    }

    /* ANIMAÇÕES */
    @keyframes fadeDown {
      from {
        opacity: 0;
        transform: translateY(-30px);
      }
      to {
        opacity: 1;
        transform: translateY(0);
      }
    }

    @keyframes fadeUp {
      from {
        opacity: 0;
        transform: translateY(40px);
      }
      to {
        opacity: 1;
        transform: translateY(0);
      }
    }

  </style>
</head>

<body>

<header>
  <h1>Água e Gelo do Jorge 🧊</h1>
  <p>Entrega rápida • Qualidade garantida</p>
</header>

<img src="https://images.unsplash.com/photo-1604908177522-432f2b2c7e0d" alt="Gelo">

<div class="card">
  <h2>💰 Nossos Produtos</h2>
  <p>🧊 Saco de gelo – R$8,00</p>
  <p>💧 Água mineral – R$7,00</p>
</div>

<div class="card">
  <h2>🚚 Entrega</h2>
  <p>Entrega rápida na sua região</p>
  <p>Peça direto pelo WhatsApp</p>
</div>

<a href="https://wa.me/+55 21 99242-9919">
  <button>📲 Fazer Pedido Agora</button>
</a>

<footer>
  © 2026 Água e Gelo do Jorge
</footer>

</body>
</html>
