<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Ruan Assistec | Full System</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;700&display=swap');
        body { font-family: 'Inter', sans-serif; transition: all 0.3s ease; }
        .view { display: none; }
        .view.active { display: block; }
        .cart-badge { position: absolute; top: -5px; right: -5px; background: #ef4444; color: white; border-radius: 50%; padding: 2px 6px; font-size: 10px; }
    </style>
</head>
<body class="bg-slate-50 min-h-screen">

    <div id="login-view" class="view active flex items-center justify-center min-h-screen p-4">
        <div class="bg-white p-8 rounded-3xl shadow-2xl w-full max-w-md border border-slate-100 text-center">
            <h1 class="text-3xl font-black mb-2 tracking-tighter text-slate-900">RUAN <span class="text-cyan-500">ASSISTEC</span></h1>
            <p class="text-slate-400 mb-8 uppercase text-xs tracking-widest font-bold font-mono">Controle de Acesso</p>
            <div class="space-y-4">
                <button onclick="switchView('cliente')" class="w-full bg-cyan-600 text-white font-bold py-4 rounded-2xl hover:bg-cyan-700 transition">SOU CLIENTE (COMPRAR)</button>
                <div class="relative py-4 text-slate-300">
                    <span class="bg-white px-2 relative z-10 text-xs font-bold uppercase">Ou Admin</span>
                    <hr class="absolute top-1/2 w-full border-slate-100">
                </div>
                <input type="password" id="login-pass" class="w-full p-4 bg-slate-50 border border-slate-200 rounded-2xl focus:ring-2 focus:ring-cyan-500 outline-none" placeholder="Senha do Administrador">
                <button onclick="checkLogin()" class="w-full bg-slate-900 text-white font-bold py-4 rounded-2xl hover:bg-slate-800 transition">ACESSAR PAINEL ADM</button>
            </div>
            <p id="login-error" class="text-red-500 text-sm mt-4 hidden">Senha incorreta!</p>
        </div>
    </div>

    <div id="admin-view" class="view">
        <header class="bg-slate-900 text-white p-5 shadow-xl border-b-4 border-cyan-500 sticky top-0 z-50 flex justify-between items-center">
            <h1 class="text-xl font-black">RUAN <span class="text-cyan-400">ADM</span></h1>
            <nav class="space-x-4 flex items-center">
                <button onclick="switchTab('os')" class="text-xs font-bold">ORDENS</button>
                <button onclick="switchTab('produtos')" class="text-xs font-bold">ESTOQUE</button>
                <button onclick="logout()" class="text-red-400"><i class="fas fa-power-off"></i></button>
            </nav>
        </header>

        <main class="container mx-auto p-4 md:p-8">
            <div id="tab-os" class="tab-content">
                <div class="grid grid-cols-1 md:grid-cols-2 gap-4 mb-6">
                    <div class="bg-white p-6 rounded-2xl shadow-sm border-l-4 border-cyan-500">
                        <p class="text-slate-500 text-xs font-bold uppercase">Ordens Totais</p>
                        <h3 id="adm-count-os" class="text-2xl font-bold">0</h3>
                    </div>
                    <div class="bg-white p-6 rounded-2xl shadow-sm border-l-4 border-red-500">
                        <p class="text-slate-500 text-xs font-bold uppercase">Estoque Baixo</p>
                        <h3 id="adm-count-alert" class="text-2xl font-bold">0</h3>
                    </div>
                </div>
                <section class="bg-white p-6 rounded-2xl shadow border mb-6">
                    <h2 class="font-bold mb-4">Nova Ordem de Serviço</h2>
                    <form id="osForm" class="grid grid-cols-1 md:grid-cols-4 gap-3">
                        <input type="text" id="os-cliente" placeholder="Cliente" class="p-3 border rounded-xl outline-none" required>
                        <input type="number" id="os-whats" placeholder="WhatsApp (DDD)" class="p-3 border rounded-xl outline-none" required>
                        <input type="text" id="os-aparelho" placeholder="Aparelho" class="p-3 border rounded-xl outline-none" required>
                        <button class="bg-cyan-600 text-white font-bold rounded-xl py-3">ABRIR OS</button>
                    </form>
                </section>
                <div class="bg-white rounded-2xl shadow overflow-x-auto">
                    <table class="w-full text-left">
                        <thead class="bg-slate-50 text-xs uppercase font-bold text-slate-400"><tr class="border-b"><th class="p-4">ID</th><th class="p-4">Cliente</th><th class="p-4">Equipamento</th><th class="p-4">Status</th><th class="p-4 text-right">Ações</th></tr></thead>
                        <tbody id="osTableBody" class="divide-y divide-slate-100"></tbody>
                    </table>
                </div>
            </div>

            <div id="tab-produtos" class="tab-content hidden">
                <section class="bg-white p-6 rounded-2xl shadow border mb-6">
                    <h2 class="font-bold mb-4 text-slate-800">Adicionar ao Estoque/Loja</h2>
                    <form id="prodForm" class="grid grid-cols-1 md:grid-cols-4 gap-3">
                        <input type="text" id="p-nome" placeholder="Nome do Produto" class="p-3 border rounded-xl outline-none" required>
                        <input type="number" id="p-qtd" placeholder="Qtd" class="p-3 border rounded-xl outline-none" required>
                        <input type="number" step="0.01" id="p-preco" placeholder="Preço (R$)" class="p-3 border rounded-xl outline-none" required>
                        <button class="bg-slate-900 text-white font-bold rounded-xl py-3">SALVAR PEÇA</button>
                    </form>
                </section>
                <div class="bg-white rounded-2xl shadow overflow-hidden">
                    <table class="w-full text-left">
                        <thead class="bg-slate-50 text-xs uppercase font-bold text-slate-400 border-b"><tr><th class="p-4">Produto</th><th class="p-4">Qtd</th><th class="p-4 text-right">Preço</th><th class="p-4 text-right">Ação</th></tr></thead>
                        <tbody id="prodTableBody" class="divide-y divide-slate-100"></tbody>
                    </table>
                </div>
            </div>
        </main>
    </div>

    <div id="cliente-view" class="view">
        <header class="bg-white p-5 shadow-md border-b flex justify-between items-center sticky top-0 z-50">
            <h1 class="text-xl font-black text-slate-800">RUAN <span class="text-cyan-500">LOJA</span></h1>
            <div class="flex gap-4 items-center">
                <button onclick="openCart()" class="relative p-2 bg-slate-100 rounded-full text-slate-700">
                    <i class="fas fa-shopping-cart text-lg"></i>
                    <span id="cart-count" class="cart-badge">0</span>
                </button>
                <button onclick="logout()" class="text-slate-400 text-xs font-bold uppercase">Sair</button>
            </div>
        </header>

        <main class="container mx-auto p-4 md:p-8">
            <h2 class="text-2xl font-bold text-slate-800 mb-6">Produtos Disponíveis</h2>
            <div id="loja-grid" class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-4 gap-6">
                </div>
        </main>

        <div id="cart-modal" class="fixed inset-0 bg-black bg-opacity-50 z-[100] hidden items-end md:items-center justify-center p-4">
            <div class="bg-white w-full max-w-lg rounded-t-3xl md:rounded-3xl p-6 shadow-2xl overflow-y-auto max-h-[90vh]">
                <div class="flex justify-between items-center mb-6 border-b pb-4">
                    <h2 class="text-xl font-black">Meu Carrinho</h2>
                    <button onclick="closeCart()" class="text-slate-400 text-2xl">&times;</button>
                </div>
                
                <div id="cart-items" class="space-y-4 mb-6">
                    </div>

                <div class="border-t pt-4 space-y-4">
                    <div class="flex justify-between font-bold text-lg">
                        <span>Total:</span>
                        <span id="cart-total" class="text-cyan-600">R$ 0,00</span>
                    </div>

                    <form id="checkoutForm" class="space-y-3">
                        <label class="block text-xs font-bold text-slate-500 uppercase">Horário de Busca</label>
                        <input type="time" id="c-hora" class="w-full p-3 bg-slate-50 border rounded-xl outline-none" required>
                        
                        <label class="block text-xs font-bold text-slate-500 uppercase">Forma de Pagamento</label>
                        <select id="c-pgto" class="w-full p-3 bg-slate-50 border rounded-xl outline-none" required>
                            <option value="Pix">Pix</option>
                            <option value="Dinheiro">Dinheiro</option>
                            <option value="Cartão de Crédito/Débito">Cartão de Crédito/Débito</option>
                        </select>

                        <button type="submit" class="w-full bg-green-500 text-white font-bold py-4 rounded-2xl hover:bg-green-600 transition shadow-lg shadow-green-100">FINALIZAR PEDIDO NO WHATSAPP</button>
                    </form>
                </div>
            </div>
        </div>
    </div>

    <script>
        const SENHA_ADM = "admin123";
        let ordens = JSON.parse(localStorage.getItem('r_db_os')) || [];
        let produtos = JSON.parse(localStorage.getItem('r_db_prod')) || [];
        let carrinho = [];

        // NAVEGAÇÃO
        function switchView(view) {
            document.querySelectorAll('.view').forEach(v => v.classList.remove('active'));
            document.getElementById(view + '-view').classList.add('active');
            renderAll();
        }

        function checkLogin() {
            const p = document.getElementById('login-pass').value;
            if(p === SENHA_ADM) { switchView('admin'); } 
            else { document.getElementById('login-error').classList.remove('hidden'); }
        }

        function logout() { location.reload(); }

        function switchTab(tab) {
            document.querySelectorAll('.tab-content').forEach(t => t.classList.add('hidden'));
            document.getElementById('tab-' + tab).classList.remove('hidden');
        }

        // ADM: PRODUTOS
        document.getElementById('prodForm').addEventListener('submit', (e) => {
            e.preventDefault();
            produtos.push({
                id: Date.now(),
                nome: document.getElementById('p-nome').value,
                qtd: parseInt(document.getElementById('p-qtd').value),
                preco: parseFloat(document.getElementById('p-preco').value)
            });
            save();
            e.target.reset();
        });

        // ADM: ORDENS
        document.getElementById('osForm').addEventListener('submit', (e) => {
            e.preventDefault();
            ordens.unshift({
                id: Math.floor(1000 + Math.random() * 9000),
                cliente: document.getElementById('os-cliente').value,
                whatsapp: document.getElementById('os-whats').value,
                aparelho: document.getElementById('os-aparelho').value,
                status: 'Pendente'
            });
            save();
            e.target.reset();
        });

        // CLIENTE: CARRINHO
        function addToCart(prodId) {
            const p = produtos.find(item => item.id === prodId);
            if(p.qtd <= 0) return alert('Produto esgotado!');
            
            const noCarrinho = carrinho.find(c => c.id === prodId);
            if(noCarrinho) { noCarrinho.qtdCarrinho++; } 
            else { carrinho.push({...p, qtdCarrinho: 1}); }
            
            renderAll();
        }

        function removeFromCart(index) {
            carrinho.splice(index, 1);
            renderAll();
        }

        function openCart() { document.getElementById('cart-modal').classList.remove('hidden'); document.getElementById('cart-modal').classList.add('flex'); }
        function closeCart() { document.getElementById('cart-modal').classList.add('hidden'); }

        document.getElementById('checkoutForm').addEventListener('submit', (e) => {
            e.preventDefault();
            if(carrinho.length === 0) return alert('Carrinho vazio!');
            
            const hora = document.getElementById('c-hora').value;
            const pgto = document.getElementById('c-pgto').value;
            let msg = `*NOVO PEDIDO - RUAN ASSISTEC*%0A%0A`;
            let total = 0;
            
            carrinho.forEach(i => {
                msg += `• ${i.nome} (${i.qtdCarrinho}x) - R$ ${i.preco}%0A`;
                total += i.preco * i.qtdCarrinho;
            });
            
            msg += `%0A*Total:* R$ ${total.toFixed(2)}`;
            msg += `%0A*Busca:* ${hora}`;
            msg += `%0A*Pagamento:* ${pgto}`;
            
            window.open(`https://wa.me/5521999999999?text=${msg}`); // Troque pelo seu número
            carrinho = [];
            closeCart();
            renderAll();
        });

        // RENDERIZAÇÃO
        function save() {
            localStorage.setItem('r_db_os', JSON.stringify(ordens));
            localStorage.setItem('r_db_prod', JSON.stringify(produtos));
            renderAll();
        }

        function renderAll() {
            // ADM OS
            const osBody = document.getElementById('osTableBody');
            if(osBody) {
                osBody.innerHTML = ordens.map((os, i) => `
                    <tr class="border-b">
                        <td class="p-4 text-xs font-mono">#${os.id}</td>
                        <td class="p-4 font-bold text-slate-700">${os.cliente}<br><span class="text-[10px] text-slate-400">${os.whatsapp}</span></td>
                        <td class="p-4">${os.aparelho}</td>
                        <td class="p-4"><span class="px-3 py-1 text-[10px] font-black rounded-full ${os.status === 'Concluído' ? 'bg-green-100 text-green-700' : 'bg-amber-100 text-amber-700'}">${os.status}</span></td>
                        <td class="p-4 text-right">
                            <button onclick="mudarStatus(${i})" class="text-cyan-500 mr-2"><i class="fas fa-sync"></i></button>
                            <button onclick="deletarOS(${i})" class="text-red-300"><i class="fas fa-trash"></i></button>
                        </td>
                    </tr>`).join('');
            }

            // ADM PRODUTOS
            const prBody = document.getElementById('prodTableBody');
            if(prBody) {
                prBody.innerHTML = produtos.map((p, i) => `
                    <tr class="border-b">
                        <td class="p-4 font-bold">${p.nome}</td>
                        <td class="p-4">${p.qtd}</td>
                        <td class="p-4 text-right font-bold text-green-600">R$ ${p.preco.toFixed(2)}</td>
                        <td class="p-4 text-right"><button onclick="deletarProd(${i})" class="text-red-400"><i class="fas fa-times"></i></button></td>
                    </tr>`).join('');
            }

            // LOJA CLIENTE
            const grid = document.getElementById('loja-grid');
            if(grid) {
                grid.innerHTML = produtos.map(p => `
                    <div class="bg-white p-4 rounded-3xl shadow-sm border border-slate-100 flex flex-col justify-between">
                        <div>
                            <div class="w-full h-32 bg-slate-50 rounded-2xl mb-4 flex items-center justify-center text-slate-200">
                                <i class="fas fa-box-open text-4xl"></i>
                            </div>
                            <h3 class="font-bold text-slate-800">${p.nome}</h3>
                            <p class="text-xs text-slate-400 mb-2">${p.qtd} em estoque</p>
                        </div>
                        <div class="flex items-center justify-between mt-4">
                            <span class="font-black text-cyan-600">R$ ${p.preco.toFixed(2)}</span>
                            <button onclick="addToCart(${p.id})" class="bg-slate-900 text-white p-2 px-4 rounded-xl text-xs font-bold hover:bg-cyan-600 transition">+ Carrinho</button>
                        </div>
                    </div>`).join('');
            }

            // MODAL CARRINHO
            const cartItems = document.getElementById('cart-items');
            if(cartItems) {
                let total = 0;
                cartItems.innerHTML = carrinho.map((c, i) => {
                    total += c.preco * c.qtdCarrinho;
                    return `
                    <div class="flex justify-between items-center bg-slate-50 p-3 rounded-xl">
                        <div>
                            <p class="font-bold text-sm">${c.nome}</p>
                            <p class="text-xs text-slate-500">${c.qtdCarrinho}x de R$ ${c.preco.toFixed(2)}</p>
                        </div>
                        <button onclick="removeFromCart(${i})" class="text-red-400"><i class="fas fa-trash"></i></button>
                    </div>`;
                }).join('');
                document.getElementById('cart-total').innerText = `R$ ${total.toFixed(2)}`;
                document.getElementById('cart-count').innerText = carrinho.length;
            }

            // DASHBOARD ADM
            if(document.getElementById('adm-count-os')) {
                document.getElementById('adm-count-os').innerText = ordens.length;
                document.getElementById('adm-count-alert').innerText = produtos.filter(p => p.qtd < 3).length;
            }
        }

        window.mudarStatus = (i) => { ordens[i].status = ordens[i].status === 'Pendente' ? 'Concluído' : 'Pendente'; save(); };
        window.deletarOS = (i) => { if(confirm('Remover OS?')) { ordens.splice(i, 1); save(); } };
        window.deletarProd = (i) => { if(confirm('Remover Produto?')) { produtos.splice(i, 1); save(); } };

        renderAll();
    </script>
</body>
</html>
