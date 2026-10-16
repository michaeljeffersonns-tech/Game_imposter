[game.html](https://github.com/user-attachments/files/33550369/game.html)
<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Crypto Imposter Adventure</title>
<style>
* { margin: 0; padding: 0; box-sizing: border-box; }
body {
    font-family: 'Segoe UI', Tahoma, sans-serif;
    background: linear-gradient(135deg, #0f0c29, #302b63, #24243e);
    background-attachment: fixed;
    min-height: 100vh;
    display: flex; align-items: center; justify-content: center;
    padding: 10px; color: white; overflow-x: hidden;
}
body::before {
    content: ''; position: fixed; inset: 0;
    background:
        radial-gradient(circle at 20% 30%, rgba(102,126,234,0.15), transparent 50%),
        radial-gradient(circle at 80% 70%, rgba(255,215,0,0.1), transparent 50%);
    pointer-events: none; z-index: 0;
}
.game-wrapper {
    max-width: 900px; width: 100%;
    background: linear-gradient(145deg, #1a1a2e, #0f3460);
    border-radius: 20px; overflow: hidden;
    box-shadow: 0 25px 60px rgba(0,0,0,0.6), 0 0 0 2px rgba(255,215,0,0.2);
    position: relative; z-index: 1;
}
.game-header {
    background: linear-gradient(135deg, #667eea, #764ba2, #f093fb);
    background-size: 200% 200%;
    animation: gradientMove 5s ease infinite;
    padding: 18px; text-align: center;
    position: relative; overflow: hidden;
}
@keyframes gradientMove { 0%,100% { background-position: 0% 50%; } 50% { background-position: 100% 50%; } }
.game-header h1 { font-size: 1.3em; text-shadow: 0 2px 10px rgba(0,0,0,0.4); margin-bottom: 4px; letter-spacing: 1px; }
.game-header p { font-size: 0.75em; opacity: 0.95; }
.sound-toggle {
    position: absolute; top: 12px; right: 12px;
    width: 38px; height: 38px; border-radius: 50%;
    background: rgba(0,0,0,0.4); border: 2px solid rgba(255,255,255,0.3);
    color: white; font-size: 1.1em; cursor: pointer;
    display: flex; align-items: center; justify-content: center;
    z-index: 10; transition: all 0.2s;
}
.sound-toggle:hover { background: rgba(0,0,0,0.6); transform: scale(1.1); }
.sound-toggle.muted { opacity: 0.5; }
.story-panel {
    background: linear-gradient(135deg, #2c1810, #3d2317);
    padding: 14px 20px; border-bottom: 3px solid #ffd700;
    color: #ffd700; font-size: 0.85em; min-height: 50px;
    display: flex; align-items: center; font-style: italic;
    text-shadow: 0 0 10px rgba(255,215,0,0.5);
}
.intro-panel {
    padding: 30px 25px;
    background: linear-gradient(135deg, #0f3460, #1a1a2e);
    min-height: 520px;
    display: flex; flex-direction: column; justify-content: center; align-items: center;
    position: relative; overflow: hidden;
}
.intro-panel::before {
    content: '🌲🌳🌲🌳🌲';
    position: absolute; top: 10px; left: 0; right: 0;
    text-align: center; font-size: 1.5em; opacity: 0.3; letter-spacing: 30px;
}
.intro-title {
    text-align: center; color: #ffd700; font-size: 1.6em;
    margin-bottom: 20px; font-weight: bold;
    text-shadow: 0 0 20px rgba(255,215,0,0.8), 0 0 40px rgba(255,215,0,0.4);
    animation: pulseGlow 2s infinite;
}
@keyframes pulseGlow {
    0%, 100% { text-shadow: 0 0 20px rgba(255,215,0,0.8), 0 0 40px rgba(255,215,0,0.4); }
    50% { text-shadow: 0 0 30px rgba(255,215,0,1), 0 0 60px rgba(255,215,0,0.6); }
}
.pesan-rahasia-intro {
    background: linear-gradient(135deg, rgba(255,215,0,0.15), rgba(255,140,0,0.1));
    border: 3px solid #ffd700; padding: 25px; border-radius: 15px;
    margin: 15px 0; color: #ffd700; font-size: 0.9em;
    line-height: 1.8; max-width: 600px;
    box-shadow: 0 0 30px rgba(255,215,0,0.3), inset 0 0 30px rgba(255,215,0,0.1);
    animation: boxGlow 3s infinite;
}
@keyframes boxGlow {
    0%, 100% { box-shadow: 0 0 30px rgba(255,215,0,0.3), inset 0 0 30px rgba(255,215,0,0.1); }
    50% { box-shadow: 0 0 50px rgba(255,215,0,0.5), inset 0 0 40px rgba(255,215,0,0.2); }
}
.pesan-rahasia-intro .pesan-judul { font-size: 1.1em; font-weight: bold; margin-bottom: 15px; text-align: center; }
.pesan-rahasia-intro .pesan-isi { color: #fff; font-style: italic; }
.pesan-rahasia-intro .pesan-penting {
    margin-top: 15px; padding-top: 15px;
    border-top: 1px dashed #ffd700;
    color: #ff7676; font-weight: bold; text-align: center;
    animation: pulseRed 1.5s infinite;
}
@keyframes pulseRed { 0%, 100% { opacity: 1; } 50% { opacity: 0.6; } }
.progress-tracker {
    display: flex; justify-content: space-around;
    padding: 12px 8px;
    background: linear-gradient(180deg, #0a0a15, #15152a);
    border-bottom: 2px solid rgba(102,126,234,0.3);
}
.progress-item { text-align: center; font-size: 0.6em; color: #555; flex: 1; transition: all 0.4s; }
.progress-item .icon { font-size: 1.6em; display: block; transition: all 0.4s; filter: grayscale(1); }
.progress-item.done { color: #28a745; }
.progress-item.done .icon { filter: grayscale(0); }
.progress-item.active { color: #ffd700; transform: scale(1.15); }
.progress-item.active .icon { filter: grayscale(0); animation: pulse 1s infinite; }
@keyframes pulse { 0%, 100% { transform: scale(1); } 50% { transform: scale(1.2); } }
.game-screen {
    position: relative; height: 350px;
    background: linear-gradient(180deg, #0a2818, #0d2818);
    overflow: hidden;
    box-shadow: inset 0 0 40px rgba(0,0,0,0.5);
}
.maze {
    position: absolute; inset: 0;
    display: grid;
    grid-template-columns: repeat(10, 10%);
    grid-template-rows: repeat(7, 14.28%);
}
.cell { position: relative; display: flex; align-items: center; justify-content: center; }
.cell.path { background: linear-gradient(135deg, #1a4d2e, #215c37); border: 1px solid #0d2818; }
.cell.wall { background: #0d2818; }
.cell.tree::before { content: '🌲'; font-size: 1.6em; filter: drop-shadow(0 2px 4px rgba(0,0,0,0.5)); }
.cell.rock::before { content: '🪨'; font-size: 1.3em; filter: drop-shadow(0 2px 4px rgba(0,0,0,0.5)); }
.player {
    position: absolute; z-index: 20; transition: all 0.3s ease;
    pointer-events: none; transform: translate(-50%, -50%);
    filter: drop-shadow(0 0 15px rgba(102,126,234,0.9));
}
.entitas {
    position: absolute; z-index: 10;
    transform: translate(-50%, -50%);
    cursor: pointer; transition: all 0.3s ease;
    animation: entitasBounce 1.5s infinite;
}
@keyframes entitasBounce {
    0%, 100% { transform: translate(-50%, -50%) scale(1); }
    50% { transform: translate(-50%, -55%) scale(1.1); }
}
.entitas.peti { filter: drop-shadow(0 0 20px rgba(255,215,0,1)); }
.entitas.pintu { filter: drop-shadow(0 0 25px rgba(40,167,69,1)); }
.entitas.npc { filter: drop-shadow(0 0 12px rgba(102,126,234,0.9)); }
.entitas-label {
    position: absolute; top: -25px; left: 50%; transform: translateX(-50%);
    background: linear-gradient(135deg, #ffd700, #ff8c00);
    color: #000; padding: 4px 10px; border-radius: 12px;
    font-size: 0.65em; font-weight: bold; white-space: nowrap;
    box-shadow: 0 4px 10px rgba(0,0,0,0.4);
}
.game-info {
    display: flex; justify-content: space-around;
    padding: 10px;
    background: linear-gradient(180deg, #15152a, #0a0a15);
    font-size: 0.8em; flex-wrap: wrap; gap: 5px;
    border-top: 2px solid rgba(102,126,234,0.3);
}
.info-item {
    display: flex; align-items: center; gap: 5px;
    background: rgba(102,126,234,0.1);
    padding: 5px 12px; border-radius: 20px;
    border: 1px solid rgba(102,126,234,0.3);
}
.info-item .value { color: #ffd700; font-weight: bold; }
.game-kontrol {
    display: flex; justify-content: center; gap: 10px;
    padding: 15px;
    background: linear-gradient(180deg, #0a0a15, #15152a);
    flex-wrap: wrap;
    border-top: 1px solid rgba(102,126,234,0.2);
}
.control-btn {
    width: 58px; height: 58px;
    background: linear-gradient(145deg, #667eea, #764ba2);
    border: 2px solid rgba(255,255,255,0.2);
    border-radius: 15px; color: white; font-size: 1.4em;
    cursor: pointer; display: flex; align-items: center; justify-content: center;
    user-select: none;
    box-shadow: 0 6px 15px rgba(102,126,234,0.4);
    transition: all 0.15s;
}
.control-btn:hover { background: linear-gradient(145deg, #764ba2, #667eea); transform: translateY(-2px); }
.control-btn:active { transform: scale(0.92); }
.control-btn.aksi { background: linear-gradient(145deg, #ff6b6b, #ee5a24); width: 75px; }
.game-konten {
    padding: 20px; min-height: 220px;
    background: linear-gradient(180deg, #0f3460, #1a1a2e);
    color: #ddd;
}
.game-konten h2 { color: #ffd700; margin-bottom: 12px; font-size: 1.15em; }
.game-konten h3 { color: #ffd700; margin: 15px 0 10px 0; font-size: 1em; }
.game-konten p { line-height: 1.6; margin-bottom: 8px; font-size: 0.88em; }
.dialog-box {
    background: linear-gradient(135deg, rgba(102,126,234,0.1), rgba(118,75,162,0.1));
    padding: 14px; border-radius: 12px;
    border-left: 4px solid #667eea;
    margin: 10px 0;
}
.dialog-box .nama { color: #ffd700; font-weight: bold; margin-bottom: 6px; }
.btn-group { display: flex; gap: 10px; margin-top: 12px; flex-wrap: wrap; }
.btn {
    flex: 1; min-width: 100px;
    background: linear-gradient(145deg, #667eea, #764ba2);
    color: white; border: none;
    padding: 13px 15px; border-radius: 12px;
    font-size: 0.9em; font-weight: 600;
    cursor: pointer; transition: all 0.15s;
}
.btn:hover { background: linear-gradient(145deg, #764ba2, #667eea); transform: translateY(-2px); }
.btn:active { transform: translateY(0) scale(0.98); }
.btn.success { background: linear-gradient(145deg, #28a745, #20c997); }
.btn.danger { background: linear-gradient(145deg, #dc3545, #c82333); }
.btn.info { background: linear-gradient(145deg, #17a2b8, #138496); }
input {
    width: 100%; padding: 12px 15px;
    border: 2px solid rgba(102,126,234,0.3);
    background: linear-gradient(135deg, #1a1a2e, #15152a);
    color: white; border-radius: 10px;
    font-size: 0.95em; margin-bottom: 10px;
    font-family: inherit;
}
input:focus {
    outline: none; border-color: #ffd700;
    box-shadow: 0 0 20px rgba(255,215,0,0.4);
}
input::placeholder { color: #666; }
label {
    display: block; margin-bottom: 6px;
    font-size: 0.85em; color: #a6e3a1; font-weight: bold;
}
.hitung-box {
    background: linear-gradient(135deg, rgba(40,167,69,0.1), rgba(40,167,69,0.05));
    border-left: 4px solid #28a745;
    padding: 14px; border-radius: 12px; margin: 12px 0;
    font-family: 'Courier New', monospace;
    font-size: 0.72em; white-space: pre-wrap;
    word-break: break-all;
    overflow-wrap: break-word;
    color: #a6e3a1;
    max-height: 400px; overflow-y: auto;
    line-height: 1.8;
}
.tersangka-grid {
    display: grid; grid-template-columns: repeat(3, 1fr);
    gap: 12px; margin: 15px 0;
}
.tersangka-card {
    background: linear-gradient(145deg, rgba(255,255,255,0.05), rgba(255,255,255,0.02));
    border: 3px solid #2c2c3e;
    border-radius: 15px; padding: 18px 10px;
    cursor: pointer; transition: all 0.3s;
    text-align: center; user-select: none;
}
.tersangka-card:hover { border-color: #667eea; transform: translateY(-5px); }
.tersangka-card.selected {
    border-color: #ffd700;
    background: linear-gradient(145deg, rgba(255,215,0,0.2), rgba(255,140,0,0.1));
    transform: scale(1.05);
}
.tersangka-body { display: block; margin-bottom: 10px; }
.tersangka-body svg { width: 55px; height: 110px; }
.tersangka-nama { font-size: 0.95em; font-weight: bold; color: #ffd700; }
.tabel-verifikasi {
    width: 100%; border-collapse: collapse;
    margin: 12px 0; font-size: 0.78em;
    background: rgba(0,0,0,0.2); border-radius: 10px;
    overflow: hidden;
}
.tabel-verifikasi th {
    background: linear-gradient(135deg, #667eea, #764ba2);
    color: #ffd700; padding: 10px; text-align: left;
}
.tabel-verifikasi td {
    padding: 10px; color: #ddd;
    border-bottom: 1px solid rgba(255,255,255,0.08);
}
.tabel-verifikasi tr.benar td { background: rgba(40,167,69,0.2); color: #28a745; font-weight: bold; }
.tabel-verifikasi tr.salah td { background: rgba(220,53,69,0.15); color: #dc3545; }
.skor-panel {
    background: linear-gradient(135deg, rgba(255,215,0,0.15), rgba(255,140,0,0.08));
    border: 2px solid #ffd700;
    border-radius: 15px; padding: 20px;
    text-align: center; margin: 15px 0;
}
.skor-panel .skor-besar { font-size: 3em; font-weight: bold; color: #ffd700; line-height: 1; }
.skor-panel .skor-label { font-size: 0.8em; color: #a6e3a1; margin-top: 5px; letter-spacing: 2px; }
.skor-panel .skor-detail { display: flex; justify-content: space-around; margin-top: 15px; flex-wrap: wrap; gap: 10px; }
.skor-panel .skor-item { background: rgba(0,0,0,0.3); padding: 8px 15px; border-radius: 10px; font-size: 0.8em; }
.skor-panel .skor-item .angka { color: #ffd700; font-weight: bold; font-size: 1.2em; display: block; margin-top: 3px; }
.leaderboard {
    background: rgba(102,126,234,0.08);
    border: 2px solid rgba(102,126,234,0.4);
    border-radius: 15px; padding: 15px; margin: 15px 0;
}
.leaderboard h3 { color: #ffd700; margin-bottom: 10px; text-align: center; font-size: 1em; }
.leaderboard-item {
    display: flex; justify-content: space-between; align-items: center;
    padding: 8px 12px; background: rgba(0,0,0,0.3);
    border-radius: 8px; margin-bottom: 6px; font-size: 0.85em;
}
.leaderboard-item.medali-1 { border-left: 4px solid #ffd700; }
.leaderboard-item.medali-2 { border-left: 4px solid #c0c0c0; }
.leaderboard-item.medali-3 { border-left: 4px solid #cd7f32; }
.leaderboard-item.medali-aktif { background: linear-gradient(90deg, rgba(255,215,0,0.2), rgba(255,215,0,0.05)); border: 1px solid #ffd700; }
.leaderboard-item .nama-pemain { color: #ffd700; font-weight: bold; }
.leaderboard-item .skor-pemain { color: #a6e3a1; font-weight: bold; }
.notif {
    position: fixed; top: 20px; left: 50%;
    transform: translateX(-50%) translateY(-100px);
    background: linear-gradient(135deg, #28a745, #20c997);
    color: white; padding: 14px 30px; border-radius: 30px;
    font-weight: bold; font-size: 0.9em; z-index: 1000;
    transition: transform 0.5s cubic-bezier(0.68, -0.55, 0.265, 1.55);
    pointer-events: none;
    box-shadow: 0 10px 30px rgba(40,167,69,0.5);
    max-width: 90%; text-align: center;
}
.notif.show { transform: translateX(-50%) translateY(0); }
.finish-menang {
    background: linear-gradient(135deg, #28a745, #20c997);
    padding: 30px 25px; border-radius: 20px;
    text-align: center; margin: 20px 0;
    animation: winPulse 1.5s infinite;
}
@keyframes winPulse {
    0%, 100% { box-shadow: 0 20px 50px rgba(40,167,69,0.5); }
    50% { box-shadow: 0 25px 60px rgba(40,167,69,0.8); }
}
.finish-menang .icon { font-size: 4.5em; margin-bottom: 10px; animation: bounce 1s infinite; }
@keyframes bounce { 0%, 100% { transform: translateY(0); } 50% { transform: translateY(-15px); } }
.finish-menang h2 { color: white; font-size: 1.8em; margin-bottom: 10px; }
.finish-menang p { color: white; font-size: 1em; }
.finish-kalah {
    background: linear-gradient(135deg, #dc3545, #721c24);
    padding: 30px 25px; border-radius: 20px;
    text-align: center; margin: 20px 0;
}
.finish-kalah .icon { font-size: 4.5em; margin-bottom: 10px; animation: shake 0.8s infinite; }
@keyframes shake {
    0%, 100% { transform: rotate(0deg); }
    25% { transform: rotate(-5deg); }
    75% { transform: rotate(5deg); }
}
.finish-kalah h2 { color: white; font-size: 1.8em; margin-bottom: 10px; }
.finish-kalah p { color: white; font-size: 1em; }
.hidden { display: none; }
::-webkit-scrollbar { width: 8px; }
::-webkit-scrollbar-track { background: rgba(0,0,0,0.3); border-radius: 10px; }
::-webkit-scrollbar-thumb { background: linear-gradient(135deg, #667eea, #764ba2); border-radius: 10px; }
@media (max-width: 600px) {
    .game-screen { height: 280px; }
    .control-btn { width: 50px; height: 50px; font-size: 1.2em; }
    .control-btn.aksi { width: 65px; }
    .tersangka-grid { gap: 6px; }
    .tersangka-body svg { width: 42px; height: 85px; }
    .tabel-verifikasi { font-size: 0.68em; }
    .hitung-box { font-size: 0.65em; }
    .game-header h1 { font-size: 1.1em; }
    .skor-panel .skor-besar { font-size: 2.2em; }
    .sound-toggle { width: 32px; height: 32px; font-size: 0.9em; }
}
</style>
</head>
<body>

<div class="game-wrapper">
    <div class="game-header">
        <h1>🎮 Crypto Imposter Adventure</h1>
        <p>3 Orang Tersesat &bull; 1 Imposter &bull; RSA + Current</p>
        <button class="sound-toggle" id="btnSound" title="Mute/Unmute">🔊</button>
    </div>

    <div id="introPage" class="intro-panel">
        <div class="intro-title">📜 PESAN RAHASIA DITERIMA</div>
        <div class="pesan-rahasia-intro">
            <div class="pesan-judul">🔐 DARI: KEPALA DESA</div>
            <div class="pesan-isi">
                "3 orang tersesat di hutan labirin. Salah satunya adalah IMPOSTER.
                <br><br>
                Imposter memegang <strong>KUNCI PRIVAT</strong> untuk membuka peta keluar.
                <br><br>
                Kamu memegang <strong>KUNCI PUBLIK</strong>.
                <br><br>
                Temukan imposter dengan memecahkan puzzle RSA dan Algoritma Current!"
            </div>
            <div class="pesan-penting">
                ⚠️ NYAWA: 2 &bull; +2 POIN / LEVEL<br>
                Kalau salah 2 kali, kamu KALAH!
            </div>
        </div>
        <button class="btn success" type="button" id="btnMulai" 
                style="margin-top:20px;padding:15px 30px;font-size:1.05em;max-width:300px;">
            🎮 Mulai Petualangan
        </button>
    </div>

    <div id="gamePage" class="hidden">
        <div class="story-panel">
            <span id="storyText">Setup permainan dulu ya!</span>
        </div>

        <div class="progress-tracker" id="trackerBar" style="display:none;">
            <div class="progress-item active" id="pt-1"><div class="icon">📦</div><div>Cari Peti</div></div>
            <div class="progress-item" id="pt-2"><div class="icon">🔐</div><div>Puzzle RSA</div></div>
            <div class="progress-item" id="pt-3"><div class="icon">🧑</div><div>Cari Ciri</div></div>
            <div class="progress-item" id="pt-4"><div class="icon">🚪</div><div>Pintu Keluar</div></div>
            <div class="progress-item" id="pt-5"><div class="icon">🗺️</div><div>Buka Peta</div></div>
        </div>

        <div class="game-screen" id="screenBar" style="display:none;">
            <div class="maze" id="maze"></div>
        </div>

        <div class="game-info" id="hudBar" style="display:none;">
            <div class="info-item"><span>👤</span><span class="value" id="hudPemain">-</span></div>
            <div class="info-item"><span>💬</span><span class="value" id="hudCiri">0/3 Ciri</span></div>
            <div class="info-item"><span>❤️</span><span class="value" id="hudNyawa">2</span></div>
            <div class="info-item"><span>⭐</span><span class="value" id="hudSkor">0</span></div>
            <div class="info-item"><span>🏆</span><span class="value" id="hudLevel">Lv.1</span></div>
        </div>

        <div class="game-kontrol" id="ctrlBar" style="display:none;">
            <button class="control-btn" id="btnKiri" type="button">⬅️</button>
            <button class="control-btn" id="btnAtas" type="button">⬆️</button>
            <button class="control-btn" id="btnBawah" type="button">⬇️</button>
            <button class="control-btn" id="btnKanan" type="button">➡️</button>
            <button class="control-btn aksi" id="btnAksi" type="button">🎯</button>
        </div>

        <div class="game-konten" id="gameKonten"></div>
    </div>
</div>

<div class="notif" id="notif"></div>

<script>
/* ============================================================
   0. SISTEM AUDIO
   ============================================================ */
const AudioEngine = {
    ctx: null, masterGain: null, bgmGain: null, sfxGain: null,
    bgmPlaying: false, bgmTimer: null, muted: false, volume: 0.5,
    init() {
        if (this.ctx) return;
        const Ctx = window.AudioContext || window.webkitAudioContext;
        this.ctx = new Ctx();
        this.masterGain = this.ctx.createGain();
        this.masterGain.gain.value = this.volume;
        this.masterGain.connect(this.ctx.destination);
        this.bgmGain = this.ctx.createGain();
        this.bgmGain.gain.value = 0.25;
        this.bgmGain.connect(this.masterGain);
        this.sfxGain = this.ctx.createGain();
        this.sfxGain.gain.value = 0.9;
        this.sfxGain.connect(this.masterGain);
    },
    playTone(freq, duration, type, volume, target, when) {
        if (!this.ctx || this.muted) return;
        type = type || 'square'; volume = volume || 0.25;
        target = target || 'sfx'; when = when || 0;
        const t = this.ctx.currentTime + when;
        const osc = this.ctx.createOscillator();
        const gain = this.ctx.createGain();
        osc.type = type; osc.frequency.value = freq;
        const dest = target === 'bgm' ? this.bgmGain : this.sfxGain;
        gain.gain.setValueAtTime(0, t);
        gain.gain.linearRampToValueAtTime(volume, t + 0.01);
        gain.gain.exponentialRampToValueAtTime(0.001, t + duration);
        osc.connect(gain); gain.connect(dest);
        osc.start(t); osc.stop(t + duration + 0.05);
    },
    sfxClick()  { this.playTone(800, 0.06); this.playTone(1200, 0.04, 'square', 0.15, 'sfx', 0.03); },
    sfxOpen()   { [523,659,784,1047].forEach((f,i) => this.playTone(f, 0.15, 'triangle', 0.3, 'sfx', i*0.06)); },
    sfxDialog() { this.playTone(440, 0.08, 'sine', 0.3); this.playTone(587, 0.1, 'sine', 0.25, 'sfx', 0.07); },
    sfxWin()    {
        [523,659,784,1047,1319].forEach((f,i) => this.playTone(f, 0.25, 'square', 0.28, 'sfx', i*0.12));
        [1047,1319,1568].forEach(f => this.playTone(f, 0.6, 'triangle', 0.2, 'sfx', 0.7));
    },
    sfxWrong()  {
        this.playTone(220, 0.15, 'sawtooth', 0.3);
        this.playTone(165, 0.2, 'sawtooth', 0.3, 'sfx', 0.12);
        this.playTone(110, 0.3, 'sawtooth', 0.25, 'sfx', 0.28);
    },
    sfxGameOver() { [523,466,415,349].forEach((f,i) => this.playTone(f, 0.35, 'sine', 0.3, 'sfx', i*0.18)); },
    sfxLevelUp()  { [392,523,659,784,1047].forEach((f,i) => this.playTone(f, 0.15, 'triangle', 0.25, 'sfx', i*0.07)); },
    sfxSelect()   { this.playTone(1000, 0.04, 'square', 0.2); },
    sfxMove()     { this.playTone(180, 0.05, 'square', 0.08); },
    bgmMelodi: [
        [220,1],[261.63,1],[329.63,1],[293.66,1],[261.63,1],[220,1],[196,2],
        [220,1],[261.63,1],[329.63,1],[392,1],[349.23,1],[329.63,1],[261.63,2],
        [196,1],[220,1],[261.63,1],[293.66,1],[329.63,1],[293.66,1],[261.63,2],
        [220,1],[196,1],[174.61,1],[196,1],[220,4]
    ],
    bgmBass: [[110,4],[98,4],[110,4],[87.31,4],[98,4],[110,4],[87.31,4],[110,4]],
    beatDuration: 0.4,
    startBGM() {
        if (!this.ctx || this.bgmPlaying) return;
        this.bgmPlaying = true;
        this._scheduleBGM();
    },
    _scheduleBGM() {
        if (!this.bgmPlaying || !this.ctx) return;
        let totalMelodi = 0;
        for (const [_, d] of this.bgmMelodi) totalMelodi += d;
        const loopDur = totalMelodi * this.beatDuration;
        let time = 0;
        for (const [freq, dur] of this.bgmMelodi) {
            const durSec = dur * this.beatDuration;
            this._playBGM(freq, durSec * 0.9, 'triangle', 0.12, time);
            time += durSec;
        }
        time = 0;
        for (const [freq, dur] of this.bgmBass) {
            const durSec = dur * this.beatDuration;
            this._playBGM(freq, durSec * 0.9, 'sine', 0.18, time);
            time += durSec;
        }
        this.bgmTimer = setTimeout(() => {
            if (this.bgmPlaying) this._scheduleBGM();
        }, loopDur * 1000 - 50);
    },
    _playBGM(freq, duration, type, volume, when) {
        if (!this.ctx || this.muted) return;
        const t = this.ctx.currentTime + when;
        const osc = this.ctx.createOscillator();
        const gain = this.ctx.createGain();
        osc.type = type; osc.frequency.value = freq;
        gain.gain.setValueAtTime(0, t);
        gain.gain.linearRampToValueAtTime(volume, t + 0.05);
        gain.gain.linearRampToValueAtTime(volume * 0.7, t + duration * 0.5);
        gain.gain.exponentialRampToValueAtTime(0.001, t + duration);
        osc.connect(gain); gain.connect(this.bgmGain);
        osc.start(t); osc.stop(t + duration + 0.05);
    },
    toggleMute() {
        this.muted = !this.muted;
        if (this.masterGain) this.masterGain.gain.value = this.muted ? 0 : this.volume;
        return this.muted;
    }
};
function playSFX(name) {
    if (!AudioEngine.ctx || AudioEngine.muted) return;
    if (typeof AudioEngine[name] === 'function') AudioEngine[name]();
}

/* ============================================================
   1. TABEL CURRENT FINAL (REKONSTRUKSI) - JANGAN DIUBAH!
   ============================================================ */
const TABEL_BASE = {
    'A': 101, 'B': 126, 'C': 523, 'D': 358, 'E': 548, 'F': 136,
    'G': 121, 'H': 340, 'I': 355, 'J': 890, 'K': 144, 'L': 336,
    'M': 577, 'N': 108, 'O': 587, 'P': 976, 'Q': 184, 'R': 810,
    'S': 365, 'T': 460, 'U': 111, 'V': 576, 'W': 568, 'X': 258,
    'Y': 593, 'Z': 186
};
const TABEL_BASE_INV = {};
for (const h in TABEL_BASE) TABEL_BASE_INV[TABEL_BASE[h]] = h;
const HURUF = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ';

const POOL_KARAKTER = [
    { nama: 'Michael', warna: '#FFD700', peta: 'Utara 100m, lalu kiri',
      pertanyaan: 'Makan siang kamu apa?',
      jawaban: 'AKU MAKAN NASI GORENG' },
    { nama: 'Citra', warna: '#4ECDC4', peta: 'Selatan 50m, lalu kanan',
      pertanyaan: 'Minuman favorit kamu apa?',
      jawaban: 'AKU SUKA MINUM KOPI' },
    { nama: 'Dedi', warna: '#BB8FCE', peta: 'Timur 200m, lurus terus',
      pertanyaan: 'Hobi kamu apa?',
      jawaban: 'AKU SUKA MAIN BOLA' },
    { nama: 'Eka', warna: '#A8E6CF', peta: 'Barat 75m, lalu kanan',
      pertanyaan: 'Hewan peliharaan kamu apa?',
      jawaban: 'AKU SUKA KUCING' },
    { nama: 'Fani', warna: '#FF8B94', peta: 'Utara 150m, lalu kiri',
      pertanyaan: 'Musik favorit kamu apa?',
      jawaban: 'AKU SUKA DENGAR MUSIK' },
    { nama: 'Gilang', warna: '#C7CEEA', peta: 'Selatan 300m, lurus',
      pertanyaan: 'Warna favorit kamu apa?',
      jawaban: 'AKU SUKA WARNA BIRU' },
    { nama: 'Hana', warna: '#FFB6B9', peta: 'Timur 120m, lalu kanan',
      pertanyaan: 'Kota favorit kamu apa?',
      jawaban: 'AKU SUKA KOTA MEDAN' }
];

const MAZE = [
    [1,1,1,1,1,1,1,1,1,1],
    [1,0,0,0,0,0,0,0,0,1],
    [1,0,1,0,1,0,1,0,0,1],
    [0,0,1,0,0,0,1,0,0,0],
    [1,0,1,1,1,0,1,0,0,1],
    [1,0,0,0,0,0,0,0,0,1],
    [1,1,1,1,1,1,1,1,1,1]
];

const game = {
    playerRow: 3, playerCol: 0,
    nyawa: 2, skor: 0, level: 1,
    p: 41, q: 43, rsa: null,
    kunciCurrent: 7, pesanCiri: '',
    encArr: [], encStr: '',
    karakter: [], imposter: null, dataImposter: null,
    posisiNPC: [],
    sudahBukaPeti: false,
    ciriDitemukan: [false, false, false],
    tebakan: null,
    posisiPeti: {row: 1, col: 4},
    posisiPintu: {row: 3, col: 9},
    pemainAktif: '',
    leaderboard: {},
    riwayatTebakan: []
};

/* ============================================================
   3. ALGORITMA CURRENT
   ============================================================ */
function currentEncrypt(plaintext, kunci) {
    plaintext = plaintext.toUpperCase();
    const offset = (kunci - 1) % 26;
    const out = [];
    for (const c of plaintext) {
        if (c === ' ') out.push('SP');
        else {
            const idx = HURUF.indexOf(c);
            if (idx !== -1) out.push(TABEL_BASE[HURUF[(idx + offset) % 26]]);
        }
    }
    return out;
}
function currentDecrypt(cipherArr, kunci) {
    const offset = (kunci - 1) % 26;
    let out = '';
    for (const t of cipherArr) {
        if (t === 'SP') out += ' ';
        else {
            const hurufCipher = TABEL_BASE_INV[parseInt(t)];
            if (hurufCipher) {
                const idxCipher = HURUF.indexOf(hurufCipher);
                out += HURUF[((idxCipher - offset) % 26 + 26) % 26];
            }
        }
    }
    return out;
}
function formatCiphertextFull(encStr) {
    return encStr.split(',').map(x => x.trim()).join(', ');
}

/* ============================================================
   4. RSA
   ============================================================ */
function gcd(a, b) { while (b) { [a, b] = [b, a % b]; } return a; }
function modInverse(e, phi) {
    for (let d = 2; d < phi; d++) if ((e * d) % phi === 1) return d;
    return null;
}
function generateRSA(p, q) {
    const n = p * q;
    const phi = (p - 1) * (q - 1);
    let e = 3;
    while (e < phi && gcd(e, phi) !== 1) e += 2;
    const d = modInverse(e, phi);
    return { e, d, n, phi, p, q };
}
function modPow(base, exp, mod) {
    let result = 1;
    base = base % mod;
    while (exp > 0) {
        if (exp % 2 === 1) result = (result * base) % mod;
        exp = Math.floor(exp / 2);
        base = (base * base) % mod;
    }
    return result;
}

/* ============================================================
   5. UTILITAS
   ============================================================ */
function shuffle(arr) {
    const a = arr.slice();
    for (let i = a.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [a[i], a[j]] = [a[j], a[i]];
    }
    return a;
}
function randInt(min, max) { return Math.floor(Math.random() * (max - min + 1)) + min; }
function showNotif(msg) {
    const n = document.getElementById('notif');
    n.textContent = msg;
    n.classList.add('show');
    setTimeout(() => n.classList.remove('show'), 2000);
}
function setStory(text) { document.getElementById('storyText').textContent = text; }
function setProgress(step) {
    for (let i = 1; i <= 5; i++) {
        const el = document.getElementById('pt-' + i);
        if (!el) continue;
        el.className = 'progress-item';
        if (i < step) el.classList.add('done');
        else if (i === step) el.classList.add('active');
    }
}
function updateHUD() {
    const jmlCiri = game.ciriDitemukan.filter(Boolean).length;
    document.getElementById('hudPemain').textContent = game.pemainAktif || '-';
    document.getElementById('hudCiri').textContent = jmlCiri + '/3 Ciri';
    document.getElementById('hudNyawa').textContent = game.nyawa;
    document.getElementById('hudSkor').textContent = game.skor;
    document.getElementById('hudLevel').textContent = 'Lv.' + game.level;
}
function getCellPosition(row, col) {
    const cellW = 100 / 10;
    const cellH = 100 / 7;
    return {
        left: (col * cellW + cellW / 2) + '%',
        top: (row * cellH + cellH / 2) + '%'
    };
}
function buatSVGKarakter(warna) {
    return '<svg viewBox="0 0 50 100" style="width:48px;height:96px;">' +
        '<circle cx="25" cy="20" r="15" fill="#FFD700"/>' +
        '<circle cx="20" cy="17" r="2" fill="#000"/>' +
        '<circle cx="30" cy="17" r="2" fill="#000"/>' +
        '<path d="M 19 25 Q 25 30 31 25" stroke="#000" fill="none" stroke-width="1.5"/>' +
        '<rect x="15" y="36" width="20" height="35" fill="' + warna + '" rx="3"/>' +
        '<rect x="5" y="40" width="10" height="25" fill="' + warna + '" rx="3"/>' +
        '<rect x="35" y="40" width="10" height="25" fill="' + warna + '" rx="3"/>' +
        '<rect x="17" y="70" width="7" height="25" fill="#333" rx="2"/>' +
        '<rect x="26" y="70" width="7" height="25" fill="#333" rx="2"/>' +
        '</svg>';
}

/* ============================================================
   6. LEADERBOARD
   ============================================================ */
function tambahSkorPemain(nama, poin) {
    if (!game.leaderboard[nama]) game.leaderboard[nama] = 0;
    game.leaderboard[nama] += poin;
}
function renderLeaderboard() {
    const entries = Object.entries(game.leaderboard).sort((a, b) => b[1] - a[1]);
    if (entries.length === 0) return '<div class="leaderboard"><h3>🏆 Leaderboard</h3><p style="text-align:center;font-size:0.85em;color:#888;">Belum ada skor</p></div>';
    let html = '<div class="leaderboard"><h3>🏆 Leaderboard</h3>';
    entries.forEach(([nama, skor], i) => {
        let cls = 'leaderboard-item';
        if (i === 0) cls += ' medali-1';
        else if (i === 1) cls += ' medali-2';
        else if (i === 2) cls += ' medali-3';
        if (nama === game.pemainAktif) cls += ' medali-aktif';
        const medali = i === 0 ? '🥇' : i === 1 ? '🥈' : i === 2 ? '🥉' : '  ';
        html += '<div class="' + cls + '"><span class="nama-pemain">' + medali + ' ' + nama + '</span><span class="skor-pemain">' + skor + ' poin</span></div>';
    });
    html += '</div>';
    return html;
}

/* ============================================================
   7. FORM SETUP
   ============================================================ */
function tampilkanFormSetup() {
    document.getElementById('trackerBar').style.display = 'none';
    document.getElementById('screenBar').style.display = 'none';
    document.getElementById('hudBar').style.display = 'none';
    document.getElementById('ctrlBar').style.display = 'none';

    const pool = shuffle(POOL_KARAKTER);
    game.karakter = [pool[0], pool[1], pool[2]];
    const savedPemain = game.pemainAktif || '';

    let html = '<h2>🎮 Setup Permainan</h2>';
    html += '<h3>👤 Siapa yang Main?</h3>';
    html += '<label>Nama Pemain</label>';
    html += '<input type="text" id="inputPemainAktif" value="' + savedPemain + '" maxlength="15" placeholder="Contoh: Andi">';
    html += '<h3 style="margin-top:20px;">🧑 3 Karakter di Hutan (bisa diubah)</h3>';
    for (let i = 0; i < 3; i++) {
        html += '<label>Karakter ' + (i + 1) + '</label>';
        html += '<input type="text" id="inputKarakter' + i + '" value="' + game.karakter[i].nama + '" maxlength="15">';
    }
    html += renderLeaderboard();
    html += '<div class="btn-group">';
    html += '<button class="btn" type="button" id="btnAcakNama">🎲 Acak Karakter</button>';
    html += '<button class="btn success" type="button" id="btnMulaiMain">✅ Simpan & Mulai</button>';
    html += '</div>';

    document.getElementById('gameKonten').innerHTML = html;
    document.getElementById('btnAcakNama').onclick = acakKarakter;
    document.getElementById('btnMulaiMain').onclick = simpanSetupDanMulai;
}

function acakKarakter() {
    playSFX('sfxClick');
    const pool = shuffle(POOL_KARAKTER);
    for (let i = 0; i < 3; i++) document.getElementById('inputKarakter' + i).value = pool[i].nama;
    showNotif('🎲 Karakter diacak!');
}

function simpanSetupDanMulai() {
    playSFX('sfxClick');
    const namaPemain = document.getElementById('inputPemainAktif').value.trim();
    const namaKarakter = [0, 1, 2].map(i => document.getElementById('inputKarakter' + i).value.trim());

    if (!namaPemain) { showNotif('❌ Nama pemain harus diisi!'); return; }
    if (namaKarakter.some(n => !n)) { showNotif('❌ Semua nama karakter harus diisi!'); return; }
    if (new Set(namaKarakter).size !== 3) { showNotif('❌ Nama karakter tidak boleh sama!'); return; }

    game.pemainAktif = namaPemain;
    for (let i = 0; i < 3; i++) game.karakter[i].nama = namaKarakter[i];

    document.getElementById('trackerBar').style.display = 'flex';
    document.getElementById('screenBar').style.display = 'block';
    document.getElementById('hudBar').style.display = 'flex';
    document.getElementById('ctrlBar').style.display = 'flex';

    mulaiLevelBaru();
}

/* ============================================================
   8. LEVEL BARU
   ============================================================ */
function mulaiLevelBaru() {
    game.playerRow = 3;
    game.playerCol = 0;
    game.nyawa = 2;
    game.sudahBukaPeti = false;
    game.ciriDitemukan = [false, false, false];
    game.tebakan = null;
    game.riwayatTebakan = [];
    game.rsa = generateRSA(game.p, game.q);
    game.kunciCurrent = randInt(1, 26);

    const idxImposter = randInt(0, 2);
    game.imposter = game.karakter[idxImposter];
    game.dataImposter = { ciri: game.imposter.jawaban };
    game.pesanCiri = game.imposter.jawaban.toUpperCase();
    game.encArr = currentEncrypt(game.pesanCiri, game.kunciCurrent);
    game.encStr = game.encArr.join(',');

    game.posisiNPC = [
        { row: 1, col: 1 },
        { row: 5, col: 4 },
        { row: 1, col: 7 }
    ];

    buildMaze();
    renderEntitas();
    updateHUD();
    setProgress(1);
    setStory('🔺 Level ' + game.level + ' — Jalan ke 📦 untuk mulai!');

    document.getElementById('gameKonten').innerHTML =
        '<h2>🔺 Level ' + game.level + '</h2>' +
        '<p>Pemain: <strong style="color:#ffd700;">' + game.pemainAktif + '</strong></p>' +
        '<p>Pesan rahasia baru sudah dikirim! Imposter sudah bersembunyi.</p>' +
        '<div class="hitung-box">' +
        '⭐ Skor Saat Ini: ' + game.skor + '\n' +
        '❤️ Nyawa: ' + game.nyawa + '\n' +
        '🎯 Misi: Temukan imposter untuk +2 poin!' +
        '</div>' +
        '<p style="color:#ffd700;">📦 Jalan ke peti untuk mulai!</p>';
}

/* ============================================================
   9. MAZE
   ============================================================ */
function buildMaze() {
    const mazeEl = document.getElementById('maze');
    mazeEl.innerHTML = '';
    for (let r = 0; r < 7; r++) {
        for (let c = 0; c < 10; c++) {
            const cell = document.createElement('div');
            cell.className = 'cell';
            if (MAZE[r][c] === 1) {
                cell.classList.add('wall');
                if (r === 0 || r === 6 || c === 0 || c === 9) cell.classList.add('tree');
                else cell.classList.add('rock');
            } else cell.classList.add('path');
            mazeEl.appendChild(cell);
        }
    }
}

function renderEntitas() {
    const screen = document.getElementById('screenBar');
    screen.querySelectorAll('.entitas, .player').forEach(el => el.remove());

    const player = document.createElement('div');
    player.className = 'player';
    player.innerHTML = buatSVGKarakter('#667eea');
    const posP = getCellPosition(game.playerRow, game.playerCol);
    player.style.left = posP.left;
    player.style.top = posP.top;
    screen.appendChild(player);

    if (!game.sudahBukaPeti) {
        const peti = document.createElement('div');
        peti.className = 'entitas peti';
        peti.innerHTML = '<span style="font-size:2.8em;">📦</span><div class="entitas-label">Peti</div>';
        const pos = getCellPosition(game.posisiPeti.row, game.posisiPeti.col);
        peti.style.left = pos.left;
        peti.style.top = pos.top;
        screen.appendChild(peti);
    }

    for (let j = 0; j < game.posisiNPC.length; j++) {
        const npc = document.createElement('div');
        npc.className = 'entitas npc';
        npc.innerHTML = buatSVGKarakter(game.karakter[j].warna) +
            '<div class="entitas-label">' + game.karakter[j].nama + '</div>';
        const pos = getCellPosition(game.posisiNPC[j].row, game.posisiNPC[j].col);
        npc.style.left = pos.left;
        npc.style.top = pos.top;
        screen.appendChild(npc);
    }

    if (game.sudahBukaPeti) {
        const pintu = document.createElement('div');
        pintu.className = 'entitas pintu';
        pintu.innerHTML = '<span style="font-size:3.2em;">🚪</span><div class="entitas-label">KELUAR</div>';
        const pos = getCellPosition(game.posisiPintu.row, game.posisiPintu.col);
        pintu.style.left = pos.left;
        pintu.style.top = pos.top;
        screen.appendChild(pintu);
    }
}

/* ============================================================
   10. GERAK
   ============================================================ */
function gerak(dr, dc) {
    if (!game.pemainAktif) {
        showNotif('⚠️ Isi nama pemain dulu!');
        return;
    }
    const newRow = game.playerRow + dr;
    const newCol = game.playerCol + dc;
    if (newRow < 0 || newRow >= 7 || newCol < 0 || newCol >= 10) return;
    if (MAZE[newRow][newCol] === 1) return;
    game.playerRow = newRow;
    game.playerCol = newCol;
    playSFX('sfxMove');
    renderEntitas();
    cekInteraksi();
}

function cekInteraksi() {
    if (!game.sudahBukaPeti && game.playerRow === game.posisiPeti.row && game.playerCol === game.posisiPeti.col) {
        tampilkanPuzzleRSA();
        return;
    }
    for (let i = 0; i < game.posisiNPC.length; i++) {
        if (game.playerRow === game.posisiNPC[i].row && game.playerCol === game.posisiNPC[i].col) {
            lihatCiri(i);
            return;
        }
    }
    if (game.sudahBukaPeti && game.playerRow === game.posisiPintu.row && game.playerCol === game.posisiPintu.col) {
        tampilkanPintuKeluar();
        return;
    }
}

/* ============================================================
   11. PUZZLE RSA
   ============================================================ */
function tampilkanPuzzleRSA() {
    setProgress(2);
    const rsa = game.rsa;
    document.getElementById('gameKonten').innerHTML =
        '<h2>📦 Peti Terkunci!</h2>' +
        '<p>Hitung kunci privat RSA untuk membuka peti!</p>' +
        '<div class="hitung-box">' +
        'p = ' + rsa.p + '\nq = ' + rsa.q + '\n\n' +
        'n = p × q = ' + rsa.n + '\n' +
        'φ(n) = (p-1)(q-1) = ' + rsa.phi + '\n' +
        'e = ' + rsa.e + '\nd = ' + rsa.d + '\n\n' +
        '✅ Kunci Privat = (' + rsa.d + ', ' + rsa.n + ')' +
        '</div>' +
        '<div class="btn-group"><button class="btn success" type="button" id="btnBukaPeti">🔓 Buka Peti</button></div>';
    document.getElementById('btnBukaPeti').onclick = bukaPeti;
}

/* ============================================================
   12. ISI PETI → DESKRIPSI MISI + KUNCI
   ============================================================ */
function bukaPeti() {
    game.sudahBukaPeti = true;
    playSFX('sfxOpen');
    showNotif('📦 Peti terbuka!');
    setStory('Peti berisi gulungan pesan rahasia dan kunci Current!');
    renderEntitas();

    document.getElementById('gameKonten').innerHTML =
        '<h2>📜 Isi Peti</h2>' +
        '<p>Kamu menemukan gulungan pesan kuno. Di dalamnya tertulis:</p>' +
        '<div class="hitung-box" style="border-left-color:#ffd700;background:rgba(255,215,0,0.08);color:#ffd700;">' +
        '<strong>📢 MISI:</strong>\n\n' +
        '"Imposter adalah orang yang JAWABANnya cocok dengan\n' +
        'pertanyaan: <strong>' + game.imposter.pertanyaan + '</strong>"\n\n' +
        'Dekripsi ciphertext dari para karakter untuk menemukan\n' +
        'siapa yang jawabannya cocok dengan pertanyaan tersebut.\n\n' +
        '⚠️ WARNING: Salah tebak = nyawa -1!' +
        '</div>' +
        '<div class="hitung-box" style="border-left-color:#28a745;">' +
        '🔑 KUNCI CURRENT: ' + game.kunciCurrent + '\n\n' +
        '(Gunakan kunci ini untuk dekripsi ciphertext NPC)' +
        '</div>' +
        '<div class="btn-group"><button class="btn" type="button" id="btnLanjut">🚶 Cari Karakter</button></div>';

    document.getElementById('btnLanjut').onclick = function() {
        playSFX('sfxClick');
        setStory('Cari 3 karakter! Dekati mereka untuk lihat ciphertext jawaban.');
        setProgress(3);
        document.getElementById('gameKonten').innerHTML =
            '<h2>🚶 Cari Karakter!</h2>' +
            '<p>Gunakan tombol ⬅️⬆️⬇️➡️ untuk jalan.</p>' +
            '<p>Dekati 🧑 untuk lihat ciphertext jawaban mereka.</p>' +
            '<p style="color:#ffd700;">Dekripsi ciphertext mereka untuk cari yang cocok dengan pertanyaan di peti!</p>' +
            '<div class="hitung-box">' +
            '🔑 KUNCI CURRENT: ' + game.kunciCurrent +
            '</div>';
    };
}

/* ============================================================
   13. LIHAT CIRI NPC
   ============================================================ */
function lihatCiri(idx) {
    if (game.ciriDitemukan[idx]) return;
    game.ciriDitemukan[idx] = true;
    playSFX('sfxDialog');
    updateHUD();

    const p = game.karakter[idx];
    const encJawaban = currentEncrypt(p.jawaban, game.kunciCurrent);
    const encStr = encJawaban.join(',');

    showNotif('🧑 ' + p.nama + ' menjawab!');

    let html = '<h2>🧑 ' + p.nama + '</h2>';
    html += '<div class="dialog-box" style="border-left-color:#667eea;background:rgba(102,126,234,0.1);">';
    html += '<div class="nama">💬 CIPHERTEXT JAWABAN:</div>';
    html += '<p style="font-family:Courier New,monospace;font-size:0.85em;word-break:break-all;color:#a6e3a1;">' + formatCiphertextFull(encStr) + '</p>';
    html += '<p style="font-size:0.8em;color:#aaa;margin-top:8px;">🔑 Dekripsi dengan Kunci Current ' + game.kunciCurrent + '</p>';
    html += '</div>';

    html += '<p style="color:#ffd700;">📋 Ciphertext yang sudah ditemukan:</p>';
    html += '<div class="hitung-box">';
    for (let i = 0; i < game.karakter.length; i++) {
        if (game.ciriDitemukan[i]) {
            const enc = currentEncrypt(game.karakter[i].jawaban, game.kunciCurrent);
            html += '🧑 ' + game.karakter[i].nama + ':\n';
            html += formatCiphertextFull(enc.join(',')) + '\n\n';
        } else {
            html += '🧑 ' + game.karakter[i].nama + ': ??? (belum ditemukan)\n\n';
        }
    }
    html += '</div>';
    if (game.ciriDitemukan.every(Boolean)) html += '<p style="color:#28a745;">✅ Semua ciphertext ditemukan! Cari pintu keluar 🚪</p>';
    html += '<div class="btn-group"><button class="btn" type="button" id="btnTutup">🚶 Lanjut Jalan</button></div>';

    document.getElementById('gameKonten').innerHTML = html;
    document.getElementById('btnTutup').onclick = function() {
        playSFX('sfxClick');
        document.getElementById('gameKonten').innerHTML =
            '<h2>🚶 Cari Karakter!</h2>' +
            '<p>Gunakan tombol ⬅️⬆️⬇️➡️ untuk jalan.</p>' +
            '<p style="color:#ffd700;">Dekripsi ciphertext mereka untuk cari yang cocok dengan pertanyaan di peti!</p>' +
            '<div class="hitung-box">' +
            '🔑 KUNCI CURRENT: ' + game.kunciCurrent +
            '</div>';
    };
}

/* ============================================================
   14. PINTU KELUAR
   ============================================================ */
function tampilkanPintuKeluar() {
    if (!game.ciriDitemukan.every(Boolean)) {
        showNotif('❌ Cari semua karakter dulu!');
        return;
    }
    setProgress(4);
    game.tebakan = null;

    let html = '<h2>🚪 Pintu Keluar</h2>';
    html += '<p>Pilih siapa imposter-nya!</p>';
    html += '<p style="color:#ffd700;">💡 Ingat: dekripsi ciphertext NPC, lalu cocokkan dengan pertanyaan di peti!</p>';
    html += '<div class="tersangka-grid">';
    for (let i = 0; i < game.karakter.length; i++) {
        html += '<div class="tersangka-card" id="card-' + i + '">';
        html += '<span class="tersangka-body">' + buatSVGKarakter(game.karakter[i].warna) + '</span>';
        html += '<div class="tersangka-nama">' + game.karakter[i].nama + '</div>';
        html += '</div>';
    }
    html += '</div>';
    html += '<div class="btn-group"><button class="btn danger" type="button" id="btnKonfirmasi">🎯 Pilih sebagai Imposter</button></div>';

    document.getElementById('gameKonten').innerHTML = html;

    for (let j = 0; j < game.karakter.length; j++) {
        document.getElementById('card-' + j).onclick = function() {
            document.querySelectorAll('.tersangka-card').forEach(c => c.classList.remove('selected'));
            this.classList.add('selected');
            game.tebakan = j;
            playSFX('sfxSelect');
        };
    }
    document.getElementById('btnKonfirmasi').onclick = konfirmasiTangkap;
}

/* ============================================================
   15. KONFIRMASI
   ============================================================ */
function konfirmasiTangkap() {
    if (game.tebakan === null) {
        showNotif('⚠️ Pilih satu karakter dulu!');
        return;
    }
    const terpilih = game.karakter[game.tebakan];
    const benar = (terpilih.nama === game.imposter.nama);

    game.riwayatTebakan.push({
        nama: terpilih.nama,
        benar: benar,
        nyawaSetelah: benar ? game.nyawa : game.nyawa - 1
    });

    if (benar) {
        playSFX('sfxWin');
        game.skor += 2;
        tambahSkorPemain(game.pemainAktif, 2);
        updateHUD();
        showNotif('🎉 Benar! +2 poin untuk ' + game.pemainAktif);
        setStory('Imposter tertangkap! Pintu terbuka!');
        setProgress(5);
        tampilkanHasil(true);
    } else {
        game.nyawa--;
        playSFX('sfxWrong');
        updateHUD();
        if (game.nyawa <= 0) {
            playSFX('sfxGameOver');
            showNotif('💀 Game Over!');
            setStory('Kamu gagal menemukan imposter!');
            tampilkanHasil(false);
        } else {
            showNotif('❌ Salah! Nyawa -1');
            document.getElementById('gameKonten').innerHTML =
                '<h2>❌ SALAH!</h2>' +
                '<div style="background:linear-gradient(135deg,rgba(220,53,69,0.3),rgba(220,53,69,0.1));border:2px solid #dc3545;padding:18px;border-radius:12px;text-align:center;margin:12px 0;">' +
                '<p><strong>' + terpilih.nama + '</strong> bukan imposter.</p>' +
                '<p>Nyawa tersisa: <strong style="color:#ffd700;font-size:1.3em;">' + game.nyawa + '</strong></p>' +
                '</div>' +
                '<p>Coba lagi! Dekripsi ciphertext dengan teliti.</p>' +
                '<div class="btn-group"><button class="btn danger" type="button" id="btnCobaLagi">🎯 Coba Lagi</button></div>';
            document.getElementById('btnCobaLagi').onclick = tampilkanPintuKeluar;
        }
    }
}

/* ============================================================
   16. HASIL AKHIR (MENANG / GAME OVER) + PENJELASAN MOD
   ============================================================ */
function tampilkanHasil(menang) {
    const offset = (game.kunciCurrent - 1) % 26;
    let html = '';

    if (menang) {
        html += '<div class="finish-menang">';
        html += '<div class="icon">🎉</div>';
        html += '<h2>SELAMAT! KAMU MENANG!</h2>';
        html += '<p>Imposter tertangkap! Pintu terbuka!</p>';
        html += '</div>';
        html += '<div class="skor-panel">';
        html += '<div class="skor-label">SKOR ' + game.pemainAktif.toUpperCase() + '</div>';
        html += '<div class="skor-besar">' + (game.leaderboard[game.pemainAktif] || 0) + '</div>';
        html += '<div class="skor-detail">';
        html += '<div class="skor-item">Level<span class="angka">' + game.level + '</span></div>';
        html += '<div class="skor-item">Nyawa<span class="angka">' + game.nyawa + '</span></div>';
        html += '<div class="skor-item">+Poin<span class="angka">+2</span></div>';
        html += '</div></div>';
    } else {
        html += '<div class="finish-kalah">';
        html += '<div class="icon">💀</div>';
        html += '<h2>GAME OVER</h2>';
        html += '<p>Kamu gagal menemukan imposter!</p>';
        html += '<p style="font-size:1.2em;margin-top:10px;">🚪🔒 PINTU TERKUNCI! 🔒🚪</p>';
        html += '</div>';
        html += '<div class="skor-panel">';
        html += '<div class="skor-label">SKOR AKHIR</div>';
        html += '<div class="skor-besar">' + (game.leaderboard[game.pemainAktif] || 0) + '</div>';
        html += '<div class="skor-detail">';
        html += '<div class="skor-item">Level<span class="angka">' + game.level + '</span></div>';
        html += '<div class="skor-item">Nyawa<span class="angka">0</span></div>';
        html += '<div class="skor-item">Total Tebakan<span class="angka">' + game.riwayatTebakan.length + '</span></div>';
        html += '</div></div>';
    }

    // ============ RIWAYAT TEBAKAN ============
    if (game.riwayatTebakan.length > 0) {
        html += '<h3>📜 RIWAYAT TEBAKAN</h3>';
        html += '<table class="tabel-verifikasi">';
        html += '<tr><th>No</th><th>Karakter</th><th>Hasil</th><th>Nyawa</th></tr>';
        game.riwayatTebakan.forEach((r, i) => {
            html += '<tr class="' + (r.benar ? 'benar' : 'salah') + '">';
            html += '<td>' + (i + 1) + '</td>';
            html += '<td>' + r.nama + '</td>';
            html += '<td>' + (r.benar ? '✅ BENAR' : '❌ SALAH') + '</td>';
            html += '<td>' + r.nyawaSetelah + '</td>';
            html += '</tr>';
        });
        html += '</table>';
    }

    // ============ VERIFIKASI PERHITUNGAN ============
    html += '<h3>📊 VERIFIKASI — CARA MENDAPATKAN JAWABAN</h3>';
    html += '<div class="hitung-box">';
    html += '<strong style="color:#ffd700;">1️⃣ PERTANYAAN (dari isi peti):</strong>\n\n';
    html += '"' + game.imposter.pertanyaan + '"\n\n';
    html += '<strong style="color:#ffd700;">2️⃣ KUNCI CURRENT & OFFSET:</strong>\n\n';
    html += 'Kunci Current : ' + game.kunciCurrent + '\n';
    html += 'Offset        : (' + game.kunciCurrent + ' − 1) mod 26 = ' + offset + '\n\n';
    html += '<strong style="color:#ffd700;">3️⃣ JAWABAN SETIAP KARAKTER:</strong>\n\n';
    for (const p of game.karakter) {
        const encC = currentEncrypt(p.jawaban, game.kunciCurrent);
        const decC = currentDecrypt(encC, game.kunciCurrent);
        const isCocok = (p.jawaban === game.dataImposter.ciri);
        html += '<strong>' + p.nama + ':</strong>\n';
        html += '  Ciphertext : ' + formatCiphertextFull(encC.join(',')) + '\n';
        html += '  Dekripsi   : "' + decC + '"\n';
        html += '  Status     : ' + (isCocok ? '✅ COCOK' : '❌ Tidak cocok') + '\n\n';
    }
    html += '</div>';

    html += '<div class="hitung-box" style="border-left-color:#ffd700;background:linear-gradient(135deg,rgba(255,215,0,0.15),rgba(255,140,0,0.05));">';
    html += '<strong style="color:#ffd700;">4️⃣ KESIMPULAN:</strong>\n\n';
    html += 'Pertanyaan         : "' + game.imposter.pertanyaan + '"\n';
    html += 'Ciri imposter      : "' + game.dataImposter.ciri + '"\n';
    html += 'Karakter yang cocok: <strong style="color:#28a745;">' + game.imposter.nama + '</strong>\n';
    html += 'Kamu pilih         : <strong>' + game.karakter[game.tebakan].nama + '</strong>\n';
    html += 'Status             : <strong style="color:' + (menang ? '#28a745' : '#dc3545') + ';">' + (menang ? '✅ BENAR' : '❌ SALAH') + '</strong>';
    html += '</div>';

    // ============ PENJELASAN MOD & RUMUS ============
    html += '<h3>🧮 PENJELASAN MOD & RUMUS</h3>';

    html += '<div class="hitung-box" style="border-left-color:#17a2b8;background:rgba(23,162,184,0.08);">';
    html += '<strong style="color:#17a2b8;">📌 1. ALGORITMA CURRENT — MOD 26</strong>\n\n';
    html += 'Algoritma Current pakai MOD 26 untuk pergeseran alfabet.\n\n';
    html += '<strong style="color:#ffd700;">Rumus Enkripsi:</strong>\n';
    html += 'C = TABEL[(pos(M) + offset) mod 26]\n\n';
    html += '<strong style="color:#ffd700;">Rumus Dekripsi:</strong>\n';
    html += 'M = HURUF[((pos(C) − offset) mod 26 + 26) mod 26]\n\n';
    html += '<strong style="color:#ffd700;">Offset:</strong>\n';
    html += 'offset = (kunci − 1) mod 26\n';
    html += 'Dengan kunci ' + game.kunciCurrent + ':\n';
    html += 'offset = (' + game.kunciCurrent + ' − 1) mod 26 = ' + offset + '\n\n';
    html += '<strong style="color:#ffd700;">Contoh Enkripsi (Huruf Pertama):</strong>\n';
    const firstTok = game.encArr[0];
    if (firstTok !== 'SP') {
        const firstHuruf = TABEL_BASE_INV[parseInt(firstTok)];
        if (firstHuruf) {
            const idxC = HURUF.indexOf(firstHuruf);
            const posAsli = ((idxC - offset) % 26 + 26) % 26;
            html += 'Plaintext awal: "' + HURUF[posAsli] + '" (posisi ' + posAsli + ')\n';
            html += '(' + posAsli + ' + ' + offset + ') mod 26 = ' + ((posAsli + offset) % 26) + '\n';
            html += 'HURUF[' + ((posAsli + offset) % 26) + '] = "' + HURUF[(posAsli + offset) % 26] + '"\n';
            html += 'TABEL["' + HURUF[(posAsli + offset) % 26] + '"] = ' + firstTok + '\n';
        }
    }
    html += '\n<em>Kenapa pakai mod 26?</em>\n';
    html += 'Karena alfabet cuma 26 huruf. Kalau posisi + offset lebih dari 25,\n';
    html += 'harus kembali ke awal (wrap around). Contoh: Z (25) + 1 = 26 → mod 26 = 0 → A.';
    html += '</div>';

    html += '<div class="hitung-box" style="border-left-color:#17a2b8;background:rgba(23,162,184,0.08);">';
    html += '<strong style="color:#17a2b8;">📌 2. RSA — MOD n & MOD φ(n)</strong>\n\n';
    html += 'RSA pakai MOD untuk batasi angka raksasa.\n\n';
    html += '<strong style="color:#ffd700;">Pembangkitan Kunci:</strong>\n';
    html += 'n = p × q = ' + game.rsa.p + ' × ' + game.rsa.q + ' = ' + game.rsa.n + '\n';
    html += 'φ(n) = (p−1)(q−1) = ' + (game.rsa.p-1) + ' × ' + (game.rsa.q-1) + ' = ' + game.rsa.phi + '\n';
    html += 'e = ' + game.rsa.e + '\n';
    html += 'd × e ≡ 1 (mod φ(n))\n';
    html += 'd × ' + game.rsa.e + ' ≡ 1 (mod ' + game.rsa.phi + ')\n';
    html += 'd = ' + game.rsa.d + '\n\n';
    html += '<strong style="color:#ffd700;">Verifikasi d:</strong>\n';
    html += game.rsa.e + ' × ' + game.rsa.d + ' = ' + (game.rsa.e * game.rsa.d) + '\n';
    html += (game.rsa.e * game.rsa.d) + ' mod ' + game.rsa.phi + ' = ' + ((game.rsa.e * game.rsa.d) % game.rsa.phi) + ' ✅\n\n';
    html += '<strong style="color:#ffd700;">Enkripsi (Kunci Current → Cipher):</strong>\n';
    html += 'C = k^e mod n\n';
    html += 'C = ' + game.kunciCurrent + '^' + game.rsa.e + ' mod ' + game.rsa.n + '\n';
    const cKunci = modPow(game.kunciCurrent, game.rsa.e, game.rsa.n);
    html += 'C = ' + cKunci + '\n\n';
    html += '<strong style="color:#ffd700;">Dekripsi (Cipher → Kunci Current):</strong>\n';
    html += 'k = C^d mod n\n';
    html += 'k = ' + cKunci + '^' + game.rsa.d + ' mod ' + game.rsa.n + '\n';
    html += 'k = ' + modPow(cKunci, game.rsa.d, game.rsa.n) + ' ✅\n\n';
    html += '<em>Kenapa pakai mod n?</em>\n';
    html += 'Tanpa mod, ' + game.kunciCurrent + '^' + game.rsa.e + ' = ' + Math.pow(game.kunciCurrent, game.rsa.e) + ' (angka besar).\n';
    html += 'Dengan mod ' + game.rsa.n + ', hasilnya tetap ≤ ' + game.rsa.n + '.\n';
    html += 'Komputer lebih efisien hitung angka kecil.';
    html += '</div>';

    // ============ TABEL PERBANDINGAN ============
    html += '<h3>📋 TABEL PERBANDINGAN</h3>';
    html += '<table class="tabel-verifikasi">';
    html += '<tr><th>Karakter</th><th>Pertanyaan</th><th>Jawaban</th><th>Status</th></tr>';
    for (let i = 0; i < game.karakter.length; i++) {
        const pp = game.karakter[i];
        const isImp = (pp.nama === game.imposter.nama);
        const isPilih = (i === game.tebakan);
        let cls = '';
        if (isImp) cls = 'benar';
        else if (isPilih && !menang) cls = 'salah';
        html += '<tr class="' + cls + '">';
        html += '<td>' + pp.nama + (isImp ? ' 👑' : '') + '</td>';
        html += '<td>"' + pp.pertanyaan + '"</td>';
        html += '<td>"' + pp.jawaban + '"</td>';
        html += '<td>' + (isImp ? '✅ IMPOSTER' : '') + '</td>';
        html += '</tr>';
    }
    html += '</table>';

    // ============ KUNCI PRIVAT RSA (kalau menang) ============
    if (menang) {
        html += '<div class="hitung-box">';
        html += '<strong style="color:#ffd700;">🔑 KUNCI PRIVAT RSA:</strong>\n\n';
        html += 'p            : ' + game.rsa.p + '\n';
        html += 'q            : ' + game.rsa.q + '\n';
        html += 'n            : ' + game.rsa.n + '\n';
        html += 'φ(n)         : ' + game.rsa.phi + '\n';
        html += 'e (publik)   : ' + game.rsa.e + '\n';
        html += 'd (privat)   : ' + game.rsa.d + '\n\n';
        html += 'Kunci Privat : (' + game.rsa.d + ', ' + game.rsa.n + ')\n';
        html += 'Peta         : ' + game.imposter.peta;
        html += '</div>';
    }

    html += renderLeaderboard();
    html += '<div class="btn-group" style="margin-top:20px;">';
    if (menang) {
        html += '<button class="btn success" id="btnLanjutLevel" type="button">➡️ Lanjut Level ' + (game.level + 1) + '</button>';
        html += '<button class="btn info" id="btnUlangLevel" type="button">🔄 Ulang Level</button>';
        html += '<button class="btn danger" id="btnMenuUtama" type="button">🏠 Menu Utama</button>';
    } else {
        html += '<button class="btn success" id="btnUlangLevel" type="button">🔄 Coba Lagi</button>';
        html += '<button class="btn danger" id="btnMenuUtama" type="button">🏠 Menu Utama</button>';
    }
    html += '</div>';

    document.getElementById('gameKonten').innerHTML = html;
    if (menang) document.getElementById('btnLanjutLevel').onclick = lanjutLevel;
    document.getElementById('btnUlangLevel').onclick = ulangLevel;
    document.getElementById('btnMenuUtama').onclick = keMenuUtama;
}

/* ============================================================
   17. AKSI LEVEL
   ============================================================ */
function lanjutLevel() {
    playSFX('sfxLevelUp');
    game.level++;
    showNotif('➡️ Level ' + game.level + ' dimulai!');
    mulaiLevelBaru();
}
function ulangLevel() {
    playSFX('sfxClick');
    showNotif('🔄 Level diulang!');
    mulaiLevelBaru();
}
function keMenuUtama() {
    playSFX('sfxClick');
    document.getElementById('gamePage').classList.add('hidden');
    document.getElementById('introPage').classList.remove('hidden');
}

/* ============================================================
   18. INIT
   ============================================================ */
function initGame() {
    game.level = 1;
    game.skor = 0;
    game.nyawa = 2;
    game.playerRow = 3;
    game.playerCol = 0;
    game.sudahBukaPeti = false;
    game.ciriDitemukan = [false, false, false];
    game.tebakan = null;
    game.riwayatTebakan = [];

    const pool = shuffle(POOL_KARAKTER);
    game.karakter = [pool[0], pool[1], pool[2]];

    buildMaze();
    renderEntitas();
    updateHUD();
    setProgress(1);
    setStory('Setup permainan dulu ya!');
    tampilkanFormSetup();

    document.getElementById('btnKiri').onclick = () => gerak(0, -1);
    document.getElementById('btnKanan').onclick = () => gerak(0, 1);
    document.getElementById('btnAtas').onclick = () => gerak(-1, 0);
    document.getElementById('btnBawah').onclick = () => gerak(1, 0);
    document.getElementById('btnAksi').onclick = () => { playSFX('sfxClick'); cekInteraksi(); };
}

document.getElementById('btnMulai').onclick = function() {
    AudioEngine.init();
    if (AudioEngine.ctx.state === 'suspended') AudioEngine.ctx.resume();
    AudioEngine.startBGM();
    playSFX('sfxClick');
    document.getElementById('introPage').classList.add('hidden');
    document.getElementById('gamePage').classList.remove('hidden');
    initGame();
};

document.getElementById('btnSound').onclick = function() {
    const muted = AudioEngine.toggleMute();
    this.textContent = muted ? '🔇' : '🔊';
    this.classList.toggle('muted', muted);
    if (!muted) playSFX('sfxClick');
};

document.addEventListener('click', function resumeAudio() {
    if (AudioEngine.ctx && AudioEngine.ctx.state === 'suspended') {
        AudioEngine.ctx.resume();
    }
}, { once: true });
</script>
</body>
</html>
