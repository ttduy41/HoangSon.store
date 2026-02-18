<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>VINHMODLAG.STORE | FULL VERSION</title>
    <script src="https://www.gstatic.com/firebasejs/9.17.1/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.17.1/firebase-auth-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.17.1/firebase-database-compat.js"></script>
    <style>
        :root { --primary: #00f2ff; --accent: #ff0055; --bg: #0d1117; --card: #161b22; --text: #c9d1d9; }
        body { font-family: 'Segoe UI', sans-serif; background: var(--bg); color: var(--text); margin: 0; padding-bottom: 50px; }
        input, select, textarea { width: 100%; padding: 12px; margin: 10px 0; border-radius: 8px; border: 1px solid #30363d; background: #0d1117; color: white; box-sizing: border-box; }
        .btn { width: 100%; padding: 12px; border: none; border-radius: 8px; cursor: pointer; font-weight: bold; transition: 0.3s; margin-top: 10px; }
        .btn-main { background: var(--primary); color: #000; }
        .container { max-width: 1000px; margin: auto; padding: 20px; }
        header { background: var(--card); padding: 15px 5%; border-bottom: 1px solid #30363d; display: flex; justify-content: space-between; align-items: center; position: sticky; top:0; z-index: 100; }
        
        /* CHIA KHU VỰC */
        .section-title { border-left: 4px solid var(--primary); padding-left: 10px; margin: 30px 0 15px 0; color: var(--primary); text-transform: uppercase; }
        .card { background: var(--card); padding: 20px; border-radius: 12px; border: 1px solid #30363d; margin-bottom: 20px; }
        .grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(300px, 1fr)); gap: 20px; }
        
        #auth-overlay { position: fixed; inset: 0; background: var(--bg); display: flex; justify-content: center; align-items: center; z-index: 10000; }
        #admin-panel { display: none; border: 2px solid var(--accent); padding: 20px; background: #1a0b0b; border-radius: 15px; margin-bottom: 40px; }
        .history-item { padding: 10px; border-bottom: 1px solid #333; font-size: 14px; }
    </style>
</head>
<body>

    <div id="auth-overlay">
        <div style="background: var(--card); padding: 30px; border-radius: 15px; width: 90%; max-width: 400px; text-align: center;">
            <h2 style="color: var(--primary);">VINHMODLAG.STORE</h2>
            <input type="text" id="u-user" placeholder="Email đăng nhập">
            <input type="password" id="u-pass" placeholder="Mật khẩu">
            <button class="btn btn-main" onclick="handleAuth()">VÀO SHOP</button>
        </div>
    </div>

    <div id="main-shop" style="display: none;">
        <header>
            <h3 style="margin:0;">VINH MOD</h3>
            <div style="background: #238636; padding: 6px 15px; border-radius: 20px; font-weight: bold;">💰 <span id="u-bal">0</span>đ</div>
        </header>

        <div class="container">
            <div id="admin-panel">
                <h3 style="color: var(--accent);">🛠 KHU VỰC DUYỆT TIỀN & KHO</h3>
                <div style="background: #000; padding: 15px; border-radius: 10px; margin-bottom: 15px;">
                    <p><b>CỘNG TIỀN CHO KHÁCH:</b></p>
                    <input type="text" id="adm-email" placeholder="Nhập Email khách cần cộng tiền">
                    <input type="number" id="adm-money" placeholder="Nhập số tiền cần cộng">
                    <button class="btn" style="background: #ffaa00;" onclick="adminAddMoney()">XÁC NHẬN CỘNG TIỀN</button>
                </div>
                <hr>
                <p><b>CẬP NHẬT KHO:</b></p>
                <select id="adm-sel"><option value="acc">ACC CLONE LV 5</option><option value="br">MENU BR MOD</option><option value="lag">FAKE LAG ADR</option></select>
                <input type="number" id="adm-p-vv" placeholder="Giá vĩnh viễn">
                <textarea id="adm-input" rows="3" placeholder="Dán danh sách mã vào đây..."></textarea>
                <button class="btn btn-main" style="background: white;" onclick="admUpdate()">LƯU KHO</button>
            </div>

            <h2 class="section-title">🛒 Khu 1: Mua đồ (Mod & Acc)</h2>
            <div class="grid" id="product-grid"></div>

            <h2 class="section-title">💳 Khu 2: Nạp tiền vào tài khoản</h2>
            <div class="card" style="text-align: center;">
                <p>Quét mã QR dưới đây để Bank cho Admin (Duyệt nhanh 1-2 phút)</p>
                <img src="https://i.ibb.co/LhyS3XyN/1000093692.jpg" style="width: 250px; border-radius: 10px; border: 5px solid white;">
                <div style="text-align: left; background: #002b36; padding: 15px; margin-top: 15px; border-radius: 10px;">
                    <p>Nội dung chuyển khoản: <b style="color: yellow;" id="ck-nd">NAP TIEN EMAIL</b></p>
                    <p><i>Sau khi chuyển khoản, hãy gửi yêu cầu dưới đây:</i></p>
                    <input type="number" id="bank-val" placeholder="Nhập số tiền vừa bank">
                    <button class="btn btn-main" onclick="confirmBank()">GỬI YÊU CẦU DUYỆT TIỀN</button>
                </div>
            </div>

            <h2 class="section-title">📜 Khu 3: Lịch sử giao dịch</h2>
            <div class="grid">
                <div class="card">
                    <h4>Lịch sử mua đồ</h4>
                    <div id="buy-hist">Chưa có giao dịch</div>
                </div>
                <div class="card">
                    <h4>Lịch sử nạp tiền</h4>
                    <div id="nap-hist">Chưa có yêu cầu</div>
                </div>
            </div>
        </div>
    </div>

    <script>
        const firebaseConfig = {
            apiKey: "AIzaSyDQh9wzSEwVEYe0VbPyfTnWi5aGS-lo7iY",
            authDomain: "vinhmod-e6ebe.firebaseapp.com",
            databaseURL: "https://vinhmod-e6ebe-default-rtdb.firebaseio.com",
            projectId: "vinhmod-e6ebe",
            appId: "1:280126075845:web:7d443d6ca997d5aa37d1ac"
        };
        firebase.initializeApp(firebaseConfig);
        const auth = firebase.auth(); const db = firebase.database();
        let uKey = ""; let userEmail = ""; let isAdmin = false;

        function handleAuth() {
            userEmail = document.getElementById('u-user').value.trim();
            const pass = document.getElementById('u-pass').value;
            if(userEmail === "HoangVinh@vinh.store" && pass === "HV0308hv") { isAdmin = true; uKey = "admin"; startShop(); return; }
            auth.signInWithEmailAndPassword(userEmail, pass).then(res => { uKey = res.user.uid; startShop(); })
                .catch(() => { auth.createUserWithEmailAndPassword(userEmail, pass).then(res => { uKey = res.user.uid; startShop(); }); });
        }

        function startShop() {
            document.getElementById('auth-overlay').style.display = 'none';
            document.getElementById('main-shop').style.display = 'block';
            if(isAdmin) document.getElementById('admin-panel').style.display = 'block';
            document.getElementById('ck-nd').innerText = "NAP TIEN " + userEmail;
            
            db.ref('users/'+uKey+'/bal').on('value', s => document.getElementById('u-bal').innerText = (s.val() || 0).toLocaleString());
            db.ref('prods').on('value', s => renderProds(s.val()));
            db.ref('users/'+uKey+'/history').on('value', s => renderHistory(s.val(), 'buy-hist'));
            db.ref('users/'+uKey+'/nap_logs').on('value', s => renderHistory(s.val(), 'nap-hist'));
        }

        function renderProds(data) {
            const grid = document.getElementById('product-grid'); grid.innerHTML = "";
            const items = [{id:'acc', n:'ACC CLONE LV 5'}, {id:'br', n:'MENU BR MOD'}, {id:'lag', n:'FAKE LAG ADR'}];
            items.forEach(p => {
                const d = (data && data[p.id]) ? data[p.id] : {pvv: 0, items: []};
                grid.innerHTML += `<div class="card">
                    <h3>${p.n}</h3>
                    <p style="color:var(--primary); font-size: 20px; font-weight:bold;">${parseInt(d.pvv).toLocaleString()}đ</p>
                    <p style="font-size:12px;">Kho còn: ${(d.items||[]).length}</p>
                    <button class="btn btn-main" onclick="buy('${p.id}')">MUA VĨNH VIỄN</button>
                </div>`;
            });
        }

        function renderHistory(data, elementId) {
            const div = document.getElementById(elementId);
            if(!data) return;
            div.innerHTML = Object.values(data).reverse().map(i => `<div class="history-item">⏰ ${i.time}<br>➡️ ${i.msg}</div>`).join('');
        }

        function confirmBank() {
            const val = document.getElementById('bank-val').value;
            if(!val) return;
            const log = { time: new Date().toLocaleString(), msg: "Yêu cầu nạp " + parseInt(val).toLocaleString() + "đ (Chờ Duyệt)", email: userEmail, uid: uKey, amount: val };
            db.ref('users/'+uKey+'/nap_logs').push(log);
            db.ref('admin_requests').push(log); // Gửi tới khu Admin
            alert("Đã gửi yêu cầu! Duy sẽ duyệt sớm cho bạn.");
        }

        function adminAddMoney() {
            const targetEmail = document.getElementById('adm-email').value;
            const money = parseInt(document.getElementById('adm-money').value);
            // Tìm user qua email (Duy duyệt tay)
            alert("Để cộng tiền: Duy vào Firebase -> Realtime Database -> Tìm User có email này và sửa Bal (Số dư) nhé. Hoặc Duy copy UID khách dán vào code để nạp tự động.");
        }

        function admUpdate() {
            const id = document.getElementById('adm-sel').value;
            const items = document.getElementById('adm-input').value.split('\n').filter(i => i.trim() !== "");
            db.ref('prods/'+id).set({ pvv: document.getElementById('adm-p-vv').value, items: items });
            alert("Đã cập nhật kho!");
        }

        function buy(id) {
            db.ref('prods/'+id).once('value', s => {
                const d = s.val(); if(!d || !d.items) return alert("Hết hàng!");
                db.ref('users/'+uKey+'/bal').once('value', b => {
                    const bal = b.val() || 0;
                    if(bal < d.pvv && !isAdmin) return alert("Không đủ tiền!");
                    const gift = d.items.shift();
                    db.ref('prods/'+id+'/items').set(d.items);
                    if(!isAdmin) db.ref('users/'+uKey+'/bal').set(bal - d.pvv);
                    db.ref('users/'+uKey+'/history').push({ time: new Date().toLocaleString(), msg: "Mua " + id + ": " + gift });
                    alert("THÀNH CÔNG! Mã: " + gift);
                });
            });
        }
    </script>
</body>
</html>
