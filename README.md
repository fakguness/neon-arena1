<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Neon Arena - Online Multiplayer Shooter</title>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }

        body {
            background: #0a0a1a;
            color: #fff;
            font-family: 'Segoe UI', system-ui, -apple-system, sans-serif;
            overflow: hidden;
            height: 100vh;
            width: 100vw;
        }

        /* ===== REKLAM ALANI 1: ÜST BANNER ===== */
        .ad-top {
            position: fixed;
            top: 0; left: 0; right: 0;
            height: 90px;
            background: rgba(0,0,0,0.8);
            display: flex;
            align-items: center;
            justify-content: center;
            z-index: 1000;
            border-bottom: 1px solid rgba(0,212,255,0.2);
        }

        .ad-placeholder {
            background: rgba(255,255,255,0.05);
            border: 2px dashed rgba(255,255,255,0.2);
            display: flex;
            align-items: center;
            justify-content: center;
            color: rgba(255,255,255,0.4);
            font-size: 12px;
            text-align: center;
        }

        .ad-top .ad-placeholder {
            width: 728px; height: 90px; max-width: 95vw;
        }

        /* ===== REKLAM ALANI 2: SOL YAN ===== */
        .ad-left {
            position: fixed;
            left: 0; top: 90px; bottom: 0;
            width: 160px;
            background: rgba(0,0,0,0.6);
            display: flex;
            align-items: center;
            justify-content: center;
            z-index: 999;
            border-right: 1px solid rgba(0,212,255,0.1);
        }

        .ad-left .ad-placeholder {
            width: 160px; height: 600px;
            writing-mode: vertical-rl;
        }

        /* ===== REKLAM ALANI 3: SAĞ YAN ===== */
        .ad-right {
            position: fixed;
            right: 0; top: 90px; bottom: 0;
            width: 160px;
            background: rgba(0,0,0,0.6);
            display: flex;
            align-items: center;
            justify-content: center;
            z-index: 999;
            border-left: 1px solid rgba(0,212,255,0.1);
        }

        .ad-right .ad-placeholder {
            width: 160px; height: 600px;
            writing-mode: vertical-rl;
        }

        /* ===== REKLAM ALANI 4: MOBİL ALT ===== */
        .ad-mobile {
            position: fixed;
            bottom: 0; left: 0; right: 0;
            height: 50px;
            background: rgba(0,0,0,0.9);
            display: none;
            align-items: center;
            justify-content: center;
            z-index: 1000;
            border-top: 1px solid rgba(0,212,255,0.2);
        }

        .ad-mobile .ad-placeholder {
            width: 320px; height: 50px;
        }

        /* ===== ANA OYUN ALANI ===== */
        .game-area {
            position: fixed;
            top: 90px;
            left: 160px;
            right: 160px;
            bottom: 0;
            display: flex;
            flex-direction: column;
        }

        #gameCanvas {
            flex: 1;
            width: 100%;
            height: 100%;
            cursor: crosshair;
            touch-action: none;
        }

        /* ===== UI PANEL ===== */
        .ui-panel {
            position: absolute;
            top: 10px; left: 10px;
            background: rgba(0,0,0,0.7);
            border: 1px solid rgba(0,212,255,0.3);
            border-radius: 8px;
            padding: 12px;
            min-width: 180px;
            backdrop-filter: blur(5px);
            z-index: 100;
        }

        .ui-title {
            font-size: 0.75em;
            color: rgba(255,255,255,0.5);
            text-transform: uppercase;
            letter-spacing: 1px;
            margin-bottom: 8px;
        }

        .score-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 4px 0;
            font-size: 0.9em;
            border-bottom: 1px solid rgba(255,255,255,0.05);
        }

        .score-item:last-child { border-bottom: none; }

        .score-name {
            display: flex;
            align-items: center;
            gap: 6px;
        }

        .score-color {
            width: 10px; height: 10px;
            border-radius: 50%;
            display: inline-block;
        }

        .score-value {
            font-family: 'Courier New', monospace;
            font-weight: 700;
            color: #ffa502;
        }

        /* ===== HP BAR ===== */
        .hp-bar-container {
            position: absolute;
            bottom: 20px; left: 50%;
            transform: translateX(-50%);
            width: 200px;
            z-index: 100;
        }

        .hp-bar-bg {
            width: 100%; height: 20px;
            background: rgba(0,0,0,0.7);
            border: 2px solid rgba(255,255,255,0.2);
            border-radius: 10px;
            overflow: hidden;
        }

        .hp-bar-fill {
            height: 100%;
            background: linear-gradient(90deg, #ff4757, #ffa502, #2ed573);
            transition: width 0.2s ease;
            border-radius: 8px;
        }

        .hp-text {
            text-align: center;
            margin-top: 4px;
            font-size: 0.8em;
            color: rgba(255,255,255,0.7);
        }

        /* ===== CHAT ===== */
        .chat-panel {
            position: absolute;
            bottom: 60px; right: 10px;
            width: 280px;
            max-height: 200px;
            background: rgba(0,0,0,0.7);
            border: 1px solid rgba(0,212,255,0.3);
            border-radius: 8px;
            display: flex;
            flex-direction: column;
            z-index: 100;
            backdrop-filter: blur(5px);
        }

        .chat-messages {
            flex: 1;
            overflow-y: auto;
            padding: 8px;
            font-size: 0.8em;
            max-height: 150px;
        }

        .chat-msg {
            margin-bottom: 4px;
            line-height: 1.4;
        }

        .chat-name {
            font-weight: 700;
            color: #00d4ff;
        }

        .chat-input-area {
            display: flex;
            padding: 6px;
            border-top: 1px solid rgba(255,255,255,0.1);
        }

        .chat-input {
            flex: 1;
            background: rgba(255,255,255,0.1);
            border: 1px solid rgba(255,255,255,0.2);
            border-radius: 4px;
            padding: 6px 10px;
            color: #fff;
            font-size: 0.85em;
            outline: none;
        }

        .chat-input:focus {
            border-color: rgba(0,212,255,0.5);
        }

        .chat-send {
            background: rgba(0,212,255,0.3);
            border: none;
            color: #fff;
            padding: 6px 12px;
            margin-left: 6px;
            border-radius: 4px;
            cursor: pointer;
            font-size: 0.85em;
        }

        .chat-send:hover {
            background: rgba(0,212,255,0.5);
        }

        /* ===== KILL FEED ===== */
        .kill-feed {
            position: absolute;
            top: 10px; right: 10px;
            z-index: 100;
            display: flex;
            flex-direction: column;
            align-items: flex-end;
            gap: 4px;
        }

        .kill-msg {
            background: rgba(0,0,0,0.7);
            padding: 6px 12px;
            border-radius: 4px;
            font-size: 0.85em;
            animation: slideIn 0.3s ease, fadeOut 0.5s ease 4s forwards;
            border-left: 3px solid #ff4757;
        }

        @keyframes slideIn {
            from { transform: translateX(50px); opacity: 0; }
            to { transform: translateX(0); opacity: 1; }
        }

        @keyframes fadeOut {
            to { opacity: 0; transform: translateX(20px); }
        }

        /* ===== GİRİŞ EKRANI ===== */
        .login-screen, .game-over-screen {
            position: fixed;
            top: 0; left: 0; right: 0; bottom: 0;
            background: linear-gradient(135deg, #0c0c1d 0%, #1a1a2e 50%, #16213e 100%);
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            z-index: 2000;
            transition: opacity 0.5s ease;
        }

        .login-screen.hidden, .game-over-screen.hidden {
            opacity: 0;
            pointer-events: none;
        }

        .game-title {
            font-size: 4em;
            font-weight: 900;
            text-transform: uppercase;
            letter-spacing: 6px;
            background: linear-gradient(90deg, #00d4ff, #7b2ff7, #f107a3);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
            margin-bottom: 10px;
            text-shadow: 0 0 40px rgba(0,212,255,0.3);
        }

        .game-subtitle {
            font-size: 1.1em;
            color: rgba(255,255,255,0.5);
            letter-spacing: 3px;
            margin-bottom: 40px;
        }

        .login-box {
            background: rgba(255,255,255,0.05);
            border: 1px solid rgba(0,212,255,0.3);
            border-radius: 16px;
            padding: 40px;
            width: 400px;
            max-width: 90vw;
            backdrop-filter: blur(10px);
        }

        .input-group {
            margin-bottom: 20px;
        }

        .input-label {
            display: block;
            font-size: 0.85em;
            color: rgba(255,255,255,0.6);
            margin-bottom: 8px;
            text-transform: uppercase;
            letter-spacing: 1px;
        }

        .input-field {
            width: 100%;
            padding: 14px 16px;
            background: rgba(0,0,0,0.3);
            border: 2px solid rgba(255,255,255,0.1);
            border-radius: 8px;
            color: #fff;
            font-size: 1em;
            outline: none;
            transition: all 0.3s ease;
        }

        .input-field:focus {
            border-color: #00d4ff;
            box-shadow: 0 0 15px rgba(0,212,255,0.2);
        }

        .btn {
            width: 100%;
            padding: 16px;
            font-size: 1em;
            font-weight: 700;
            text-transform: uppercase;
            letter-spacing: 2px;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            transition: all 0.3s ease;
            font-family: inherit;
        }

        .btn-primary {
            background: linear-gradient(90deg, #00d4ff, #7b2ff7);
            color: white;
            box-shadow: 0 4px 20px rgba(0,212,255,0.3);
        }

        .btn-primary:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 30px rgba(0,212,255,0.5);
        }

        .btn-secondary {
            background: transparent;
            color: rgba(255,255,255,0.8);
            border: 2px solid rgba(255,255,255,0.2);
            margin-top: 10px;
        }

        .btn-secondary:hover {
            border-color: rgba(255,255,255,0.5);
            background: rgba(255,255,255,0.05);
        }

        .connection-status {
            position: fixed;
            top: 100px; left: 50%;
            transform: translateX(-50%);
            padding: 8px 20px;
            border-radius: 20px;
            font-size: 0.85em;
            font-weight: 600;
            z-index: 3000;
            transition: all 0.3s ease;
        }

        .status-connecting {
            background: rgba(255,165,2,0.2);
            color: #ffa502;
            border: 1px solid rgba(255,165,2,0.5);
        }

        .status-connected {
            background: rgba(46,213,115,0.2);
            color: #2ed573;
            border: 1px solid rgba(46,213,115,0.5);
        }

        /* ===== GAME OVER EKRANI ===== */
        .game-over-content {
            text-align: center;
        }

        .game-over-title {
            font-size: 3em;
            font-weight: 900;
            text-transform: uppercase;
            margin-bottom: 20px;
        }

        .winner-announce {
            font-size: 1.5em;
            color: #ffa502;
            margin-bottom: 30px;
        }

        .final-scores {
            background: rgba(255,255,255,0.05);
            border: 1px solid rgba(0,212,255,0.2);
            border-radius: 12px;
            padding: 24px;
            width: 400px;
            max-width: 90vw;
            margin-bottom: 30px;
        }

        .final-score-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 10px 0;
            border-bottom: 1px solid rgba(255,255,255,0.05);
        }

        .final-score-item.winner {
            background: rgba(255,165,2,0.1);
            margin: 0 -24px;
            padding: 10px 24px;
            border-left: 3px solid #ffa502;
        }

        .final-score-rank {
            font-weight: 700;
            color: #ffa502;
            width: 30px;
        }

        /* ===== MOBİL KONTROLLER ===== */
        .mobile-controls {
            position: fixed;
            bottom: 60px;
            left: 0; right: 0;
            display: none;
            justify-content: space-between;
            padding: 0 20px;
            z-index: 500;
            pointer-events: none;
        }

        .mobile-joystick {
            width: 120px; height: 120px;
            background: rgba(0,212,255,0.2);
            border: 2px solid rgba(0,212,255,0.4);
            border-radius: 50%;
            position: relative;
            pointer-events: all;
        }

        .mobile-joystick-inner {
            width: 50px; height: 50px;
            background: rgba(0,212,255,0.5);
            border-radius: 50%;
            position: absolute;
            top: 50%; left: 50%;
            transform: translate(-50%, -50%);
            transition: transform 0.1s ease;
        }

        .mobile-fire-btn {
            width: 80px; height: 80px;
            background: rgba(255,71,87,0.3);
            border: 2px solid rgba(255,71,87,0.5);
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1.5em;
            pointer-events: all;
            user-select: none;
            -webkit-user-select: none;
        }

        .mobile-fire-btn:active {
            background: rgba(255,71,87,0.5);
        }

        /* ===== POWERUP INDICATOR ===== */
        .powerup-indicator {
            position: absolute;
            top: 50%; left: 50%;
            transform: translate(-50%, -50%);
            background: rgba(0,0,0,0.8);
            padding: 10px 20px;
            border-radius: 20px;
            border: 1px solid;
            font-weight: 700;
            font-size: 0.9em;
            opacity: 0;
            transition: opacity 0.3s ease;
            pointer-events: none;
            z-index: 150;
        }

        .powerup-indicator.active {
            opacity: 1;
        }

        /* ===== TIMER ===== */
        .game-timer {
            position: absolute;
            top: 10px; left: 50%;
            transform: translateX(-50%);
            background: rgba(0,0,0,0.7);
            padding: 8px 20px;
            border-radius: 20px;
            border: 1px solid rgba(0,212,255,0.3);
            font-family: 'Courier New', monospace;
            font-size: 1.2em;
            font-weight: 700;
            color: #00d4ff;
            z-index: 100;
        }

        .game-timer.warning {
            color: #ff4757;
            border-color: rgba(255,71,87,0.5);
            animation: pulse 1s infinite;
        }

        @keyframes pulse {
            0%, 100% { opacity: 1; }
            50% { opacity: 0.5; }
        }

        /* ===== MINIMAP ===== */
        .minimap {
            position: absolute;
            bottom: 20px; right: 20px;
            width: 150px; height: 100px;
            background: rgba(0,0,0,0.7);
            border: 1px solid rgba(0,212,255,0.3);
            border-radius: 4px;
            z-index: 100;
        }

        .minimap canvas {
            width: 100%; height: 100%;
        }

        /* ===== RESPONSIVE ===== */
        @media (max-width: 1100px) {
            .ad-left, .ad-right { display: none; }
            .game-area { left: 0; right: 0; }
        }

        @media (max-width: 768px) {
            .ad-top { height: 50px; }
            .ad-top .ad-placeholder { width: 320px; height: 50px; }
            .ad-mobile { display: flex; }
            .game-area { top: 50px; bottom: 50px; }
            .game-title { font-size: 2.5em; }
            .chat-panel { width: 200px; }
            .ui-panel { min-width: 140px; font-size: 0.85em; }
            .mobile-controls { display: flex; }
        }

        @media (max-width: 480px) {
            .game-title { font-size: 1.8em; letter-spacing: 2px; }
            .login-box { padding: 24px; }
            .chat-panel { display: none; }
        }

        /* ===== SCROLLBAR ===== */
        ::-webkit-scrollbar { width: 6px; }
        ::-webkit-scrollbar-track { background: rgba(0,0,0,0.2); }
        ::-webkit-scrollbar-thumb { background: rgba(0,212,255,0.3); border-radius: 3px; }
        ::-webkit-scrollbar-thumb:hover { background: rgba(0,212,255,0.5); }

        /* ===== LOBBY ===== */
        .lobby-screen {
            position: fixed;
            top: 0; left: 0; right: 0; bottom: 0;
            background: linear-gradient(135deg, #0c0c1d 0%, #1a1a2e 50%, #16213e 100%);
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            z-index: 2000;
            transition: opacity 0.5s ease;
        }

        .lobby-screen.hidden {
            opacity: 0;
            pointer-events: none;
        }

        .lobby-content {
            display: flex;
            gap: 30px;
            width: 900px;
            max-width: 95vw;
        }

        .lobby-section {
            flex: 1;
            background: rgba(255,255,255,0.05);
            border: 1px solid rgba(0,212,255,0.2);
            border-radius: 12px;
            padding: 24px;
        }

        .section-title {
            font-size: 1.2em;
            font-weight: 700;
            margin-bottom: 16px;
            color: #00d4ff;
            text-transform: uppercase;
            letter-spacing: 2px;
        }

        .room-list {
            max-height: 300px;
            overflow-y: auto;
        }

        .room-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 12px;
            background: rgba(0,0,0,0.2);
            border-radius: 8px;
            margin-bottom: 8px;
            cursor: pointer;
            transition: all 0.2s ease;
            border: 1px solid transparent;
        }

        .room-item:hover {
            border-color: rgba(0,212,255,0.4);
            background: rgba(0,212,255,0.05);
        }

        .room-item.full {
            opacity: 0.5;
            cursor: not-allowed;
        }

        .room-info h4 {
            font-size: 0.95em;
            margin-bottom: 4px;
        }

        .room-info span {
            font-size: 0.8em;
            color: rgba(255,255,255,0.5);
        }

        .room-join-btn {
            padding: 6px 16px;
            background: rgba(0,212,255,0.2);
            border: 1px solid rgba(0,212,255,0.4);
            color: #00d4ff;
            border-radius: 4px;
            cursor: pointer;
            font-size: 0.85em;
        }

        .room-join-btn:hover {
            background: rgba(0,212,255,0.4);
        }

        .create-room-form {
            display: flex;
            flex-direction: column;
            gap: 12px;
        }

        .create-room-form .input-field {
            padding: 10px 14px;
        }

        .checkbox-group {
            display: flex;
            align-items: center;
            gap: 8px;
            font-size: 0.9em;
            color: rgba(255,255,255,0.7);
        }

        .checkbox-group input {
            width: 18px; height: 18px;
            accent-color: #00d4ff;
        }

        @media (max-width: 768px) {
            .lobby-content { flex-direction: column; }
        }
    </style>
<base target="_blank">
</head>
<body>
    <!-- ===== REKLAM ALANI 1: ÜST BANNER ===== -->
    <div class="ad-top">
        <div class="ad-placeholder" id="adTop">
            <span>📢 Üst Banner Reklam Alanı<br><small>728x90 | Responsive</small></span>
        </div>
    </div>

    <!-- ===== REKLAM ALANI 2: SOL YAN ===== -->
    <div class="ad-left" id="adLeft">
        <div class="ad-placeholder">
            <span>📢 Sol Yan Reklam<br><small>160x600</small></span>
        </div>
    </div>

    <!-- ===== REKLAM ALANI 3: SAĞ YAN ===== -->
    <div class="ad-right" id="adRight">
        <div class="ad-placeholder">
            <span>📢 Sağ Yan Reklam<br><small>160x600</small></span>
        </div>
    </div>

    <!-- ===== REKLAM ALANI 4: MOBİL ALT ===== -->
    <div class="ad-mobile" id="adMobile">
        <div class="ad-placeholder">
            <span>📢 Mobil Banner<br><small>320x50</small></span>
        </div>
    </div>

    <!-- ===== BAĞLANTI DURUMU ===== -->
    <div class="connection-status status-connecting" id="connStatus">
        🔄 Sunucuya bağlanılıyor...
    </div>

    <!-- ===== GİRİŞ EKRANI ===== -->
    <div class="login-screen" id="loginScreen">
        <h1 class="game-title">Neon Arena</h1>
        <p class="game-subtitle">Online Multiplayer Shooter</p>

        <div class="login-box">
            <div class="input-group">
                <label class="input-label">Oyuncu Adı</label>
                <input type="text" class="input-field" id="playerName" placeholder="Adınızı girin..." maxlength="15" value="Oyuncu">
            </div>

            <button class="btn btn-primary" id="playBtn">🎮 HIZLI OYNA</button>
            <button class="btn btn-secondary" id="createRoomBtn">🏠 ODA BUL</button>
        </div>

        <div style="margin-top: 30px; text-align: center; color: rgba(255,255,255,0.4); font-size: 0.85em;">
            <p>🎯 WASD / Ok tuşları ile hareket et</p>
            <p>🖱️ Mouse ile nişan al, tıkla ateş et</p>
            <p>📱 Mobilde joystick ile kontrol et</p>
        </div>
    </div>

    <!-- ===== LOBBY EKRANI ===== -->
    <div class="lobby-screen hidden" id="lobbyScreen">
        <h1 class="game-title" style="font-size: 2.5em; margin-bottom: 30px;">Neon Arena</h1>

        <div class="lobby-content">
            <div class="lobby-section">
                <h3 class="section-title">🏛️ Aktif Odalar</h3>
                <div class="room-list" id="roomList">
                    <div class="room-item" onclick="joinRoom('arena-1')">
                        <div class="room-info">
                            <h4>🔥 Arena 1 - Deathmatch</h4>
                            <span>3/8 oyuncu | 🎮 Oynanıyor</span>
                        </div>
                        <button class="room-join-btn">KATIL</button>
                    </div>
                    <div class="room-item" onclick="joinRoom('arena-2')">
                        <div class="room-info">
                            <h4>⚡ Arena 2 - Hızlı Maç</h4>
                            <span>5/8 oyuncu | 🎮 Oynanıyor</span>
                        </div>
                        <button class="room-join-btn">KATIL</button>
                    </div>
                    <div class="room-item" onclick="joinRoom('arena-3')">
                        <div class="room-info">
                            <h4>🛡️ Arena 3 - Yeni Başlayan</h4>
                            <span>1/8 oyuncu | ⏳ Bekliyor</span>
                        </div>
                        <button class="room-join-btn">KATIL</button>
                    </div>
                    <div class="room-item full">
                        <div class="room-info">
                            <h4>💀 Arena 4 - Prolar</h4>
                            <span>8/8 oyuncu | 🎮 Oynanıyor</span>
                        </div>
                        <span style="color:#ff4757; font-size:0.85em;">DOLU</span>
                    </div>
                </div>
            </div>

            <div class="lobby-section">
                <h3 class="section-title">🏗️ Oda Oluştur</h3>
                <div class="create-room-form">
                    <input type="text" class="input-field" id="newRoomName" placeholder="Oda adı..." maxlength="20">
                    <select class="input-field" id="newRoomMaxPlayers" style="background: rgba(0,0,0,0.3); color: #fff;">
                        <option value="4">4 Oyuncu</option>
                        <option value="6">6 Oyuncu</option>
                        <option value="8" selected>8 Oyuncu</option>
                    </select>
                    <div class="checkbox-group">
                        <input type="checkbox" id="newRoomPrivate">
                        <label for="newRoomPrivate">Özel Oda</label>
                    </div>
                    <button class="btn btn-primary" id="createRoomSubmitBtn" onclick="createRoom()">ODA OLUŞTUR</button>
                </div>
            </div>
        </div>

        <button class="btn btn-secondary" id="backToLoginBtn" onclick="showLogin()" style="width: auto; padding: 12px 30px; margin-top: 20px;">← Geri</button>
    </div>

    <!-- ===== GAME OVER EKRANI ===== -->
    <div class="game-over-screen hidden" id="gameOverScreen">
        <div class="game-over-content">
            <h2 class="game-over-title" id="gameOverTitle">MAÇ BİTTİ</h2>
            <div class="winner-announce" id="winnerAnnounce">🏆 Kazanan: -</div>

            <div class="final-scores" id="finalScores"></div>

            <div style="display: flex; gap: 12px;">
                <button class="btn btn-primary" onclick="playAgain()">TEKRAR OYNA</button>
                <button class="btn btn-secondary" onclick="showLobby()">LOBBİYE DÖN</button>
            </div>
        </div>
    </div>

    <!-- ===== OYUN ALANI ===== -->
    <div class="game-area" id="gameArea" style="display: none;">
        <canvas id="gameCanvas"></canvas>

        <!-- UI Panel - Skor Tablosu -->
        <div class="ui-panel" id="scorePanel">
            <div class="ui-title">📊 Skor Tablosu</div>
            <div id="scoreList"></div>
        </div>

        <!-- HP Bar -->
        <div class="hp-bar-container" id="hpBarContainer" style="display: none;">
            <div class="hp-bar-bg">
                <div class="hp-bar-fill" id="hpBarFill" style="width: 100%;"></div>
            </div>
            <div class="hp-text" id="hpText">100 / 100 HP</div>
        </div>

        <!-- Timer -->
        <div class="game-timer" id="gameTimer">05:00</div>

        <!-- Kill Feed -->
        <div class="kill-feed" id="killFeed"></div>

        <!-- Chat -->
        <div class="chat-panel" id="chatPanel">
            <div class="chat-messages" id="chatMessages"></div>
            <div class="chat-input-area">
                <input type="text" class="chat-input" id="chatInput" placeholder="Mesaj yaz..." maxlength="100">
                <button class="chat-send" onclick="sendChat()">➤</button>
            </div>
        </div>

        <!-- Powerup Indicator -->
        <div class="powerup-indicator" id="powerupIndicator">⚡ ÇİFT ATIŞ!</div>

        <!-- Minimap -->
        <div class="minimap" id="minimapContainer">
            <canvas id="minimapCanvas" width="150" height="100"></canvas>
        </div>

        <!-- Mobil Kontroller -->
        <div class="mobile-controls" id="mobileControls">
            <div class="mobile-joystick" id="mobileJoystick">
                <div class="mobile-joystick-inner" id="joystickInner"></div>
            </div>
            <div class="mobile-fire-btn" id="mobileFireBtn">🔥</div>
        </div>
    </div>

    <script>

        // ==================== KONFİGÜRASYON ====================
        const CONFIG = {
            GAME_WIDTH: 1200,
            GAME_HEIGHT: 800,
            PLAYER_RADIUS: 15,
            BULLET_SPEED: 12,
            PLAYER_SPEED: 5,
            BOT_COUNT: 7,
            GAME_DURATION: 300000, // 5 dakika
            POWERUP_SPAWN_INTERVAL: 8000
        };

        // ==================== GLOBAL DEĞİŞKENLER ====================
        let myName = '';
        let gameState = 'menu'; // menu, lobby, playing, ended
        let players = new Map(); // id -> player object
        let myId = 'player_0';
        let bullets = [];
        let powerUps = [];
        let obstacles = [];
        let particles = [];
        let scores = [];
        let chatMessages = [];
        let killFeed = [];
        let gameTime = 0;
        let camera = { x: 0, y: 0 };
        let lastTime = 0;
        let gameTimer = null;
        let powerUpTimer = null;
        let botTimer = null;
        let shakeScreen = 0;

        // Input
        let inputs = { up: false, down: false, left: false, right: false, shoot: false };
        let mousePos = { x: 0, y: 0 };
        let mouseAngle = 0;
        let isMouseDown = false;

        // Canvas
        const canvas = document.getElementById('gameCanvas');
        const ctx = canvas.getContext('2d');
        const minimapCanvas = document.getElementById('minimapCanvas');
        const minimapCtx = minimapCanvas.getContext('2d');

        // Ses
        let soundEnabled = true;
        const AudioContext = window.AudioContext || window.webkitAudioContext;
        let audioCtx = null;

        function initAudio() {
            if (!audioCtx) audioCtx = new AudioContext();
        }

        function playSound(type) {
            if (!soundEnabled || !audioCtx) return;
            try {
                const osc = audioCtx.createOscillator();
                const gain = audioCtx.createGain();
                osc.connect(gain);
                gain.connect(audioCtx.destination);

                switch(type) {
                    case 'shoot':
                        osc.frequency.setValueAtTime(600, audioCtx.currentTime);
                        osc.frequency.exponentialRampToValueAtTime(300, audioCtx.currentTime + 0.08);
                        gain.gain.setValueAtTime(0.08, audioCtx.currentTime);
                        gain.gain.exponentialRampToValueAtTime(0.01, audioCtx.currentTime + 0.08);
                        osc.start(); osc.stop(audioCtx.currentTime + 0.08);
                        break;
                    case 'hit':
                        osc.type = 'sawtooth';
                        osc.frequency.setValueAtTime(150, audioCtx.currentTime);
                        gain.gain.setValueAtTime(0.1, audioCtx.currentTime);
                        gain.gain.exponentialRampToValueAtTime(0.01, audioCtx.currentTime + 0.15);
                        osc.start(); osc.stop(audioCtx.currentTime + 0.15);
                        break;
                    case 'kill':
                        osc.type = 'square';
                        osc.frequency.setValueAtTime(400, audioCtx.currentTime);
                        osc.frequency.exponentialRampToValueAtTime(800, audioCtx.currentTime + 0.2);
                        gain.gain.setValueAtTime(0.1, audioCtx.currentTime);
                        gain.gain.exponentialRampToValueAtTime(0.01, audioCtx.currentTime + 0.3);
                        osc.start(); osc.stop(audioCtx.currentTime + 0.3);
                        break;
                    case 'powerup':
                        osc.type = 'sine';
                        osc.frequency.setValueAtTime(400, audioCtx.currentTime);
                        osc.frequency.exponentialRampToValueAtTime(1200, audioCtx.currentTime + 0.3);
                        gain.gain.setValueAtTime(0.1, audioCtx.currentTime);
                        gain.gain.exponentialRampToValueAtTime(0.01, audioCtx.currentTime + 0.3);
                        osc.start(); osc.stop(audioCtx.currentTime + 0.3);
                        break;
                    case 'explosion':
                        const buffer = audioCtx.createBuffer(1, audioCtx.sampleRate * 0.3, audioCtx.sampleRate);
                        const data = buffer.getChannelData(0);
                        for (let i = 0; i < data.length; i++) {
                            data[i] = (Math.random() * 2 - 1) * Math.pow(1 - i / data.length, 2);
                        }
                        const noise = audioCtx.createBufferSource();
                        noise.buffer = buffer;
                        const noiseGain = audioCtx.createGain();
                        noiseGain.gain.setValueAtTime(0.2, audioCtx.currentTime);
                        noiseGain.gain.exponentialRampToValueAtTime(0.01, audioCtx.currentTime + 0.3);
                        noise.connect(noiseGain);
                        noiseGain.connect(audioCtx.destination);
                        noise.start();
                        break;
                }
            } catch(e) {}
        }

        // ==================== BOT İSİMLERİ ====================
        const botNames = ['Shadow', 'Viper', 'Ghost', 'Titan', 'Blaze', 'Frost', 'Storm', 'Raven', 'Wolf', 'Phoenix'];
        const botColors = ['#ff4757', '#ffa502', '#2ed573', '#ff6b81', '#7b2ff7', '#fff200', '#ff9ff3', '#00d4ff', '#e74c3c', '#9b59b6'];

        // ==================== HARİTA OLUŞTURMA ====================
        function generateMap() {
            obstacles = [];
            for (let i = 0; i < 15; i++) {
                let x, y, w, h, safe;
                do {
                    x = Math.random() * (CONFIG.GAME_WIDTH - 150) + 50;
                    y = Math.random() * (CONFIG.GAME_HEIGHT - 150) + 50;
                    w = Math.random() * 80 + 40;
                    h = Math.random() * 80 + 40;
                    const cx = x + w/2, cy = y + h/2;
                    const dist = Math.sqrt((cx - CONFIG.GAME_WIDTH/2)**2 + (cy - CONFIG.GAME_HEIGHT/2)**2);
                    safe = dist > 150;
                } while (!safe);

                obstacles.push({
                    x, y, width: w, height: h,
                    type: Math.random() > 0.7 ? 'breakable' : 'solid',
                    hp: 3
                });
            }
        }

        // ==================== OYUNCU OLUŞTURMA ====================
        function createPlayer(id, name, isBot = false, color = '#00d4ff') {
            let x, y, safe;
            do {
                x = Math.random() * (CONFIG.GAME_WIDTH - 100) + 50;
                y = Math.random() * (CONFIG.GAME_HEIGHT - 100) + 50;
                safe = true;
                for (const obs of obstacles) {
                    if (x > obs.x - 30 && x < obs.x + obs.width + 30 &&
                        y > obs.y - 30 && y < obs.y + obs.height + 30) {
                        safe = false; break;
                    }
                }
            } while (!safe);

            return {
                id, name, x, y,
                angle: Math.random() * Math.PI * 2,
                hp: 100, maxHp: 100,
                alive: true,
                invulnerable: 120,
                color,
                speed: CONFIG.PLAYER_SPEED,
                powerUps: { double: false, speed: false, shield: false },
                powerUpTimer: 0,
                speedBoost: 0,
                shieldTimer: 0,
                lastShot: 0,
                isBot,
                botTarget: null,
                botChangeDir: 0,
                score: 0,
                kills: 0,
                deaths: 0
            };
        }

        // ==================== BOT AI ====================
        function updateBot(bot) {
            if (!bot.alive) return;

            // Yön değiştirme zamanı
            bot.botChangeDir--;
            if (bot.botChangeDir <= 0) {
                bot.botChangeDir = Math.random() * 60 + 30;
                // En yakın oyuncuyu hedef al
                let nearest = null;
                let nearestDist = Infinity;
                players.forEach((p, id) => {
                    if (id !== bot.id && p.alive) {
                        const d = Math.sqrt((p.x - bot.x)**2 + (p.y - bot.y)**2);
                        if (d < nearestDist) {
                            nearestDist = d;
                            nearest = p;
                        }
                    }
                });
                bot.botTarget = nearest;
            }

            if (bot.botTarget && bot.botTarget.alive) {
                const dx = bot.botTarget.x - bot.x;
                const dy = bot.botTarget.y - bot.y;
                const dist = Math.sqrt(dx*dx + dy*dy);
                bot.angle = Math.atan2(dy, dx);

                // Hareket
                const speed = bot.powerUps.speed ? CONFIG.PLAYER_SPEED * 1.5 : CONFIG.PLAYER_SPEED;
                let moveX = 0, moveY = 0;

                if (dist > 200) {
                    moveX = Math.cos(bot.angle) * speed;
                    moveY = Math.sin(bot.angle) * speed;
                } else if (dist < 100) {
                    moveX = -Math.cos(bot.angle) * speed;
                    moveY = -Math.sin(bot.angle) * speed;
                }

                // Engel kontrolü
                let newX = bot.x + moveX;
                let newY = bot.y + moveY;
                let collides = false;
                for (const obs of obstacles) {
                    if (newX + CONFIG.PLAYER_RADIUS > obs.x && newX - CONFIG.PLAYER_RADIUS < obs.x + obs.width &&
                        newY + CONFIG.PLAYER_RADIUS > obs.y && newY - CONFIG.PLAYER_RADIUS < obs.y + obs.height) {
                        collides = true; break;
                    }
                }
                if (!collides) {
                    bot.x = Math.max(CONFIG.PLAYER_RADIUS, Math.min(CONFIG.GAME_WIDTH - CONFIG.PLAYER_RADIUS, newX));
                    bot.y = Math.max(CONFIG.PLAYER_RADIUS, Math.min(CONFIG.GAME_HEIGHT - CONFIG.PLAYER_RADIUS, newY));
                }

                // Ateş
                if (dist < 400 && Math.random() < 0.05) {
                    botShoot(bot);
                }
            }

            // Güçlendirme toplama
            powerUps.forEach(p => {
                const d = Math.sqrt((p.x - bot.x)**2 + (p.y - bot.y)**2);
                if (d < 50) {
                    collectPowerUp(bot, p);
                }
            });
        }

        function botShoot(bot) {
            const now = Date.now();
            if (now - bot.lastShot < (bot.powerUps.double ? 100 : 150)) return;
            bot.lastShot = now;

            const angles = bot.powerUps.double ? 
                [bot.angle - 0.15, bot.angle + 0.15] : [bot.angle];

            angles.forEach(angle => {
                bullets.push({
                    x: bot.x + Math.cos(angle) * 20,
                    y: bot.y + Math.sin(angle) * 20,
                    angle,
                    ownerId: bot.id,
                    life: 120,
                    damage: bot.powerUps.double ? 15 : 25,
                    color: bot.color
                });
            });
        }

        // ==================== GÜÇLENDİRME SİSTEMİ ====================
        function spawnPowerUp() {
            const types = ['double', 'speed', 'shield', 'health'];
            const type = types[Math.floor(Math.random() * types.length)];
            const colors = { double: '#00ff88', speed: '#ffa502', shield: '#00d4ff', health: '#ff4757' };

            let x, y, safe;
            do {
                x = Math.random() * (CONFIG.GAME_WIDTH - 100) + 50;
                y = Math.random() * (CONFIG.GAME_HEIGHT - 100) + 50;
                safe = true;
                for (const obs of obstacles) {
                    if (x > obs.x && x < obs.x + obs.width && y > obs.y && y < obs.y + obs.height) {
                        safe = false; break;
                    }
                }
            } while (!safe);

            powerUps.push({
                id: Date.now() + Math.random(),
                x, y, type,
                color: colors[type],
                phase: 0
            });
        }

        function collectPowerUp(player, powerUp) {
            const idx = powerUps.indexOf(powerUp);
            if (idx > -1) powerUps.splice(idx, 1);

            playSound('powerup');

            switch(powerUp.type) {
                case 'double':
                    player.powerUps.double = true;
                    player.powerUpTimer = 600;
                    if (!player.isBot) showPowerUpIndicator('double');
                    break;
                case 'speed':
                    player.powerUps.speed = true;
                    player.speedBoost = 300;
                    if (!player.isBot) showPowerUpIndicator('speed');
                    break;
                case 'shield':
                    player.powerUps.shield = true;
                    player.shieldTimer = 300;
                    if (!player.isBot) showPowerUpIndicator('shield');
                    break;
                case 'health':
                    player.hp = Math.min(100, player.hp + 50);
                    if (!player.isBot) {
                        showPowerUpIndicator('health');
                        updateHPBar(player.hp);
                    }
                    break;
            }
        }

        // ==================== ÇARPIŞMA & HASAR ====================
        function checkCollision(a, b) {
            return a.x < b.x + b.width && a.x + a.width > b.x &&
                   a.y < b.y + b.height && a.y + a.height > b.y;
        }

        function createExplosion(x, y, color, count = 15) {
            for (let i = 0; i < count; i++) {
                particles.push({
                    x, y,
                    vx: (Math.random() - 0.5) * 10,
                    vy: (Math.random() - 0.5) * 10,
                    life: 1,
                    decay: Math.random() * 0.03 + 0.02,
                    size: Math.random() * 5 + 2,
                    color
                });
            }
            shakeScreen = 8;
            playSound('explosion');
        }

        function playerHit(player, damage) {
            if (player.invulnerable > 0) return;
            if (player.powerUps.shield) damage *= 0.5;

            player.hp -= damage;
            playSound('hit');

            if (player.hp <= 0) {
                player.alive = false;
                player.deaths++;
                createExplosion(player.x, player.y, player.color, 20);

                // Respawn
                setTimeout(() => respawnPlayer(player), 3000);
            } else if (!player.isBot) {
                shakeScreen = 5;
                updateHPBar(player.hp);
            }
        }

        function respawnPlayer(player) {
            let x, y, safe;
            do {
                x = Math.random() * (CONFIG.GAME_WIDTH - 100) + 50;
                y = Math.random() * (CONFIG.GAME_HEIGHT - 100) + 50;
                safe = true;
                for (const obs of obstacles) {
                    if (x > obs.x - 30 && x < obs.x + obs.width + 30 &&
                        y > obs.y - 30 && y < obs.y + obs.height + 30) {
                        safe = false; break;
                    }
                }
            } while (!safe);

            player.x = x;
            player.y = y;
            player.hp = 100;
            player.alive = true;
            player.invulnerable = 120;
            player.powerUps = { double: false, speed: false, shield: false };
            player.powerUpTimer = 0;
            player.speedBoost = 0;
            player.shieldTimer = 0;

            if (!player.isBot) {
                updateHPBar(100);
                addChatMessage('Sistem', `${player.name} yeniden doğdu!`, '#00d4ff');
            }
        }

        // ==================== OYUN DÖNGÜSÜ ====================
        function startGame() {
            gameState = 'playing';
            gameTime = 0;
            bullets = [];
            powerUps = [];
            particles = [];
            killFeed = [];

            generateMap();

            // Oyuncuları oluştur
            players.clear();

            // Ben
            const me = createPlayer(myId, myName, false, '#00d4ff');
            players.set(myId, me);

            // Botlar
            for (let i = 0; i < CONFIG.BOT_COUNT; i++) {
                const botId = 'bot_' + i;
                const bot = createPlayer(botId, botNames[i % botNames.length], true, botColors[i % botColors.length]);
                players.set(botId, bot);
            }

            // Skorları sıfırla
            scores = [];
            players.forEach((p, id) => {
                p.score = 0; p.kills = 0; p.deaths = 0;
            });

            // Başlangıç güçlendirmeleri
            spawnPowerUp();
            spawnPowerUp();

            // Timer
            gameTimer = setInterval(() => {
                gameTime += 1000;
                updateTimer();
                if (gameTime >= CONFIG.GAME_DURATION) {
                    endGame();
                }
            }, 1000);

            // Güçlendirme spawn
            powerUpTimer = setInterval(() => {
                if (powerUps.length < 5) spawnPowerUp();
            }, CONFIG.POWERUP_SPAWN_INTERVAL);

            // Bot AI
            botTimer = setInterval(() => {
                players.forEach((p, id) => {
                    if (p.isBot) updateBot(p);
                });
            }, 50);

            showGame();
            addChatMessage('Sistem', '🎮 Oyun başladı! 5 dakika sürecek.', '#ffa502');
            addChatMessage('Sistem', `🤖 ${CONFIG.BOT_COUNT} bot oyuna katıldı!`, '#00d4ff');

            initAudio();
        }

        function endGame() {
            gameState = 'ended';
            clearInterval(gameTimer);
            clearInterval(powerUpTimer);
            clearInterval(botTimer);

            // Kazananı bul
            let winner = null;
            let bestScore = -1;
            players.forEach(p => {
                if (p.score > bestScore) {
                    bestScore = p.score;
                    winner = p;
                }
            });

            showGameOver(winner);
        }

        // ==================== INPUT YÖNETİMİ ====================
        document.addEventListener('keydown', (e) => {
            switch(e.key) {
                case 'w': case 'W': case 'ArrowUp': inputs.up = true; break;
                case 's': case 'S': case 'ArrowDown': inputs.down = true; break;
                case 'a': case 'A': case 'ArrowLeft': inputs.left = true; break;
                case 'd': case 'D': case 'ArrowRight': inputs.right = true; break;
                case ' ': inputs.shoot = true; break;
            }
            if (['w','a','s','d','ArrowUp','ArrowDown','ArrowLeft','ArrowRight',' '].includes(e.key)) {
                e.preventDefault();
            }
        });

        document.addEventListener('keyup', (e) => {
            switch(e.key) {
                case 'w': case 'W': case 'ArrowUp': inputs.up = false; break;
                case 's': case 'S': case 'ArrowDown': inputs.down = false; break;
                case 'a': case 'A': case 'ArrowLeft': inputs.left = false; break;
                case 'd': case 'D': case 'ArrowRight': inputs.right = false; break;
                case ' ': inputs.shoot = false; break;
            }
        });

        canvas.addEventListener('mousemove', (e) => {
            const rect = canvas.getBoundingClientRect();
            const scaleX = CONFIG.GAME_WIDTH / rect.width;
            const scaleY = CONFIG.GAME_HEIGHT / rect.height;
            mousePos.x = (e.clientX - rect.left) * scaleX;
            mousePos.y = (e.clientY - rect.top) * scaleY;

            const me = players.get(myId);
            if (me) {
                const dx = mousePos.x - me.x;
                const dy = mousePos.y - me.y;
                mouseAngle = Math.atan2(dy, dx);
            }
        });

        canvas.addEventListener('mousedown', (e) => {
            if (e.button === 0) { isMouseDown = true; inputs.shoot = true; }
        });

        canvas.addEventListener('mouseup', (e) => {
            if (e.button === 0) { isMouseDown = false; inputs.shoot = false; }
        });

        // Mobil joystick
        let joystickActive = false;
        let joystickCenter = { x: 0, y: 0 };
        const joystick = document.getElementById('mobileJoystick');
        const joystickInner = document.getElementById('joystickInner');

        joystick.addEventListener('touchstart', (e) => {
            e.preventDefault();
            const touch = e.touches[0];
            const rect = joystick.getBoundingClientRect();
            joystickCenter = { x: rect.left + rect.width/2, y: rect.top + rect.height/2 };
            joystickActive = true;
            updateJoystick(touch.clientX, touch.clientY);
        }, { passive: false });

        joystick.addEventListener('touchmove', (e) => {
            e.preventDefault();
            if (joystickActive) updateJoystick(e.touches[0].clientX, e.touches[0].clientY);
        }, { passive: false });

        joystick.addEventListener('touchend', (e) => {
            e.preventDefault();
            joystickActive = false;
            joystickInner.style.transform = 'translate(-50%, -50%)';
            inputs.up = inputs.down = inputs.left = inputs.right = false;
        }, { passive: false });

        function updateJoystick(clientX, clientY) {
            const maxDist = 35;
            let dx = clientX - joystickCenter.x;
            let dy = clientY - joystickCenter.y;
            const dist = Math.sqrt(dx*dx + dy*dy);
            if (dist > maxDist) { dx = dx/dist*maxDist; dy = dy/dist*maxDist; }
            joystickInner.style.transform = `translate(calc(-50% + ${dx}px), calc(-50% + ${dy}px))`;
            inputs.left = dx < -10; inputs.right = dx > 10;
            inputs.up = dy < -10; inputs.down = dy > 10;
        }

        const fireBtn = document.getElementById('mobileFireBtn');
        fireBtn.addEventListener('touchstart', (e) => { e.preventDefault(); inputs.shoot = true; }, { passive: false });
        fireBtn.addEventListener('touchend', (e) => { e.preventDefault(); inputs.shoot = false; }, { passive: false });

        // ==================== OYUNCU HAREKETİ & ATIŞ ====================
        function updatePlayer() {
            const me = players.get(myId);
            if (!me || !me.alive) return;

            // Hareket
            const speed = me.powerUps.speed ? CONFIG.PLAYER_SPEED * 1.5 : CONFIG.PLAYER_SPEED;
            let dx = 0, dy = 0;
            if (inputs.up) dy -= 1;
            if (inputs.down) dy += 1;
            if (inputs.left) dx -= 1;
            if (inputs.right) dx += 1;

            if (dx !== 0 || dy !== 0) {
                const len = Math.sqrt(dx*dx + dy*dy);
                dx /= len; dy /= len;
            }

            let newX = me.x + dx * speed;
            let newY = me.y + dy * speed;

            // Engel kontrolü
            let collides = false;
            for (const obs of obstacles) {
                if (newX + CONFIG.PLAYER_RADIUS > obs.x && newX - CONFIG.PLAYER_RADIUS < obs.x + obs.width &&
                    newY + CONFIG.PLAYER_RADIUS > obs.y && newY - CONFIG.PLAYER_RADIUS < obs.y + obs.height) {
                    collides = true; break;
                }
            }
            if (!collides) {
                me.x = Math.max(CONFIG.PLAYER_RADIUS, Math.min(CONFIG.GAME_WIDTH - CONFIG.PLAYER_RADIUS, newX));
                me.y = Math.max(CONFIG.PLAYER_RADIUS, Math.min(CONFIG.GAME_HEIGHT - CONFIG.PLAYER_RADIUS, newY));
            }

            me.angle = mouseAngle;

            // Ateş
            const now = Date.now();
            if (inputs.shoot && now - me.lastShot > (me.powerUps.double ? 100 : 150)) {
                me.lastShot = now;
                shoot(me);
            }

            // Güçlendirme toplama
            powerUps.forEach(p => {
                const d = Math.sqrt((p.x - me.x)**2 + (p.y - me.y)**2);
                if (d < 30) collectPowerUp(me, p);
            });
        }

        function shoot(player) {
            const angles = player.powerUps.double ? 
                [player.angle - 0.15, player.angle + 0.15] : [player.angle];

            angles.forEach(angle => {
                bullets.push({
                    x: player.x + Math.cos(angle) * 20,
                    y: player.y + Math.sin(angle) * 20,
                    angle,
                    ownerId: player.id,
                    life: 120,
                    damage: player.powerUps.double ? 15 : 25,
                    color: player.color
                });
            });
            playSound('shoot');
        }

        // ==================== OYUN GÜNCELLEME ====================
        function updateGame() {
            if (gameState !== 'playing') return;

            // Oyuncu güncelle
            updatePlayer();

            // Mermiler
            for (let i = bullets.length - 1; i >= 0; i--) {
                const b = bullets[i];
                b.x += Math.cos(b.angle) * CONFIG.BULLET_SPEED;
                b.y += Math.sin(b.angle) * CONFIG.BULLET_SPEED;
                b.life--;

                // Harita dışı
                if (b.x < 0 || b.x > CONFIG.GAME_WIDTH || b.y < 0 || b.y > CONFIG.GAME_HEIGHT || b.life <= 0) {
                    bullets.splice(i, 1); continue;
                }

                // Engel çarpışması
                let hitObstacle = false;
                for (let j = obstacles.length - 1; j >= 0; j--) {
                    const obs = obstacles[j];
                    if (b.x > obs.x && b.x < obs.x + obs.width && b.y > obs.y && b.y < obs.y + obs.height) {
                        if (obs.type === 'breakable') {
                            obs.hp--;
                            if (obs.hp <= 0) {
                                createExplosion(obs.x + obs.width/2, obs.y + obs.height/2, '#ffa502', 10);
                                obstacles.splice(j, 1);
                            }
                        }
                        hitObstacle = true; break;
                    }
                }
                if (hitObstacle) { bullets.splice(i, 1); continue; }

                // Oyuncu çarpışması
                for (const [pid, player] of players) {
                    if (pid === b.ownerId || !player.alive || player.invulnerable > 0) continue;

                    const dx = b.x - player.x;
                    const dy = b.y - player.y;
                    if (Math.sqrt(dx*dx + dy*dy) < CONFIG.PLAYER_RADIUS + 4) {
                        const owner = players.get(b.ownerId);
                        if (owner) {
                            owner.kills++;
                            owner.score += 100;
                        }
                        playerHit(player, b.damage);

                        if (!player.alive && owner) {
                            addKillFeed(owner.name, player.name);
                            if (owner.id === myId) playSound('kill');
                        }

                        bullets.splice(i, 1); break;
                    }
                }
            }

            // Güçlendirme süreleri
            players.forEach(p => {
                if (p.powerUpTimer > 0) {
                    p.powerUpTimer--;
                    if (p.powerUpTimer <= 0) p.powerUps.double = false;
                }
                if (p.speedBoost > 0) {
                    p.speedBoost--;
                    if (p.speedBoost <= 0) p.powerUps.speed = false;
                }
                if (p.shieldTimer > 0) {
                    p.shieldTimer--;
                    if (p.shieldTimer <= 0) p.powerUps.shield = false;
                }
                if (p.invulnerable > 0) p.invulnerable--;
            });

            // Parçacıklar
            particles = particles.filter(p => {
                p.x += p.vx; p.y += p.vy;
                p.life -= p.decay;
                p.vx *= 0.98; p.vy *= 0.98;
                return p.life > 0;
            });

            // Ekran sarsıntısı
            if (shakeScreen > 0) shakeScreen *= 0.9;
            if (shakeScreen < 0.5) shakeScreen = 0;

            // Skor paneli
            updateScorePanel();
        }

        // ==================== ÇİZİM ====================
        let stars = [];
        for (let i = 0; i < 200; i++) {
            stars.push({
                x: Math.random() * CONFIG.GAME_WIDTH,
                y: Math.random() * CONFIG.GAME_HEIGHT,
                size: Math.random() * 2 + 0.5,
                speed: Math.random() * 0.5 + 0.1,
                brightness: Math.random()
            });
        }

        function resizeCanvas() {
            const area = document.getElementById('gameArea');
            const rect = area.getBoundingClientRect();
            canvas.width = rect.width;
            canvas.height = rect.height;
        }

        window.addEventListener('resize', resizeCanvas);

        function render() {
            if (gameState !== 'playing' && gameState !== 'ended') {
                requestAnimationFrame(render);
                return;
            }

            const me = players.get(myId);
            if (!me) {
                requestAnimationFrame(render);
                return;
            }

            // Kamera
            const targetCamX = me.x - canvas.width / 2;
            const targetCamY = me.y - canvas.height / 2;
            camera.x += (targetCamX - camera.x) * 0.1;
            camera.y += (targetCamY - camera.y) * 0.1;
            camera.x = Math.max(0, Math.min(CONFIG.GAME_WIDTH - canvas.width, camera.x));
            camera.y = Math.max(0, Math.min(CONFIG.GAME_HEIGHT - canvas.height, camera.y));

            ctx.save();

            if (shakeScreen > 0) {
                ctx.translate((Math.random()-0.5)*shakeScreen, (Math.random()-0.5)*shakeScreen);
            }
            ctx.translate(-camera.x, -camera.y);

            // Arka plan
            ctx.fillStyle = '#0a0a1a';
            ctx.fillRect(camera.x, camera.y, canvas.width, canvas.height);

            // Izgara
            ctx.strokeStyle = 'rgba(0,212,255,0.03)';
            ctx.lineWidth = 1;
            const gridSize = 50;
            const startX = Math.floor(camera.x / gridSize) * gridSize;
            const startY = Math.floor(camera.y / gridSize) * gridSize;
            for (let x = startX; x < camera.x + canvas.width + gridSize; x += gridSize) {
                ctx.beginPath(); ctx.moveTo(x, camera.y); ctx.lineTo(x, camera.y + canvas.height); ctx.stroke();
            }
            for (let y = startY; y < camera.y + canvas.height + gridSize; y += gridSize) {
                ctx.beginPath(); ctx.moveTo(camera.x, y); ctx.lineTo(camera.x + canvas.width, y); ctx.stroke();
            }

            // Yıldızlar
            stars.forEach(star => {
                ctx.globalAlpha = 0.3 + Math.sin(Date.now() * 0.001 + star.x) * 0.3;
                ctx.fillStyle = '#fff';
                ctx.beginPath();
                ctx.arc(star.x, star.y, star.size, 0, Math.PI * 2);
                ctx.fill();
            });
            ctx.globalAlpha = 1;

            // Engeller
            obstacles.forEach(obs => {
                if (obs.type === 'breakable') {
                    ctx.fillStyle = 'rgba(255,165,2,0.3)';
                    ctx.strokeStyle = '#ffa502';
                } else {
                    ctx.fillStyle = 'rgba(0,212,255,0.1)';
                    ctx.strokeStyle = 'rgba(0,212,255,0.3)';
                }
                ctx.lineWidth = 2;
                ctx.fillRect(obs.x, obs.y, obs.width, obs.height);
                ctx.strokeRect(obs.x, obs.y, obs.width, obs.height);
                ctx.strokeStyle = 'rgba(255,255,255,0.05)';
                ctx.beginPath();
                ctx.moveTo(obs.x, obs.y); ctx.lineTo(obs.x + obs.width, obs.y + obs.height);
                ctx.moveTo(obs.x + obs.width, obs.y); ctx.lineTo(obs.x, obs.y + obs.height);
                ctx.stroke();
            });

            // Güçlendirmeler
            powerUps.forEach(p => {
                p.phase += 0.05;
                const pulse = 1 + Math.sin(p.phase) * 0.2;
                ctx.save();
                ctx.translate(p.x, p.y);
                ctx.scale(pulse, pulse);
                ctx.shadowBlur = 20;
                ctx.shadowColor = p.color;
                ctx.fillStyle = p.color;
                ctx.beginPath();
                ctx.arc(0, 0, 12, 0, Math.PI * 2);
                ctx.fill();
                ctx.shadowBlur = 0;
                ctx.fillStyle = '#fff';
                ctx.font = 'bold 14px Arial';
                ctx.textAlign = 'center';
                ctx.textBaseline = 'middle';
                const icons = { double: '⚡', speed: '🔥', shield: '🛡️', health: '❤️' };
                ctx.fillText(icons[p.type] || '?', 0, 0);
                ctx.restore();
            });

            // Mermiler
            bullets.forEach(b => {
                ctx.save();
                ctx.translate(b.x, b.y);
                ctx.rotate(b.angle);
                ctx.shadowBlur = 10;
                ctx.shadowColor = b.color;
                ctx.fillStyle = b.color;
                ctx.fillRect(-8, -2, 16, 4);
                ctx.shadowBlur = 0;
                ctx.restore();
            });

            // Parçacıklar
            particles.forEach(p => {
                ctx.globalAlpha = p.life;
                ctx.fillStyle = p.color;
                ctx.beginPath();
                ctx.arc(p.x, p.y, p.size * p.life, 0, Math.PI * 2);
                ctx.fill();
            });
            ctx.globalAlpha = 1;

            // Oyuncular
            players.forEach((player, id) => {
                if (!player.alive) return;

                ctx.save();
                ctx.translate(player.x, player.y);

                if (player.invulnerable > 0) {
                    ctx.globalAlpha = 0.5 + Math.sin(Date.now() * 0.01) * 0.3;
                }

                if (player.powerUps.shield) {
                    ctx.strokeStyle = '#00d4ff';
                    ctx.lineWidth = 2;
                    ctx.beginPath();
                    ctx.arc(0, 0, CONFIG.PLAYER_RADIUS + 8, 0, Math.PI * 2);
                    ctx.stroke();
                }

                ctx.fillStyle = player.color;
                ctx.beginPath();
                ctx.arc(0, 0, CONFIG.PLAYER_RADIUS, 0, Math.PI * 2);
                ctx.fill();

                ctx.fillStyle = 'rgba(255,255,255,0.3)';
                ctx.beginPath();
                ctx.arc(-3, -3, CONFIG.PLAYER_RADIUS * 0.4, 0, Math.PI * 2);
                ctx.fill();

                ctx.rotate(player.angle);
                ctx.fillStyle = player.color;
                ctx.fillRect(CONFIG.PLAYER_RADIUS - 2, -3, 14, 6);

                ctx.restore();
                ctx.globalAlpha = 1;

                // İsim
                ctx.fillStyle = id === myId ? '#00d4ff' : '#fff';
                ctx.font = id === myId ? 'bold 12px Arial' : '11px Arial';
                ctx.textAlign = 'center';
                ctx.fillText((id === myId ? '★ ' : '') + player.name, player.x, player.y - CONFIG.PLAYER_RADIUS - 8);

                // HP bar
                if (player.hp < 100) {
                    const bw = 30, bh = 4;
                    const bx = player.x - bw/2, by = player.y + CONFIG.PLAYER_RADIUS + 4;
                    ctx.fillStyle = 'rgba(0,0,0,0.5)';
                    ctx.fillRect(bx, by, bw, bh);
                    ctx.fillStyle = player.hp > 50 ? '#2ed573' : '#ff4757';
                    ctx.fillRect(bx, by, bw * (player.hp/100), bh);
                }
            });

            // Harita sınırları
            ctx.strokeStyle = 'rgba(255,71,87,0.5)';
            ctx.lineWidth = 3;
            ctx.strokeRect(0, 0, CONFIG.GAME_WIDTH, CONFIG.GAME_HEIGHT);

            ctx.restore();

            // Minimap
            renderMinimap();

            requestAnimationFrame(render);
        }

        function renderMinimap() {
            const w = minimapCanvas.width, h = minimapCanvas.height;
            const sx = w / CONFIG.GAME_WIDTH, sy = h / CONFIG.GAME_HEIGHT;

            minimapCtx.fillStyle = 'rgba(0,0,0,0.8)';
            minimapCtx.fillRect(0, 0, w, h);

            minimapCtx.fillStyle = 'rgba(0,212,255,0.3)';
            obstacles.forEach(obs => {
                minimapCtx.fillRect(obs.x * sx, obs.y * sy, obs.width * sx, obs.height * sy);
            });

            players.forEach((p, id) => {
                if (!p.alive) return;
                minimapCtx.fillStyle = id === myId ? '#00d4ff' : p.color;
                minimapCtx.beginPath();
                minimapCtx.arc(p.x * sx, p.y * sy, id === myId ? 4 : 3, 0, Math.PI * 2);
                minimapCtx.fill();
            });

            minimapCtx.strokeStyle = 'rgba(255,255,255,0.3)';
            minimapCtx.lineWidth = 1;
            minimapCtx.strokeRect(camera.x * sx, camera.y * sy, canvas.width * sx, canvas.height * sy);
        }

        // ==================== UI FONKSİYONLARI ====================
        function showGame() {
            document.getElementById('loginScreen').classList.add('hidden');
            document.getElementById('lobbyScreen').classList.add('hidden');
            document.getElementById('gameOverScreen').classList.add('hidden');
            document.getElementById('gameArea').style.display = 'flex';
            document.getElementById('hpBarContainer').style.display = 'block';
            resizeCanvas();
        }

        function showLobby() {
            document.getElementById('loginScreen').classList.add('hidden');
            document.getElementById('lobbyScreen').classList.remove('hidden');
            document.getElementById('gameOverScreen').classList.add('hidden');
            document.getElementById('gameArea').style.display = 'none';
            document.getElementById('hpBarContainer').style.display = 'none';
            updateStatus('connected', '✅ Bağlı');
        }

        function showLogin() {
            document.getElementById('loginScreen').classList.remove('hidden');
            document.getElementById('lobbyScreen').classList.add('hidden');
            document.getElementById('gameOverScreen').classList.add('hidden');
            document.getElementById('gameArea').style.display = 'none';
        }

        function showGameOver(winner) {
            document.getElementById('gameOverScreen').classList.remove('hidden');
            document.getElementById('gameArea').style.display = 'none';

            const isWinner = winner && winner.id === myId;
            document.getElementById('gameOverTitle').textContent = isWinner ? '🏆 ZAFER!' : 'MAÇ BİTTİ';
            document.getElementById('gameOverTitle').style.color = isWinner ? '#2ed573' : '#ff4757';
            document.getElementById('winnerAnnounce').textContent = winner ? `Kazanan: ${winner.name}` : 'Berabere!';

            const scoresDiv = document.getElementById('finalScores');
            scoresDiv.innerHTML = '';

            const sorted = Array.from(players.values()).sort((a, b) => b.score - a.score);
            sorted.forEach((p, i) => {
                const div = document.createElement('div');
                div.className = 'final-score-item' + (p.id === (winner?.id) ? ' winner' : '');
                div.innerHTML = `
                    <span class="final-score-rank">#${i + 1}</span>
                    <span style="flex:1; text-align:left; margin-left:10px;">${p.name}</span>
                    <span style="color:#ffa502; font-weight:700;">${p.score} pts</span>
                    <span style="color:#2ed573; margin-left:10px;">${p.kills}K</span>
                    <span style="color:#ff4757; margin-left:5px;">${p.deaths}D</span>
                `;
                scoresDiv.appendChild(div);
            });
        }

        function updateScorePanel() {
            const list = document.getElementById('scoreList');
            list.innerHTML = '';
            const sorted = Array.from(players.values()).sort((a, b) => b.score - a.score);
            sorted.forEach(p => {
                const div = document.createElement('div');
                div.className = 'score-item';
                div.innerHTML = `
                    <div class="score-name">
                        <span class="score-color" style="background:${p.color}"></span>
                        <span>${p.alive ? '' : '💀'} ${p.name}</span>
                    </div>
                    <span class="score-value">${p.score}</span>
                `;
                list.appendChild(div);
            });
        }

        function updateHPBar(hp) {
            const fill = document.getElementById('hpBarFill');
            const text = document.getElementById('hpText');
            const pct = Math.max(0, hp);
            fill.style.width = pct + '%';
            text.textContent = Math.ceil(pct) + ' / 100 HP';
        }

        function updateTimer() {
            const timer = document.getElementById('gameTimer');
            const remaining = Math.max(0, CONFIG.GAME_DURATION - gameTime);
            const mins = Math.floor(remaining / 60000);
            const secs = Math.floor((remaining % 60000) / 1000);
            timer.textContent = `${String(mins).padStart(2,'0')}:${String(secs).padStart(2,'0')}`;
            timer.classList.toggle('warning', remaining < 60000);
        }

        function addKillFeed(killer, victim) {
            const feed = document.getElementById('killFeed');
            const div = document.createElement('div');
            div.className = 'kill-msg';
            div.innerHTML = `<span style="color:#ffa502">${killer}</span> → <span style="color:#ff4757">${victim}</span>`;
            feed.appendChild(div);
            setTimeout(() => div.remove(), 5000);
            if (feed.children.length > 5) feed.removeChild(feed.firstChild);
        }

        function addChatMessage(name, message, color) {
            const chat = document.getElementById('chatMessages');
            const div = document.createElement('div');
            div.className = 'chat-msg';
            div.innerHTML = `<span class="chat-name" style="color:${color}">${name}:</span> ${escapeHtml(message)}`;
            chat.appendChild(div);
            chat.scrollTop = chat.scrollHeight;
        }

        function showPowerUpIndicator(type) {
            const ind = document.getElementById('powerupIndicator');
            const texts = { double: '⚡ ÇİFT ATIŞ!', speed: '🔥 HIZLI HAREKET!', shield: '🛡️ KALKAN!', health: '❤️ CAN YENİLENDİ!' };
            const colors = { double: '#00ff88', speed: '#ffa502', shield: '#00d4ff', health: '#ff4757' };
            ind.textContent = texts[type] || 'GÜÇLENDİRME!';
            ind.style.borderColor = colors[type] || '#fff';
            ind.style.color = colors[type] || '#fff';
            ind.classList.add('active');
            setTimeout(() => ind.classList.remove('active'), 2000);
        }

        function updateStatus(type, text) {
            const status = document.getElementById('connStatus');
            status.textContent = text;
            status.className = 'connection-status status-' + type;
            status.style.opacity = '1';
        }

        function escapeHtml(text) {
            const div = document.createElement('div');
            div.textContent = text;
            return div.innerHTML;
        }

        // ==================== BUTON OLAYLARI ====================
        document.getElementById('playBtn').addEventListener('click', () => {
            myName = document.getElementById('playerName').value.trim() || 'Oyuncu';
            updateStatus('connecting', '🔄 Arena'ya bağlanılıyor...');
            setTimeout(() => {
                updateStatus('connected', '✅ Arena 1'e bağlandı');
                startGame();
            }, 1500);
        });

        document.getElementById('createRoomBtn').addEventListener('click', showLobby);

        document.getElementById('createRoomSubmitBtn').addEventListener('click', createRoom);

        function createRoom() {
            myName = document.getElementById('playerName').value.trim() || 'Oyuncu';
            const roomName = document.getElementById('newRoomName').value.trim() || 'Yeni Oda';
            updateStatus('connecting', `🔄 ${roomName} oluşturuluyor...`);
            setTimeout(() => {
                updateStatus('connected', `✅ ${roomName} oluşturuldu`);
                addChatMessage('Sistem', `🏠 ${roomName} odası oluşturuldu!`, '#00d4ff');
                startGame();
            }, 1000);
        }

        function joinRoom(roomId) {
            myName = document.getElementById('playerName').value.trim() || 'Oyuncu';
            updateStatus('connecting', `🔄 ${roomId} odasına bağlanılıyor...`);
            setTimeout(() => {
                updateStatus('connected', `✅ ${roomId} odasına bağlandı`);
                addChatMessage('Sistem', `🚪 ${roomId} odasına katıldınız!`, '#00d4ff');
                startGame();
            }, 1200);
        }

        function playAgain() {
            document.getElementById('gameOverScreen').classList.add('hidden');
            startGame();
        }

        function sendChat() {
            const input = document.getElementById('chatInput');
            const msg = input.value.trim();
            if (msg) {
                addChatMessage(myName, msg, '#fff');
                // Bot yanıtı
                setTimeout(() => {
                    const botResponses = ['GG!', 'İyi oyun!', 'Neredesin?', 'Gel buraya!', 'Vuramadım 😅'];
                    const randomBot = Array.from(players.values()).filter(p => p.isBot)[0];
                    if (randomBot && Math.random() < 0.3) {
                        addChatMessage(randomBot.name, botResponses[Math.floor(Math.random() * botResponses.length)], randomBot.color);
                    }
                }, 1000 + Math.random() * 2000);
                input.value = '';
            }
        }

        document.getElementById('chatInput').addEventListener('keypress', (e) => {
            if (e.key === 'Enter') sendChat();
        });

        // ==================== ANA DÖNGÜ ====================
        function gameLoop() {
            updateGame();
            requestAnimationFrame(gameLoop);
        }

        // Başlat
        render();
        gameLoop();

        // Simüle bağlantı durumu
        setTimeout(() => {
            updateStatus('connected', '✅ Sunucuya bağlandı');
            setTimeout(() => document.getElementById('connStatus').style.opacity = '0', 2000);
        }, 1000);

        console.log('Neon Arena yüklendi!');
        console.log('Reklam alanları: #adTop, #adLeft, #adRight, #adMobile');
    </script>
</body>
</html>
