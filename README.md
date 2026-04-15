
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Ruan Assistec | Enterprise System</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;700;900&display=swap');
        body { font-family: 'Inter', sans-serif; }
        .view { display: none; }
        .view.active { display: block; }
        .sidebar-link.active { background: #0ea5e9; color: white; }
    </style>
</head>
<body class="bg-gray-100 min-h-screen">

    <div id="login-view" class="view active flex items-center justify-center min-h-screen bg-slate-900">
        <div class="bg-white p-10 rounded-3xl shadow-2xl w-full max-w-md text-center">
            <div class="mb-6">
                <i class="fas fa-tools text-5xl text-cyan-500 mb-4"></i>
                <h1 class="text-3xl font-black tracking-tighter">RUAN <span class="text-cyan-500">ASSISTEC</span></h1>
                <p class="text-gray-400 text-xs uppercase font-bold tracking-widest mt-1">Gestão de Mercado & Técnica</p>
            </div>
            <div class="space-y-4">
                <button onclick="switchView('cliente')" class="w-full bg-gray-100 text-gray-700 font-bold py-4 rounded-2xl hover:bg-gray-200 transition">ENTRAR COMO CLIENTE</button>
                <div class="relative py-2 text-gray-300">
                    <hr class="border-gray-100">
                    <span class="absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 bg-white px-2 text-[10px] font-bold">ACESSO RESTRITO</span>
                </div>
                <input type="password" id="login-pass" class="w-full p-4 bg-gray-50 border border-gray-200 rounded-2xl focus:ring-2 focus:ring-cyan-500 outline-none transition" placeholder="Senha do Administrador">
                <button onclick="checkLogin()" class="w-full bg-cyan-600 text-white font-bold py-4 rounded-2xl hover:bg-cyan-700 shadow-lg shadow-cyan-200 transition">ENTRAR NO PAINEL ADM</button>
            </div>
            <p id="login-error" class="text-red-500 text-xs mt-4 hidden font-bold">SENHA INCORRETA!</p>
        </div>
    </div>

    <div id="admin-view" class="view flex flex-col md:flex-row min-h-screen">
        <aside class="w-full md:w-64 bg-slate-900 text-white flex flex-col">
            <div class="p-6 border-b border-slate-800">
                <h2 class="text-xl font-black italic">ADM <span class="text-cyan-400">PRO</span></h2>
            </div>
            <nav class="flex-1 p-4 space-y-2">
                <button onclick="switchTab('dashboard')" class="sidebar-link w-full text-left p-3 rounded-xl flex items-center gap-3 font-semibold transition hover:bg-slate-800">
                    <i class="fas fa-chart-line"></i> Dashboard
                </button>
                <button onclick="switchTab('os')" class="sidebar-link w-full text-left p-3 rounded-xl flex items-center gap-3 font-semibold transition hover:bg-slate-800">
                    <i class="fas fa-clipboard-list"></i> Ordens de Serviço
                </button>
                <button onclick="switchTab('estoque')" class="sidebar-link w-full text-left p-3 rounded-xl flex items-center gap-3 font-semibold transition hover:bg-slate-800">
                    <i class="fas fa-box"></i> Mercado / Estoque
                </button>
            </nav>
            <button onclick="logout()" class="p-6 text-red-400 hover:text-red-300 font-bold text-sm text-left border-t border-slate-800">
                <i class="fas fa-sign-out-alt mr-2"></i> SAIR DO SISTEMA
            </button>
        </aside>

        <main class="flex-1 p-4 md:p-8 overflow-y-auto">
            
            <div id="tab-dashboard" class="tab-content space-y-6">
                <h2 class="text-2xl font-black text-slate-800">Resumo Geral</h2>
                <div class="grid grid-cols-1 md:grid-cols-4 gap-4">
                    <div class="bg-white p-6 rounded-3xl shadow-sm border border-slate-100">
                        <p class="text-gray-400 text-xs font-bold uppercase">Vendas/Ordens</p>
                        <h3 id="dash-os-total" class="text-3xl font-black text-slate-800">0</h3>
                    </div>
                    <div class="bg-white p-6 rounded-3xl shadow-sm border border-slate-100">
                        <p class="text-gray-400 text-xs font-bold uppercase">Faturamento Est.</p>
                        <h3 id="dash-faturamento" class="text-3xl font-black text-green-600">R$ 0</h3>
                    </div>
                    <div class="bg-white p-6 rounded-3xl shadow-sm border border-slate-100">
                        <p class="text-gray-400 text-xs font-bold uppercase">Itens Estoque</p>
                        <h3 id="dash-estoque-total" class="text-3xl font-black text-cyan-600">0</h3>
                    </div>
                    <div class="bg-white p-6 rounded-3xl shadow-sm border border-red-100">
                        <p class="text-red-400 text-xs font-bold uppercase">Atenção Estoque</p>
                        <h3 id="dash-alerta" class="text-3xl font-black text-red-600">0</h3>
                    </div>
                </div>
            </div>

            <div id="tab-os" class="tab-content hidden space-y-6">
                <div class="bg-white p-6 rounded-3xl shadow-sm">
                    <h3 class="font-black mb-4 uppercase text-sm">Registrar Nova OS</h3>
                    <form id="osForm" class="grid grid-cols-1 md:grid-cols-4 gap-3">
                        <input type="text" id="os-cliente" placeholder="Cliente" class="p-3 bg-gray-50 rounded-xl outline-none border" required>
                        <input type="number" id="os-whats" placeholder="WhatsApp (DDD)" class="p-3 bg-gray-50 rounded-xl outline-none border" required>
                        <input type="text" id="os-aparelho" placeholder="Aparelho" class="p-3 bg-gray-50 rounded-xl outline-none border" required>
                        <button class="bg-cyan-600 text-white font-bold rounded-xl py-3 hover:bg-cyan-700 transition">ABRIR OS</button>
                    </form>
                </div>
                <div class="bg-white rounded-3xl shadow-sm overflow-hidden">
                    <table class="w-full text-left">
                        <thead class="bg-slate-50 text-[10px] uppercase font-bold text-slate-400 border-b">
                            <tr><th class="p-4">ID</th><th class="p-4">Cliente</th><th class="p-4">Equipamento</th><th class="p-4">Status</th><th class="p-4 text-right">Ações</th></tr>
                        </thead>
                        <tbody id="osTableBody" class="divide-y divide-gray-50"></tbody>
                    </table>
                </div>
            </div>

            <div id="tab-estoque" class="tab-content hidden space-y-6">
                <div class="bg-white p-6 rounded-3xl shadow-sm">
                    <h3 class="font-black mb-4 uppercase text-sm text-cyan-600">Gerenciar Produtos de Mercado</h3>
                    <form id="prodForm" class="grid grid-cols-1 md:grid-cols-4 gap-3">
                        <input type="text" id="p-nome" placeholder="Produto/Peça" class="p-3 bg-gray-50 rounded-xl border outline-none" required>
                        <input type="number" id="p-qtd" placeholder="Qtd Inicial" class="p-3 bg-gray-50 rounded-xl border outline-none" required>
                        <input type="number" step="0.01" id="p-preco" placeholder="Preço Venda" class="p-3 bg-gray-50 rounded-xl border outline-none" required>
                        <button class="bg-slate-900 text-white font-bold rounded-xl py-3 hover:bg-slate-800 transition">CADASTRAR</button>
                    </form>
                </div>
                <div class="bg-white rounded-3xl shadow-sm overflow-hidden">
                    <table class="w-full text-left border-collapse">
                        <thead class="bg-gray-50 text-[10px] uppercase font-bold text-gray-400">
                            <tr><th class="p-4">Item</th><th class="p-4">Qtd</th><th class="p-4">Preço</th><th class="p-4 text-right">Controle</th></tr>
                        </thead>
                        <tbody id="prodTableBody" class="divide-y divide-gray-50"></tbody>
                    </table>
                </div>
            </div>
        </main>
    </div>

    <div id="cliente-view" class="view">
        <header class="bg-white p-5 shadow-sm border-b sticky top-0 z-50 flex justify-between items-center">
            <h1 class="text-xl font-black">RUAN <span class="text-cyan-500">MERCADO</span></h1>
            <div class="flex gap-4">
                <button onclick="openCart()" class="relative p-2 bg-slate-100 rounded-full">
                    <i class="fas fa-shopping-basket"></i>
                    <span id="cart-count" class="absolute -top-1 -right-1 bg-red-500 text-white text-[10px] font-bold px-1.5 rounded-full">0</span>
                </button>
                <button onclick="logout()" class="text-xs font-bold text-gray-400 uppercase">Sair</button>
            </div>
        </header>
        <main class="container mx-auto p-6">
            <div id="loja-grid" class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-4 gap-6"></div>
        </main>

        <div id="cart-modal" class="fixed inset-0 bg-slate-900/60 backdrop-blur-sm z-[100] hidden items-end md:items-center justify-center p-4">
            <div class="bg-white w-full max-w-lg rounded-3xl p-6 shadow-2xl">
                <div class="flex justify-between items-center mb-6 border-b pb-4">
                    <h2 class="text-xl font-black italic">Carrinho</h2>
                    <button onclick="closeCart()" class="text-2xl">&times;</button>
                </div>
                <div id="cart-items" class="max-h-60 overflow-y-auto space-y-3 mb-6"></div>
                <div class="space-y-4 border-t pt-4 text-sm">
                    <div class="flex justify-between font-black text-lg"><span>TOTAL:</span><span id="cart-total" class="text-cyan-600">R$ 0,00</span></div>
                    <div class="grid grid-cols-2 gap-2">
                        <div>
                            <label class="text-[10px] font-bold text-gray-400">HORA BUSCA</label>
                            <input type="time" id="c-hora" class="w-full p-3 bg-gray-50 rounded-xl border">
                        </div>
                        <div>
                            <label class="text-[10px] font-bold text-gray-400">PAGAMENTO</label>
                            <select id="c-pgto" class="w-full p-3 bg-gray-50 rounded-xl border">
                                <option>Pix</option><option>Dinheiro</option><option>Cartão</option>
                            </select>
                        </div>
                    </div>
                    <button onclick="finalizarPedido()" class="w-full bg-green-500 text-white font-black py-4 rounded-2xl hover:shadow-lg transition">FECHAR PEDIDO NO WHATSAPP</button>
                </div>
            </div>
        </div>
    </div>

    <script>
        const SENHA_MESTRA = "admin123";
        let ordens = JSON.parse(localStorage.getItem('r_os')) || [];
        let produtos = JSON.parse(localStorage.getItem('r_pr')) || [];
        let carrinho = [];

        // SISTEMA DE TELAS E LOGIN
        function switchView(view) {
            document.querySelectorAll('.view').forEach(v => v.classList.remove('active'));
            document.getElementById(view + '-view').classList.add('active');
            renderAll();
        }

        function checkLogin() {
            if(document.getElementById('login-pass').value === SENHA_MESTRA) switchView('admin');
            else document.getElementById('login-error').classList.remove('hidden');
        }

        function logout() { location.reload(); }

        function switchTab(tab) {
            document.querySelectorAll('.tab-content').forEach(t => t.classList.add('hidden'));
            document.querySelectorAll('.sidebar-link').forEach(l => l.classList.remove('active'));
            document.getElementById('tab-' + tab).classList.remove('hidden');
            event.currentTarget.classList.add('active');
        }

        // ADM: PRODUTOS & OS
        document.getElementById('osForm').addEventListener('submit', (e) => {
            e.preventDefault();
            ordens.unshift({
                id: Math.floor(Math.random() * 9000) + 1000,
                cliente: document.getElementById('os-cliente').value,
                whatsapp: document.getElementById('os-whats').value,
                aparelho: document.getElementById('os-aparelho').value,
                status: 'Pendente'
            });
            save(); e.target.reset();
        });

        document.getElementById('prodForm').addEventListener('submit', (e) => {
            e.preventDefault();
            produtos.push({
                id: Date.now(),
                nome: document.getElementById('p-nome').value,
                qtd: parseInt(document.getElementById('p-qtd').value),
                preco: parseFloat(document.getElementById('p-preco').value)
            });
            save(); e.target.reset();
        });

        // LOJA / CARRINHO
        function addToCart(id) {
            const p = produtos.find(item => item.id === id);
            const ja = carrinho.find(c => c.id === id);
            if(ja) ja.qtdCart++; else carrinho.push({...p, qtdCart: 1});
            renderAll();
        }

        function openCart() { document.getElementById('cart-modal').classList.remove('hidden'); document.getElementById('cart-modal').classList.add('flex'); }
        function closeCart() { document.getElementById('cart-modal').classList.add('hidden'); }

        function finalizarPedido() {
            let total = 0;
            let msg = `*NOVO PEDIDO - RUAN MERCADO*%0A%0A`;
            carrinho.forEach(i => {
                msg += `• ${i.nome} (${i.qtdCart}x) - R$ ${i.preco}%0A`;
                total += i.preco * i.qtdCart;
            });
            msg += `%0A*TOTAL:* R$ ${total.toFixed(2)}%0A*BUSCA:* ${document.getElementById('c-hora').value}%0A*PAGAMENTO:* ${document.getElementById('c-pgto').value}`;
            window.open(`https://wa.me/5521999999999?text=${msg}`); // Coloque seu número aqui
            carrinho = []; renderAll(); closeCart();
        }

        // RENDER GERAL
        function save() {
            localStorage.setItem('r_os', JSON.stringify(ordens));
            localStorage.setItem('r_pr', JSON.stringify(produtos));
            renderAll();
        }

        function renderAll() {
            // Dashboard ADM
            if(document.getElementById('dash-os-total')) {
                document.getElementById('dash-os-total').innerText = ordens.length;
                document.getElementById('dash-estoque-total').innerText = produtos.length;
                document.getElementById('dash-alerta').innerText = produtos.filter(p => p.qtd < 5).length;
                let fat = produtos.reduce((acc, p) => acc + (p.preco * p.qtd), 0);
                document.getElementById('dash-faturamento').innerText = `R$ ${fat.toFixed(0)}`;
            }

            // Ordens ADM
            const osBody = document.getElementById('osTableBody');
            if(osBody) osBody.innerHTML = ordens.map((o, i) => `
                <tr class="hover:bg-slate-50 transition text-sm">
                    <td class="p-4 text-gray-400 font-mono">#${o.id}</td>
                    <td class="p-4 font-bold">${o.cliente}</td>
                    <td class="p-4 font-medium text-slate-500">${o.aparelho}</td>
                    <td class="p-4"><span class="px-3 py-1 text-[10px] font-black rounded-full ${o.status==='Concluído'?'bg-green-100 text-green-700':'bg-amber-100 text-amber-700'}">${o.status}</span></td>
                    <td class="p-4 text-right">
                        <button onclick="mStatus(${i})" class="text-cyan-500 mr-2"><i class="fas fa-check-circle"></i></button>
                        <button onclick="dOS(${i})" class="text-red-300"><i class="fas fa-trash"></i></button>
                    </td>
                </tr>`).join('');

            // Estoque ADM
            const prBody = document.getElementById('prodTableBody');
            if(prBody) prBody.innerHTML = produtos.map((p, i) => `
                <tr class="hover:bg-gray-50 transition text-sm">
                    <td class="p-4 font-bold text-slate-700">${p.nome}</td>
                    <td class="p-4 font-black ${p.qtd < 5 ? 'text-red-500' : 'text-slate-400'}">${p.qtd} un</td>
                    <td class="p-4 text-green-600 font-bold">R$ ${p.preco.toFixed(2)}</td>
                    <td class="p-4 text-right">
                        <button onclick="dPr(${i})" class="bg-red-50 text-red-500 p-2 px-3 rounded-lg text-xs"><i class="fas fa-times"></i></button>
                    </td>
                </tr>`).join('');

            // Loja Cliente
            const grid = document.getElementById('loja-grid');
            if(grid) grid.innerHTML = produtos.map(p => `
                <div class="bg-white p-5 rounded-[2rem] shadow-sm border border-slate-100 flex flex-col justify-between hover:shadow-xl transition">
                    <div>
                        <div class="w-full h-24 bg-slate-50 rounded-2xl mb-4 flex items-center justify-center text-slate-200 text-4xl"><i class="fas fa-tag"></i></div>
                        <h3 class="font-bold text-slate-800">${p.nome}</h3>
                        <p class="text-[10px] font-bold text-gray-400 uppercase tracking-tighter">${p.qtd} DISPONÍVEIS</p>
                    </div>
                    <div class="mt-4 flex items-center justify-between">
                        <span class="text-lg font-black text-cyan-600 font-mono">R$ ${p.preco.toFixed(2)}</span>
                        <button onclick="addToCart(${p.id})" class="bg-slate-900 text-white p-2 px-4 rounded-xl text-xs font-bold hover:scale-105 transition">PEGAR</button>
                    </div>
                </div>`).join('');

            // Carrinho
            document.getElementById('cart-count').innerText = carrinho.length;
            const cItems = document.getElementById('cart-items');
            if(cItems) {
                let total = 0;
                cItems.innerHTML = carrinho.map((c, i) => {
                    total += c.preco * c.qtdCart;
                    return `<div class="flex justify-between p-3 bg-gray-50 rounded-2xl">
                        <div class="text-xs"><b>${c.nome}</b><br>${c.qtdCart}x - R$ ${c.preco.toFixed(2)}</div>
                        <button onclick="carrinho.splice(${i},1);renderAll()" class="text-red-400">&times;</button>
                    </div>`;
                }).join('');
                document.getElementById('cart-total').innerText = `R$ ${total.toFixed(2)}`;
            }
        }

        // AÇÕES SIMPLES
        window.mStatus = (i) => { ordens[i].status = ordens[i].status === 'Pendente' ? 'Concluído' : 'Pendente'; save(); };
        window.dOS = (i) => { if(confirm('Excluir?')) { ordens.splice(i,1); save(); } };
        window.dPr = (i) => { if(confirm('Remover?')) { produtos.splice(i,1); save(); } };

        renderAll();
    </script>
</body>
</html>
