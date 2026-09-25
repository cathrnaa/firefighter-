<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
    <title>🔥 FIREWATCH: SIMULASI PEMADAMAN HUTAN 🚒</title>
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            user-select: none;
            -webkit-user-select: none;
            -webkit-touch-callout: none;
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
        }

        html, body {
            width: 100vw;
            height: 100dvh;
            overflow: hidden;
            background-color: #0d1a0d;
            color: #ffffff;
            touch-action: none;
        }

        #game-container {
            position: relative;
            width: 100vw;
            height: 100dvh;
            display: flex;
            flex-direction: column;
        }

        #canvas-container {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: 1;
        }

        canvas {
            width: 100%;
            height: 100%;
            display: block;
        }

        /* HUD Overlay */
        #hud {
            position: absolute;
            top: 10px;
            left: 10px;
            right: 10px;
            z-index: 10;
            display: flex;
            justify-content: space-between;
            align-items: center;
            pointer-events: none;
        }

        .hud-card {
            background: rgba(15, 23, 15, 0.9);
            border: 2px solid #22c55e;
            border-radius: 14px;
            padding: 6px 12px;
            display: flex;
            align-items: center;
            gap: 6px;
            font-weight: bold;
            font-size: 12px;
            box-shadow: 0 4px 12px rgba(0,0,0,0.5);
            backdrop-filter: blur(4px);
        }

        .hud-card span {
            color: #fbbf24;
        }

        #reset-btn {
            pointer-events: auto;
            background: #ef4444;
            color: white;
            border: none;
            padding: 8px 12px;
            border-radius: 12px;
            font-weight: bold;
            font-size: 11px;
            cursor: pointer;
        }

        /* Mission Banner */
        #mission-banner {
            position: absolute;
            top: 60px;
            left: 50%;
            transform: translateX(-50%);
            background: rgba(30, 41, 59, 0.95);
            border: 2px solid #38bdf8;
            color: #f8fafc;
            padding: 8px 16px;
            border-radius: 20px;
            font-size: 12px;
            font-weight: bold;
            z-index: 9;
            box-shadow: 0 4px 12px rgba(0,0,0,0.5);
            pointer-events: none;
            text-align: center;
            max-width: 90%;
        }

        /* Touch Controls */
        #controls-overlay {
            position: absolute;
            bottom: 12px;
            left: 12px;
            right: 12px;
            height: 150px;
            z-index: 10;
            display: flex;
            justify-content: space-between;
            align-items: flex-end;
            pointer-events: none;
        }

        #joystick-wrapper {
            position: relative;
            width: 130px;
            height: 130px;
            background: rgba(255, 255, 255, 0.12);
            border: 2px solid rgba(255, 255, 255, 0.3);
            border-radius: 50%;
            pointer-events: auto;
            touch-action: none;
            backdrop-filter: blur(2px);
        }

        #joystick-stick {
            position: absolute;
            top: 50%;
            left: 50%;
            width: 50px;
            height: 50px;
            margin-top: -25px;
            margin-left: -25px;
            background: radial-gradient(circle, #ef4444 0%, #b91c1c 100%);
            border: 2px solid #fca5a5;
            border-radius: 50%;
            box-shadow: 0 4px 10px rgba(0,0,0,0.5);
            pointer-events: none;
        }

        #action-btn {
            pointer-events: auto;
            width: 85px;
            height: 85px;
            border-radius: 50%;
            background: linear-gradient(135deg, #0284c7, #0369a1);
            border: 3px solid #38bdf8;
            color: white;
            font-weight: 900;
            font-size: 13px;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            box-shadow: 0 6px 16px rgba(0,0,0,0.5);
            touch-action: none;
            cursor: pointer;
        }

        #action-btn:active {
            transform: scale(0.92);
        }

        /* Modals & Panels */
        .modal {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(10, 20, 10, 0.92);
            z-index: 100;
            display: none;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            padding: 12px;
            backdrop-filter: blur(6px);
            overflow-y: auto;
        }

        .modal.active {
            display: flex;
        }

        .panel {
            background: #1e293b;
            border: 2px solid #3b82f6;
            border-radius: 18px;
            width: 100%;
            max-width: 440px;
            padding: 16px;
            box-shadow: 0 10px 25px rgba(0,0,0,0.7);
            display: flex;
            flex-direction: column;
            gap: 12px;
            max-height: 94vh;
            overflow-y: auto;
            position: relative;
        }

        .logbook-sticky {
            background: #0f172a;
            border: 2px solid #f59e0b;
            border-radius: 12px;
            padding: 10px;
            font-size: 12px;
            color: #f1f5f9;
            box-shadow: 0 4px 10px rgba(0,0,0,0.4);
        }

        .logbook-sticky h4 {
            color: #fbbf24;
            display: flex;
            align-items: center;
            gap: 6px;
            margin-bottom: 4px;
            font-size: 13px;
        }

        .logbook-grid {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 6px;
            text-align: center;
        }

        .logbook-item {
            background: #1e293b;
            padding: 6px;
            border-radius: 6px;
            border: 1px solid #334155;
        }

        .panel-header {
            text-align: center;
            border-bottom: 2px solid #334155;
            padding-bottom: 8px;
        }

        .panel-header h2 {
            color: #f3f4f6;
            font-size: 18px;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 6px;
        }

        .btn {
            width: 100%;
            padding: 12px;
            border: none;
            border-radius: 12px;
            font-size: 13px;
            font-weight: bold;
            color: white;
            cursor: pointer;
            display: flex;
            justify-content: center;
            align-items: center;
            gap: 6px;
            box-shadow: 0 4px 10px rgba(0,0,0,0.3);
            touch-action: manipulation;
        }

        .btn-primary { background: linear-gradient(135deg, #22c55e, #16a34a); border: 1px solid #4ade80; }
        .btn-secondary { background: linear-gradient(135deg, #3b82f6, #1d4ed8); border: 1px solid #60a5fa; }
        .btn-warning { background: linear-gradient(135deg, #f59e0b, #d97706); border: 1px solid #fbbf24; }
        .btn-danger { background: linear-gradient(135deg, #ef4444, #dc2626); border: 1px solid #fca5a5; }
        .btn:disabled { opacity: 0.5; cursor: not-allowed; }

        /* Keypad */
        .keypad-display {
            background: #0f172a;
            border: 2px solid #334155;
            border-radius: 10px;
            padding: 8px;
            text-align: right;
            font-size: 22px;
            font-family: monospace;
            color: #38bdf8;
            font-weight: bold;
            min-height: 44px;
        }

        .keypad-grid {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 6px;
        }

        .keypad-btn {
            background: #334155;
            border: none;
            border-radius: 8px;
            padding: 12px;
            font-size: 16px;
            font-weight: bold;
            color: white;
        }

        /* Interactive Bar Chart */
        .chart-area {
            display: flex;
            justify-content: space-around;
            align-items: flex-end;
            height: 160px;
            border-bottom: 2px solid #64748b;
            border-left: 2px solid #64748b;
            padding-bottom: 2px;
            background: #0f172a;
            border-radius: 8px;
            padding-top: 10px;
        }

        .chart-bar-wrap {
            display: flex;
            flex-direction: column;
            align-items: center;
            height: 100%;
            justify-content: flex-end;
            width: 50px;
        }

        .chart-bar {
            width: 36px;
            background: linear-gradient(to top, #ef4444, #f97316);
            border-radius: 4px 4px 0 0;
            min-height: 4px;
            touch-action: none;
        }

        .chart-val { font-size: 11px; font-weight: bold; color: #fbbf24; margin-bottom: 2px; }
        .chart-label { margin-top: 4px; font-weight: bold; font-size: 12px; color: #94a3b8; }

        /* Custom Checkbox Strategy */
        .water-check-card {
            background: #0f172a;
            border: 2px solid #334155;
            border-radius: 10px;
            padding: 10px;
            display: flex;
            align-items: center;
            justify-content: space-between;
            margin-bottom: 6px;
            cursor: pointer;
        }

        .water-check-card.selected {
            border-color: #38bdf8;
            background: #1e293b;
        }

        .water-check-card input {
            width: 20px;
            height: 20px;
            accent-color: #38bdf8;
        }

        #water-warn-box {
            background: rgba(239, 68, 68, 0.2);
            border: 1px solid #ef4444;
            color: #fca5a5;
            padding: 8px 12px;
            border-radius: 8px;
            font-size: 12px;
            display: none;
            font-weight: bold;
            text-align: center;
        }

        .quiz-opt {
            background: #334155;
            border: 2px solid #475569;
            border-radius: 8px;
            padding: 10px;
            font-weight: bold;
            font-size: 13px;
            cursor: pointer;
        }

        .quiz-opt.selected { border-color: #38bdf8; background: #0f172a; }

        #toast {
            position: absolute;
            top: 100px;
            left: 50%;
            transform: translateX(-50%);
            background: rgba(15, 23, 42, 0.95);
            border: 2px solid #f59e0b;
            color: white;
            padding: 8px 16px;
            border-radius: 20px;
            font-weight: bold;
            font-size: 12px;
            z-index: 200;
            pointer-events: none;
            opacity: 0;
            transition: opacity 0.3s ease;
            text-align: center;
            width: 85%;
            max-width: 320px;
        }

        #toast.show { opacity: 1; }
    </style>
</head>
<body>

    <div id="game-container">
        <!-- Canvas Layer -->
        <div id="canvas-container">
            <canvas id="gameCanvas"></canvas>
        </div>

        <!-- HUD -->
        <div id="hud">
            <div class="hud-card">🔥 <span>FIREWATCH</span></div>
            <div class="hud-card">📋 DATA: <span id="data-count-val">0/3</span></div>
            <div class="hud-card">💧 AIR: <span id="water-tank-val">5.000L</span></div>
            <button id="reset-btn">RESET</button>
        </div>

        <!-- Mission Banner -->
        <div id="mission-banner">
            📍 MISI: Kumpulkan data di Titik A, B, dan C!
        </div>

        <!-- Touch Controls -->
        <div id="controls-overlay">
            <div id="joystick-wrapper">
                <div id="joystick-stick"></div>
            </div>
            <div id="action-btn">
                <span>🔍</span>
                <span>AKSI</span>
            </div>
        </div>

        <!-- Toast Notification -->
        <div id="toast">Message</div>

        <!-- MODAL 1: DISCOVERY OF FIRE DATA -->
        <div id="modal-discovery" class="modal">
            <div class="panel">
                <div class="panel-header">
                    <h2>🔥 OBSERVASI TITIK API</h2>
                </div>
                <div id="discovery-content" style="text-align: center; font-size: 14px; line-height: 1.6;">
                    <!-- Filled by JS -->
                </div>
                <button id="btn-collect-data" class="btn btn-warning">📋 CATAT DATA KE LOGBOOK</button>
            </div>
        </div>

        <!-- MODAL 2: DATA CENTER & MATH PROCESSING -->
        <div id="modal-datacenter" class="modal">
            <div class="panel">
                <div class="panel-header">
                    <h2>📊 RESCUE DATA CENTER</h2>
                    <p style="font-size: 11px; color: #94a3b8;">Pengolahan Data & Perencanaan Pemadaman</p>
                </div>

                <!-- FIXED LOGBOOK PANEL -->
                <div class="logbook-sticky">
                    <h4>📋 CATATAN DATA TERUKUR (LOGBOOK)</h4>
                    <div class="logbook-grid">
                        <div class="logbook-item">
                            <b>TITIK A</b><br>🔥 20 ha<br>💧 2.000L
                        </div>
                        <div class="logbook-item">
                            <b>TITIK B</b><br>🔥 35 ha<br>💧 3.000L
                        </div>
                        <div class="logbook-item">
                            <b>TITIK C</b><br>🔥 15 ha<br>💧 1.500L
                        </div>
                    </div>
                </div>

                <!-- Step 1: Comparison -->
                <div id="dc-step-1" class="dc-step">
                    <p style="font-size: 13px; font-weight: bold; text-align: center;">1. Urutkan titik kebakaran dari area TERKECIL ke TERBESAR!</p>
                    <div id="drag-container" style="display: flex; flex-direction: column; gap: 6px; margin: 8px 0;"></div>
                    <button id="btn-submit-step1" class="btn btn-secondary">✅ PERIKSA URUTAN</button>
                </div>

                <!-- Step 2: Total Area -->
                <div id="dc-step-2" class="dc-step" style="display: none;">
                    <p style="font-size: 13px; font-weight: bold; text-align: center;">2. Hitung TOTAL Luas Area Terbakar (ha):</p>
                    <div class="keypad-display" id="keypad-input">_</div>
                    <div class="keypad-grid" style="margin-top: 6px;">
                        <button class="keypad-btn" onclick="app.pressKey('1')">1</button>
                        <button class="keypad-btn" onclick="app.pressKey('2')">2</button>
                        <button class="keypad-btn" onclick="app.pressKey('3')">3</button>
                        <button class="keypad-btn" onclick="app.pressKey('4')">4</button>
                        <button class="keypad-btn" onclick="app.pressKey('5')">5</button>
                        <button class="keypad-btn" onclick="app.pressKey('6')">6</button>
                        <button class="keypad-btn" onclick="app.pressKey('7')">7</button>
                        <button class="keypad-btn" onclick="app.pressKey('8')">8</button>
                        <button class="keypad-btn" onclick="app.pressKey('9')">9</button>
                        <button class="keypad-btn" style="background: #ef4444;" onclick="app.pressKey('clear')">C</button>
                        <button class="keypad-btn" onclick="app.pressKey('0')">0</button>
                        <button class="keypad-btn" style="background: #f59e0b;" onclick="app.pressKey('back')">⌫</button>
                    </div>
                    <button id="btn-submit-step2" class="btn btn-secondary" style="margin-top: 8px;">✅ PERIKSA TOTAL</button>
                </div>

                <!-- Step 3: Bar Chart Builder -->
                <div id="dc-step-3" class="dc-step" style="display: none;">
                    <p style="font-size: 12px; text-align: center; font-weight: bold;">3. Buat Diagram Batang! Geser puncak batang sesuai Luas Area (A=20, B=35, C=15):</p>
                    <div class="chart-area" style="margin: 8px 0;">
                        <div class="chart-bar-wrap">
                            <div class="chart-val" id="val-A">0 ha</div>
                            <div class="chart-bar" id="bar-A"></div>
                            <div class="chart-label">A</div>
                        </div>
                        <div class="chart-bar-wrap">
                            <div class="chart-val" id="val-B">0 ha</div>
                            <div class="chart-bar" id="bar-B"></div>
                            <div class="chart-label">B</div>
                        </div>
                        <div class="chart-bar-wrap">
                            <div class="chart-val" id="val-C">0 ha</div>
                            <div class="chart-bar" id="bar-C"></div>
                            <div class="chart-label">C</div>
                        </div>
                    </div>
                    <button id="btn-submit-step3" class="btn btn-secondary">✅ PERIKSA DIAGRAM</button>
                </div>

                <!-- Step 4: Custom Water Strategy Selection -->
                <div id="dc-step-4" class="dc-step" style="display: none;">
                    <p style="font-size: 12px; text-align: center; font-weight: bold;">
                        4. Stok Air Utamamu: <span style="color: #38bdf8;">💧 5.000 Liter</span>.<br>
                        Pilih titik api mana saja yang mau dipadamkan dulu!
                    </p>

                    <div style="margin: 10px 0;">
                        <label class="water-check-card" id="card-chk-A">
                            <div>
                                <b>🔥 TITIK A</b> <span style="font-size: 11px; color: #94a3b8;">(Luas 20 ha)</span><br>
                                <span style="color: #38bdf8; font-size: 12px;">💧 Kebutuhan Air: 2.000 Liter</span>
                            </div>
                            <input type="checkbox" id="chk-A" onchange="app.updateWaterSelection()">
                        </label>

                        <label class="water-check-card" id="card-chk-B">
                            <div>
                                <b>🔥 TITIK B</b> <span style="font-size: 11px; color: #94a3b8;">(Luas 35 ha)</span><br>
                                <span style="color: #38bdf8; font-size: 12px;">💧 Kebutuhan Air: 3.000 Liter</span>
                            </div>
                            <input type="checkbox" id="chk-B" onchange="app.updateWaterSelection()">
                        </label>

                        <label class="water-check-card" id="card-chk-C">
                            <div>
                                <b>🔥 TITIK C</b> <span style="font-size: 11px; color: #94a3b8;">(Luas 15 ha)</span><br>
                                <span style="color: #38bdf8; font-size: 12px;">💧 Kebutuhan Air: 1.500 Liter</span>
                            </div>
                            <input type="checkbox" id="chk-C" onchange="app.updateWaterSelection()">
                        </
