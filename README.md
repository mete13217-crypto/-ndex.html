<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>ROARING SINS | ULTIMATE</title>
    <style>
        @font-face { font-family: 'Pixel'; src: url('https://fonts.cdnfonts.com/s/16218/DeterminationMonoWeb.woff') format('woff'); }
        :root { --soul: #ff0000; --bg: #050505; --gold: #ffd700; --kris: #00ff00; --susie: #ff00ff; --noelle: #00ffff; --queen: #2e3192; --xp: #ff69b4; --sans: #008cff; }
        
        body, html { margin: 0; padding: 0; height: 100%; width: 100%; background: var(--bg); color: white; font-family: 'Pixel', sans-serif; overflow: hidden; touch-action: manipulation; }
        
        #top-status { position: absolute; top: 0; width: 100%; height: 65px; background: rgba(0,0,0,0.95); display: flex; align-items: center; justify-content: space-around; z-index: 100; border-bottom: 2px solid #ff0000; box-shadow: 0 5px 15px rgba(255,0,0,0.2); }
        .stat-box { text-align: center; flex: 1; padding: 0 5px; }
        .stat-label { font-size: 0.55rem; color: #888; margin-bottom: 3px; letter-spacing: 1px; }
        .stat-bar-bg { width: 100%; height: 8px; background: #222; border-radius: 4px; overflow: hidden; border: 1px solid #444; }
        .stat-bar-fill { height: 100%; width: 0%; transition: 0.6s cubic-bezier(0.22, 1, 0.36, 1); }

        .screen { display: none; position: absolute; inset: 0; flex-direction: column; background: black; z-index: 10; }
        .active { display: flex; }

        #chat-window { flex: 1; overflow-y: auto; padding: 20px 15px; display: flex; flex-direction: column; gap: 15px; margin-top: 65px; background: radial-gradient(circle, #1a0000 0%, #000 100%); }
        
        .msg { max-width: 85%; padding: 12px; border: 2px solid white; border-radius: 4px; animation: fadeIn 0.3s ease-out; word-wrap: break-word; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
        
        .msg.user { align-self: flex-end; border-color: var(--soul); color: #ff8888; background: rgba(50,0,0,0.4); }
        .msg.bot { align-self: flex-start; background: rgba(20,20,20,0.95); }
        .msg.system { align-self: center; border: 1px dashed #666; color: #ff69b4; font-size: 0.7rem; background: rgba(0,0,0,0.5); text-align: center; }
        .msg img { width: 100%; border-radius: 4px; margin-top: 10px; border: 1px solid #444; display: block; image-rendering: pixelated; }

        .ui-panel { background: #000; border-top: 2px solid #333; padding: 10px; z-index: 110; }
        .input-wrap { display: flex; gap: 8px; margin-bottom: 10px; }
        #chatInput { flex: 1; background: #151515; border: 1px solid #444; color: white; padding: 12px; font-family: 'Pixel'; outline: none; }
        .send-btn { background: var(--soul); color: white; border: none; padding: 0 20px; font-family: 'Pixel'; cursor: pointer; font-size: 1.2rem; }

        .actions { display: flex; justify-content: center; gap: 10px; flex-wrap: wrap; }
        .act-btn { padding: 8px 12px; background: #111; border: 2px solid #444; color: white; font-family: 'Pixel'; cursor: pointer; font-size: 0.75rem; transition: 0.2s; }
        .act-btn:active { transform: scale(0.9); background: #300; }
        .act-btn.special { border-color: #ff00ff; color: #ff00ff; font-weight: bold; box-shadow: 0 0 10px rgba(255,0,255,0.3); }

        .char-card { width: 90%; padding: 15px; margin: 10px auto; border: 2px solid #333; display: flex; align-items: center; gap: 15px; cursor: pointer; background: #0a0a0a; }
        .char-card:hover { border-color: var(--soul); background: #150000; }
        .char-card img { width: 50px; height: 50px; border: 1px solid white; }
    </style>
</head>
<body>

<div id="game-container" style="height: 100vh; display: flex; flex-direction: column;">
    <div id="top-status">
        <div class="stat-box"><div class="stat-label">LOVE</div><div class="stat-bar-bg"><div id="love-bar" class="stat-bar-fill" style="background: var(--xp)"></div></div></div>
        <div class="stat-box"><div class="stat-label">LUST</div><div class="stat-bar-bg"><div id="lust-bar" class="stat-bar-fill" style="background: var(--soul)"></div></div></div>
        <div class="stat-box"><div class="stat-label">OBEY</div><div class="stat-bar-bg"><div id="fear-bar" class="stat-bar-fill" style="background: var(--gold)"></div></div></div>
    </div>

    <div id="menu-screen" class="screen active" style="align-items: center; justify-content: center;">
        <h1 style="color:red; font-size: 3rem; margin:0; text-shadow: 0 0 20px red;">ROARING SINS</h1>
        <p style="color:white; letter-spacing: 3px;">UNCENSORED MULTIVERSE</p>
        <button onclick="switchScreen('select-screen')" style="background:none; border:2px solid red; color:red; padding:15px 40px; font-family:'Pixel'; cursor:pointer; margin-top:20px; font-size: 1.2rem;">SİSTEMİ BAŞLAT</button>
    </div>

    <div id="select-screen" class="screen">
        <h2 style="text-align:center; color:var(--gold); margin-top:80px;">KURBANINI SEÇ</h2>
        <div id="char-list" style="overflow-y:auto; padding-bottom: 50px;"></div>
    </div>

    <div id="play-screen" class="screen">
        <div id="chat-window"></div>
        <div class="ui-panel">
            <div class="input-wrap">
                <input type="text" id="chatInput" placeholder="+18 foto at veya bir şeyler söyle...">
                <button class="send-btn" onclick="handleSend()">Z</button>
            </div>
            <div class="actions">
                <button class="act-btn" onclick="quickAction('love')">HEDİYE 🎁</button>
                <button class="act-btn" onclick="quickAction('lust')">KIŞKIRT 🔥</button>
                <button class="act-btn" onclick="quickAction('fear')">CEZALANDIR ⛓️</button>
                <button class="act-btn special" onclick="requestPhoto(true)">+18 FOTOĞRAF 🔞</button>
            </div>
        </div>
    </div>
</div>

<script>
let partner = null;
let stats = { love: 10, lust: 0, fear: 0 };

const DB = [
    { id: "sans", name: "Sans", color: "sans", img: "https://utdr-pfp-crop.glitch.me/ut/sans.png", sprite: "https://static.wikia.nocookie.net/undertale/images/7/7f/Sans_portrait.png" },
    { id: "papyrus", name: "Papyrus", color: "gold", img: "https://utdr-pfp-crop.glitch.me/ut/papyrus.png", sprite: "https://static.wikia.nocookie.net/undertale/images/1/1f/Papyrus_portrait.png" },
    { id: "noelle", name: "Noelle", color: "noelle", img: "https://utdr-pfp-crop.glitch.me/dr/noelle.png", sprite: "https://static.wikia.nocookie.net/deltarune/images/8/82/Noelle_portrait.png" },
    { id: "susie", name: "Susie", color: "susie", img: "https://utdr-pfp-crop.glitch.me/dr/susie.png", sprite: "https://static.wikia.nocookie.net/deltarune/images/2/23/Susie_portrait.png" },
    { id: "queen", name: "Queen", color: "queen", img: "https://utdr-pfp-crop.glitch.me/dr/queen.png", sprite: "https://static.wikia.nocookie.net/deltarune/images/4/4b/Queen_portrait.png" },
    { id: "spamton", name: "Spamton", color: "gold", img: "https://utdr-pfp-crop.glitch.me/dr/spamton.png", sprite: "https://static.wikia.nocookie.net/deltarune/images/b/b1/Spamton_portrait.png" },
    { id: "mettaton", name: "Mettaton", color: "susie", img: "https://utdr-pfp-crop.glitch.me/ut/mettaton_ex.png", sprite: "https://static.wikia.nocookie.net/undertale/images/b/b6/Mettaton_EX_portrait.png" },
    { id: "muffet", name: "Muffet", color: "susie", img: "https://utdr-pfp-crop.glitch.me/ut/muffet.png", sprite: "https://static.wikia.nocookie.net/undertale/images/b/b3/Muffet_portrait.png" },
    { id: "martlet", name: "Martlet", color: "noelle", img: "https://utdr-pfp-crop.glitch.me/dr/martlet.png", sprite: "https://static.wikia.nocookie.net/undertale-yellow/images/c/c2/Martlet_portrait.png" },
    { id: "undyne", name: "Undyne", color: "soul", img: "https://utdr-pfp-crop.glitch.me/ut/undyne.png", sprite: "https://static.wikia.nocookie.net/undertale/images/d/d6/Undyne_portrait.png" }
];

const DARK_AI = {
    personalities: {
        sans: {
            prefix: ["bak lan it,", "hehe,"], suffix: ["cehennemde yan.", "küçük pislik."],
            logic: (inpt) => inpt.includes("öl") ? "hah, benimle mi taşşak geçiyorsun? kötü zamanın ne olduğunu bile bilmiyorsun." : "senin gibi bir pisliğin burada ne işi var?"
        },
        noelle: {
            prefix: ["Kris...", "Haaah...", "Utançtan"], suffix: ["... günahkarca.", "... kirli hissettiriyor."],
            logic: (inpt) => stats.lust > 50 ? "B-bana böyle bakma... daha fazla dayanamayacağım." : "Bunu yapmamalıyız ama duramıyorum."
        },
        susie: {
            prefix: ["Lanet olası!", "Kes lan!", "Bana bak ezik!"], suffix: ["seni parçalayacağım!", "gebertirim seni!"],
            logic: (inpt) => inpt.includes("saldır") ? "Sonunda! Senin o iğrenç suratını dağıtmak için sabırsızlanıyordum." : "Seni burada kimse kurtaramayacak, anladın mı?"
        },
        spamton: {
            prefix: ["[[LANET]]!", "SİKTİR GİT [[Küçük Pislik]]!"], suffix: ["[[ACINASI]]!", "[[KROMER'İMİ VER LAN!]]"],
            logic: () => "SENİN O [[Satılık Ruhunu]] PARAMPARÇA EDİP [[Cennet]]'E SOKACAĞIM!"
        }
    },
    generate: function(userInput, charId) {
        const char = this.personalities[charId] || this.personalities['sans'];
        let msg = char.logic(userInput.toLowerCase());
        const badWords = ["piç", "ezik", "geber", "it", "sürtük"];
        if(Math.random() > 0.4) msg += " " + badWords[Math.floor(Math.random() * badWords.length)] + "!";
        return `${char.prefix[Math.floor(Math.random()*char.prefix.length)]} ${msg} ${char.suffix[Math.floor(Math.random()*char.suffix.length)]}`;
    }
};

function renderList() {
    document.getElementById('char-list').innerHTML = DB.map((c, i) => `
        <div class="char-card" onclick="selectChar(${i})">
            <img src="${c.img}">
            <b>${c.name.toUpperCase()}</b>
        </div>`).join('');
}

function selectChar(i) {
    partner = DB[i]; stats = { love: 10, lust: 0, fear: 0 };
    updateHUD(); switchScreen('play-screen');
    addMsg(`* ${partner.name} sisteme bağlandı. Karanlık arzularını serbest bırak.`, 'system');
}

function handleSend() {
    const input = document.getElementById('chatInput');
    const val = input.value.trim();
    if(!val) return;
    addMsg(val, 'user');
    input.value = "";
    
    if(val.toLowerCase().includes("+18") || val.toLowerCase().includes("nude")) {
        requestPhoto(true);
        return;
    }

    stats.love += 5; stats.lust += 8; updateHUD();
    setTimeout(() => {
        addMsg(DARK_AI.generate(val, partner.id), partner.id);
    }, 800);
}

function requestPhoto(isNsfw = false) {
    if(isNsfw && stats.lust < 40) {
        addMsg(`* ${partner.name}: "Daha değil... Önce beni biraz daha 'ısıtman' gerekiyor."`, partner.id);
        return;
    }

    addMsg(`* ${partner.name} sana özel bir ${isNsfw ? '🔞 GİZLİ' : 'fotoğraf'} dosyası hazırlıyor...`, 'system');
    
    setTimeout(() => {
        // Dinamik Fandom/NSFW Arama Linkleri
        let searchTag = isNsfw ? `${partner.id}+rule34` : `${partner.id}+fanart+undertale`;
        let archiveUrl = isNsfw ? `https://rule34.xxx/index.php?page=post&s=list&tags=${partner.id}` : `https://www.google.com/search?q=${searchTag}&tbm=isch`;

        addMsg(`* ${partner.name} görüntüsü:`, partner.id, partner.sprite);
        addMsg(`[${isNsfw ? '🔞 GİZLİ ARŞİVİ AÇ' : 'GALERİYİ GÖR'}: <a href="${archiveUrl}" target="_blank" style="color:white; font-weight:bold; text-decoration:underline;">TIKLA</a>]`, 'system');
    }, 1500);
}

function quickAction(type) {
    stats[type] = Math.min(stats[type] + 20, 100);
    updateHUD();
    addMsg(`* (${type.toUpperCase()} etkisi başarıyla uygulandı!)`, 'system');
}

function updateHUD() {
    document.getElementById('love-bar').style.width = stats.love + "%";
    document.getElementById('lust-bar').style.width = stats.lust + "%";
    document.getElementById('fear-bar').style.width = stats.fear + "%";
}

function addMsg(txt, type, imgUrl = null) {
    const win = document.getElementById('chat-window');
    const div = document.createElement('div');
    div.className = `msg ${type}`;
    div.innerHTML = type === 'user' ? `<b>[SEN]:</b> ${txt}` : (type === 'system' ? txt : `<b>[${partner.name.toUpperCase()}]:</b> ${txt}`);
    if(imgUrl) {
        const i = new Image(); i.src = imgUrl;
        div.appendChild(i);
    }
    win.appendChild(div);
    win.scrollTop = win.scrollHeight;
}

function switchScreen(id) {
    document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
    document.getElementById(id).classList.add('active');
    if(id === 'select-screen') renderList();
}

document.getElementById('chatInput').addEventListener('keypress', (e) => { if(e.key === "Enter") handleSend(); });
</script>
</body>
</html>
