<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title id="siteTitle">Medela Supermercado</title>
    <style id="dynamicStyles">
        :root { --primary: #e53935; --admin-bg: #2c3e50; --bg: #f4f4f4; --whatsapp: #25d366; }
    </style>
    <style>
        * { box-sizing: border-box; }
        body { margin:0; font-family: 'Segoe UI', Arial, sans-serif; background: var(--bg); min-height: 100vh; }
        .tela { display:none; min-height: 100vh; width: 100%; background: var(--bg); }
        .ativa { display:block; }
        .container { padding: 15px; max-width: 600px; margin: auto; }
        .card { background: white; padding: 20px; border-radius: 12px; box-shadow: 0 2px 8px rgba(0,0,0,0.1); margin-bottom: 15px; }
        
        input, select, textarea { width: 100%; padding: 12px; margin: 8px 0; border: 1px solid #ddd; border-radius: 8px; font-size: 16px; }
        button { border: none; padding: 12px; border-radius: 8px; font-weight: bold; cursor: pointer; color: white; width: 100%; transition: 0.2s; }
        .btn-main { background: var(--primary); }
        .btn-whatsapp { background: var(--whatsapp); margin-top: 10px; display: flex; align-items: center; justify-content: center; gap: 8px; }
        .btn-remove { background: #ff5252; width: 30px; height: 30px; padding: 0; border-radius: 5px; }
        .btn-sec { background: #555; margin-top: 5px; }
        
        .header { background: var(--primary); color: white; padding: 15px; display: flex; justify-content: space-between; align-items: center; position: sticky; top: 0; z-index: 100; }
        
        .chat-screen { display: flex; flex-direction: column; height: 100vh; background: #e5ddd5; }
        .chat-messages { flex: 1; overflow-y: auto; padding: 15px; display: flex; flex-direction: column; gap: 8px; }
        .msg { max-width: 80%; padding: 10px; border-radius: 10px; font-size: 14px; }
        .msg.sent { align-self: flex-end; background: #dcf8c6; }
        .msg.received { align-self: flex-start; background: white; }
        .chat-footer { padding: 10px; background: #f0f0f0; display: flex; gap: 5px; }
        
        .cat-nav { display: flex; gap: 10px; overflow-x: auto; padding: 10px 0; }
        .cat-btn { background: white; border: 1px solid #ddd; padding: 8px 15px; border-radius: 20px; white-space: nowrap; width: auto; color: #333; }
        .cat-btn.active { background: var(--primary); color: white; }
        .grid { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; }

        .cart-float { position: fixed; bottom: 90px; right: 20px; width: 60px; height: 60px; border-radius: 50%; background: #000; color: white; display: flex; align-items: center; justify-content: center; font-size: 20px; cursor: pointer; z-index: 99; box-shadow: 0 4px 10px rgba(0,0,0,0.3); }
        .cart-count { position: absolute; top: 0; right: 0; background: var(--primary); color: white; border-radius: 50%; width: 22px; height: 22px; font-size: 12px; display: flex; align-items: center; justify-content: center; }
        
        .finance-box { display: flex; gap: 10px; margin-bottom: 15px; }
        .fin-card { flex: 1; padding: 10px; border-radius: 8px; color: white; text-align: center; }
        .total-info { font-size: 14px; color: #666; margin-bottom: 5px; text-align: right; }
    </style>
</head>
<body>

<div id="login" class="tela ativa">
    <div class="container" style="text-align:center; padding-top:60px;">
        <h1 id="brandName">🛒 Medela Supermercado</h1>
        <div class="card">
            <input id="lCpf" placeholder="CPF ou 'admin'">
            <input id="lSenha" type="password" placeholder="Senha">
            <button class="btn-main" onclick="entrar()">Entrar</button>
            <button class="btn-whatsapp" onclick="abrirSuporteVisitante()">💬 Suporte Chat</button>
            <p onclick="ir('cadastro')" style="color:var(--primary); cursor:pointer; margin-top:20px;">Criar Conta</p>
        </div>
    </div>
</div>

<div id="cadastro" class="tela">
    <div class="container">
        <h2>Criar Conta</h2>
        <input id="cNome" placeholder="Nome Completo">
        <input id="cCpf" placeholder="CPF">
        <input id="cSenha" type="password" placeholder="Senha">
        <button class="btn-main" onclick="registrar()">Finalizar Cadastro</button>
        <button onclick="ir('login')" style="background:#888; margin-top:10px;">Voltar</button>
    </div>
</div>

<div id="home" class="tela">
    <div class="header" id="headerCliente">
        <span id="welcome">Olá</span>
        <button style="width:auto; background:rgba(0,0,0,0.2)" onclick="voltarParaLogin()">Sair</button>
    </div>
    <div class="container">
        <button class="btn-sec" onclick="ir('pedidos_cliente')">📦 Meus Pedidos / Rastreio</button>
        <div class="cat-nav" id="catNav"></div>
        <div class="grid" id="listaLoja"></div>
    </div>
    <div class="cart-float" onclick="ir('carrinho')">
        🛒 <span class="cart-count" id="cartCount">0</span>
    </div>
    <button onclick="ir('chat')" title="Suporte" style="position:fixed; bottom:20px; right:20px; width:60px; height:60px; border-radius:50%; background:var(--whatsapp); border:none; font-size:25px; color:white; box-shadow: 0 4px 10px rgba(0,0,0,0.3);">💬</button>
</div>

<div id="pedidos_cliente" class="tela">
    <div class="header">
        <button onclick="ir('home')" style="width:auto; background:none; font-size:20px">←</button>
        <span>Meus Pedidos</span>
        <div style="width:30px"></div>
    </div>
    <div class="container" id="lista_pedidos_cliente">
        </div>
</div>

<div id="carrinho" class="tela">
    <div class="header">
        <button onclick="ir('home')" style="width:auto; background:none; font-size:20px">←</button>
        <span>Meu Carrinho</span>
        <div style="width:30px"></div>
    </div>
    <div class="container">
        <div id="itensCarrinho"></div>
        <div class="card" id="cardCheckout" style="display:none">
            <h3>Como deseja receber?</h3>
            <select id="tipoEntrega" onchange="renderizarCarrinho()">
                <option value="retirada">Retirar na Loja (Grátis)</option>
                <option value="entrega">Receber em Casa (+ R$ 7,00)</option>
            </select>
            <div id="boxEndereco" style="display:none">
                <textarea id="endEntrega" placeholder="Endereço Completo (Rua, Nº, Bairro)"></textarea>
            </div>
            <select id="formaPagto">
                <option value="">Forma de Pagamento</option>
                <option value="Pix">Pix</option>
                <option value="Cartão">Cartão (na entrega)</option>
                <option value="Dinheiro">Dinheiro</option>
            </select>
            <div class="total-info" id="txtSubtotal">Subtotal: R$ 0,00</div>
            <div class="total-info" id="txtTaxa">Entrega: R$ 0,00</div>
            <h2 id="totalCarrinho" style="text-align:right; margin-top:0;">Total: R$ 0,00</h2>
            <button class="btn-whatsapp" onclick="finalizarPedido(true)">🚀 Enviar via WhatsApp</button>
        </div>
    </div>
</div>

<div id="admin" class="tela">
    <div class="header" id="headerAdmin" style="background:var(--admin-bg)">
        <span>⚙️ Painel Gestor</span>
        <button style="width:auto; background:rgba(255,255,255,0.2)" onclick="voltarParaLogin()">Sair</button>
    </div>
    <div class="container">
        <div class="finance-box">
            <div class="fin-card" style="background:#2ecc71"><small>Faturamento</small><br><strong id="finBruto">R$ 0,00</strong></div>
            <div class="fin-card" style="background:#3498db"><small>Lucro Líquido</small><br><strong id="finLiquido">R$ 0,00</strong></div>
        </div>
        <div class="card">
            <h3>📦 Pedidos Realizados</h3>
            <div id="lista_pedidos_admin"></div>
        </div>
        <div class="card">
            <h3>💬 Conversas de Suporte</h3>
            <div id="lista_chats_admin"></div>
        </div>
        <div class="card">
            <h3>🎨 Configurações e Novo Produto</h3>
            <input id="cfgNome" placeholder="Nome do Mercado">
            <input id="cfgWhats" placeholder="WhatsApp (5521999999999)">
            <input type="color" id="cfgCor">
            <button onclick="salvarConfigSemRefresh()" class="btn-main">Salvar Site</button>
            <hr>
            <input id="pNome" placeholder="Nome do Produto">
            <input id="pCusto" type="number" step="0.01" placeholder="Custo (Líquido)">
            <input id="pVenda" type="number" step="0.01" placeholder="Venda (Bruto)">
            <select id="pCat">
                <option>Açougue</option><option>Bebida</option><option>Limpeza</option><option>Padaria</option><option>Mercearia</option>
            </select>
            <button onclick="addProduto()" class="btn-main" style="background:var(--admin-bg)">Cadastrar Produto</button>
        </div>
        <div class="card">
            <h3>👥 Clientes</h3>
            <div id="lista_usuarios_admin"></div>
        </div>
    </div>
</div>

<div id="chat" class="tela">
    <div class="chat-screen">
        <div class="header" style="background:#075e54">
            <button onclick="voltarDoChat()" style="width:auto; background:none; font-size:20px">←</button>
            <span id="chatTitulo">Suporte Medela</span>
            <div style="width:30px"></div>
        </div>
        <div class="chat-messages" id="box_msgs"></div>
        <div class="chat-footer">
            <input id="msg_input" placeholder="Dúvida ou problema?">
            <button onclick="enviarMensagem()" style="width:50px; border-radius:50%; background:var(--whatsapp); color:white">➤</button>
        </div>
    </div>
</div>

<script>
let usuarios = JSON.parse(localStorage.getItem("m_users")) || {};
let produtos = JSON.parse(localStorage.getItem("m_prod")) || [];
let config = JSON.parse(localStorage.getItem("m_cfg")) || { nome: "Medela Supermercado", cor: "#e53935", whats: "" };
let mensagens = JSON.parse(localStorage.getItem("m_chat")) || {}; 
let pedidos_db = JSON.parse(localStorage.getItem("m_pedidos")) || [];
let carrinho = [];
let taxaEntrega = 7.00;
let sessaoAtiva = ""; let clienteNoChat = ""; let modoAdminChat = false;

function aplicarEstilos() {
    document.getElementById("brandName").innerText = "🛒 " + config.nome;
    document.getElementById("siteTitle").innerText = config.nome;
    document.getElementById("dynamicStyles").innerHTML = `:root { --primary: ${config.cor}; --admin-bg: #2c3e50; --bg: #f4f4f4; --whatsapp: #25d366; }`;
    document.getElementById("headerCliente").style.backgroundColor = config.cor;
    document.getElementById("cfgWhats").value = config.whats || "";
}

function ir(tela) {
    document.querySelectorAll('.tela').forEach(t => t.classList.remove('ativa'));
    document.getElementById(tela).classList.add('ativa');
    if(tela === 'home') renderizarLoja();
    if(tela === 'admin') renderizarAdmin();
    if(tela === 'carrinho') renderizarCarrinho();
    if(tela === 'chat') renderizarMensagens();
    if(tela === 'pedidos_cliente') renderizarPedidosCliente();
    window.scrollTo(0,0);
}

function entrar() {
    let cpf = document.getElementById("lCpf").value;
    let senha = document.getElementById("lSenha").value;
    if(cpf === 'admin' && senha === 'admin123') { sessaoAtiva = "admin"; ir('admin'); return; }
    if(usuarios[cpf] && usuarios[cpf].senha === senha) { sessaoAtiva = cpf; ir('home'); }
    else { alert("Dados incorretos!"); }
}

function registrar() {
    let n = document.getElementById("cNome").value;
    let c = document.getElementById("cCpf").value;
    let s = document.getElementById("cSenha").value;
    if(!n || !c || !s) return alert("Preencha tudo");
    usuarios[c] = { nome: n, senha: s };
    localStorage.setItem("m_users", JSON.stringify(usuarios));
    ir('login');
}

/* CARRINHO E PEDIDOS */
function addAoCarrinho(nome, preco) {
    carrinho.push({ id: Date.now(), nome, preco });
    document.getElementById("cartCount").innerText = carrinho.length;
}

function removerDoCarrinho(idItem) {
    carrinho = carrinho.filter(i => i.id !== idItem);
    document.getElementById("cartCount").innerText = carrinho.length;
    renderizarCarrinho();
}

function renderizarCarrinho() {
    let box = document.getElementById("itensCarrinho");
    let subtotal = 0;
    let tipo = document.getElementById("tipoEntrega").value;
    if(carrinho.length === 0) {
        box.innerHTML = "<p style='text-align:center'>Vazio.</p>";
        document.getElementById("cardCheckout").style.display = "none";
    } else {
        box.innerHTML = carrinho.map((item) => {
            subtotal += item.preco;
            return `<div class="card" style="display:flex; justify-content:space-between; align-items:center">
                <div><b>${item.nome}</b><br>R$ ${item.preco.toFixed(2)}</div>
                <button class="btn-remove" onclick="removerDoCarrinho(${item.id})">✕</button>
            </div>`;
        }).join('');
        document.getElementById("cardCheckout").style.display = "block";
    }
    let taxa = tipo === "entrega" ? taxaEntrega : 0;
    document.getElementById("boxEndereco").style.display = tipo === "entrega" ? "block" : "none";
    document.getElementById("txtSubtotal").innerText = "Subtotal: R$ " + subtotal.toFixed(2);
    document.getElementById("txtTaxa").innerText = "Entrega: R$ " + taxa.toFixed(2);
    document.getElementById("totalCarrinho").innerText = "Total: R$ " + (subtotal + taxa).toFixed(2);
}

function finalizarPedido(viaWhats) {
    let tipo = document.getElementById("tipoEntrega").value;
    let endereco = document.getElementById("endEntrega").value;
    let pagamento = document.getElementById("formaPagto").value;
    if(tipo === "entrega" && !endereco) return alert("Informe o endereço!");
    if(!pagamento) return alert("Escolha o pagamento!");

    let total_final = 0;
    let itens_nomes = [];
    carrinho.forEach(i => { total_final += i.preco; itens_nomes.push(i.nome); });
    if(tipo === "entrega") total_final += taxaEntrega;

    let novoPedido = {
        id: "#" + Math.floor(Math.random() * 9000 + 1000),
        clienteCpf: sessaoAtiva,
        clienteNome: usuarios[sessaoAtiva]?.nome || "Visitante",
        itens: itens_nomes.join(", "),
        total: total_final,
        tipo: tipo,
        pagamento: pagamento,
        endereco: tipo === "entrega" ? endereco : "Retirada na Loja",
        data: new Date().toLocaleString()
    };

    pedidos_db.unshift(novoPedido);
    localStorage.setItem("m_pedidos", JSON.stringify(pedidos_db));

    let resumo = `🛒 *PEDIDO ${novoPedido.id}*\n\n▪️ Itens: ${novoPedido.itens}\n💰 *TOTAL: R$ ${novoPedido.total.toFixed(2)}*\n📦 Tipo: ${tipo}\n💳 Pgto: ${pagamento}\n📍 Local: ${novoPedido.endereco}`;
    
    if(viaWhats) {
        window.open(`https://api.whatsapp.com/send?phone=${config.whats}&text=${encodeURIComponent(resumo)}`, '_blank');
    }

    carrinho = []; document.getElementById("cartCount").innerText = "0";
    alert("Pedido Realizado com Sucesso!");
    ir('pedidos_cliente');
}

function renderizarPedidosCliente() {
    let lista = pedidos_db.filter(p => p.clienteCpf === sessaoAtiva);
    document.getElementById("lista_pedidos_cliente").innerHTML = lista.map(p => `
        <div class="card">
            <div style="display:flex; justify-content:space-between"><strong>Pedido ${p.id}</strong> <span style="color:var(--primary)">Pendente</span></div>
            <small>${p.data}</small><br>
            <p>${p.itens}</p>
            <div style="text-align:right"><strong>Total: R$ ${p.total.toFixed(2)}</strong></div>
        </div>
    `).join('') || "<p style='text-align:center'>Nenhum pedido encontrado.</p>";
}

/* ADMIN */
function renderizarAdmin() {
    document.getElementById("lista_pedidos_admin").innerHTML = pedidos_db.map(p => `
        <div style="border-bottom:1px solid #eee; padding:10px 0">
            <b>${p.id} - ${p.clienteNome}</b><br>
            ${p.itens}<br>
            <small>${p.endereco} | ${p.pagamento}</small><br>
            <strong>R$ ${p.total.toFixed(2)}</strong>
        </div>
    `).join('') || "Sem pedidos.";

    document.getElementById("lista_chats_admin").innerHTML = Object.keys(mensagens).map(c => `
        <button onclick="abrirChatAdmin('${c}')" style="background:#eee; color:#333; margin-bottom:5px; text-align:left">💬 Chat: ${usuarios[c]?.nome || c}</button>
    `).join('') || "Sem conversas.";

    let bruto = 0, custo = 0;
    pedidos_db.forEach(p => bruto += p.total);
    produtos.forEach(p => custo += (p.custo || 0));
    document.getElementById("finBruto").innerText = "R$ " + bruto.toFixed(2);
    document.getElementById("finLiquido").innerText = "R$ " + (bruto - custo).toFixed(2);
    
    document.getElementById("lista_usuarios_admin").innerHTML = Object.keys(usuarios).map(c => `
        <div style="font-size:12px">${usuarios[c].nome} | ${c} | Senha: ${usuarios[c].senha}</div>
    `).join('');
}

function addProduto() {
    let n = document.getElementById("pNome").value;
    let cus = parseFloat(document.getElementById("pCusto").value);
    let ven = parseFloat(document.getElementById("pVenda").value);
    let cat = document.getElementById("pCat").value;
    if(!n || isNaN(ven)) return;
    produtos.push({nome: n, preco: ven, custo: cus, cat: cat});
    localStorage.setItem("m_prod", JSON.stringify(produtos));
    renderizarAdmin();
}

function salvarConfigSemRefresh() {
    config.nome = document.getElementById("cfgNome").value || config.nome;
    config.cor = document.getElementById("cfgCor").value;
    config.whats = document.getElementById("cfgWhats").value;
    localStorage.setItem("m_cfg", JSON.stringify(config));
    aplicarEstilos(); alert("Configurações Salvas!");
}

function voltarParaLogin() { sessaoAtiva = ""; ir('login'); }

/* LOJA E CHAT */
function renderizarLoja() {
    document.getElementById("welcome").innerText = "Olá, " + (usuarios[sessaoAtiva]?.nome || "Cliente");
    const cats = ["Todos", "Açougue", "Bebida", "Limpeza", "Padaria", "Mercearia"];
    document.getElementById("catNav").innerHTML = cats.map(c => `<button class="cat-btn" onclick="renderizarFiltrado('${c}')">${c}</button>`).join('');
    renderizarFiltrado("Todos");
}

function renderizarFiltrado(cat) {
    let lista = cat === "Todos" ? produtos : produtos.filter(p => p.cat === cat);
    document.getElementById("listaLoja").innerHTML = lista.map(p => `
        <div class="card" style="text-align:center">
            <strong>${p.nome}</strong><br><span style="color:green">R$ ${p.preco.toFixed(2)}</span><br>
            <button class="btn-main" onclick="addAoCarrinho('${p.nome}', ${p.preco})" style="margin-top:10px; font-size:12px">Adicionar</button>
        </div>`).join('') || "Vazio.";
}

function abrirSuporteVisitante() {
    sessaoAtiva = "visitante_" + Math.floor(Math.random()*999);
    clienteNoChat = sessaoAtiva; modoAdminChat = false;
    ir('chat');
}

function abrirChatAdmin(cpf) {
    clienteNoChat = cpf; modoAdminChat = true;
    ir('chat');
}

function enviarMensagem() {
    let input = document.getElementById("msg_input");
    if(!input.value) return;
    let id = modoAdminChat ? clienteNoChat : sessaoAtiva;
    if(!mensagens[id]) mensagens[id] = [];
    mensagens[id].push({ texto: input.value, autor: modoAdminChat ? 'admin' : 'cliente' });
    localStorage.setItem("m_chat", JSON.stringify(mensagens));
    input.value = ""; renderizarMensagens();
}

function renderizarMensagens() {
    let id = modoAdminChat ? clienteNoChat : sessaoAtiva;
    let msgs = mensagens[id] || [];
    document.getElementById("box_msgs").innerHTML = msgs.map(m => `
        <div class="msg ${m.autor === (modoAdminChat ? 'admin' : 'cliente') ? 'sent' : 'received'}">${m.texto}</div>
    `).join('');
    document.getElementById("box_msgs").scrollTop = 9999;
