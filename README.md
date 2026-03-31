<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Medela Supermercado - App PDV</title>
    <style>
        :root { --primary: #e53935; --admin-bg: #2c3e50; --bg: #f4f4f4; --whatsapp: #25d366; --caixa: #f39c12; }
        * { box-sizing: border-box; font-family: 'Segoe UI', Arial, sans-serif; -webkit-tap-highlight-color: transparent; }
        body { margin: 0; background: var(--bg); color: #333; }
        
        .tela { display: none; min-height: 100vh; width: 100%; }
        .ativa { display: block !important; }
        .container { max-width: 500px; margin: auto; padding: 15px; }
        .card { background: white; padding: 20px; border-radius: 15px; box-shadow: 0 4px 12px rgba(0,0,0,0.1); margin-bottom: 15px; }
        
        input, select, textarea { width: 100%; padding: 12px; margin: 8px 0; border: 1px solid #ccc; border-radius: 8px; font-size: 16px; outline: none; }
        button { width: 100%; padding: 14px; border: none; border-radius: 10px; font-weight: bold; cursor: pointer; color: white; margin-top: 5px; }
        button:active { transform: scale(0.96); opacity: 0.8; }
        
        .btn-main { background: var(--primary); }
        .btn-sec { background: var(--admin-bg); }
        .btn-caixa { background: var(--caixa); }
        .btn-whats { background: var(--whatsapp); display: flex; align-items: center; justify-content: center; gap: 10px; }
        .btn-del { background: #ff5252; width: auto; padding: 5px 10px; font-size: 12px; }

        .header { background: var(--primary); color: white; padding: 15px; display: flex; justify-content: space-between; align-items: center; position: sticky; top: 0; z-index: 100; height: 60px; }
        
        .cat-bar { display: flex; gap: 8px; overflow-x: auto; padding: 10px 0; margin-bottom: 10px; scrollbar-width: none; }
        .cat-item { background: white; padding: 8px 15px; border-radius: 20px; font-size: 13px; white-space: nowrap; border: 1px solid #ddd; cursor: pointer; }
        .cat-active { background: var(--primary); color: white; border-color: var(--primary); }

        .grid { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; }
        .prod-card { background: white; padding: 15px; border-radius: 12px; text-align: center; border: 1px solid #eee; }
        
        .tab-menu { display: flex; background: #fff; margin-bottom: 15px; border-radius: 10px; overflow: hidden; border: 1px solid #ddd; }
        .tab-btn { flex: 1; padding: 12px; background: #f9f9f9; color: #333; border-radius: 0; margin: 0; font-size: 12px; }
        .tab-btn.active { background: var(--admin-bg); color: white; }
    </style>
</head>
<body>

<div id="tela_login" class="tela ativa">
    <div class="container" style="padding-top: 50px; text-align: center;">
        <h1>🛒 Medela App</h1>
        <div class="card">
            <input type="text" id="login_cpf" placeholder="CPF, admin ou caixa">
            <input type="password" id="login_senha" placeholder="Senha">
            <button type="button" class="btn-main" onclick="funcaoLogin(event)">ENTRAR NO SISTEMA</button>
            <button type="button" class="btn-whats" onclick="alert('Suporte via WhatsApp acionado!')">💬 SUPORTE</button>
            <p onclick="irPara('tela_cadastro')" style="margin-top:20px; color:var(--primary); cursor:pointer;">Criar Conta Cliente</p>
        </div>
    </div>
</div>

<div id="tela_cadastro" class="tela">
    <div class="container">
        <h2>Cadastro</h2>
        <div class="card">
            <input id="cad_nome" placeholder="Nome Completo">
            <input id="cad_cpf" placeholder="CPF">
            <input id="cad_senha" type="password" placeholder="Senha">
            <button type="button" class="btn-main" onclick="funcaoCadastro(event)">CADASTRAR</button>
            <button type="button" class="btn-sec" onclick="irPara('tela_login')">VOLTAR</button>
        </div>
    </div>
</div>

<div id="tela_home" class="tela">
    <div class="header">
        <span id="txt_boas_vindas">Olá!</span>
        <button type="button" onclick="logout()" style="width:auto; margin:0; background:rgba(0,0,0,0.2); padding:8px 15px">Sair</button>
    </div>
    <div class="container">
        <div class="cat-bar" id="cliente_cat_bar"></div>
        <div class="grid" id="lista_produtos"></div>
    </div>
</div>

<div id="tela_caixa" class="tela">
    <div class="header" style="background:var(--caixa)">
        <span>PDV - CAIXA ABERTO</span>
        <button type="button" onclick="logout()" style="width:auto; margin:0; background:rgba(0,0,0,0.2)">Sair</button>
    </div>
    <div class="container">
        <div class="card">
            <h3>Nova Venda</h3>
            <select id="caixa_select_prod"></select>
            <button type="button" class="btn-caixa" onclick="caixaAddProd()">+ ADICIONAR ITEM</button>
        </div>
        
        <div id="caixa_carrinho_lista" class="card">
            <h4>Itens da Venda</h4>
            <div id="caixa_itens"></div>
            <hr>
            <input type="number" id="caixa_desconto" placeholder="Desconto em R$" oninput="caixaAtualizarLista()">
            <select id="caixa_pgto">
                <option value="Dinheiro">Dinheiro</option>
                <option value="Pix">Pix</option>
                <option value="Cartão">Cartão de Crédito/Débito</option>
            </select>
            <h2 id="caixa_total_exibição">Total: R$ 0,00</h2>
            <button type="button" class="btn-main" onclick="caixaFinalizar()">FECHAR VENDA (IMPRIMIR)</button>
        </div>

        <div class="card">
            <h4>Vendas do Turno</h4>
            <div id="caixa_vendas_dia" style="font-size:12px"></div>
        </div>
    </div>
</div>

<div id="tela_admin" class="tela">
    <div class="header" style="background:var(--admin-bg)">
        <span>PAINEL GERAL</span>
        <button type="button" onclick="logout()" style="width:auto; margin:0; background:rgba(255,255,255,0.1)">Sair</button>
    </div>
    <div class="container">
        <div class="tab-menu">
            <button class="tab-btn active" onclick="mudarTab('tab_fin', this)">Balanço</button>
            <button class="tab-btn" onclick="mudarTab('tab_prod', this)">Produtos</button>
            <button class="tab-btn" onclick="mudarTab('tab_hist', this)">Histórico</button>
        </div>

        <div id="tab_fin" class="tab-content">
            <div style="display:flex; gap:10px; margin-bottom:15px;">
                <div style="flex:1; background:#2ecc71; color:white; padding:15px; border-radius:10px; text-align:center;">
                    <small>BRUTO</small><br><strong id="adm_bruto">R$ 0,00</strong>
                </div>
                <div style="flex:1; background:#3498db; color:white; padding:15px; border-radius:10px; text-align:center;">
                    <small>LÍQUIDO</small><br><strong id="adm_liquido">R$ 0,00</strong>
                </div>
            </div>
        </div>

        <div id="tab_prod" class="tab-content" style="display:none">
            <div class="card">
                <h3>Novo Produto</h3>
                <input id="adm_p_nome" placeholder="Nome do Produto">
                <select id="adm_p_cat">
                    <option>Mercearia</option><option>Açougue</option><option>Bebidas</option>
                    <option>Hortifruti</option><option>Limpeza</option><option>Padaria</option>
                </select>
                <input id="adm_p_custo" type="number" step="0.01" placeholder="Custo (Líquido)">
                <input id="adm_p_venda" type="number" step="0.01" placeholder="Venda (Bruto)">
                <button type="button" class="btn-main" onclick="admAddProduto(event)">SALVAR</button>
            </div>
        </div>

        <div id="tab_hist" class="tab-content" style="display:none">
            <div id="adm_lista_pedidos"></div>
        </div>
    </div>
</div>

<script>
let db = {
    users: JSON.parse(localStorage.getItem("db_users")) || {},
    prods: JSON.parse(localStorage.getItem("db_prods")) || [],
    pedidos: JSON.parse(localStorage.getItem("db_pedidos")) || [],
    cats: ["Todos", "Mercearia", "Açougue", "Bebidas", "Hortifruti", "Limpeza", "Padaria"]
};

function salvar() { localStorage.setItem("db_users", JSON.stringify(db.users)); localStorage.setItem("db_prods", JSON.stringify(db.prods)); localStorage.setItem("db_pedidos", JSON.stringify(db.pedidos)); }

let sessao = ""; 
let carrinhoCaixa = []; 
let filtroCat = "Todos";

function irPara(id) {
    document.querySelectorAll('.tela').forEach(t => t.classList.remove('ativa'));
    document.getElementById(id).classList.add('ativa');
    if(id === 'tela_home') rendHome();
    if(id === 'tela_admin') rendAdmin();
    if(id === 'tela_caixa') rendCaixa();
}

function mudarTab(id, btn) {
    document.querySelectorAll('.tab-content').forEach(c => c.style.display = 'none');
    document.querySelectorAll('.tab-btn').forEach(b => b.classList.remove('active'));
    document.getElementById(id).style.display = 'block';
    btn.classList.add('active');
}

function funcaoLogin(e) {
    e.preventDefault();
    const c = document.getElementById("login_cpf").value;
    const s = document.getElementById("login_senha").value;
    if(c === 'admin' && s === 'admin123') { sessao = "admin"; irPara('tela_admin'); }
    else if(c === 'caixa' && s === 'caixa123') { sessao = "caixa"; irPara('tela_caixa'); }
    else if(db.users[c] && db.users[c].senha === s) { sessao = c; irPara('tela_home'); }
    else alert("Acesso Negado!");
}

function funcaoCadastro(e) {
    e.preventDefault();
    db.users[document.getElementById("cad_cpf").value] = { nome: document.getElementById("cad_nome").value, senha: document.getElementById("cad_senha").value };
    salvar(); alert("Cadastrado!"); irPara('tela_login');
}

// --- LOGICA CLIENTE ---
function rendHome() {
    document.getElementById("txt_boas_vindas").innerText = "Olá, " + (db.users[sessao]?.nome || "Cliente");
    const bar = document.getElementById("cliente_cat_bar");
    bar.innerHTML = db.cats.map(c => `<div class="cat-item ${filtroCat===c?'cat-active':''}" onclick="setFiltro('${c}')">${c}</div>`).join('');
    
    const lista = document.getElementById("lista_produtos");
    const filtrados = filtroCat === "Todos" ? db.prods : db.prods.filter(p => p.cat === filtroCat);
    lista.innerHTML = filtrados.map(p => `<div class="prod-card"><strong>${p.nome}</strong><br><small>${p.cat}</small><br><b style="color:green">R$ ${p.venda.toFixed(2)}</b></div>`).join('') || "Vazio";
}

function setFiltro(c) { filtroCat = c; rendHome(); }

// --- LOGICA CAIXA (PDV) ---
function rendCaixa() {
    const sel = document.getElementById("caixa_select_prod");
    sel.innerHTML = db.prods.map((p, i) => `<option value="${i}">${p.nome} - R$ ${p.venda.toFixed(2)}</option>`).join('');
    caixaAtualizarLista();
    
    const hist = document.getElementById("caixa_vendas_dia");
    const vendasHoje = db.pedidos.filter(p => p.id.startsWith("PDV-"));
    hist.innerHTML = vendasHoje.map(v => `<div>${v.id} - R$ ${v.bruto.toFixed(2)} (${v.pgto})</div>`).join('');
}

function caixaAddProd() {
    const idx = document.getElementById("caixa_select_prod").value;
    if(db.prods[idx]) { carrinhoCaixa.push({...db.prods[idx], cartId: Date.now()}); caixaAtualizarLista(); }
}

function caixaRemoverItem(cartId) {
    carrinhoCaixa = carrinhoCaixa.filter(i => i.cartId !== cartId);
    caixaAtualizarLista();
}

function caixaAtualizarLista() {
    const desc = parseFloat(document.getElementById("caixa_desconto").value) || 0;
    let subtotal = carrinhoCaixa.reduce((a, b) => a + b.venda, 0);
    let total = subtotal - desc;
    
    document.getElementById("caixa_itens").innerHTML = carrinhoCaixa.map(i => `
        <div style="display:flex; justify-content:space-between; margin-bottom:5px">
            <span>${i.nome}</span>
            <span>R$ ${i.venda.toFixed(2)} <button class="btn-del" onclick="caixaRemoverItem(${i.cartId})">X</button></span>
        </div>
    `).join('') || "Carrinho vazio";
    
    document.getElementById("caixa_total_exibição").innerText = "Total: R$ " + (total > 0 ? total.toFixed(2) : "0.00");
}

function caixaFinalizar() {
    if(!carrinhoCaixa.length) return;
    const desc = parseFloat(document.getElementById("caixa_desconto").value) || 0;
    const pgto = document.getElementById("caixa_pgto").value;
    let bruto = carrinhoCaixa.reduce((a, b) => a + b.venda, 0) - desc;
    let custo = carrinhoCaixa.reduce((a, b) => a + b.custo, 0);
    
    db.pedidos.unshift({ 
        id: "PDV-"+Math.floor(Date.now()/1000), 
        bruto: bruto, 
        liquido: bruto - custo, 
        itens: carrinhoCaixa.length + " itens", 
        pgto: pgto 
    });
    salvar();
    carrinhoCaixa = [];
    document.getElementById("caixa_desconto").value = "";
    rendCaixa();
    alert("Venda Concluída!");
}

// --- ADMIN ---
function rendAdmin() {
    document.getElementById("adm_bruto").innerText = "R$ " + db.pedidos.reduce((a, b) => a + b.bruto, 0).toFixed(2);
    document.getElementById("adm_liquido").innerText = "R$ " + db.pedidos.reduce((a, b) => a + b.liquido, 0).toFixed(2);
    document.getElementById("adm_lista_pedidos").innerHTML = db.pedidos.map(p => `<div class="card"><b>${p.id}</b><br>Bruto: R$ ${p.bruto.toFixed(2)} | Lucro: R$ ${p.liquido.toFixed(2)}</div>`).join('');
}

function admAddProduto(e) {
    e.preventDefault();
    const n = document.getElementById("adm_p_nome").value;
    const cat = document.getElementById("adm_p_cat").value;
    const c = parseFloat(document.getElementById("adm_p_custo").value);
    const v = parseFloat(document.getElementById("adm_p_venda").value);
    if(n && !isNaN(c)) { db.prods.push({ nome: n, cat: cat, custo: c, venda: v }); salvar(); alert("Salvo!"); rendAdmin(); }
}

function logout() { sessao = ""; irPara('tela_login'); }
</script>
</body>
</html>
