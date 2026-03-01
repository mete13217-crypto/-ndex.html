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
        #top-status { position: absolute; top: 0; width: 100%; height: 65px; background: rgba(0,0,0,0.95); display: flex; align-items: center; justify-content: space-around; z-index: 100; border-bottom: 2px solid #ff0000; }
        .stat-box { text-align: center; flex: 1; padding: 0 5px; }
        .stat-label { font-size: 0.55rem; color: #888; margin-bottom: 3px; }
        .stat-bar-bg { width: 100%; height: 8px; background: #222; border-radius: 4px; overflow: hidden; border: 1px solid #444; }
        .stat-bar-fill { height: 100%; width: 0%; transition: 0.6s; }
        .screen { display: none; position: absolute; inset: 0; flex-direction: column; background: black; z-index: 10; }
        .active { display: flex; }
        #chat-window { flex: 1; overflow-y: auto; padding: 20px 15px; display: flex; flex-direction: column; gap: 15px; margin-top: 65px; background: radial-gradient(circle, #1a0000 0%, #000 100%); }
        .msg { max-width: 85%; padding: 12px; border: 2px solid white; border-radius: 4px; animation: fadeIn 0.3s; word-wrap: break-word; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; } }
        .msg.user { align-self: flex-end; border-color: var(--soul); color: #ff8888; }
        .msg.bot { align-self: flex-start; background: rgba(20,20,20,0.95); }
        .msg.system { align-self: center; border: 1px dashed #666; color: #ff69b4; font-size: 0.7rem; text-align: center; }
        .msg img { width: 100%; border-radius: 4px; margin-top: 10px; border: 1px solid #444; }
        .ui-panel { background: #000; border-top: 2px solid #333; padding: 10px; z-index: 110; }
        .input-wrap { display: flex; gap: 8px; margin-bottom: 10px; }
        #chatInput { flex: 1; background: #151515; border: 1px solid #444; color: white; padding: 12px; font-family: 'Pixel'; outline: none; }
        .send-btn { background: var(--soul); color: white; border: none; padding: 0 20px; font-family: 'Pixel'; cursor: pointer; }
        .actions { display: flex; justify-content: center; gap: 10px; flex-wrap: wrap; }
        .act-btn { padding: 8px 12px; background: #111; border: 2px solid #444; color: white; font-family: 'Pixel'; cursor: pointer; font-size: 0.7rem; }
        .char-card { width: 90%; padding: 15px; margin: 10px auto; border: 2px solid #333; display: flex; align-items: center; gap: 15px; cursor: pointer; background: #0a0a0a; }
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
        <h1 style="color:red; font-size: 2.5rem; text-align:center;">ROARING SINS</h1>
        <button onclick="switchScreen('select-screen')" style="border:2px solid red; color:red; padding:15px; background:none; font-family:'Pixel';">BAŞLAT</button>
    </div>
    <div id="select-screen" class="screen"><div id="char-list" style="overflow-y:auto; margin-top:80px;"></div></div>
    <div id="play-screen" class="screen">
        <div id="chat-window"></div>
        <div class="ui-panel">
            <div class="input-wrap"><input type="text" id="chatInput" placeholder="Mesaj yaz..."><button class="send-btn" onclick="handleSend()">Z</button></div>
            <div class="actions">
                <button class="act-btn" onclick="quickAction('love')">HEDİYE</button>
                <button class="act-btn" onclick="quickAction('lust')">KIŞKIRT</button>
                <button class="act-btn" onclick="requestPhoto(true)">+18 FOTO 🔞</button>
            </div>
        </div>
    </div>
</div>
<script>
let partner = null; let stats = { love: 10, lust: 0, fear: 0 };
const DB = [
    { id: "sans", name: "Sans", color: "sans", img: "https://utdr-pfp-crop.glitch.me/ut/sans.png", sprite: "https://static.wikia.nocookie.net/undertale/images/7/7f/Sans_portrait.png" },
    { id: "noelle", name: "Noelle", color: "noelle", img: "https://utdr-pfp-crop.glitch.me/dr/noelle.png", sprite: "https://static.wikia.nocookie.net/deltarune/images/8/82/Noelle_portrait.png" },
    { id: "susie", name: "Susie", color: "susie", img: "https://utdr-pfp-crop.glitch.me/dr/susie.png", sprite: "https://static.wikia.nocookie.net/deltarune/images/2/23/Susie_portrait.png" }
];
function renderList() { document.getElementById('char-list').innerHTML = DB.map((c, i) => `<div class="char-card" onclick="selectChar(${i})"><img src="${c.img}"><b>${c.name.toUpperCase()}</b></div>`).join(''); }
function selectChar(i) { partner = DB[i]; switchScreen('play-screen'); addMsg(`* ${partner.name} bağlandı.`, 'system'); updateHUD(); }
function handleSend() {
    const input = document.getElementById('chatInput'); const val = input.value.trim(); if(!val) return;
    addMsg(val, 'user'); input.value = "";
    if(val.toLowerCase().includes("+18")) { requestPhoto(true); return; }
    stats.lust += 5; updateHUD();
    setTimeout(() => { addMsg(`* ${partner.name} sana garip bir şekilde bakıyor.`, partner.id); }, 800);
}
function requestPhoto(nsfw) {
    if(nsfw && stats.lust < 30) { addMsg("* Biraz daha yakınlaşmalıyız...", 'system'); return; }
    let url = nsfw ? `https://rule34.xxx/index.php?page=post&s=list&tags=${partner.id}` : `https://www.google.com/search?q=${partner.id}+fanart`;
    addMsg(`* ${partner.name} fotoğraf gönderdi:`, partner.id, partner.sprite);
    addMsg(`[ARŞİVİ AÇ: <a href="${url}" target="_blank" style="color:white">TIKLA</a>]`, 'system');
}
function quickAction(t) { stats[t] = Math.min(stats[t]+20, 100); updateHUD(); }
function updateHUD() { document.getElementById('love-bar').style.width=stats.love+"%"; document.getElementById('lust-bar').style.width=stats.lust+"%"; document.getElementById('fear-bar').style.width=stats.fear+"%"; }
function addMsg(t, type, img) {
    const win = document.getElementById('chat-window'); const div = document.createElement('div');
    div.className = `msg ${type}`; div.innerHTML = t;
    if(img) { const i = new Image(); i.src = img; div.appendChild(i); }
    win.appendChild(div); win.scrollTop = win.scrollHeight;
}
function switchScreen(id) { document.querySelectorAll('.screen').forEach(s => s.classList.remove('active')); document.getElementById(id).classList.add('active'); if(id==='select-screen') renderList(); }
</script>
</body>
</html>
