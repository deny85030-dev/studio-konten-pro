<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>🎬 Studio Konten Pro</title>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body {
            font-family: 'Segoe UI', system-ui, sans-serif;
            background: #0a0e1a;
            color: #e8edf5;
            min-height: 100vh;
        }

        /* ===== SIDEBAR ===== */
        .sidebar {
            position: fixed;
            left: 0;
            top: 0;
            width: 240px;
            height: 100vh;
            background: #0d1423;
            border-right: 1px solid #1a2a4a;
            padding: 20px 16px;
            display: flex;
            flex-direction: column;
            z-index: 100;
            overflow-y: auto;
        }
        .sidebar .logo {
            display: flex;
            align-items: center;
            gap: 12px;
            padding-bottom: 16px;
            border-bottom: 1px solid #1a2a4a;
            margin-bottom: 16px;
        }
        .sidebar .logo .icon {
            width: 40px;
            height: 40px;
            background: linear-gradient(135deg, #6c5ce7, #a855f7);
            border-radius: 12px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 20px;
            flex-shrink: 0;
        }
        .sidebar .logo h1 { font-size: 16px; font-weight: 700; }
        .sidebar .logo small { display: block; font-size: 9px; color: #6c5ce7; font-weight: 600; letter-spacing: 1px; text-transform: uppercase; }

        .sidebar nav {
            display: flex;
            flex-direction: column;
            gap: 4px;
            flex: 1;
        }
        .sidebar nav button {
            background: transparent;
            border: none;
            color: #667799;
            padding: 10px 14px;
            border-radius: 10px;
            text-align: left;
            font-size: 13px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.2s;
            display: flex;
            align-items: center;
            gap: 10px;
        }
        .sidebar nav button:hover { background: #1a2a4a; color: #c8d0e0; }
        .sidebar nav button.active {
            background: rgba(108, 92, 231, 0.15);
            color: #a855f7;
            border: 1px solid rgba(108, 92, 231, 0.2);
        }
        .sidebar nav button i { width: 18px; }

        /* ===== API SECTION ===== */
        .api-section {
            border-top: 1px solid #1a2a4a;
            padding-top: 14px;
            margin-top: 14px;
        }
        .api-section .label {
            font-size: 10px;
            font-weight: 700;
            color: #667799;
            text-transform: uppercase;
            letter-spacing: 0.5px;
            display: flex;
            align-items: center;
            gap: 8px;
            cursor: pointer;
            margin-bottom: 8px;
        }
        .api-section .label .dot {
            width: 8px;
            height: 8px;
            border-radius: 50%;
            display: inline-block;
            flex-shrink: 0;
        }
        .api-section .label .dot.on { background: #10b981; }
        .api-section .label .dot.off { background: #ef4444; }
        
        .api-section .settings {
            display: flex;
            flex-direction: column;
            gap: 8px;
        }
        .api-section .settings input {
            width: 100%;
            padding: 8px 12px;
            background: #0d1423;
            border: 1px solid #1a2a4a;
            border-radius: 8px;
            color: #e8edf5;
            font-size: 12px;
            outline: none;
            font-family: monospace;
        }
        .api-section .settings input:focus { border-color: #6c5ce7; }
        .api-section .settings input::placeholder { color: #445566; }
        
        .api-section .settings .btn-row {
            display: flex;
            gap: 6px;
        }
        .api-section .settings .btn-row button {
            flex: 1;
            padding: 6px 10px;
            border: none;
            border-radius: 8px;
            font-size: 11px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.2s;
        }
        .api-section .settings .btn-row .test-btn {
            background: #1a2a4a;
            color: #667799;
        }
        .api-section .settings .btn-row .test-btn:hover { background: #2a3a5a; color: #c8d0e0; }
        .api-section .settings .btn-row .get-btn {
            background: rgba(108, 92, 231, 0.15);
            color: #a855f7;
            border: 1px solid rgba(108, 92, 231, 0.2);
        }
        .api-section .settings .btn-row .get-btn:hover { background: rgba(108, 92, 231, 0.25); }
        
        .api-status-text {
            font-size: 10px;
            color: #667799;
            text-align: center;
            padding: 4px 0;
        }
        .api-status-text .valid { color: #10b981; }
        .api-status-text .invalid { color: #ef4444; }

        /* ===== MAIN ===== */
        .main {
            margin-left: 240px;
            padding: 24px 32px 40px;
            min-height: 100vh;
        }

        /* ===== HEADER ===== */
        .header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding-bottom: 16px;
            border-bottom: 1px solid #1a2a4a;
            margin-bottom: 24px;
            flex-wrap: wrap;
            gap: 10px;
        }
        .header h2 { font-size: 20px; }
        .header p { font-size: 13px; color: #667799; }
        .header .badge {
            background: rgba(108, 92, 231, 0.15);
            border: 1px solid rgba(108, 92, 231, 0.2);
            color: #a855f7;
            padding: 6px 16px;
            border-radius: 20px;
            font-size: 11px;
            font-weight: 600;
            white-space: nowrap;
        }

        /* ===== TABS ===== */
        .tab { display: none; animation: fadeIn 0.3s ease; }
        .tab.active { display: block; }
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(8px); }
            to { opacity: 1; transform: translateY(0); }
        }

        /* ===== CARDS ===== */
        .card {
            background: #141b2d;
            border: 1px solid #1a2a4a;
            border-radius: 16px;
            padding: 20px 24px;
            margin-bottom: 20px;
        }
        .card .card-title {
            font-size: 12px;
            font-weight: 700;
            color: #667799;
            text-transform: uppercase;
            letter-spacing: 0.5px;
            margin-bottom: 12px;
            display: flex;
            align-items: center;
            gap: 8px;
        }
        .card .card-title i { color: #6c5ce7; }

        /* ===== INPUTS ===== */
        input, select, textarea {
            background: #0d1423;
            border: 1px solid #1a2a4a;
            border-radius: 10px;
            padding: 10px 14px;
            color: #e8edf5;
            font-size: 14px;
            outline: none;
            transition: border-color 0.3s;
            width: 100%;
        }
        input:focus, select:focus, textarea:focus { border-color: #6c5ce7; }
        textarea {
            resize: vertical;
            font-family: 'Courier New', monospace;
            font-size: 13px;
            min-height: 50px;
        }
        .input-group {
            display: flex;
            gap: 10px;
        }
        .input-group > * { flex: 1; }

        /* ===== BUTTONS ===== */
        .btn {
            padding: 10px 20px;
            border: none;
            border-radius: 10px;
            font-size: 13px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s;
            display: inline-flex;
            align-items: center;
            justify-content: center;
            gap: 8px;
            white-space: nowrap;
        }
        .btn-primary {
            background: linear-gradient(135deg, #6c5ce7, #a855f7);
            color: white;
        }
        .btn-primary:hover { transform: translateY(-2px); box-shadow: 0 8px 30px rgba(108, 92, 231, 0.3); }
        .btn-primary:disabled { opacity: 0.5; cursor: not-allowed; transform: none; }
        .btn-success {
            background: linear-gradient(135deg, #10b981, #059669);
            color: white;
        }
        .btn-success:hover { transform: translateY(-2px); box-shadow: 0 8px 30px rgba(16, 185, 129, 0.3); }
        .btn-secondary {
            background: #1a2a4a;
            color: #667799;
        }
        .btn-secondary:hover { background: #2a3a5a; color: #c8d0e0; }
        .btn-sm { padding: 6px 14px; font-size: 11px; }
        .btn-block { width: 100%; }

        /* ===== ROW ===== */
        .row { display: flex; gap: 16px; flex-wrap: wrap; }
        .row > * { flex: 1; min-width: 180px; }

        /* ===== TITLES ===== */
        .titles-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 8px;
            margin-top: 10px;
        }
        .title-item {
            background: #0d1423;
            border: 1px solid #1a2a4a;
            border-radius: 8px;
            padding: 8px 12px;
            font-size: 13px;
            cursor: pointer;
            transition: all 0.2s;
            display: flex;
            align-items: center;
            gap: 8px;
            color: #c8d0e0;
        }
        .title-item:hover { border-color: #6c5ce7; background: #1a1a3a; }
        .title-item .num {
            background: rgba(108, 92, 231, 0.2);
            color: #a855f7;
            font-weight: 700;
            font-size: 10px;
            padding: 2px 8px;
            border-radius: 12px;
            flex-shrink: 0;
        }

        /* ===== SCENE ===== */
        .scene-editor {
            display: flex;
            flex-direction: column;
            gap: 12px;
            max-height: 400px;
            overflow-y: auto;
            padding-right: 4px;
        }
        .scene-item {
            background: #0d1423;
            border: 1px solid #1a2a4a;
            border-radius: 12px;
            padding: 14px 16px;
        }
        .scene-item .scene-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 8px;
            padding-bottom: 6px;
            border-bottom: 1px solid #1a2a4a;
        }
        .scene-item .scene-header .num { font-size: 12px; font-weight: 700; color: #a855f7; }
        .scene-item label {
            font-size: 9px;
            font-weight: 700;
            color: #667799;
            text-transform: uppercase;
            letter-spacing: 0.5px;
            display: block;
            margin-bottom: 3px;
        }
        .scene-item textarea { min-height: 40px; font-size: 13px; padding: 8px 12px; border-radius: 8px; }

        /* ===== AUDIO ===== */
        .audio-list {
            display: flex;
            flex-direction: column;
            gap: 8px;
            max-height: 350px;
            overflow-y: auto;
        }
        .audio-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            background: #0d1423;
            border: 1px solid #1a2a4a;
            border-radius: 10px;
            padding: 10px 14px;
        }
        .audio-item .info { flex: 1; min-width: 0; }
        .audio-item .info .num { font-size: 10px; font-weight: 700; color: #a855f7; }
        .audio-item .info .text { font-size: 13px; color: #c8d0e0; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
        .audio-item .play-btn {
            width: 32px;
            height: 32px;
            border-radius: 50%;
            border: none;
            background: rgba(108, 92, 231, 0.2);
            color: #a855f7;
            cursor: pointer;
            transition: all 0.2s;
            flex-shrink: 0;
        }
        .audio-item .play-btn:hover { background: rgba(108, 92, 231, 0.4); }

        /* ===== VISUAL ===== */
        .visual-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 16px;
        }
        .visual-item {
            background: #0d1423;
            border: 1px solid #1a2a4a;
            border-radius: 12px;
            overflow: hidden;
        }
        .visual-item .img-wrap {
            position: relative;
            aspect-ratio: 16/9;
            background: #080c18;
            overflow: hidden;
        }
        .visual-item .img-wrap img {
            width: 100%;
            height: 100%;
            object-fit: cover;
        }
        .visual-item .img-wrap .spinner-overlay {
            position: absolute;
            inset: 0;
            background: rgba(8, 12, 24, 0.85);
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            transition: opacity 0.4s;
        }
        .visual-item .img-wrap .spinner-overlay.hidden {
            opacity: 0;
            pointer-events: none;
        }
        .visual-item .img-wrap .spinner-overlay .loader {
            width: 24px;
            height: 24px;
            border: 3px solid rgba(255,255,255,0.1);
            border-top: 3px solid #6c5ce7;
            border-radius: 50%;
            animation: spin 0.7s linear infinite;
        }
        @keyframes spin { 100% { transform: rotate(360deg); } }
        .visual-item .img-wrap .spinner-overlay span {
            font-size: 10px;
            color: #a855f7;
            margin-top: 8px;
        }
        .visual-item .visual-footer {
            padding: 10px 14px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        .visual-item .visual-footer .num { font-size: 11px; font-weight: 700; color: #a855f7; }
        .visual-item .visual-footer .actions { display: flex; gap: 6px; }
        .visual-item .visual-footer .actions button {
            padding: 4px 10px;
            border: none;
            border-radius: 6px;
            font-size: 10px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.2s;
            background: #1a2a4a;
            color: #667799;
        }
        .visual-item .visual-footer .actions button:hover { background: #2a3a5a; color: #c8d0e0; }

        /* ===== PLAYER ===== */
        .player-wrap {
            background: #080c18;
            border-radius: 16px;
            border: 2px solid #1a2a4a;
            overflow: hidden;
            position: relative;
            aspect-ratio: 16/9;
        }
        .player-wrap img {
            width: 100%;
            height: 100%;
            object-fit: cover;
        }
        .player-wrap .overlay {
            position: absolute;
            bottom: 0;
            left: 0;
            right: 0;
            padding: 16px 24px 24px;
            background: linear-gradient(transparent, rgba(0,0,0,0.9));
        }
        .player-wrap .overlay p {
            color: white;
            font-size: 18px;
            font-weight: 700;
            text-align: center;
            text-shadow: 0 2px 20px rgba(0,0,0,0.9);
        }

        .player-controls {
            display: flex;
            align-items: center;
            gap: 16px;
            margin-top: 12px;
        }
        .player-controls .ctrl-btn {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            border: none;
            background: #6c5ce7;
            color: white;
            cursor: pointer;
            transition: all 0.2s;
            flex-shrink: 0;
            display: flex;
            align-items: center;
            justify-content: center;
        }
        .player-controls .ctrl-btn:hover { transform: scale(1.05); }
        .player-controls .progress-wrap {
            flex: 1;
            display: flex;
            flex-direction: column;
            gap: 2px;
        }
        .player-controls .progress-wrap .info {
            display: flex;
            justify-content: space-between;
            font-size: 10px;
            color: #667799;
        }
        .player-controls .progress-wrap .bar {
            height: 4px;
            background: #1a2a4a;
            border-radius: 4px;
            overflow: hidden;
        }
        .player-controls .progress-wrap .bar .fill {
            height: 100%;
            background: linear-gradient(90deg, #6c5ce7, #a855f7);
            border-radius: 4px;
            width: 0%;
            transition: width 0.3s;
        }

        /* ===== PLAYLIST ===== */
        .playlist {
            display: flex;
            gap: 10px;
            overflow-x: auto;
            padding: 4px 0 8px;
        }
        .playlist-item {
            flex-shrink: 0;
            width: 120px;
            background: #0d1423;
            border: 1px solid #1a2a4a;
            border-radius: 10px;
            padding: 8px;
            cursor: pointer;
            transition: all 0.2s;
        }
        .playlist-item:hover { border-color: #6c5ce7; }
        .playlist-item .thumb {
            aspect-ratio: 16/9;
            background: #080c18;
            border-radius: 6px;
            overflow: hidden;
        }
        .playlist-item .thumb img {
            width: 100%;
            height: 100%;
            object-fit: cover;
        }
        .playlist-item .name {
            font-size: 10px;
            color: #667799;
            margin-top: 4px;
            text-align: center;
        }

        /* ===== TOAST ===== */
        .toast-container {
            position: fixed;
            bottom: 24px;
            right: 24px;
            z-index: 200;
            display: flex;
            flex-direction: column;
            gap: 8px;
            max-width: 380px;
            width: 100%;
            pointer-events: none;
        }
        .toast {
            background: #141b2d;
            border: 1px solid #1a2a4a;
            border-radius: 12px;
            padding: 12px 16px;
            font-size: 13px;
            font-weight: 500;
            display: flex;
            align-items: center;
            gap: 10px;
            pointer-events: auto;
            animation: slideUp 0.4s ease;
            box-shadow: 0 10px 40px rgba(0,0,0,0.6);
        }
        .toast.success { border-color: rgba(16, 185, 129, 0.3); color: #10b981; }
        .toast.error { border-color: rgba(239, 68, 68, 0.3); color: #ef4444; }
        .toast.info { border-color: rgba(59, 130, 246, 0.3); color: #3b82f6; }
        .toast.warning { border-color: rgba(251, 191, 36, 0.3); color: #fbbf24; }
        @keyframes slideUp {
            from { opacity: 0; transform: translateY(20px) scale(0.95); }
            to { opacity: 1; transform: translateY(0) scale(1); }
        }

        /* ===== MODAL ===== */
        .modal-overlay {
            position: fixed;
            inset: 0;
            background: rgba(0,0,0,0.85);
            backdrop-filter: blur(8px);
            z-index: 150;
            display: none;
            align-items: center;
            justify-content: center;
            padding: 20px;
        }
        .modal-overlay.open { display: flex; }
        .modal {
            background: #141b2d;
            border: 1px solid #1a2a4a;
            border-radius: 24px;
            padding: 32px 40px;
            max-width: 420px;
            width: 100%;
            text-align: center;
        }
        .modal .icon {
            width: 56px;
            height: 56px;
            border-radius: 16px;
            background: rgba(108, 92, 231, 0.15);
            color: #a855f7;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 24px;
            margin: 0 auto 12px;
        }
        .modal h3 { font-size: 18px; margin-bottom: 4px; }
        .modal p { font-size: 13px; color: #667799; margin-bottom: 16px; }
        .modal .ring-wrap {
            position: relative;
            width: 80px;
            height: 80px;
            margin: 0 auto 16px;
        }
        .modal .ring-wrap svg {
            width: 100%;
            height: 100%;
            transform: rotate(-90deg);
        }
        .modal .ring-wrap svg circle {
            fill: none;
            stroke-width: 6;
        }
        .modal .ring-wrap svg .bg { stroke: #1a2a4a; }
        .modal .ring-wrap svg .fill { stroke: #6c5ce7; stroke-dasharray: 251; stroke-dashoffset: 251; transition: stroke-dashoffset 0.5s; stroke-linecap: round; }
        .modal .ring-wrap .pct {
            position: absolute;
            inset: 0;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 20px;
            font-weight: 700;
        }
        .modal .btn-close {
            background: #1a2a4a;
            color: #667799;
            border: none;
            padding: 10px;
            border-radius: 10px;
            font-size: 13px;
            font-weight: 600;
            cursor: pointer;
            width: 100%;
            transition: all 0.2s;
        }
        .modal .btn-close:hover { background: #2a3a5a; color: #c8d0e0; }

        /* ===== RESPONSIVE ===== */
        @media (max-width: 768px) {
            .sidebar {
                width: 100%;
                height: auto;
                position: relative;
                padding: 10px 16px;
                flex-direction: row;
                flex-wrap: wrap;
                border-bottom: 1px solid #1a2a4a;
                border-right: none;
                gap: 6px;
            }
            .sidebar .logo { padding: 0; border: none; margin: 0; }
            .sidebar .logo h1 { font-size: 14px; }
            .sidebar nav { flex-direction: row; flex: none; gap: 2px; }
            .sidebar nav button { padding: 6px 10px; font-size: 11px; }
            .sidebar nav button span { display: none; }
            .api-section { flex: 1; min-width: 150px; border-top: none; padding-top: 0; margin-top: 0; }
            .api-section .label { margin-bottom: 4px; font-size: 9px; }
            .api-section .settings input { font-size: 10px; padding: 4px 8px; }
            .main { margin-left: 0; padding: 16px; }
            .header { flex-direction: column; align-items: flex-start; gap: 6px; }
            .header h2 { font-size: 17px; }
            .row { flex-direction: column; }
            .titles-grid { grid-template-columns: 1fr; }
            .visual-grid { grid-template-columns: 1fr; }
            .input-group { flex-direction: column; }
            .player-wrap .overlay p { font-size: 14px; }
            .playlist-item { width: 80px; }
            .modal { padding: 24px; }
            .toast-container { max-width: 90%; bottom: 16px; right: 16px; }
        }
    </style>
</head>
<body>

    <!-- ========================================================== -->
    <!-- SIDEBAR -->
    <!-- ========================================================== -->
    <aside class="sidebar">
        <div class="logo">
            <div class="icon">🎬</div>
            <div>
                <h1>Studio Konten</h1>
                <small>Pro Version</small>
            </div>
        </div>

        <nav>
            <button class="active" onclick="switchTab('tab-ide')" id="nav-ide">
                <i class="fas fa-lightbulb"></i> <span>1. Ide</span>
            </button>
            <button onclick="switchTab('tab-suara')" id="nav-suara">
                <i class="fas fa-microphone"></i> <span>2. Suara</span>
            </button>
            <button onclick="switchTab('tab-visual')" id="nav-visual">
                <i class="fas fa-image"></i> <span>3. Visual</span>
            </button>
            <button onclick="switchTab('tab-render')" id="nav-render">
                <i class="fas fa-film"></i> <span>4. Render</span>
            </button>
        </nav>

        <!-- API SECTION - JELAS TERLIHAT -->
        <div class="api-section">
            <div class="label">
                <span class="dot off" id="apiDot"></span>
                🔑 API Key (WAJIB)
            </div>
            <div class="settings">
                <input type="password" id="apiKey" placeholder="sk-or-v1-...">
                <div class="btn-row">
                    <button class="test-btn" onclick="testAPI()">🔑 Test</button>
                    <button class="get-btn" onclick="openKeyPage()">📋 Ambil Key</button>
                </div>
                <div class="api-status-text" id="apiStatus">⏳ Masukkan API Key</div>
            </div>
        </div>
    </aside>

    <!-- ========================================================== -->
    <!-- MAIN -->
    <!-- ========================================================== -->
    <main class="main">

        <!-- HEADER -->
        <div class="header">
            <div>
                <h2 id="pageTitle">🎯 Ideasi & Naskah</h2>
                <p id="pageSub">OpenRouter AI + Pollinations Image + Web Speech</p>
            </div>
            <span class="badge"><i class="fas fa-circle-nodes"></i> OpenRouter AI</span>
        </div>

        <!-- ========================================================== -->
        <!-- TAB 1: IDE -->
        <!-- ========================================================== -->
        <div class="tab active" id="tab-ide">

            <!-- Research -->
            <div class="card">
                <div class="card-title"><i class="fas fa-search"></i> Riset Judul (AI)</div>
                <div class="input-group">
                    <input type="text" id="inputResearch" placeholder="Contoh: rahasia gunung padang..." style="flex:1;">
                    <button class="btn btn-primary" onclick="researchTitles()" id="btnResearch">
                        <i class="fas fa-chart-line"></i> Riset 10 Judul
                    </button>
                </div>
                <div id="titlesContainer" style="display:none; margin-top:12px;">
                    <div class="titles-grid" id="titlesGrid"></div>
                </div>
            </div>

            <!-- Generate -->
            <div class="card">
                <div class="card-title"><i class="fas fa-wand-magic-sparkles"></i> Buat Naskah</div>
                <div class="row">
                    <div>
                        <label style="font-size:11px;color:#667799;font-weight:600;">Format Video</label>
                        <div style="display:flex;gap:10px;margin-top:4px;">
                            <label style="display:flex;align-items:center;gap:6px;font-size:13px;cursor:pointer;">
                                <input type="radio" name="format" value="short" checked> Shorts
                            </label>
                            <label style="display:flex;align-items:center;gap:6px;font-size:13px;cursor:pointer;">
                                <input type="radio" name="format" value="long"> YouTube
                            </label>
                        </div>
                    </div>
                    <div>
                        <label style="font-size:11px;color:#667799;font-weight:600;">Jumlah Adegan</label>
                        <select id="sceneCount" style="margin-top:4px;">
                            <option value="3">3 Adegan</option>
                            <option value="5">5 Adegan</option>
                            <option value="8">8 Adegan</option>
                            <option value="12" selected>12 Adegan</option>
                        </select>
                    </div>
                </div>
                <div style="margin-top:12px;">
                    <div class="input-group">
                        <input type="text" id="inputTopic" placeholder="Topik cerita...">
                        <button class="btn btn-success" onclick="generateNaskah()" id="btnGenerate">
                            <i class="fas fa-wand-magic-sparkles"></i> Buat Naskah
                        </button>
                    </div>
                </div>
            </div>

            <!-- Output -->
            <div id="outputContainer" style="display:none;">

                <!-- Metadata -->
                <div class="card">
                    <div class="card-title"><i class="fab fa-youtube" style="color:#ef4444;"></i> Metadata</div>
                    <div style="display:flex;gap:8px;margin-bottom:10px;flex-wrap:wrap;">
                        <button class="btn btn-secondary btn-sm" onclick="copyMetadata()"><i class="fas fa-copy"></i> Salin</button>
                    </div>
                    <div class="row">
                        <div><label style="font-size:10px;color:#667799;font-weight:700;text-transform:uppercase;">Judul</label><textarea id="outTitle" rows="2" style="margin-top:4px;"></textarea></div>
                        <div><label style="font-size:10px;color:#667799;font-weight:700;text-transform:uppercase;">Hashtag</label><textarea id="outTags" rows="2" style="margin-top:4px;color:#a855f7;"></textarea></div>
                    </div>
                    <div style="margin-top:8px;">
                        <label style="font-size:10px;color:#667799;font-weight:700;text-transform:uppercase;">Deskripsi</label>
                        <textarea id="outDesc" rows="3" style="margin-top:4px;"></textarea>
                    </div>
                </div>

                <!-- Script -->
                <div class="card">
                    <div class="card-title"><i class="fas fa-file-lines"></i> Skrip Narasi <span id="sceneBadge" style="margin-left:auto;font-size:10px;background:#1a2a4a;padding:2px 12px;border-radius:12px;">0 Adegan</span></div>
                    <div class="scene-editor" id="scriptEditor"></div>
                    <div style="margin-top:12px;padding-top:12px;border-top:1px solid #1a2a4a;display:flex;justify-content:flex-end;">
                        <button class="btn btn-primary" onclick="switchTab('tab-suara')">Lanjut ke Suara <i class="fas fa-arrow-right"></i></button>
                    </div>
                </div>
            </div>
        </div>

        <!-- ========================================================== -->
        <!-- TAB 2: SUARA -->
        <!-- ========================================================== -->
        <div class="tab" id="tab-suara">
            <div class="card">
                <div class="card-title"><i class="fas fa-microphone"></i> Text-to-Speech</div>
                <div style="display:flex;gap:10px;margin-bottom:16px;flex-wrap:wrap;">
                    <button class="btn btn-success btn-sm" onclick="playAllAudio()"><i class="fas fa-play"></i> Putar Semua</button>
                    <button class="btn btn-secondary btn-sm" onclick="stopAllAudio()"><i class="fas fa-stop"></i> Stop</button>
                </div>
                <div class="audio-list" id="audioList">
                    <div style="text-align:center;padding:30px 0;color:#667799;font-size:13px;">Buat naskah dulu di Langkah 1</div>
                </div>
                <div style="margin-top:12px;padding-top:12px;border-top:1px solid #1a2a4a;display:flex;justify-content:space-between;">
                    <button class="btn btn-secondary btn-sm" onclick="switchTab('tab-ide')"><i class="fas fa-arrow-left"></i> Kembali</button>
                    <button class="btn btn-primary" onclick="switchTab('tab-visual')">Lanjut ke Visual <i class="fas fa-arrow-right"></i></button>
                </div>
            </div>
        </div>

        <!-- ========================================================== -->
        <!-- TAB 3: VISUAL -->
        <!-- ========================================================== -->
        <div class="tab" id="tab-visual">
            <div class="card">
                <div class="card-title"><i class="fas fa-image"></i> Storyboard Visual</div>
                <div style="display:flex;gap:10px;margin-bottom:16px;flex-wrap:wrap;">
                    <button class="btn btn-success btn-sm" onclick="generateAllImages()"><i class="fas fa-wand-magic-sparkles"></i> Render Semua</button>
                </div>
                <div class="visual-grid" id="visualGrid">
                    <div style="grid-column:span 2;text-align:center;padding:30px 0;color:#667799;font-size:13px;">Buat naskah dulu di Langkah 1</div>
                </div>
                <div style="margin-top:12px;padding-top:12px;border-top:1px solid #1a2a4a;display:flex;justify-content:space-between;">
                    <button class="btn btn-secondary btn-sm" onclick="switchTab('tab-suara')"><i class="fas fa-arrow-left"></i> Kembali</button>
                    <button class="btn btn-primary" onclick="switchTab('tab-render')">Render Video <i class="fas fa-arrow-right"></i></button>
                </div>
            </div>
        </div>

        <!-- ========================================================== -->
        <!-- TAB 4: RENDER -->
        <!-- ========================================================== -->
        <div class="tab" id="tab-render">
            <div class="card">
                <div class="card-title"><i class="fas fa-film"></i> Render Video</div>
                <button class="btn btn-primary btn-block" onclick="startRender()" id="btnRender" style="margin-bottom:16px;">
                    <i class="fas fa-play"></i> Mulai Render
                </button>

                <!-- Player -->
                <div class="player-wrap">
                    <img id="playerImg" src="https://placehold.co/1280x720/0d1423/334466?text=Studio+Konten+Pro">
                    <div class="overlay">
                        <p id="playerText">Siapkan naskah & gambar dulu</p>
                    </div>
                </div>

                <!-- Controls -->
                <div class="player-controls">
                    <button class="ctrl-btn" onclick="togglePlayer()" id="playerBtn"><i class="fas fa-play"></i></button>
                    <div class="progress-wrap">
                        <div class="info">
                            <span id="playerStatus">Berhenti</span>
                            <span id="playerTime">0 / 0</span>
                        </div>
                        <div class="bar"><div class="fill" id="playerProgress"></div></div>
                    </div>
                </div>

                <!-- Playlist -->
                <div style="margin-top:16px;">
                    <div style="font-size:10px;color:#667799;font-weight:700;text-transform:uppercase;margin-bottom:8px;">Daftar Adegan</div>
                    <div class="playlist" id="playlistContainer">
                        <div style="width:100%;text-align:center;padding:16px 0;color:#667799;font-size:13px;">Belum ada adegan</div>
                    </div>
                </div>

                <div style="margin-top:12px;padding-top:12px;border-top:1px solid #1a2a4a;display:flex;justify-content:space-between;">
                    <button class="btn btn-secondary btn-sm" onclick="switchTab('tab-visual')"><i class="fas fa-arrow-left"></i> Kembali</button>
                    <button class="btn btn-secondary btn-sm" onclick="switchTab('tab-ide')">Ke Awal <i class="fas fa-arrow-right"></i></button>
                </div>
            </div>
        </div>

    </main>

    <!-- ========================================================== -->
    <!-- TOAST -->
    <!-- ========================================================== -->
    <div class="toast-container" id="toastContainer"></div>

    <!-- ========================================================== -->
    <!-- MODAL -->
    <!-- ========================================================== -->
    <div class="modal-overlay" id="modalOverlay">
        <div class="modal">
            <div class="icon"><i class="fas fa-film"></i></div>
            <h3>Mengekspor Video...</h3>
            <p id="modalStatus">Memproses...</p>
            <div class="ring-wrap">
                <svg viewBox="0 0 100 100">
                    <circle class="bg" cx="50" cy="50" r="40"/>
                    <circle class="fill" id="modalRing" cx="50" cy="50" r="40"/>
                </svg>
                <div class="pct" id="modalPct">0%</div>
            </div>
            <button class="btn-close" onclick="cancelRender()">Batalkan</button>
        </div>
    </div>

    <!-- ========================================================== -->
    <!-- CANVAS -->
    <!-- ========================================================== -->
    <canvas id="renderCanvas" style="display:none;"></canvas>

    <!-- ========================================================== -->
    <!-- SCRIPT -->
    <!-- ========================================================== -->
    <script>
        // ============================================================
        // STATE
        // ============================================================
        let story = { title: '', description: '', hashtags: '', scenes: [] };
        let playerIndex = 0;
        let playerTimer = null;
        let isRendering = false;
        let renderCancel = false;
        let mediaRecorder = null;
        let recordedChunks = [];
        let speechSynth = window.speechSynthesis;

        // ============================================================
        // API KEY - DIPERBAIKI
        // ============================================================
        function getAPIKey() {
            const key = document.getElementById('apiKey').value.trim();
            if (key) localStorage.setItem('openrouter_key', key);
            return key;
        }

        function updateAPIStatus(valid, msg) {
            const dot = document.getElementById('apiDot');
            const status = document.getElementById('apiStatus');
            if (valid) {
                dot.className = 'dot on';
                status.innerHTML = `<span class="valid">✅ ${msg || 'Valid'}</span>`;
            } else {
                dot.className = 'dot off';
                status.innerHTML = `<span class="invalid">❌ ${msg || 'Tidak valid'}</span>`;
            }
        }

        async function testAPI() {
            const key = getAPIKey();
            if (!key) {
                updateAPIStatus(false, 'Key kosong!');
                showToast('❌ Masukkan API Key!', 'error');
                return;
            }

            // Validasi format key
            if (!key.startsWith('sk-or-v1-')) {
                updateAPIStatus(false, 'Format key salah! Harus sk-or-v1-...');
                showToast('❌ Format key salah! Harus diawali sk-or-v1-', 'error');
                return;
            }

            showToast('🔍 Menguji...', 'info');
            try {
                const res = await fetch('https://openrouter.ai/api/v1/models', {
                    headers: { 'Authorization': `Bearer ${key}` }
                });
                
                if (res.ok) {
                    updateAPIStatus(true, 'OpenRouter siap!');
                    showToast('✅ API Key valid!', 'success');
                } else {
                    const err = await res.text();
                    updateAPIStatus(false, 'Key tidak valid');
                    showToast(`❌ ${err}`, 'error');
                }
            } catch(e) {
                updateAPIStatus(false, 'Gagal koneksi');
                showToast('❌ Gagal koneksi ke OpenRouter', 'error');
            }
        }

        function openKeyPage() {
            window.open('https://openrouter.ai/keys', '_blank');
            showToast('📋 Ambil API Key dari OpenRouter (sk-or-v1-...)', 'info');
        }

        // ============================================================
        // TOAST
        // ============================================================
        function showToast(msg, type = 'success') {
            const container = document.getElementById('toastContainer');
            const types = { success: 'success', error: 'error', info: 'info', warn: 'warning' };
            const icons = { success: 'fa-check-circle', error: 'fa-circle-exclamation', info: 'fa-info-circle', warn: 'fa-triangle-exclamation' };
            const toast = document.createElement('div');
            toast.className = `toast ${types[type] || 'info'}`;
            toast.innerHTML = `<i class="fas ${icons[type] || icons.info}"></i> ${msg}`;
            container.appendChild(toast);
            setTimeout(() => {
                toast.style.opacity = '0';
                toast.style.transform = 'translateY(20px)';
                setTimeout(() => toast.remove(), 300);
            }, 4000);
        }

        // ============================================================
        // NAVIGATION
        // ============================================================
        function switchTab(tabId) {
            document.querySelectorAll('.tab').forEach(el => el.classList.remove('active'));
            document.getElementById(tabId).classList.add('active');
            
            const titles = {
                'tab-ide': '🎯 Ideasi & Naskah',
                'tab-suara': '🎤 Text-to-Speech',
                'tab-visual': '🖼️ Storyboard Visual',
                'tab-render': '🎬 Render Video'
            };
            const subs = {
                'tab-ide': 'OpenRouter AI + Pollinations Image + Web Speech',
                'tab-suara': 'Gunakan suara bawaan browser (gratis)',
                'tab-visual': 'Generate gambar dengan Pollinations AI (gratis)',
                'tab-render': 'Gabungkan gambar + narasi ke dalam video'
            };
            document.getElementById('pageTitle').textContent = titles[tabId] || 'Studio Konten';
            document.getElementById('pageSub').textContent = subs[tabId] || '';
            
            document.querySelectorAll('.sidebar nav button').forEach(b => b.classList.remove('active'));
            const navMap = { 'tab-ide': 'nav-ide', 'tab-suara': 'nav-suara', 'tab-visual': 'nav-visual', 'tab-render': 'nav-render' };
            const activeNav = document.getElementById(navMap[tabId]);
            if (activeNav) activeNav.classList.add('active');
        }

        // ============================================================
        // CALL OPENROUTER
        // ============================================================
        async function callOpenRouter(messages) {
            const key = getAPIKey();
            if (!key) throw new Error('API Key tidak tersedia');

            const response = await fetch('https://openrouter.ai/api/v1/chat/completions', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json',
                    'Authorization': `Bearer ${key}`,
                    'HTTP-Referer': window.location.href,
                    'X-Title': 'Studio Konten Pro'
                },
                body: JSON.stringify({
                    model: 'meta-llama/llama-3.3-70b-instruct:free',
                    messages: messages,
                    temperature: 0.8,
                    max_tokens: 4096
                })
            });

            if (!response.ok) {
                const err = await response.text();
                throw new Error(`Error ${response.status}: ${err}`);
            }
            return await response.json();
        }

        // ============================================================
        // RESEARCH
        // ============================================================
        async function researchTitles() {
            const topic = document.getElementById('inputResearch').value.trim();
            if (!topic) { showToast('Masukkan topik riset!', 'error'); return; }

            const btn = document.getElementById('btnResearch');
            const orig = btn.innerHTML;
            btn.innerHTML = '<span class="fas fa-spinner fa-spin"></span> Mencari...';
            btn.disabled = true;

            try {
                const result = await callOpenRouter([
                    { role: 'system', content: 'Kamu asisten riset. Berikan 10 judul video menarik dalam format JSON array. Hanya array JSON, tanpa teks lain.' },
                    { role: 'user', content: `Riset 10 judul video viral tentang: "${topic}"` }
                ]);

                const raw = result.choices[0].message.content;
                let titles = [];
                try {
                    const clean = raw.replace(/```json|```|```javascript|```js/gi, '').trim();
                    titles = JSON.parse(clean);
                } catch(e) {
                    const match = raw.match(/\[([^\]]*)\]/);
                    if (match) titles = JSON.parse(match[0]);
                    else titles = raw.split(',').map(s => s.replace(/["'\[\]]/g, '').trim()).filter(Boolean);
                }

                titles = titles.slice(0, 10);
                const grid = document.getElementById('titlesGrid');
                grid.innerHTML = '';
                titles.forEach((t, i) => {
                    const item = document.createElement('div');
                    item.className = 'title-item';
                    item.innerHTML = `<span class="num">${i+1}</span> ${t}`;
                    item.onclick = () => {
                        document.getElementById('inputTopic').value = t;
                        showToast(`"${t}" dipilih!`, 'success');
                    };
                    grid.appendChild(item);
                });
                document.getElementById('titlesContainer').style.display = 'block';
                showToast(`✅ ${titles.length} judul ditemukan!`, 'success');
            } catch(e) {
                showToast(`❌ Gagal: ${e.message}`, 'error');
            } finally {
                btn.innerHTML = orig;
                btn.disabled = false;
            }
        }

        // ============================================================
        // GENERATE NASKAH
        // ============================================================
        async function generateNaskah() {
            const topic = document.getElementById('inputTopic').value.trim();
            if (!topic) { showToast('Tulis topik cerita!', 'error'); return; }

            const btn = document.getElementById('btnGenerate');
            const orig = btn.innerHTML;
            btn.innerHTML = '<span class="fas fa-spinner fa-spin"></span> Menulis...';
            btn.disabled = true;

            const count = document.getElementById('sceneCount').value;

            try {
                const result = await callOpenRouter([
                    { role: 'system', content: `Kamu penulis naskah profesional. Buat naskah video ${count} adegan. Format JSON murni: {"title":"","description":"","hashtags":"","scenes":[{"scene":1,"narration":"narasi","visualPrompt":"english prompt"}]}. Hanya JSON, tanpa teks lain.` },
                    { role: 'user', content: `Buat naskah video estetik dengan tema: "${topic}" ${count} adegan. Bahasa Indonesia. Cerita sinematik.` }
                ]);

                const raw = result.choices[0].message.content;
                const clean = raw.replace(/```json|```|```javascript|```js/gi, '').trim();
                
                let parsed;
                try { parsed = JSON.parse(clean); }
                catch(e) {
                    const fixed = clean.replace(/(\w+):/g, '"$1":').replace(/'/g, '"');
                    parsed = JSON.parse(fixed);
                }

                story = parsed;
                if (!story.scenes || story.scenes.length === 0) throw new Error('Tidak ada adegan');

                story.scenes = story.scenes.map((s, i) => ({
                    scene: i + 1,
                    narration: s.narration || `Adegan ${i+1}`,
                    visualPrompt: s.visualPrompt || 'cinematic scene, beautiful lighting, 8k'
                }));

                renderOutput();
                showToast(`✅ ${story.scenes.length} adegan berhasil dibuat!`, 'success');
            } catch(e) {
                // Fallback
                story = {
                    title: topic || 'Cerita Menarik',
                    description: 'Sebuah cerita yang menginspirasi',
                    hashtags: '#cerita #inspirasi #video',
                    scenes: [
                        { scene: 1, narration: `Mulailah dengan ${topic} yang menarik perhatian.`, visualPrompt: 'cinematic scene, dramatic lighting' },
                        { scene: 2, narration: 'Kembangkan alur cerita dengan konflik dan resolusi.', visualPrompt: 'storytelling scene, emotional' },
                        { scene: 3, narration: 'Akhiri dengan pesan yang menginspirasi penonton.', visualPrompt: 'uplifting scene, hopeful' }
                    ]
                };
                renderOutput();
                showToast('⚠️ Menggunakan template lokal', 'warn');
            } finally {
                btn.innerHTML = orig;
                btn.disabled = false;
            }
        }

        // ============================================================
        // RENDER OUTPUT
        // ============================================================
        function renderOutput() {
            document.getElementById('outTitle').value = story.title || 'Judul';
            document.getElementById('outDesc').value = story.description || 'Deskripsi';
            document.getElementById('outTags').value = story.hashtags || '#video';
            document.getElementById('sceneBadge').textContent = `${story.scenes.length} Adegan`;

            const editor = document.getElementById('scriptEditor');
            editor.innerHTML = '';
            story.scenes.forEach((s, i) => {
                const div = document.createElement('div');
                div.className = 'scene-item';
                div.innerHTML = `
                    <div class="scene-header">
                        <span class="num">Adegan ${s.scene}</span>
                    </div>
                    <label>Narasi</label>
                    <textarea onchange="updateScene(${i},'narration',this.value)" rows="2">${s.narration}</textarea>
                    <label style="margin-top:6px;">Visual Prompt</label>
                    <input type="text" id="prompt-${i}" onchange="updateScene(${i},'visualPrompt',this.value)" value="${s.visualPrompt}">
                `;
                editor.appendChild(div);
            });

            document.getElementById('outputContainer').style.display = 'block';
            buildAudioList();
            buildVisualGrid();
            buildPlaylist();
        }

        function updateScene(idx, prop, val) {
            if (story.scenes[idx]) {
                story.scenes[idx][prop] = val;
                if (prop === 'narration') buildAudioList();
                if (prop === 'visualPrompt') buildVisualGrid();
                buildPlaylist();
            }
        }

        function copyMetadata() {
            const txt = document.getElementById('outTitle').value + '\n\n' + 
                       document.getElementById('outDesc').value + '\n\n' + 
                       document.getElementById('outTags').value;
            navigator.clipboard.writeText(txt).then(() => showToast('✅ Metadata disalin!', 'success'));
        }

        // ============================================================
        // AUDIO
        // ============================================================
        function buildAudioList() {
            const container = document.getElementById('audioList');
            container.innerHTML = '';
            if (!story.scenes || story.scenes.length === 0) {
                container.innerHTML = '<div style="text-align:center;padding:30px 0;color:#667799;font-size:13px;">Buat naskah dulu di Langkah 1</div>';
                return;
            }
            story.scenes.forEach((s, i) => {
                const div = document.createElement('div');
                div.className = 'audio-item';
                div.innerHTML = `
                    <div class="info">
                        <div class="num">Adegan ${s.scene}</div>
                        <div class="text">${s.narration}</div>
                    </div>
                    <button class="play-btn" onclick="playAudio(${i})"><i class="fas fa-play"></i></button>
                `;
                container.appendChild(div);
            });
        }

        function playAudio(idx) {
            speechSynth.cancel();
            const text = story.scenes[idx].narration;
            const utterance = new SpeechSynthesisUtterance(text);
            utterance.lang = 'id-ID';
            utterance.rate = 0.95;
            const voices = speechSynth.getVoices();
            const indo = voices.find(v => v.lang.includes('id'));
            if (indo) utterance.voice = indo;
            speechSynth.speak(utterance);
            showToast(`🔊 Memutar adegan ${idx+1}`, 'info');
        }

        function playAllAudio() {
            speechSynth.cancel();
            if (!story.scenes || story.scenes.length === 0) return;
            const texts = story.scenes.map(s => s.narration);
            let i = 0;
            function playNext() {
                if (i >= texts.length) return;
                const utterance = new SpeechSynthesisUtterance(texts[i]);
                utterance.lang = 'id-ID';
                utterance.rate = 0.95;
                const voices = speechSynth.getVoices();
                const indo = voices.find(v => v.lang.includes('id'));
                if (indo) utterance.voice = indo;
                utterance.onend = () => { i++; playNext(); };
                speechSynth.speak(utterance);
            }
            playNext();
            showToast('🔊 Memutar semua narasi', 'info');
        }

        function stopAllAudio() {
            speechSynth.cancel();
            showToast('⏹️ Audio dihentikan', 'info');
        }

        // ============================================================
        // VISUAL
        // ============================================================
        function buildVisualGrid() {
            const grid = document.getElementById('visualGrid');
            grid.innerHTML = '';
            if (!story.scenes || story.scenes.length === 0) {
                grid.innerHTML = '<div style="grid-column:span 2;text-align:center;padding:30px 0;color:#667799;font-size:13px;">Buat naskah dulu di Langkah 1</div>';
                return;
            }

            const isShort = document.querySelector('input[name="format"]:checked').value === 'short';
            const w = isShort ? 540 : 960;
            const h = isShort ? 960 : 540;

            story.scenes.forEach((s, i) => {
                const prompt = s.visualPrompt || 'cinematic scene';
                const encoded = encodeURIComponent(prompt + ', masterpiece, 8k, no text');
                const url = `https://image.pollinations.ai/prompt/${encoded}?width=${w}&height=${h}&nologo=true&private=true&seed=${i+99}`;

                const item = document.createElement('div');
                item.className = 'visual-item';
                item.innerHTML = `
                    <div class="img-wrap">
                        <div class="spinner-overlay" id="spinner-${i}">
                            <div class="loader"></div>
                            <span>Membuat gambar...</span>
                        </div>
                        <img id="img-${i}" src="${url}" 
                             onload="document.getElementById('spinner-${i}').classList.add('hidden')"
                             onerror="this.src='https://placehold.co/${w}x${h}/0a0e1a/6c5ce7?text=Scene+${s.scene}'">
                    </div>
                    <div class="visual-footer">
                        <span class="num">Adegan ${s.scene}</span>
                        <div class="actions">
                            <button onclick="regenerateImage(${i})"><i class="fas fa-rotate"></i></button>
                        </div>
                    </div>
                `;
                grid.appendChild(item);
            });
        }

        function regenerateImage(idx) {
            const s = story.scenes[idx];
            const isShort = document.querySelector('input[name="format"]:checked').value === 'short';
            const w = isShort ? 540 : 960;
            const h = isShort ? 960 : 540;
            const seed = Math.floor(Math.random() * 9999);
            const prompt = s.visualPrompt || 'cinematic scene';
            const encoded = encodeURIComponent(prompt + ', masterpiece, 8k, no text');
            const url = `https://image.pollinations.ai/prompt/${encoded}?width=${w}&height=${h}&nologo=true&private=true&seed=${seed}`;
            
            const spinner = document.getElementById(`spinner-${idx}`);
            if (spinner) spinner.classList.remove('hidden');
            
            const img = document.getElementById(`img-${idx}`);
            img.src = url;
            img.onload = () => {
                if (spinner) spinner.classList.add('hidden');
                showToast(`🔄 Gambar adegan ${idx+1} diperbarui`, 'success');
            };
            img.onerror = () => {
                if (spinner) spinner.classList.add('hidden');
                img.src = `https://placehold.co/${w}x${h}/0a0e1a/6c5ce7?text=Scene+${s.scene}`;
            };
        }

        function generateAllImages() {
            showToast('🖼️ Memulai render semua gambar...', 'info');
            story.scenes.forEach((_, i) => {
                setTimeout(() => regenerateImage(i), i * 800);
            });
        }

        // ============================================================
        // PLAYER
        // ============================================================
        function buildPlaylist() {
            const container = document.getElementById('playlistContainer');
            container.innerHTML = '';
            if (!story.scenes || story.scenes.length === 0) {
                container.innerHTML = '<div style="width:100%;text-align:center;padding:16px 0;color:#667799;font-size:13px;">Belum ada adegan</div>';
                return;
            }

            story.scenes.forEach((s, i) => {
                const img = document.getElementById(`img-${i}`);
                const div = document.createElement('div');
                div.className = 'playlist-item';
                div.onclick = () => selectScene(i);
                div.innerHTML = `
                    <div class="thumb">
                        <img src="${img ? img.src : 'https://placehold.co/160x90/0a0e1a/334466'}">
                    </div>
                    <div class="name">Scene ${s.scene}</div>
                `;
                container.appendChild(div);
            });
        }

        function selectScene(idx) {
            playerIndex = idx;
            const img = document.getElementById(`img-${idx}`);
            document.getElementById('playerImg').src = img ? img.src : 'https://placehold.co/1280x720/0d1423/334466';
            document.getElementById('playerText').textContent = story.scenes[idx].narration;
            document.getElementById('playerStatus').textContent = `Adegan ${idx+1}/${story.scenes.length}`;
        }

        function togglePlayer() {
            if (playerTimer) {
                clearInterval(playerTimer);
                playerTimer = null;
                document.getElementById('playerBtn').innerHTML = '<i class="fas fa-play"></i>';
                document.getElementById('playerStatus').textContent = 'Berhenti';
                return;
            }

            if (!story.scenes || story.scenes.length === 0) {
                showToast('Buat naskah dulu!', 'error');
                return;
            }

            if (playerIndex >= story.scenes.length) playerIndex = 0;
            selectScene(playerIndex);
            document.getElementById('playerBtn').innerHTML = '<i class="fas fa-pause"></i>';
            document.getElementById('playerStatus').textContent = 'Memutar...';

            playerTimer = setInterval(() => {
                playerIndex++;
                if (playerIndex >= story.scenes.length) {
                    clearInterval(playerTimer);
                    playerTimer = null;
                    document.getElementById('playerBtn').innerHTML = '<i class="fas fa-play"></i>';
                    document.getElementById('playerStatus').textContent = 'Selesai';
                    return;
                }
                selectScene(playerIndex);
            }, 4500);
        }

        // ============================================================
        // RENDER VIDEO
        // ============================================================
        async function startRender() {
            if (!story.scenes || story.scenes.length === 0) {
                showToast('Buat naskah dan gambar dulu!', 'error');
                return;
            }

            const btn = document.getElementById('btnRender');
            btn.disabled = true;
            btn.innerHTML = '<span class="fas fa-spinner fa-spin"></span> Render...';

            document.getElementById('modalOverlay').classList.add('open');
            document.getElementById('modalStatus').textContent = 'Memuat gambar...';
            updateModalProgress(0);

            renderCancel = false;
            recordedChunks = [];
            isRendering = true;

            try {
                // Load images
                const images = [];
                for (let i = 0; i < story.scenes.length; i++) {
                    if (renderCancel) throw new Error('Dibatalkan');
                    const img = document.getElementById(`img-${i}`);
                    const src = img ? img.src : `https://placehold.co/1280x720/0a0e1a/6c5ce7?text=Scene+${i+1}`;
                    const loaded = new Image();
                    loaded.crossOrigin = 'anonymous';
                    loaded.src = src;
                    await new Promise(r => { loaded.onload = r; loaded.onerror = r; });
                    images.push(loaded);
                    updateModalProgress((i + 1) / story.scenes.length * 30);
                    document.getElementById('modalStatus').textContent = `Memuat gambar ${i+1}/${story.scenes.length}`;
                }

                if (renderCancel) throw new Error('Dibatalkan');

                // Setup canvas
                const isShort = document.querySelector('input[name="format"]:checked').value === 'short';
                const canvas = document.getElementById('renderCanvas');
                canvas.width = isShort ? 720 : 1280;
                canvas.height = isShort ? 1280 : 720;
                const ctx = canvas.getContext('2d');

                const stream = canvas.captureStream(30);
                const options = { mimeType: 'video/webm;codecs=vp9' };
                if (!MediaRecorder.isTypeSupported(options.mimeType)) {
                    options.mimeType = 'video/webm;codecs=vp8';
                }
                if (!MediaRecorder.isTypeSupported(options.mimeType)) {
                    options.mimeType = 'video/webm';
                }

                mediaRecorder = new MediaRecorder(stream, options);
                mediaRecorder.ondataavailable = e => {
                    if (e.data && e.data.size > 0) recordedChunks.push(e.data);
                };
                mediaRecorder.onstop = () => {
                    if (!renderCancel && recordedChunks.length > 0) {
                        const blob = new Blob(recordedChunks, { type: options.mimeType });
                        const url = URL.createObjectURL(blob);
                        downloadVideo(url);
                    }
                    isRendering = false;
                };

                mediaRecorder.start();

                // Render scenes
                for (let i = 0; i < story.scenes.length; i++) {
                    if (renderCancel) break;
                    document.getElementById('modalStatus').textContent = `Merekam adegan ${i+1}/${story.scenes.length}`;
                    await renderScene(ctx, canvas, images[i], story.scenes[i].narration, i);
                    updateModalProgress(30 + ((i + 1) / story.scenes.length) * 60);
                }

                if (renderCancel) {
                    mediaRecorder.stop();
                    throw new Error('Dibatalkan');
                }

                document.getElementById('modalStatus').textContent = 'Menyelesaikan...';
                updateModalProgress(100);
                setTimeout(() => mediaRecorder.stop(), 300);

            } catch(e) {
                if (e.message !== 'Dibatalkan') {
                    showToast(`❌ Render gagal: ${e.message}`, 'error');
                }
                document.getElementById('modalOverlay').classList.remove('open');
            } finally {
                btn.disabled = false;
                btn.innerHTML = '<i class="fas fa-play"></i> Mulai Render';
            }
        }

        function renderScene(ctx, canvas, img, text, idx) {
            return new Promise((resolve) => {
                const duration = Math.max(text.split(' ').length * 250 + 500, 3000);
                const start = performance.now();
                const cw = canvas.width;
                const ch = canvas.height;
                const isV = cw < ch;

                function frame(time) {
                    if (renderCancel) { resolve(); return; }
                    const elapsed = time - start;
                    if (elapsed >= duration) { resolve(); return; }

                    const scale = 1 + 0.04 * (elapsed / duration);
                    ctx.clearRect(0, 0, cw, ch);
                    
                    ctx.save();
                    ctx.translate(cw/2, ch/2);
                    ctx.scale(scale, scale);
                    ctx.translate(-cw/2, -ch/2);
                    ctx.drawImage(img, 0, 0, cw, ch);
                    ctx.restore();

                    const grad = ctx.createLinearGradient(0, ch - (isV ? 300 : 200), 0, ch);
                    grad.addColorStop(0, 'transparent');
                    grad.addColorStop(1, 'rgba(0,0,0,0.85)');
                    ctx.fillStyle = grad;
                    ctx.fillRect(0, ch - (isV ? 300 : 200), cw, isV ? 300 : 200);

                    ctx.textAlign = 'center';
                    ctx.textBaseline = 'bottom';
                    const fontSize = isV ? Math.min(cw / 18, 40) : Math.min(cw / 24, 48);
                    ctx.font = `bold ${fontSize}px 'Segoe UI', system-ui, sans-serif`;
                    ctx.shadowColor = 'rgba(0,0,0,0.9)';
                    ctx.shadowBlur = 12;
                    ctx.fillStyle = '#ffffff';
                    
                    const maxW = cw * 0.85;
                    const words = text.split(' ');
                    let line = '';
                    let lines = [];
                    for (let w of words) {
                        const test = line + w + ' ';
                        if (ctx.measureText(test).width > maxW && line) {
                            lines.push(line.trim());
                            line = w + ' ';
                        } else {
                            line = test;
                        }
                    }
                    lines.push(line.trim());

                    const lineHeight = fontSize * 1.3;
                    let y = ch - (isV ? 50 : 40) - ((lines.length - 1) * lineHeight);
                    for (let l of lines) {
                        ctx.fillText(l, cw/2, y);
                        y += lineHeight;
                    }

                    ctx.shadowBlur = 0;
                    requestAnimationFrame(frame);
                }
                requestAnimationFrame(frame);
            });
        }

        function updateModalProgress(pct) {
            const ring = document.getElementById('modalRing');
            const circumference = 251;
            ring.style.strokeDashoffset = circumference - (pct / 100) * circumference;
            document.getElementById('modalPct').textContent = `${Math.round(pct)}%`;
        }

        function downloadVideo(url) {
            document.getElementById('modalOverlay').classList.remove('open');
            showToast('✅ Video selesai dirender!', 'success');
            
            const a = document.createElement('a');
            a.href = url;
            a.download = `video_${Date.now()}.webm`;
            document.body.appendChild(a);
            a.click();
            document.body.removeChild(a);
            
            document.getElementById('playerImg').src = url;
            document.getElementById('playerText').textContent = '🎬 Video siap diputar!';
        }

        function cancelRender() {
            renderCancel = true;
            if (mediaRecorder && mediaRecorder.state === 'recording') {
                mediaRecorder.stop();
            }
            document.getElementById('modalOverlay').classList.remove('open');
            showToast('⏹️ Render dibatalkan', 'warn');
        }

        // ============================================================
        // INIT
        // ============================================================
        window.onload = function() {
            const saved = localStorage.getItem('openrouter_key');
            if (saved) {
                document.getElementById('apiKey').value = saved;
                // Cek format key
                if (saved.startsWith('sk-or-v1-')) {
                    updateAPIStatus(true, 'Key tersimpan');
                    setTimeout(testAPI, 500);
                } else {
                    updateAPIStatus(false, 'Format key salah! Harus sk-or-v1-...');
                }
            } else {
                updateAPIStatus(false, 'Masukkan API Key');
                showToast('🔑 Masukkan API Key OpenRouter di sidebar', 'info');
            }

            if (window.speechSynthesis.onvoiceschanged !== undefined) {
                window.speechSynthesis.onvoiceschanged = () => {};
            }

            console.log('🎬 Studio Konten Pro siap!');
            console.log('📌 API Key harus diawali: sk-or-v1-');
        };
    </script>

</body>
</html>
