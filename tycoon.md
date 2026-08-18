<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Semiconductor Fab Pump Tycoon - Advanced Edition</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #0a0e27 0%, #1a1f3a 100%);
            color: #00ff88;
            overflow: hidden;
            height: 100vh;
        }

        .container {
            display: grid;
            grid-template-columns: 1fr 1fr 1fr;
            grid-template-rows: auto 1fr 1fr;
            gap: 8px;
            padding: 8px;
            height: 100vh;
            width: 100%;
        }

        .panel {
            background: linear-gradient(135deg, #1a1f3a 0%, #0f1426 100%);
            border: 2px solid #00ff88;
            border-radius: 8px;
            padding: 12px;
            overflow-y: auto;
            font-size: 12px;
            box-shadow: 0 0 20px rgba(0, 255, 136, 0.2);
        }

        .panel::-webkit-scrollbar {
            width: 6px;
        }

        .panel::-webkit-scrollbar-track {
            background: #0a0e27;
        }

        .panel::-webkit-scrollbar-thumb {
            background: #00ff88;
            border-radius: 3px;
        }

        header {
            grid-column: 1 / -1;
            display: flex;
            justify-content: space-between;
            align-items: center;
            background: #0a0e27;
            border: 2px solid #00ff88;
            border-radius: 8px;
            padding: 12px;
            box-shadow: 0 0 20px rgba(0, 255, 136, 0.3);
        }

        .header-title {
            font-size: 18px;
            font-weight: bold;
            color: #00ff88;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .header-logo {
            height: 30px;
            object-fit: contain;
        }

        .game-stats {
            display: flex;
            gap: 40px;
            font-size: 13px;
        }

        .stat {
            display: flex;
            flex-direction: column;
        }

        .stat-label {
            color: #888;
            font-size: 11px;
            text-transform: uppercase;
        }

        .stat-value {
            color: #00ff88;
            font-weight: bold;
            font-size: 14px;
        }

        .stat-value.warning {
            color: #ffaa00;
        }

        .stat-value.critical {
            color: #ff4444;
        }

        .section-title {
            color: #00ff88;
            font-weight: bold;
            margin-bottom: 8px;
            padding-bottom: 4px;
            border-bottom: 1px solid #00ff88;
            font-size: 13px;
        }

        .subsection {
            margin-bottom: 12px;
        }

        .pump-item {
            background: #0f1426;
            border: 1px solid #00aa66;
            border-radius: 4px;
            padding: 8px;
            margin-bottom: 6px;
            font-size: 11px;
        }

        .pump-item.critical-health {
            border-color: #ff4444;
            background: rgba(255, 68, 68, 0.1);
        }

        .pump-item.warning-health {
            border-color: #ffaa00;
            background: rgba(255, 170, 0, 0.1);
        }

        .pump-bar {
            display: flex;
            gap: 4px;
            margin-top: 4px;
            font-size: 10px;
        }

        .bar {
            flex: 1;
            background: #0a0e27;
            border: 1px solid #00ff88;
            border-radius: 2px;
            height: 16px;
            position: relative;
            overflow: hidden;
        }

        .bar-fill {
            background: linear-gradient(90deg, #00ff88 0%, #00aa66 100%);
            height: 100%;
            transition: width 0.1s;
        }

        .bar-fill.warning {
            background: linear-gradient(90deg, #ffaa00 0%, #ff8800 100%);
        }

        .bar-fill.critical {
            background: linear-gradient(90deg, #ff4444 0%, #cc0000 100%);
        }

        .bar-label {
            position: absolute;
            top: 50%;
            left: 4px;
            transform: translateY(-50%);
            font-size: 9px;
            color: #000;
            font-weight: bold;
            z-index: 1;
        }

        .cassette {
            background: #1a3a1a;
            border: 1px solid #00ff88;
            border-radius: 4px;
            padding: 8px;
            margin-bottom: 6px;
            font-size: 11px;
        }

        .cassette.completed {
            border-color: #00ff88;
            background: rgba(0, 255, 136, 0.1);
        }

        .cassette.failed {
            border-color: #ff4444;
            background: rgba(255, 68, 68, 0.1);
        }

        .progress-bar {
            width: 100%;
            height: 12px;
            background: #0a0e27;
            border: 1px solid #00ff88;
            border-radius: 2px;
            margin-top: 4px;
            overflow: hidden;
        }

        .progress-fill {
            background: linear-gradient(90deg, #00ff88 0%, #00aa66 100%);
            height: 100%;
            transition: width 0.2s;
        }

        .button {
            background: linear-gradient(135deg, #00ff88 0%, #00aa66 100%);
            color: #000;
            border: none;
            padding: 8px 16px;
            border-radius: 4px;
            cursor: pointer;
            font-weight: bold;
            font-size: 11px;
            transition: all 0.2s;
            margin-top: 4px;
            width: 100%;
        }

        .button:hover {
            transform: scale(1.05);
            box-shadow: 0 0 10px rgba(0, 255, 136, 0.5);
        }

        .button:disabled {
            opacity: 0.5;
            cursor: not-allowed;
        }

        .button.danger {
            background: linear-gradient(135deg, #ff4444 0%, #cc0000 100%);
            color: #fff;
        }

        .button.secondary {
            background: linear-gradient(135deg, #666 0%, #444 100%);
            color: #fff;
        }

        .log-entry {
            background: #0f1426;
            border-left: 3px solid #00ff88;
            padding: 6px;
            margin-bottom: 4px;
            font-size: 10px;
            border-radius: 2px;
        }

        .log-entry.warning {
            border-left-color: #ffaa00;
            background: rgba(255, 170, 0, 0.05);
        }

        .log-entry.error {
            border-left-color: #ff4444;
            background: rgba(255, 68, 68, 0.05);
        }

        .log-entry.success {
            border-left-color: #00ff88;
            background: rgba(0, 255, 136, 0.05);
        }

        .log-time {
            color: #666;
            font-size: 9px;
        }

        .modal {
            display: none;
            position: fixed;
            z-index: 1000;
            left: 0;
            top: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.7);
            backdrop-filter: blur(4px);
        }

        .modal.active {
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .modal-content {
            background: linear-gradient(135deg, #1a1f3a 0%, #0f1426 100%);
            border: 2px solid #00ff88;
            border-radius: 8px;
            padding: 20px;
            max-width: 600px;
            width: 90%;
            max-height: 80vh;
            overflow-y: auto;
            box-shadow: 0 0 40px rgba(0, 255, 136, 0.5);
        }

        .modal-title {
            font-size: 16px;
            font-weight: bold;
            color: #00ff88;
            margin-bottom: 12px;
        }

        .equipment-option {
            background: #0f1426;
            border: 1px solid #00aa66;
            border-radius: 4px;
            padding: 12px;
            margin-bottom: 10px;
            cursor: pointer;
            transition: all 0.2s;
        }

        .equipment-option:hover {
            border-color: #00ff88;
            background: rgba(0, 255, 136, 0.1);
        }

        .equipment-name {
            color: #00ff88;
            font-weight: bold;
            font-size: 12px;
        }

        .equipment-specs {
            color: #888;
            font-size: 10px;
            margin-top: 4px;
        }

        .equipment-price {
            color: #ffaa00;
            font-weight: bold;
            margin-top: 4px;
        }

        .vendor-badge {
            display: inline-block;
            background: #00ff88;
            color: #000;
            padding: 2px 6px;
            border-radius: 2px;
            font-size: 9px;
            font-weight: bold;
            margin-right: 4px;
        }

        .vendor-badge.busch {
            background: #4488ff;
            color: #fff;
        }

        .vendor-badge.pfeiffer {
            background: #ff8844;
            color: #fff;
        }

        .vendor-badge.leybold {
            background: #aa44ff;
            color: #fff;
        }

        .contract-item {
            background: #1a3a1a;
            border: 1px solid #00ff88;
            border-radius: 4px;
            padding: 8px;
            margin-bottom: 8px;
            font-size: 10px;
        }

        .contract-customer {
            color: #00ff88;
            font-weight: bold;
            font-size: 11px;
        }

        .contract-terms {
            color: #888;
            margin-top: 4px;
            font-size: 9px;
        }

        .contract-reward {
            color: #ffaa00;
            font-weight: bold;
            margin-top: 4px;
        }

        #game-controls {
            grid-column: 1 / -1;
        }

        #control-buttons {
            display: flex;
            gap: 4px;
            justify-content: space-around;
        }

        #control-buttons button {
            flex: 1;
            padding: 10px;
        }

        .efficiency-indicator {
            display: inline-block;
            width: 8px;
            height: 8px;
            border-radius: 50%;
            margin-right: 4px;
            background: #00ff88;
        }

        .efficiency-indicator.warning {
            background: #ffaa00;
        }

        .efficiency-indicator.critical {
            background: #ff4444;
        }

        /* Layout specific */
        #overview {
            grid-column: 1;
            grid-row: 2;
        }

        #system-status {
            grid-column: 2;
            grid-row: 2;
        }

        #equipment {
            grid-column: 3;
            grid-row: 2;
        }

        #contracts {
            grid-column: 1;
            grid-row: 3;
        }

        #cassettes {
            grid-column: 2;
            grid-row: 3;
        }

        #event-log {
            grid-column: 3;
            grid-row: 3;
        }

        .recipe-selector {
            display: flex;
            gap: 4px;
        }

        .recipe-btn {
            flex: 1;
            padding: 6px;
            font-size: 10px;
        }

        .financial-summary {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 4px;
            font-size: 10px;
            margin-bottom: 8px;
        }

        .financial-item {
            background: #0f1426;
            padding: 4px;
            border-radius: 2px;
            border-left: 2px solid #00ff88;
        }

        .financial-item.negative {
            border-left-color: #ff4444;
        }

        .financial-label {
            color: #888;
            font-size: 9px;
        }

        .financial-amount {
            color: #00ff88;
            font-weight: bold;
        }

        .financial-amount.negative {
            color: #ff4444;
        }

        @keyframes pulse {
            0%, 100% { opacity: 1; }
            50% { opacity: 0.5; }
        }

        .alert {
            animation: pulse 1s infinite;
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <div class="header-title">
                <span>⚙️ SEMICONDUCTOR FAB PUMP TYCOON</span>
            </div>
            <div class="game-stats">
                <div class="stat">
                    <span class="stat-label">Revenue</span>
                    <span class="stat-value" id="revenue">$0</span>
                </div>
                <div class="stat">
                    <span class="stat-label">Balance</span>
                    <span class="stat-value" id="balance">$500,000</span>
                </div>
                <div class="stat">
                    <span class="stat-label">System Temp</span>
                    <span class="stat-value" id="temp-stat">25°C</span>
                </div>
                <div class="stat">
                    <span class="stat-label">Uptime</span>
                    <span class="stat-value" id="uptime">100%</span>
                </div>
                <div class="stat">
                    <span class="stat-label">Month</span>
                    <span class="stat-value" id="month">1</span>
                </div>
            </div>
        </header>

        <div id="overview" class="panel">
            <div class="section-title">📊 System Overview</div>
            <div class="subsection">
                <div style="margin-bottom: 8px;">
                    <strong style="color: #ffaa00;">Chamber Pressure</strong>
                    <div id="chamber-pressure" style="color: #00ff88;">1.0e-6 Torr</div>
                </div>
                                <div style="margin-bottom: 8px;">
                    <strong style="color: #ffaa00;">Chamber Pressure</strong>
                    <div id="chamber-pressure" style="color: #00ff88;">1.0e-6 Torr</div>
                </div>
                <div class="bar">
                    <div class="bar-fill" id="pressure-bar" style="width: 50%;"></div>
                    <div class="bar-label">Pressure</div>
                </div>
            </div>
            <div class="subsection">
                <strong style="color: #ffaa00;">Yield Quality</strong>
                <div class="bar">
                    <div class="bar-fill" id="yield-bar" style="width: 95%;"></div>
                    <div class="bar-label" id="yield-text">95%</div>
                </div>
            </div>
            <div class="subsection">
                <strong style="color: #ffaa00;">Contamination Level</strong>
                <div class="bar">
                    <div class="bar-fill critical" id="contamination-bar" style="width: 10%;"></div>
                    <div class="bar-label" id="contamination-text">10%</div>
                </div>
            </div>
            <div class="subsection">
                <strong style="color: #00ff88;">Maintenance Schedule</strong>
                <div style="font-size: 10px; margin-top: 6px; color: #888;">
                    <div>Oil Changes: <span id="oil-changes-due" style="color: #ffaa00;">15 days</span></div>
                    <div>Filter Changes: <span id="filter-changes-due" style="color: #ffaa00;">8 days</span></div>
                    <div>Calibration: <span id="calibration-due" style="color: #00ff88;">45 days</span></div>
                </div>
            </div>
            <button class="button" onclick="performMaintenance()">🔧 Perform Maintenance ($2,500)</button>
        </div>

        <div id="system-status" class="panel">
            <div class="section-title">⚡ System Status & Vibration</div>
            <div class="subsection">
                <div style="margin-bottom: 4px;">
                    <strong style="color: #ffaa00;">Roughing Stage</strong>
                </div>
                <div id="roughing-pump" class="pump-item">
                    <div>QDP80 Rotary Vane</div>
                    <div class="pump-bar">
                        <div class="bar" style="flex: 2;">
                            <div class="bar-fill" id="rough-health" style="width: 100%;"></div>
                            <div class="bar-label">Health</div>
                        </div>
                        <div class="bar">
                            <div class="bar-fill" id="rough-temp" style="width: 30%;"></div>
                            <div class="bar-label">Temp</div>
                        </div>
                    </div>
                    <div style="margin-top: 4px; font-size: 10px; color: #888;">
                        Speed: <span id="rough-speed">100%</span> | Vib: <span id="rough-vib">12%</span> <span class="efficiency-indicator"></span>
                    </div>
                </div>
            </div>
            <div class="subsection">
                <div style="margin-bottom: 4px;">
                    <strong style="color: #ffaa00;">High Vacuum Stage</strong>
                </div>
                <div id="highvac-pump" class="pump-item">
                    <div>Turbomolecular Pump</div>
                    <div class="pump-bar">
                        <div class="bar" style="flex: 2;">
                            <div class="bar-fill" id="hv-health" style="width: 100%;"></div>
                            <div class="bar-label">Health</div>
                        </div>
                        <div class="bar">
                            <div class="bar-fill" id="hv-temp" style="width: 25%;"></div>
                            <div class="bar-label">Temp</div>
                        </div>
                    </div>
                    <div style="margin-top: 4px; font-size: 10px; color: #888;">
                        Speed: <span id="hv-speed">95%</span> | Vib: <span id="hv-vib">8%</span> <span class="efficiency-indicator"></span>
                    </div>
                </div>
            </div>
            <div class="subsection">
                <div style="margin-bottom: 4px;">
                    <strong style="color: #ffaa00;">Cryo Stage</strong>
                </div>
                <div id="cryo-pump" class="pump-item">
                    <div>Cryogenic Pump</div>
                    <div class="pump-bar">
                        <div class="bar" style="flex: 2;">
                            <div class="bar-fill" id="cryo-health" style="width: 100%;"></div>
                            <div class="bar-label">Health</div>
                        </div>
                        <div class="bar">
                            <div class="bar-fill" id="cryo-temp" style="width: 45%;"></div>
                            <div class="bar-label">Temp</div>
                        </div>
                    </div>
                    <div style="margin-top: 4px; font-size: 10px; color: #888;">
                        Speed: <span id="cryo-speed">88%</span> | Vib: <span id="cryo-vib">5%</span> <span class="efficiency-indicator"></span>
                    </div>
                </div>
            </div>
            <div class="subsection">
                <div style="margin-bottom: 4px;">
                    <strong style="color: #ffaa00;">Cooling System</strong>
                </div>
                <div class="pump-item" style="border-color: #4488ff;">
                    <div id="cooling-status">Chiller Unit (Offline)</div>
                    <div style="margin-top: 4px; font-size: 10px; color: #888;">
                        Capacity: <span id="cooling-capacity">0 kW</span>
                    </div>
                </div>
            </div>
            <button class="button" onclick="upgradeCooling()">❄️ Upgrade Cooling ($35,000)</button>
        </div>

        <div id="equipment" class="panel">
            <div class="section-title">🏭 Equipment & Upgrades</div>
            <div class="subsection">
                <strong style="color: #00ff88; font-size: 11px;">Installed Equipment</strong>
                <div style="font-size: 10px; margin-top: 6px; color: #888;">
                    <div>Roughing: <span style="color: #00ff88;">Edwards QDP80</span></div>
                    <div>High-Vac: <span style="color: #00ff88;">Turbomolecular</span></div>
                    <div>Cryo: <span style="color: #00ff88;">Standard Unit</span></div>
                    <div>Abatement: <span style="color: #ffaa00;">Pending</span></div>
                </div>
            </div>
            <div class="subsection">
                <strong style="color: #ffaa00; font-size: 11px;">Available Upgrades</strong>
                <button class="button" onclick="showEquipmentModal('roughing')">🔄 Upgrade Roughing Pump</button>
                <button class="button" onclick="showEquipmentModal('highvac')">⬆️ Upgrade Turbo Pump</button>
                <button class="button" onclick="showEquipmentModal('cryo')">❄️ Upgrade Cryo Pump</button>
                <button class="button" onclick="showEquipmentModal('abatement')">💨 Install Abatement</button>
            </div>
            <div class="subsection">
                <strong style="color: #00ff88; font-size: 11px;">Monthly Costs</strong>
                <div class="financial-summary">
                    <div class="financial-item">
                        <div class="financial-label">Oil Changes</div>
                        <div class="financial-amount negative" id="cost-oil">-$3,000</div>
                    </div>
                    <div class="financial-item">
                        <div class="financial-label">Filters</div>
                        <div class="financial-amount negative" id="cost-filters">-$1,500</div>
                    </div>
                    <div class="financial-item">
                        <div class="financial-label">Calibration</div>
                        <div class="financial-amount negative" id="cost-calibration">-$2,000</div>
                    </div>
                    <div class="financial-item">
                        <div class="financial-label">Cooling</div>
                        <div class="financial-amount negative" id="cost-cooling">-$0</div>
                    </div>
                </div>
            </div>
        </div>

        <div id="contracts" class="panel">
            <div class="section-title">📋 Contracts & Customers</div>
            <div id="contracts-list"></div>
            <button class="button secondary" onclick="showContractModal()">➕ Find New Contract</button>
        </div>

        <div id="cassettes" class="panel">
            <div class="section-title">📦 Wafer Processing Pipeline</div>
            <div class="subsection">
                <strong style="color: #00ff88; font-size: 11px;">Recipe Selection</strong>
                <div class="recipe-selector">
                    <button class="recipe-btn button secondary" onclick="selectRecipe('al-evap')">Al Evaporation</button>
                    <button class="recipe-btn button secondary" onclick="selectRecipe('oxide-dep')">Oxide Deposition</button>
                </div>
            </div>
            <div id="cassettes-list"></div>
            <button class="button" onclick="startNewCassette()" id="start-cassette">▶️ Start New Cassette</button>
        </div>

        <div id="event-log" class="panel">
            <div class="section-title">📡 Event Log</div>
            <div id="log-entries"></div>
        </div>

        <div id="game-controls" class="panel">
            <div id="control-buttons">
                <button class="button secondary" onclick="gameSpeed(0.5)">⏸️ 0.5x</button>
                <button class="button secondary" onclick="gameSpeed(1)">▶️ 1x (Normal)</button>
                <button class="button secondary" onclick="gameSpeed(2)">⏩ 2x</button>
                <button class="button secondary" onclick="gameSpeed(4)">⏭️ 4x (Fast)</button>
                <button class="button danger" onclick="resetGame()">🔄 Reset Game</button>
            </div>
        </div>
    </div>

    <!-- Equipment Selection Modal -->
    <div id="equipment-modal" class="modal">
        <div class="modal-content">
            <div class="modal-title" id="modal-title">Select Equipment</div>
            <div id="equipment-options"></div>
            <button class="button secondary" onclick="closeModal()" style="margin-top: 12px;">Cancel</button>
        </div>
    </div>

    <!-- Contract Selection Modal -->
    <div id="contract-modal" class="modal">
        <div class="modal-content">
            <div class="modal-title">Available Contracts</div>
            <div id="contract-options"></div>
            <button class="button secondary" onclick="closeModal()" style="margin-top: 12px;">Cancel</button>
        </div>
    </div>

    <script>
        // ==================== GAME STATE ====================
        const gameState = {
            balance: 500000,
            monthlyRevenue: 0,
            month: 1,
            dayOfMonth: 1,
            gameSpeed: 1,
            isRunning: true,

            // Pumps
            roughing: {
                name: 'Edwards QDP80',
                type: 'rotary-vane',
                vendor: 'edwards',
                health: 100,
                maxHealth: 100,
                temperature: 65,
                maxTemp: 100,
                speed: 100,
                vibration: 12,
                baseWearRate: 0.15,
                costPerMonth: 3000,
            },
            highvac: {
                name: 'Turbomolecular Pump',
                type: 'turbo',
                vendor: 'edwards',
                health: 100,
                maxHealth: 100,
                temperature: 58,
                maxTemp: 95,
                speed: 95,
                vibration: 8,
                baseWearRate: 0.10,
                costPerMonth: 5000,
            },
            cryo: {
                name: 'Cryogenic Pump',
                type: 'cryo',
                vendor: 'edwards',
                health: 100,
                maxHealth: 100,
                temperature: 75,
                maxTemp: 110,
                speed: 88,
                vibration: 5,
                baseWearRate: 0.08,
                costPerMonth: 7000,
            },

            // Systems
            cooling: {
                installed: false,
                capacity: 0, // kW
                costPerMonth: 0,
            },
            abatement: {
                installed: false,
                efficiency: 0,
                costPerMonth: 0,
            },

            // Processing
            currentRecipe: 'al-evap',
            cassettes: [
                { id: 1, progress: 0, recipe: null, quality: 100, status: 'idle' },
                { id: 2, progress: 0, recipe: null, quality: 100, status: 'idle' },
                { id: 3, progress: 0, recipe: null, quality: 100, status: 'idle' },
            ],
            cassettesCompleted: 0,
            cassettesFailedThisMonth: 0,

            // Contracts
            contracts: [
                { id: 1, customer: 'TSMC', cassettes: 5, deadline: 480, completed: 0, payPerCassette: 15000, penalty: -7500, active: false, daysLeft: 0 },
                { id: 2, customer: 'Samsung', cassettes: 3, deadline: 360, completed: 0, payPerCassette: 12000, penalty: -6000, active: false, daysLeft: 0 },
            ],

            // Maintenance
            oilChangeDue: 30,
            filterChangeDue: 15,
            calibrationDue: 45,
            maintenancePerformed: false,

            // Pressure targets
            recipes: {
                'al-evap': { targetPressure: 1e-5, duration: 120, contaminationRisk: 0.05, description: 'Aluminum Evaporation' },
                'oxide-dep': { targetPressure: 1e-6, duration: 180, contaminationRisk: 0.08, description: 'Oxide Deposition' },
            },
        };

        const equipment = {
            roughing: [
                { name: 'Edwards QDP80', vendor: 'edwards', specs: 'Class: Rotary Vane, Flow: 80 m³/h', price: 45000, wearMultiplier: 1.0 },
                { name: 'Busch R5 RC', vendor: 'busch', specs: 'Class: Screw, Flow: 100 m³/h, Lower vibration', price: 55000, wearMultiplier: 0.8 },
                { name: 'Leybold TRIVAC D8', vendor: 'leybold', specs: 'Class: Rotary Vane, Flow: 90 m³/h, Enhanced cooling', price: 52000, wearMultiplier: 0.85 },
            ],
            highvac: [
                { name: 'Standard Turbomolecular', vendor: 'edwards', specs: '500 L/s, Compression: 1e9', price: 180000, speedBonus: 0, wearMultiplier: 1.0 },
                { name: 'Pfeiffer Adixen HiPace 700', vendor: 'pfeiffer', specs: '700 L/s, Low vib                { name: 'Pfeiffer Adixen HiPace 700', vendor: 'pfeiffer', specs: '700 L/s, Low vibration', price: 220000, speedBonus: 5, wearMultiplier: 0.9 },
                { name: 'Varian Turbo-V 250', vendor: 'varian', specs: '800 L/s, Premium bearing', price: 240000, speedBonus: 8, wearMultiplier: 0.85 },
            ],
            cryo: [
                { name: 'Standard Cryogenic', vendor: 'edwards', specs: 'Nitrogen cooled, 2-stage', price: 280000, speedBonus: 0, wearMultiplier: 1.0 },
                { name: 'Pfeiffer Coolplex', vendor: 'pfeiffer', specs: 'Advanced cryo, 3-stage, Lower temp', price: 350000, speedBonus: 10, wearMultiplier: 0.8 },
                { name: 'Leybold CRYO-COMP', vendor: 'leybold', specs: 'Oil-free design, Extreme vacuum', price: 400000, speedBonus: 15, wearMultiplier: 0.75 },
            ],
            abatement: [
                { name: 'Basic Scrubber', vendor: 'edwards', specs: 'Single-stage wet scrubber', price: 120000, efficiency: 75 },
                { name: 'Advanced POU', vendor: 'pfeiffer', specs: 'Point-of-use, Dual-stage', price: 180000, efficiency: 90 },
                { name: 'Integrated System', vendor: 'leybold', specs: 'Full abatement suite, Remote monitoring', price: 250000, efficiency: 95 },
            ],
            cooling: [
                { name: 'Basic Chiller (20kW)', vendor: 'generic', specs: 'Industrial water chiller', price: 35000, capacity: 20 },
                { name: 'Premium Chiller (50kW)', vendor: 'generic', specs: 'Dual-circuit, Temperature-controlled', price: 75000, capacity: 50 },
                { name: 'Enterprise System (100kW)', vendor: 'generic', specs: 'Multi-zone, IoT-enabled', price: 150000, capacity: 100 },
            ],
        };

        // ==================== UTILITY FUNCTIONS ====================
        function addLog(message, type = 'info') {
            const logEntries = document.getElementById('log-entries');
            const entry = document.createElement('div');
            entry.className = `log-entry ${type}`;
            const time = `${String(gameState.month).padStart(2, '0')}:${String(gameState.dayOfMonth).padStart(2, '0')}`;
            entry.innerHTML = `<div class="log-time">[${time}]</div><div>${message}</div>`;
            logEntries.insertBefore(entry, logEntries.firstChild);
            if (logEntries.children.length > 20) {
                logEntries.removeChild(logEntries.lastChild);
            }
        }

        function formatMoney(amount) {
            return '$' + Math.floor(amount).toLocaleString();
        }

        function formatPressure(value) {
            return (Math.random() * 0.2 + 0.9) * value; // Simulate realistic jitter
        }

        function getHealthStatus(health) {
            if (health > 75) return { color: '#00ff88', status: 'Good' };
            if (health > 50) return { color: '#ffaa00', status: 'Warning' };
            return { color: '#ff4444', status: 'Critical' };
        }

        function getVibrationStatus(vibration) {
            if (vibration < 40) return 'good';
            if (vibration < 70) return 'warning';
            return 'critical';
        }

        // ==================== EQUIPMENT MODAL ====================
        function showEquipmentModal(type) {
            const modal = document.getElementById('equipment-modal');
            const title = document.getElementById('modal-title');
            const options = document.getElementById('equipment-options');
            
            title.textContent = `Select ${type.toUpperCase()} Equipment`;
            options.innerHTML = '';
            
            equipment[type].forEach(item => {
                const div = document.createElement('div');
                div.className = 'equipment-option';
                div.innerHTML = `
                    <div class="equipment-name">${item.name}</div>
                    <div class="equipment-specs">
                        <span class="vendor-badge ${item.vendor}">${item.vendor.toUpperCase()}</span>
                        ${item.specs}
                    </div>
                    <div class="equipment-price">${formatMoney(item.price)}</div>
                `;
                div.onclick = () => purchaseEquipment(type, item);
                options.appendChild(div);
            });
            
            modal.classList.add('active');
        }

        function closeModal() {
            document.getElementById('equipment-modal').classList.remove('active');
            document.getElementById('contract-modal').classList.remove('active');
        }

        function purchaseEquipment(type, item) {
            if (gameState.balance < item.price) {
                addLog(`❌ Insufficient funds for ${item.name}`, 'error');
                return;
            }

            gameState.balance -= item.price;
            addLog(`✅ Purchased ${item.name} for ${formatMoney(item.price)}`, 'success');

            if (type === 'roughing') {
                gameState.roughing = {
                    name: item.name,
                    vendor: item.vendor,
                    type: 'rotary-vane',
                    health: 100,
                    maxHealth: 100,
                    temperature: 65,
                    maxTemp: 100,
                    speed: 100,
                    vibration: Math.random() * 15 + 5,
                    baseWearRate: 0.15 * item.wearMultiplier,
                    costPerMonth: 3000,
                };
            } else if (type === 'highvac') {
                gameState.highvac = {
                    name: item.name,
                    vendor: item.vendor,
                    type: 'turbo',
                    health: 100,
                    maxHealth: 100,
                    temperature: 58,
                    maxTemp: 95,
                    speed: 95 + (item.speedBonus || 0),
                    vibration: Math.random() * 10 + 3,
                    baseWearRate: 0.10 * item.wearMultiplier,
                    costPerMonth: 5000,
                };
            } else if (type === 'cryo') {
                gameState.cryo = {
                    name: item.name,
                    vendor: item.vendor,
                    type: 'cryo',
                    health: 100,
                    maxHealth: 100,
                    temperature: 75,
                    maxTemp: 110,
                    speed: 88 + (item.speedBonus || 0),
                    vibration: Math.random() * 8 + 2,
                    baseWearRate: 0.08 * item.wearMultiplier,
                    costPerMonth: 7000,
                };
            } else if (type === 'cooling') {
                gameState.cooling = {
                    installed: true,
                    capacity: item.capacity,
                    costPerMonth: item.capacity * 50,
                };
                addLog(`❄️ Cooling capacity increased to ${item.capacity}kW`, 'success');
            } else if (type === 'abatement') {
                gameState.abatement = {
                    installed: true,
                    efficiency: item.efficiency,
                    costPerMonth: 2500,
                };
                addLog(`💨 Abatement system installed: ${item.efficiency}% efficiency`, 'success');
            }

            closeModal();
            updateUI();
        }

        function upgradeCooling() {
            showEquipmentModal('cooling');
        }

        // ==================== CONTRACT SYSTEM ====================
        function showContractModal() {
            const modal = document.getElementById('contract-modal');
            const options = document.getElementById('contract-options');
            options.innerHTML = '';

            gameState.contracts.forEach(contract => {
                if (contract.active) return;
                
                const div = document.createElement('div');
                div.className = 'equipment-option';
                div.innerHTML = `
                    <div class="equipment-name">${contract.customer}</div>
                    <div class="contract-terms">
                        Required: ${contract.cassettes} cassettes<br>
                        Deadline: ${contract.deadline} minutes<br>
                        Pay: ${formatMoney(contract.payPerCassette)} per cassette<br>
                        Penalty: ${formatMoney(contract.penalty)} per late cassette
                    </div>
                    <div class="contract-reward" style="color: #00ff88;">
                        Max Reward: ${formatMoney(contract.payPerCassette * contract.cassettes)}
                    </div>
                `;
                div.onclick = () => acceptContract(contract.id);
                options.appendChild(div);
            });

            modal.classList.add('active');
        }

        function acceptContract(contractId) {
            const contract = gameState.contracts.find(c => c.id === contractId);
            if (contract && !contract.active) {
                contract.active = true;
                contract.completed = 0;
                contract.daysLeft = contract.deadline;
                addLog(`📋 Contract accepted: ${contract.customer} (${contract.cassettes} cassettes)`, 'success');
                closeModal();
                updateUI();
            }
        }

        // ==================== CASSETTE PROCESSING ====================
        function startNewCassette() {
            const idleCassette = gameState.cassettes.find(c => c.status === 'idle');
            if (!idleCassette) {
                addLog('❌ No idle cassette slots available', 'error');
                return;
            }

            const recipe = gameState.recipes[gameState.currentRecipe];
            idleCassette.recipe = gameState.currentRecipe;
            idleCassette.progress = 0;
            idleCassette.quality = 100;
            idleCassette.status = 'processing';

            addLog(`▶️ Started ${recipe.description} (${recipe.duration} min)`, 'info');
            updateUI();
        }

        function selectRecipe(recipeName) {
            gameState.currentRecipe = recipeName;
            addLog(`🔬 Recipe selected: ${gameState.recipes[recipeName].description}`, 'info');
        }

        // ==================== MAINTENANCE ====================
        function performMaintenance() {
            if (gameState.balance < 2500) {
                addLog('❌ Insufficient funds for maintenance', 'error');
                return;
            }

            gameState.balance -= 2500;
            gameState.roughing.health = 100;
            gameState.highvac.health = 100;
            gameState.cryo.health = 100;
            gameState.oilChangeDue = 30;
            gameState.filterChangeDue = 15;
            gameState.calibrationDue = 45;
            gameState.maintenancePerformed = true;

            addLog('🔧 Full system maintenance completed', 'success');
            updateUI();
        }

        // ==================== GAME SIMULATION ====================
        function simulateGameTick() {
            // Temperature simulation
            let baseTemp = 45;
            baseTemp += gameState.roughing.speed * 0.1;
            baseTemp += gameState.highvac.speed * 0.08;
            baseTemp += gameState.cryo.temperature * 0.05;

            if (gameState.cooling.installed) {
                baseTemp -= gameState.cooling.capacity * 0.15;
            }

            const systemTemp = Math.max(25, Math.min(120, baseTemp));

            // Apply temperature to pumps
            gameState.roughing.temperature = Math.min(gameState.roughing.maxTemp, systemTemp + (Math.random() * 10 - 5));
            gameState.highvac.temperature = Math.min(gameState.highvac.maxTemp, systemTemp + (Math.random() * 8 - 4));
            gameState.cryo.temperature = Math.min(gameState.cryo.maxTemp, systemTemp + (Math.random() * 15 - 7));

            // Wear simulation
            [gameState.roughing, gameState.highvac, gameState.cryo].forEach(pump => {
                let wearRate = pump.baseWearRate * (pump.speed / 100);

                // Temperature-based wear (10°C over spec = 40% increase in wear)
                const tempOverSpec = Math.max(0, pump.temperature - (pump.maxTemp - 10));
                wearRate *= (1 + (tempOverSpec * 0.04));

                // Vibration-based wear
                if (pump.vibration > 75) {
                    wearRate *= 1.5;
                    pump.health -= pump.health * wearRate;
                } else if (pump.vibration > 50) {
                    wearRate *= 1.2;
                    pump.health -= pump.health * wearRate;
                } else {
                    pump.health -= pump.health * wearRate;
                }

                pump.health = Math.max(0, pump.health);

                // Vibration changes based on health
                if (pump.health < 50) {
                    pump.vibration += Math.random() * 3;
                } else if (pump.health < 75) {
                    pump.vibration += Math.random() * 1.5;
                }
                pump.vibration = Math.min(100, pump.vibration);
            });

            // Cassette processing
            gameState.cassettes.forEach(cassette => {
                if (cassette.status === 'processing' && cassette.recipe) {
                    const recipe = gameState.recipes[cassette.recipe];

                    // Progress increases with pump speed
                    const avgPumpSpeed = (gameState.roughing.speed + gameState.highvac.speed + gameState.cryo.speed) / 3;
                    cassette.progress += (avgPumpSpeed / 100) * gameState.gameSpeed;

                    // Contamination risk
                    if (Math.random() < recipe.contaminationRisk) {
                        cassette.quality -= Math.random() * 5;
                    }

                    // Failure if pump health too low
                    if (gameState.roughing.health < 20 || gameState.highvac.health < 20) {
                        cassette.status = 'failed';
                        cassette.quality = 0;
                        gameState.cassettesFailedThisMonth++;
                        addLog(`💥 ${cassette.recipe} failed - pump degradation`, 'error');
                    }

                    // Completion
                    if (cassette.progress >= recipe.duration) {
                        cassette.status = 'completed';
                        cassette.progress = recipe.duration;
                        gameState.cassettesCompleted++;
                        const reward = recipe.duration * 100 * (cassette.quality / 100);
                        gameState.monthlyRevenue += reward;
                        gameState.balance += reward;

                        // Contract fulfillment
                        gameState.contracts.forEach(contract => {
                            if (contract.active && contract.completed < contract.cassettes) {
                                contract.completed++;
                                const paymentActual = contract.payPerCassette * (cassette.quality / 100);
                                gameState.balance += paymentActual;
                                gameState.monthlyRevenue += paymentActual;
                                addLog(`✅ Contract: ${contract.customer} (${contract.completed}/${contract.cassettes})`, 'success');

                                if (contract.completed >= contract.cassettes) {
                                    contract.active = false;
                                    addLog(`🎉 Contract completed: ${contract.customer}!`, 'success');
                                }
                            }
                        });

                        addLog(`✅ ${cassette.recipe} completed (Quality: ${cassette.quality.toFixed(0)}%)`, 'success');
                    }
                }
            });

            // Monthly costs
            if (gameState.dayOfMonth === 1) {
                const monthlyCost = gameState.roughing.costPerMonth + gameState.highvac.costPerMonth + gameState.cryo.costPerMonth + gameState.cooling.costPerMonth + gameState.abatement.costPerMonth;
                gameState.balance -= monthlyCost;
                addLog(`💸 Monthly maintenance costs: ${formatMoney(monthlyCost)}`, 'warning');
                gameState.oilChangeDue--;
                gameState.filterChangeDue--;
                gameState.calibrationDue--;
                gameState.month++;
                gameState.dayOfMonth = 1;
                gameState.monthlyRevenue = 0;
                gameState.cassettesFailedThisMonth = 0;

                // Contract deadline tracking
                gameState.contracts.forEach(contract => {
                    if (contract.active) {
                        contract.daysLeft -= 30;
                        if (contract.daysLeft < 0) {
                            const lateCount = contract.cassettes - contract.completed;
                            gameState.balance += lateCount * contract.penalty;
                            contract.active = false;
                            addLog(`❌ Contract failed: ${contract.customer} - Late penalties applied`, 'error');
                        }
                    }
                });
            } else {
                gameState.dayOfMonth++;
            }

            // System shutdown if cooling insufficient
            if (systemTemp > 100 && !gameState.cooling.installed) {
                gameState.roughing.speed = 0;
                gameState.highvac.speed = 0;
                gameState.cryo.speed = 0;
                addLog('🚨 THERMAL SHUTDOWN: Install cooling system!', 'error');
            }

            updateUI();
        }

        // ==================== UI UPDATE ====================
        function updateUI() {
            // Header stats
            document.getElementById('balance').textContent = formatMoney(gameState.balance);
            document.getElementById('revenue').textContent = formatMoney(gameState.monthlyRevenue);
            document.getElementById('month').textContent = gameState.month;

            const avgTemp = (gameState.roughing.temperature + gameState.highvac.temperature + gameState.cryo.temperature) / 3;
            document.getElementById('temp-stat').textContent = avgTemp.toFixed(0) + '°C';
            
            const uptime = Math.round((gameState.roughing.health + gameState.highvac.health + gameState.cryo.health) / 3);
            document.getElementById('uptime').textContent = uptime + '%';

            // System overview
            const chambPressure = 1e-6 * (gameState.highvac.speed / 100) * (gameState.cryo.speed / 100);
            document.getElementById('chamber-pressure').textContent = chambPressure.toExponential(1) + ' Torr';

            const yieldQuality = gameState.cassettes.reduce((acc, c) => acc + (c.status === 'completed' ? c.quality : 0), 0) / Math.max(1, gameState.cassettes.filter(c => c.status === 'completed').length);
            document.getElementById('yield-bar').style.width = Math.min(100, yieldQuality) + '%';
            document.getElementById('yield-text').textContent = yieldQuality.toFixed(0) + '%';

            const contamination = 10 + (100 - gameState.cryo.health) * 0.3;
            document.getElementById('contamination-bar').style.width = Math.min(100, contamination) + '%';
            document.getElementById('contamination-text').textContent = contamination.toFixed(0) + '%';

            document.getElementById('oil-changes-due').textContent = Math.max(0, gameState.oilChangeDue) + ' days';
            document.getElementById('filter-changes-due').textContent = Math.max(0, gameState.filterChangeDue) + ' days';
            document.getElementById('calibration-due').textContent = Math.max(0, gameState.calibrationDue) + ' days';

            // Pump status
            updatePumpUI('roughing', gameState.roughing);
            updatePumpUI('highvac', gameState.highvac);
            updatePumpUI('cryo', gameState.cryo);

            // Cooling
            if (gameState.cooling.installed) {
                document.getElementById('cooling-status').textContent = `Chiller Unit (${gameState.cooling.capacity}kW - Active)`;
                document.getElementById('cooling-capacity').textContent = gameState.cooling.capacity + ' kW';
            } else {
                document.getElementById('cooling-status').textContent = 'Chiller Unit (Offline)';
                document.getElementById('cooling-capacity').textContent = '0 kW';
            }

            // Costs
            document.getElementById('cost-oil').textContent = formatMoney(-gameState.roughing.costPerMonth);
            document.getElementById('cost-filters').textContent = formatMoney(-gameState.highvac.costPerMonth / 2.5);
            document.getElementById('cost-calibration').textContent = formatMoney(-2000);
            document.getElementById('cost-cooling').textContent = formatMoney(-gameState.cooling.costPerMonth);

            // Cassettes
            updateCassettesUI();

            // Contracts
            updateContractsUI();
        }

        function updatePumpUI(type, pump) {
            const element = document.getElementById(type + '-pump');
            const healthStatus = getHealthStatus(pump.health);
            const vibStatus = getVibrationStatus(pump.vibration);

            element.className = 'pump-item';
            if (pump.health < 30) element.classList.add('critical-health');
            else if (pump.health < 60) element.classList.add('warning-health');

            const healthBar = element.querySelector('#' + type + '-health');
            healthBar.style.width = pump.health + '%';
            healthBar.className = 'bar-fill';
            if (pump.health < 30) healthBar.classList.add('critical');
            else if (pump.health < 60) healthBar.classList.add('warning');

            const tempBar = element.querySelector('#' + type + '-temp');
            tempBar.style.width = (pump.temperature / pump.maxTemp) * 100 + '%';
            tempBar.className = 'bar-fill';
            if (pump.temperature > pump.maxTemp - 10) tempBar.classList.add('critical');
            else if (pump.temperature > pump.maxTemp - 20) tempBar.classList.add('warning');

            document.getElementById(type + '-speed').textContent = pump.speed.toFixed(0) + '%';
            document.getElementById(type + '-vib').textContent = pump.vibration.toFixed(0) + '%';

            const vibIndicator = element.querySelector('.efficiency-indicator');
            vibIndicator.className = 'efficiency-indicator';
            if (pump.vibration > 75) vibIndicator.classList.add('critical');
            else if (pump.vibration > 50) vibIndicator.classList.add('warning');
        }

        function updateCassettesUI() {
            const container = document.getElementById('cassettes-list');
            container.innerHTML = '';

            gameState.cassettes.forEach(cassette => {
                const div = document.createElement('div');
                div.className = 'cassette';
                if (cassette.status === 'completed') div.classList.add('completed');
                if (cassette.status === 'failed') div.classList.add('failed');

                let statusText = cassette.status.toUpperCase();
                let recipeText = cassette.recipe ? gameState.recipes[cassette.recipe].description : 'Empty';

                if (cassette.status === 'processing') {
                    const recipe = gameState.recipes[cassette.recipe];
                    const progress = (cassette.progress / recipe.duration) * 100;
                    div.innerHTML = `
                        <div>${recipeText} #${cassette.id}</div>
                        <div style="font-size: 10px; color: #888; margin-top: 2px;">Quality: ${cassette.quality.toFixed(0)}%</div>
                        <div class="progress-bar">
                            <div class="progress-fill" style="width: ${progress}%"></div>
                        </div>
                        <div style="font-size: 10px; color: #888; margin-top: 4px;">${progress.toFixed(0)}% - ${cassette.progress.toFixed(0)}/${recipe.duration} min</div>
                    `;
                } else if (cassette.status === 'completed') {
                    div.innerHTML = `
                        <div>✅ ${recipeText} #${cassette.id}</div>
                        <div style="font-size: 10px; color: #00ff88; margin-top: 4px;">Quality: ${cassette.quality.toFixed(0)}%</div>
                    `;
                } else if (cassette.status === 'failed') {
                    div.innerHTML = `
                        <div>❌ ${recipeText} #${cassette.id}</div>
                        <div style="font-size: 10px; color: #ff4444; margin-top: 4px;">Failed</div>
                    `;
                } else {
                    div.innerHTML = `<div style="color: #888;">Cassette #${cassette.id} (Idle)</div>`;
                }

                container.appendChild(div);
            });
        }

        function updateContractsUI() {
            const container = document.getElementById('contracts-list');
            container.innerHTML = '';

            const activeContracts = gameState.contracts.filter(c => c.active);

            if (activeContracts.length === 0) {
                const div = document.createElement('div');
                div.style.color = '#888';
                div.style.fontSize = '10px';
                div.textContent = 'No active contracts. Find new customers to maximize profit.';
                container.appendChild(div);
                return;
            }

            activeContracts.forEach(contract => {
                const div = document.createElement('div');
                div.className = 'contract-item';
                div.innerHTML = `
                    <div class="contract-customer">${contract.customer}</div>
                    <div class="contract-terms">
                        Progress: ${contract.completed}/${contract.cassettes}<br>
                        Days Left: ${Math.max(0, Math.ceil(contract.daysLeft))}<br>
                        Reward: ${formatMoney(contract.payPerCassette * (contract.cassettes - contract.completed))}
                    </div>
                `;
                container.appendChild(div);
            });
        }

        // ==================== GAME CONTROLS ====================
        function gameSpeed(speed) {
            gameState.gameSpeed = speed;
            addLog(`⏱️ Game speed: ${speed}x`, 'info');
        }

        function resetGame() {
            if (confirm('Reset entire game? This cannot be undone.')) {
                location.reload();
            }
        }

        // ==================== MAIN LOOP ====================
        let lastUpdate = Date.now();
        function gameLoop() {
            const now = Date.now();
            const deltaTime = (now - lastUpdate) / 1000; // seconds
            lastUpdate = now;

            if (gameState.isRunning) {
                // ~1 game tick per 100ms at 1x speed
                for (let i = 0; i < Math.ceil(deltaTime * 10 * gameState.gameSpeed); i++) {
                    simulateGameTick();
                }
            }

            requestAnimationFrame(gameLoop);
        }

        // ==================== INITIALIZATION ====================
        window.addEventListener('load', () => {
            addLog('⚙️ System initialized - Semiconductor Fab Ready', 'success');
            addLog('📋 Starting balance: ' + formatMoney(gameState.balance), 'info');
            addLog('🔬 Select a recipe and start your first cassette!', 'info');
            updateUI();
            gameLoop();
        });
    </script>
</body>
</html>

