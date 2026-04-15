

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Ruan Assistec | Global Commercial System v7.5</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;700;900&display=swap');
        body { font-family: 'Inter', sans-serif; overflow: hidden; }
        .view { display: none; width: 100%; min-height: 100vh; }
        .view.active { display: flex; }
        .tab-content { display: none; }
        .tab-content.active { display: block; }
        .sidebar-link.active { background: #0ea5e9; color: white; }
        #lang-menu::-webkit-scrollbar, #cart-items-list::-webkit-scrollbar { width: 4px; }
        #lang-menu::-webkit-scrollbar-thumb, #cart-items-list::-webkit-scrollbar-thumb { background: #cbd5e1; border-radius: 10px; }
    </style>
</head>
<body class="bg-slate-100">

    <div class="fixed top-6 right-6 z-[500] flex gap-3">
        <div class="relative">
            <button onclick="toggleLangMenu()" class="bg-white border shadow-2xl rounded-full p-4 flex items-center gap-3 hover:scale-105 transition-all">
                <i class="fas fa-globe text-cyan-600 text-xl"></i>
                <span id="current-flag" class="text-xl">🇧🇷</span>
            </button>
            <div id="lang-menu" class="hidden absolute right-0 mt-3 w-64 max-h-80 bg-white rounded-[2rem] shadow-2xl border overflow-y-auto z-[501]">
                <div id="country-list" class="p-2 space-y-1"></div>
            </div>
        </div>
    </div>

    <div id="login-view" class="view active items-center justify-center bg-slate-900 p-4 relative text-center">
        <div id="lockdown-overlay" class="hidden absolute inset-0 bg-black/95 z-[300] flex flex-col items-center justify-center text-white p-6">
            <i class="fas fa-shield-virus text-6xl text-red-500 mb-4 animate-pulse"></i>
            <h2 id="txt-lock-title" class="text-2xl font-black uppercase">SECURITY LOCK</h2>
            <div id="timer" class="text-4xl font-mono text-cyan-500 mt-4">30s</div>
        </div>
        <div class="bg-white p-8 md:p-12 rounded-[3rem] shadow-2xl w-full max-w-md">
            <div class="mb-8">
                <div class="bg-cyan-500 w-16 h-16 rounded-2xl flex items-center justify-center mx-auto mb-4"><i class="fas fa-microchip text-3xl text-white"></i></div>
                <h1 class="text-3xl font-black italic">RUAN <span class="text-cyan-500 font-black">ASSISTEC</span></h1>
            </div>
            <div class="space-y-4">
                <button onclick="switchView('cliente')" id="btn-client" class="w-full bg-slate-100 text-slate-800 font-black py-5 rounded-2xl uppercase text-xs tracking-widest hover:bg-slate-200 transition">Acesso Cliente</button>
                <div class="relative py-4"><hr><span id="txt-admin-label" class="absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 bg-white px-4 text-[9px] font-black text-slate-300 uppercase italic">Administração</span></div>
                <input type="password" id="login-pass" class="w-full p-5 bg-slate-50 border rounded-2xl focus:ring-2 focus:ring-cyan-500 outline-none text-center font-bold" placeholder="••••••">
                <button onclick="checkLogin()" id="btn-auth" class="w-full bg-slate-900 text-white font-black py-5 rounded-2xl uppercase text-xs hover:bg-cyan-600 transition">Autenticar</button>
                <p id="login-error" class="text-red-500 text-[10px] hidden font-black uppercase mt-2">Invalid Password</p>
            </div>
        </div>
    </div>

    <div id="admin-view" class="view flex-col md:flex-row">
        <aside class="w-full md:w-72 bg-slate-900 text-white flex flex-col p-6 shadow-2xl">
            <h2 class="text-2xl font-black italic mb-10 text-cyan-400">RUAN ADM</h2>
            <nav class="flex-1 space-y-2">
                <button onclick="switchTab('dashboard')" id="nav-dashboard" class="sidebar-link w-full text-left p-4 rounded-xl flex items-center gap-4 font-bold active"><i class="fas fa-chart-pie"></i> <span id="txt-nav-dash">Dashboard</span></button>
                <button onclick="switchTab('os')" id="nav-os" class="sidebar-link w-full text-left p-4 rounded-xl flex items-center gap-4 font-bold"><i class="fas fa-wrench"></i> <span id="txt-nav-os">Ordens</span></button>
                <button onclick="switchTab('estoque')" id="nav-estoque" class="sidebar-link w-full text-left p-4 rounded-xl flex items-center gap-4 font-bold"><i class="fas fa-store"></i> <span id="txt-nav-stock">Mercado</span></button>
            </nav>
            <button onclick="location.reload()" class="p-4 text-red-400 font-black flex items-center gap-3 mt-auto hover:bg-red-500/10 rounded-xl transition"><i class="fas fa-power-off"></i> <span id="txt-logout">SAIR</span></button>
        </aside>
        <main class="flex-1 p-6 md:p-12 overflow-y-auto bg-slate-100">
            <div id="tab-dashboard" class="tab-content active space-y-6">
                <h2 id="txt-dash-title" class="text-2xl font-black text-slate-800 uppercase tracking-tighter">Visão Geral</h2>
                <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                    <div class="bg-white p-8 rounded-3xl shadow-sm border-l-8 border-cyan-500">
                        <p id="txt-card-os" class="text-slate-400 text-xs font-black uppercase">O.S Ativas</p>
                        <h3 id="dash-os" class="text-4xl font-black">0</h3>
                    </div>
                    <div class="bg-white p-8 rounded-3xl shadow-sm border-l-8 border-slate-800">
                        <p id="txt-card-stock" class="text-slate-400 text-xs font-black uppercase">Itens em Loja</p>
                        <h3 id="dash-stock" class="text-4xl font-black">0</h3>
                    </div>
                </div>
            </div>
            <div id="tab-os" class="tab-content space-y-6">
                <div class="bg-white p-6 rounded-3xl shadow-sm grid grid-cols-1 md:grid-cols-4 gap-4">
                    <input type="text" id="os-cliente" placeholder="Cliente" class="p-4 bg-slate-50 rounded-xl border-none ring-1 ring-slate-200">
                    <input type="text" id="os-aparelho" placeholder="Aparelho" class="p-4 bg-slate-50 rounded-xl border-none ring-1 ring-slate-200">
                    <button onclick="addOS()" id="btn-add-os" class="bg-cyan-600 text-white font-black rounded-xl uppercase text-xs">Criar O.S</button>
                </div>
                <div class="bg-white rounded-3xl shadow-sm overflow-hidden"><table class="w-full text-left"><tbody id="osTableBody" class="divide-y"></tbody></table></div>
            </div>
            <div id="tab-estoque" class="tab-content space-y-6">
                <div class="bg-white p-6 rounded-3xl shadow-sm grid grid-cols-1 md:grid-cols-4 gap-4">
                    <input type="text" id="p-nome" placeholder="Produto" class="p-4 bg-slate-50 rounded-xl border-none ring-1 ring-slate-200">
                    <input type="number" id="p-qtd" placeholder="Qtd" class="p-4 bg-slate-50 rounded-xl border-none ring-1 ring-slate-200">
                    <input type="number" id="p-preco" placeholder="Preço" class="p-4 bg-slate-50 rounded-xl border-none ring-1 ring-slate-200">
                    <button onclick="addProduto()" id="btn-add-prod" class="bg-slate-900 text-white font-black rounded-xl uppercase text-xs">Salvar</button>
                </div>
                <div class="bg-white rounded-3xl shadow-sm overflow-hidden"><table class="w-full text-left"><tbody id="prodTableBody" class="divide-y"></tbody></table></div>
            </div>
        </main>
    </div>

    <div id="cliente-view" class="view flex-col md:flex-row overflow-hidden bg-slate-50">
        <div class="flex-1 flex flex-col h-screen overflow-y-auto">
            <header class="bg-white p-6 shadow-sm border-b sticky top-0 z-50 flex justify-between items-center">
                <h1 class="text-2xl font-black italic">RUAN <span class="text-cyan-500">STORE</span></h1>
                <input type="text" id="searchBar" onkeyup="renderAll()" class="hidden md:block flex-1 max-w-md mx-6 p-4 bg-slate-50 rounded-2xl outline-none ring-1 ring-slate-100 focus:ring-2 focus:ring-cyan-500" placeholder="Buscar no mercado...">
                <div class="flex gap-2">
                    <button onclick="location.reload()" class="p-4 bg-slate-100 rounded-2xl text-slate-500 hover:text-red-500 transition"><i class="fas fa-sign-out-alt"></i></button>
                </div>
            </header>
            <main class="p-8"><div id="loja-grid" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6"></div></main>
        </div>

        <aside class="w-full md:w-96 bg-white border-l shadow-2xl h-screen flex flex-col p-8">
            <div class="flex justify-between items-center mb-8">
                <h2 id="txt-cart-title" class="text-2xl font-black italic">Meu Carrinho</h2>
                <span id="cart-badge" class="bg-cyan-500 text-white text-[10px] font-black px-3 py-1 rounded-full">0</span>
            </div>
            <div id="cart-items-list" class="flex-1 overflow-y-auto space-y-4 mb-6"></div>
            <div class="border-t pt-6 space-y-4">
                <div class="flex justify-between items-center font-black">
                    <span id="txt-total-label">Total</span>
                    <span id="cart-total" class="text-2xl text-cyan-600">R$ 0,00</span>
                </div>
                <div class="space-y-3">
                    <p id="txt-pay-label" class="text-[10px] font-black uppercase text-slate-400">Forma de Pagamento</p>
                    <select id="pay-method" class="w-full p-4 bg-slate-50 rounded-2xl border-none ring-1 ring-slate-100 font-bold outline-none text-sm">
                        <option value="Pix">Pix / Transferência</option>
                        <option value="Cartão">Cartão de Crédito</option>
                        <option value="Dinheiro">Dinheiro</option>
                    </select>
                </div>
                <button onclick="checkoutWhatsApp()" id="btn-checkout" class="w-full bg-green-500 text-white font-black py-5 rounded-2xl shadow-xl hover:bg-green-600 transition uppercase text-xs tracking-widest">Enviar Pedido WhatsApp</button>
            </div>
        </aside>
    </div>

    <script>
        const SENHA_ADM = "admin123";
        let tentativas = 0, bloqueado = false, currentLang = localStorage.getItem('ruan_lang') || 'pt';
        let ordens = JSON.parse(localStorage.getItem('db_os_g8')) || [];
        let produtos = JSON.parse(localStorage.getItem('db_pr_g8')) || [];
        let carrinho = [];

        const languages = {
            pt: { flag: '🇧🇷', currency: 'R$', client: "Acesso Cliente", admin: "Administração", auth: "Autenticar", dash: "Dashboard", os: "Ordens", stock: "Estoque", logout: "SAIR", dashTitle: "Visão Geral", cardOS: "O.S Ativas", cardStock: "Itens", addOS: "Criar O.S", addProd: "Salvar", buy: "Adicionar", error: "Senha Incorreta", lock: "BLOQUEIO DE SEGURANÇA", cart: "Meu Carrinho", total: "Total", pay: "Pagamento", checkout: "Finalizar WhatsApp" },
            en: { flag: '🇺🇸', currency: '$', client: "Customer Access", admin: "Management", auth: "Authenticate", dash: "Stats", os: "Orders", stock: "Stock", logout: "LOGOUT", dashTitle: "Overview", cardOS: "Active O.S", cardStock: "Items", addOS: "Create O.S", addProd: "Save", buy: "Add to Cart", error: "Wrong Password", lock: "SECURITY LOCK", cart: "My Cart", total: "Total", pay: "Payment", checkout: "Order via WhatsApp" },
            es: { flag: '🇪🇸', currency: '€', client: "Acceso Cliente", admin: "Administración", auth: "Autenticar", dash: "Panel", os: "Órdenes", stock: "Mercado", logout: "SALIR", dashTitle: "Resumen", cardOS: "O.S Activas", cardStock: "Items", addOS: "Crear O.S", addProd: "Guardar", buy: "Añadir", error: "Contraseña Incorrecta", lock: "BLOQUEO DE SEGURIDAD", cart: "Mi Carrito", total: "Total", pay: "Pago", checkout: "Enviar WhatsApp" }
        };

        const countries = [{code:'pt', name:'Brasil', flag:'🇧🇷'}, {code:'en', name:'USA / UK', flag:'🇺🇸'}, {code:'es', name:'España', flag:'🇪🇸'}];

        function toggleLangMenu() { document.getElementById('lang-menu').classList.toggle('hidden'); }
        function setLanguage(c) {
            currentLang = c; localStorage.setItem('ruan_lang', c); const l = languages[c];
            document.getElementById('current-flag').innerText = l.flag;
            document.getElementById('btn-client').innerText = l.client;
            document.getElementById('txt-admin-label').innerText = l.admin;
            document.getElementById('btn-auth').innerText = l.auth;
            document.getElementById('txt-nav-dash').innerText = l.dash;
            document.getElementById('txt-nav-os').innerText = l.os;
            document.getElementById('txt-nav-stock').innerText = l.stock;
            document.getElementById('txt-logout').innerText = l.logout;
            document.getElementById('txt-dash-title').innerText = l.dashTitle;
            document.getElementById('txt-card-os').innerText = l.cardOS;
            document.getElementById('txt-card-stock').innerText = l.cardStock;
            document.getElementById('btn-add-os').innerText = l.addOS;
            document.getElementById('btn-add-prod').innerText = l.addProd;
            document.getElementById('txt-cart-title').innerText = l.cart;
            document.getElementById('txt-total-label').innerText = l.total;
            document.getElementById('txt-pay-label').innerText = l.pay;
            document.getElementById('btn-checkout').innerText = l.checkout;
            document.getElementById('lang-menu').classList.add('hidden'); renderAll();
        }

        function checkLogin() {
            if(bloqueado) return;
            if(document.getElementById('login-pass').value === SENHA_ADM) { switchView('admin'); tentativas = 0; }
            else { 
                tentativas++; document.getElementById('login-error').classList.remove('hidden');
                if(tentativas >= 3) {
                    bloqueado = true; document.getElementById('lockdown-overlay').classList.remove('hidden');
                    let t = 30; const timer = setInterval(() => { t--; document.getElementById('timer').innerText = t+"s"; if(t<=0){clearInterval(timer);bloqueado=false;tentativas=0;document.getElementById('lockdown-overlay').classList.add('hidden');}}, 1000);
                }
            }
        }

        function switchView(v) { document.querySelectorAll('.view').forEach(x => x.classList.remove('active')); document.getElementById(v + '-view').classList.add('active'); renderAll(); }
        function switchTab(t) { document.querySelectorAll('.tab-content').forEach(c => c.classList.remove('active')); document.querySelectorAll('.sidebar-link').forEach(l => l.classList.remove('active')); document.getElementById('tab-' + t).classList.add('active'); document.getElementById('nav-' + t).classList.add('active'); }

        function save() { localStorage.setItem('db_os_g8', JSON.stringify(ordens)); localStorage.setItem('db_pr_g8', JSON.stringify(produtos)); renderAll(); }
        function addOS() { const c = document.getElementById('os-cliente').value; if(c) { ordens.unshift({id: Date.now(), cliente: c, aparelho: document.getElementById('os-aparelho').value}); document.getElementById('os-cliente').value=''; save(); } }
        function addProduto() { const n = document.getElementById('p-nome').value; if(n) { produtos.push({id: Date.now(), nome: n, qtd: document.getElementById('p-qtd').value, preco: parseFloat(document.getElementById('p-preco').value || 0)}); document.getElementById('p-nome').value=''; save(); } }

        function addToCart(id) {
            const p = produtos.find(x => x.id === id);
            const ja = carrinho.find(c => c.id === id);
            if(ja) ja.cQtd++; else carrinho.push({...p, cQtd: 1});
            renderAll();
        }

        function renderAll() {
            const l = languages[currentLang];
            if(document.getElementById('dash-os')) { document.getElementById('dash-os').innerText = ordens.length; document.getElementById('dash-stock').innerText = produtos.length; }
            
            const osBody = document.getElementById('osTableBody');
            if(osBody) osBody.innerHTML = ordens.map((o,i) => `<tr><td class="p-6 font-bold text-slate-800">${o.cliente}</td><td class="p-6 text-slate-500">${o.aparelho}</td><td class="p-6 text-right"><button onclick="ordens.splice(${i},1);save()" class="text-red-300"><i class="fas fa-trash"></i></button></td></tr>`).join('');
            
            const prBody = document.getElementById('prodTableBody');
            if(prBody) prBody.innerHTML = produtos.map((p,i) => `<tr><td class="p-6 font-bold">${p.nome}</td><td class="p-6 font-black text-cyan-600">${p.qtd}</td><td class="p-6 text-right font-bold">${l.currency} ${p.preco.toFixed(2)}</td></tr>`).join('');

            const grid = document.getElementById('loja-grid');
            if(grid) grid.innerHTML = produtos.map(p => `<div class="bg-white p-6 rounded-[2.5rem] shadow-sm border flex flex-col justify-between hover:shadow-xl transition"><div><h3 class="font-black text-slate-800 uppercase text-[10px] tracking-widest mb-2">${p.nome}</h3><p class="text-2xl font-black text-cyan-600 mt-2">${l.currency} ${p.preco.toFixed(2)}</p></div><button onclick="addToCart(${p.id})" class="w-full mt-6 bg-slate-900 text-white py-3 rounded-2xl font-black text-[10px] uppercase tracking-tighter hover:bg-cyan-500 transition">${l.buy}</button></div>`).join('');

            const cList = document.getElementById('cart-items-list');
            if(cList) {
                let total = 0;
                cList.innerHTML = carrinho.map((c, i) => {
                    total += c.preco * c.cQtd;
                    return `<div class="bg-slate-50 p-4 rounded-2xl flex justify-between items-center"><div class="text-xs font-black"><span>${c.nome}</span><br><span class="text-cyan-600">${c.cQtd}x</span></div><button onclick="carrinho.splice(${i},1);renderAll()" class="text-red-400 font-bold">&times;</button></div>`;
                }).join('');
                document.getElementById('cart-total').innerText = `${l.currency} ${total.toFixed(2)}`;
                document.getElementById('cart-badge').innerText = carrinho.length;
            }
        }

        function checkoutWhatsApp() {
            if(carrinho.length === 0) return alert("Adicione produtos ao carrinho primeiro!");
            let msg = `*PEDIDO RUAN STORE*%0A%0A`, total = 0;
            carrinho.forEach(i => { msg += `• ${i.nome} (${i.cQtd}x)%0A`; total += i.preco * i.cQtd; });
            msg += `%0A*TOTAL:* ${languages[currentLang].currency} ${total.toFixed(2)}%0A*PAGAMENTO:* ${document.getElementById('pay-method').value}`;
            window.open(`https://wa.me/5521999999999?text=${msg}`);
            carrinho = []; renderAll();
        }

        document.getElementById('country-list').innerHTML = countries.map(c => `<button onclick="setLanguage('${c.code}')" class="w-full text-left p-3 hover:bg-slate-50 rounded-xl flex items-center gap-3"><span class="text-xl">${c.flag}</span><span class="text-xs font-bold text-slate-700">${c.name}</span></button>`).join('');
        setLanguage(currentLang);
    </script>
</body>
</html>
