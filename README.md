<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Classroom CO₂ Calculator</title>
<link href="https://fonts.googleapis.com/css2?family=DM+Serif+Display:ital@0;1&family=DM+Sans:wght@300;400;500;600&display=swap" rel="stylesheet">
<style>
  :root {
    --teal-deep: #0d3b38;
    --teal-mid: #1a6b63;
    --teal-bright: #2a9d8f;
    --teal-light: #52c4b5;
    --teal-pale: #a8e6df;
    --mint-mist: #e8f8f5;
    --sky-blue: #4fc3c3;
    --leaf-green: #57cc99;
    --amber: #e9c46a;
    --coral: #e76f51;
    --danger: #c1121f;
    --white: #ffffff;
    --glass: rgba(255,255,255,0.72);
    --glass-border: rgba(255,255,255,0.5);
    --shadow: 0 8px 32px rgba(13,59,56,0.12);
    --shadow-lg: 0 20px 60px rgba(13,59,56,0.18);
  }

  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  body {
    font-family: 'DM Sans', sans-serif;
    background: linear-gradient(135deg, #c8f0ea 0%, #b2e4f5 40%, #d4f0e8 100%);
    min-height: 100vh;
    position: relative;
    overflow-x: hidden;
  }

  /* Animated background blobs */
  body::before, body::after {
    content: '';
    position: fixed;
    border-radius: 50%;
    filter: blur(80px);
    opacity: 0.35;
    z-index: 0;
    animation: drift 12s ease-in-out infinite alternate;
  }
  body::before {
    width: 500px; height: 500px;
    background: radial-gradient(circle, #57cc99, #2a9d8f);
    top: -100px; left: -100px;
  }
  body::after {
    width: 600px; height: 600px;
    background: radial-gradient(circle, #4fc3c3, #1a6b63);
    bottom: -150px; right: -150px;
    animation-delay: -6s;
  }

  @keyframes drift {
    0% { transform: translate(0,0) scale(1); }
    100% { transform: translate(40px, 30px) scale(1.08); }
  }

  /* Floating particles */
  .particles { position: fixed; inset: 0; pointer-events: none; z-index: 0; overflow: hidden; }
  .particle {
    position: absolute;
    border-radius: 50%;
    background: rgba(42,157,143,0.18);
    animation: floatUp linear infinite;
  }
  @keyframes floatUp {
    0%   { transform: translateY(0) scale(1); opacity: 0; }
    10%  { opacity: 1; }
    90%  { opacity: 0.6; }
    100% { transform: translateY(-110vh) scale(0.5); opacity: 0; }
  }

  .wrapper {
    position: relative;
    z-index: 1;
    min-height: 100vh;
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 2rem 1rem 3rem;
  }

  /* HEADER */
  header {
    text-align: center;
    margin-bottom: 2rem;
    animation: fadeDown 0.7s ease both;
  }
  @keyframes fadeDown {
    from { opacity: 0; transform: translateY(-20px); }
    to   { opacity: 1; transform: translateY(0); }
  }

  .logo-icon {
    font-size: 3rem;
    display: block;
    margin-bottom: 0.4rem;
    animation: breathe 4s ease-in-out infinite;
  }
  @keyframes breathe {
    0%,100% { transform: scale(1); }
    50% { transform: scale(1.08); }
  }

  h1 {
    font-family: 'DM Serif Display', serif;
    font-size: clamp(1.8rem, 5vw, 2.8rem);
    color: var(--teal-deep);
    letter-spacing: -0.02em;
    line-height: 1.1;
    margin-bottom: 0.3rem;
  }
  h1 span { color: var(--teal-bright); font-style: italic; }

  .subtitle {
    color: var(--teal-mid);
    font-size: 0.9rem;
    font-weight: 400;
    letter-spacing: 0.04em;
    opacity: 0.8;
  }

  /* CARD */
  .card {
    background: var(--glass);
    backdrop-filter: blur(20px);
    -webkit-backdrop-filter: blur(20px);
    border: 1.5px solid var(--glass-border);
    border-radius: 28px;
    box-shadow: var(--shadow-lg);
    width: 100%;
    max-width: 520px;
    padding: 2rem 1.75rem;
    animation: fadeUp 0.7s 0.15s ease both;
  }
  @keyframes fadeUp {
    from { opacity: 0; transform: translateY(24px); }
    to   { opacity: 1; transform: translateY(0); }
  }

  .section-label {
    font-size: 0.7rem;
    font-weight: 600;
    letter-spacing: 0.14em;
    text-transform: uppercase;
    color: var(--teal-bright);
    margin-bottom: 1.2rem;
    display: flex;
    align-items: center;
    gap: 0.5rem;
  }
  .section-label::after {
    content: '';
    flex: 1;
    height: 1px;
    background: linear-gradient(90deg, var(--teal-pale), transparent);
  }

  /* INPUTS */
  .input-group {
    margin-bottom: 1.25rem;
  }

  .input-label {
    display: flex;
    justify-content: space-between;
    align-items: baseline;
    margin-bottom: 0.45rem;
  }

  .input-name {
    font-size: 0.95rem;
    font-weight: 600;
    color: var(--teal-deep);
  }
  .input-name .var {
    font-family: 'DM Serif Display', serif;
    font-style: italic;
    color: var(--teal-bright);
    font-size: 1.05rem;
  }

  .input-unit {
    font-size: 0.75rem;
    color: var(--teal-mid);
    opacity: 0.8;
  }

  .input-wrap {
    position: relative;
  }

  input[type="number"], input[type="range"] {
    -webkit-appearance: none;
    appearance: none;
  }

  .num-input {
    width: 100%;
    padding: 0.7rem 1rem;
    border: 2px solid rgba(42,157,143,0.3);
    border-radius: 14px;
    background: rgba(255,255,255,0.7);
    font-family: 'DM Sans', sans-serif;
    font-size: 1.1rem;
    font-weight: 500;
    color: var(--teal-deep);
    outline: none;
    transition: border-color 0.2s, box-shadow 0.2s, background 0.2s;
  }
  .num-input:focus {
    border-color: var(--teal-bright);
    background: rgba(255,255,255,0.95);
    box-shadow: 0 0 0 4px rgba(42,157,143,0.12);
  }
  .num-input::placeholder { color: rgba(13,59,56,0.3); }

  /* Slider */
  .slider-row {
    display: flex;
    align-items: center;
    gap: 0.75rem;
    margin-top: 0.5rem;
  }

  input[type="range"] {
    flex: 1;
    height: 6px;
    border-radius: 3px;
    background: linear-gradient(90deg, var(--teal-bright) var(--val, 30%), rgba(42,157,143,0.2) var(--val, 30%));
    cursor: pointer;
    outline: none;
    border: none;
  }
  input[type="range"]::-webkit-slider-thumb {
    -webkit-appearance: none;
    width: 22px; height: 22px;
    border-radius: 50%;
    background: var(--white);
    border: 3px solid var(--teal-bright);
    box-shadow: 0 2px 8px rgba(42,157,143,0.3);
    cursor: pointer;
    transition: transform 0.15s;
  }
  input[type="range"]::-webkit-slider-thumb:hover { transform: scale(1.2); }
  input[type="range"]::-moz-range-thumb {
    width: 22px; height: 22px;
    border-radius: 50%;
    background: var(--white);
    border: 3px solid var(--teal-bright);
    box-shadow: 0 2px 8px rgba(42,157,143,0.3);
    cursor: pointer;
  }

  .slider-val {
    font-size: 0.85rem;
    font-weight: 600;
    color: var(--teal-mid);
    min-width: 2.5rem;
    text-align: right;
  }

  /* Assumptions box */
  .assumptions {
    background: rgba(42,157,143,0.08);
    border: 1px solid rgba(42,157,143,0.2);
    border-radius: 14px;
    padding: 0.85rem 1rem;
    margin: 1.25rem 0;
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 0.4rem 1rem;
  }
  .assum-item {
    font-size: 0.78rem;
    color: var(--teal-mid);
    display: flex;
    align-items: center;
    gap: 0.35rem;
  }
  .assum-item::before {
    content: '·';
    color: var(--teal-bright);
    font-size: 1.2rem;
    line-height: 1;
  }

  /* CALCULATE BUTTON */
  .calc-btn {
    width: 100%;
    padding: 1rem;
    border: none;
    border-radius: 16px;
    background: linear-gradient(135deg, var(--teal-bright), var(--teal-mid));
    color: white;
    font-family: 'DM Sans', sans-serif;
    font-size: 1.05rem;
    font-weight: 600;
    letter-spacing: 0.02em;
    cursor: pointer;
    margin-top: 0.5rem;
    position: relative;
    overflow: hidden;
    transition: transform 0.15s, box-shadow 0.15s;
    box-shadow: 0 6px 20px rgba(42,157,143,0.35);
  }
  .calc-btn::before {
    content: '';
    position: absolute;
    inset: 0;
    background: linear-gradient(135deg, rgba(255,255,255,0.15), transparent);
  }
  .calc-btn:hover { transform: translateY(-2px); box-shadow: 0 10px 28px rgba(42,157,143,0.45); }
  .calc-btn:active { transform: translateY(0); }
  .calc-btn .btn-icon { margin-right: 0.5rem; }

  /* RESULTS */
  .results {
    margin-top: 1.5rem;
    animation: fadeUp 0.5s ease both;
    display: none;
  }
  .results.visible { display: block; }

  .co2-display {
    text-align: center;
    padding: 1.5rem 1rem;
    border-radius: 20px;
    margin-bottom: 1rem;
    position: relative;
    overflow: hidden;
    transition: background 0.4s, border-color 0.4s;
    border: 2px solid transparent;
  }
  .co2-display::before {
    content: '';
    position: absolute;
    inset: 0;
    opacity: 0.08;
    border-radius: inherit;
  }

  .co2-val {
    font-family: 'DM Serif Display', serif;
    font-size: clamp(3rem, 12vw, 4.5rem);
    line-height: 1;
    margin-bottom: 0.1rem;
    transition: color 0.4s;
  }
  .co2-unit {
    font-size: 0.85rem;
    font-weight: 500;
    letter-spacing: 0.08em;
    margin-bottom: 0.75rem;
    opacity: 0.8;
  }
  .co2-badge {
    display: inline-block;
    padding: 0.3rem 1.1rem;
    border-radius: 30px;
    font-size: 0.8rem;
    font-weight: 700;
    letter-spacing: 0.1em;
    text-transform: uppercase;
    color: white;
    transition: background 0.4s;
  }

  /* Status levels */
  .level-good    { background: rgba(87,204,153,0.12); border-color: rgba(87,204,153,0.4); }
  .level-good .co2-val, .level-good .co2-unit { color: #1a6b4a; }
  .level-good .co2-badge { background: #57cc99; }

  .level-elevated { background: rgba(233,196,106,0.12); border-color: rgba(233,196,106,0.4); }
  .level-elevated .co2-val, .level-elevated .co2-unit { color: #8a6000; }
  .level-elevated .co2-badge { background: #e9a825; }

  .level-high    { background: rgba(231,111,81,0.12); border-color: rgba(231,111,81,0.4); }
  .level-high .co2-val, .level-high .co2-unit { color: #8a2e10; }
  .level-high .co2-badge { background: #e76f51; }

  .level-hazardous { background: rgba(193,18,31,0.12); border-color: rgba(193,18,31,0.4); }
  .level-hazardous .co2-val, .level-hazardous .co2-unit { color: #7a0010; }
  .level-hazardous .co2-badge { background: #c1121f; }

  /* Gauge bar */
  .gauge-wrap {
    margin-bottom: 1.25rem;
  }
  .gauge-bar {
    height: 10px;
    border-radius: 5px;
    background: linear-gradient(90deg, #57cc99 0%, #e9c46a 50%, #e76f51 75%, #c1121f 100%);
    position: relative;
    margin-bottom: 0.3rem;
    box-shadow: inset 0 1px 3px rgba(0,0,0,0.1);
  }
  .gauge-marker {
    position: absolute;
    top: 50%;
    transform: translate(-50%, -50%);
    width: 18px; height: 18px;
    border-radius: 50%;
    background: white;
    border: 3px solid var(--teal-deep);
    box-shadow: 0 2px 6px rgba(0,0,0,0.2);
    transition: left 0.6s cubic-bezier(0.34,1.56,0.64,1);
  }
  .gauge-labels {
    display: flex;
    justify-content: space-between;
    font-size: 0.68rem;
    color: var(--teal-mid);
    opacity: 0.75;
  }

  /* Stats row */
  .stats-row {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 0.65rem;
    margin-bottom: 1rem;
  }
  .stat-box {
    background: rgba(255,255,255,0.6);
    border: 1px solid rgba(42,157,143,0.2);
    border-radius: 14px;
    padding: 0.75rem 0.5rem;
    text-align: center;
  }
  .stat-label {
    font-size: 0.68rem;
    font-weight: 600;
    letter-spacing: 0.08em;
    text-transform: uppercase;
    color: var(--teal-mid);
    margin-bottom: 0.25rem;
    opacity: 0.8;
  }
  .stat-value {
    font-family: 'DM Serif Display', serif;
    font-size: 1.25rem;
    color: var(--teal-deep);
    line-height: 1;
  }
  .stat-sub {
    font-size: 0.65rem;
    color: var(--teal-mid);
    opacity: 0.7;
    margin-top: 0.15rem;
  }

  /* Recommendation */
  .recommendation {
    background: rgba(255,255,255,0.55);
    border: 1px solid rgba(42,157,143,0.2);
    border-radius: 14px;
    padding: 0.9rem 1rem;
    font-size: 0.85rem;
    color: var(--teal-deep);
    line-height: 1.5;
  }
  .recommendation strong { color: var(--teal-bright); }

  /* Formula footer */
  .formula-footer {
    margin-top: 1.5rem;
    text-align: center;
    font-size: 0.72rem;
    color: var(--teal-mid);
    opacity: 0.7;
    line-height: 1.6;
    padding: 0 0.5rem;
  }

  /* Error */
  .error-msg {
    background: rgba(193,18,31,0.08);
    border: 1px solid rgba(193,18,31,0.25);
    border-radius: 12px;
    padding: 0.75rem 1rem;
    color: #7a0010;
    font-size: 0.88rem;
    text-align: center;
    margin-top: 1rem;
    display: none;
  }
  .error-msg.visible { display: block; }

  /* Footer credit */
  footer {
    margin-top: 2rem;
    text-align: center;
    font-size: 0.75rem;
    color: var(--teal-mid);
    opacity: 0.6;
    animation: fadeUp 0.7s 0.3s ease both;
  }

  /* ── Q MODE TOGGLE ─────────────────────────────── */
  .q-mode-tabs {
    display: flex;
    background: rgba(42,157,143,0.1);
    border-radius: 14px;
    padding: 4px;
    gap: 3px;
    margin-bottom: 0.9rem;
    flex-wrap: wrap;
  }
  .q-tab {
    flex: 1;
    min-width: 0;
    padding: 0.42rem 0.5rem;
    border: none;
    border-radius: 10px;
    font-family: 'DM Sans', sans-serif;
    font-size: 0.78rem;
    font-weight: 600;
    cursor: pointer;
    background: transparent;
    color: var(--teal-mid);
    transition: background 0.2s, color 0.2s, box-shadow 0.2s;
    white-space: nowrap;
    text-align: center;
  }
  .q-tab.active {
    background: var(--white);
    color: var(--teal-deep);
    box-shadow: 0 2px 8px rgba(13,59,56,0.12);
  }
  .q-tab-label { display: block; font-size: 0.65rem; font-weight: 400; opacity: 0.75; margin-top: 1px; }

  /* Don't-know toggle */
  .dontknow-toggle {
    display: flex;
    align-items: center;
    gap: 0.55rem;
    margin-bottom: 0.85rem;
    cursor: pointer;
    user-select: none;
  }
  .dontknow-toggle input[type="checkbox"] { display: none; }
  .toggle-track {
    width: 40px; height: 22px;
    border-radius: 11px;
    background: rgba(42,157,143,0.25);
    position: relative;
    flex-shrink: 0;
    transition: background 0.25s;
  }
  .toggle-track::after {
    content: '';
    position: absolute;
    top: 3px; left: 3px;
    width: 16px; height: 16px;
    border-radius: 50%;
    background: white;
    box-shadow: 0 1px 4px rgba(0,0,0,0.2);
    transition: left 0.25s;
  }
  .dontknow-toggle input:checked + .toggle-track { background: var(--teal-bright); }
  .dontknow-toggle input:checked + .toggle-track::after { left: 21px; }
  .toggle-label { font-size: 0.83rem; color: var(--teal-mid); font-weight: 500; }

  /* Don't-know panel */
  .dontknow-panel {
    display: none;
    background: rgba(42,157,143,0.07);
    border: 1.5px dashed rgba(42,157,143,0.35);
    border-radius: 16px;
    padding: 1rem 1rem 0.85rem;
    margin-bottom: 0.9rem;
    animation: fadeUp 0.3s ease both;
  }
  .dontknow-panel.visible { display: block; }

  .dontknow-info {
    font-size: 0.78rem;
    color: var(--teal-mid);
    margin-bottom: 0.7rem;
    line-height: 1.5;
  }
  .dontknow-info strong { color: var(--teal-bright); }

  /* Extra field needed for don't-know (floor area) */
  .floor-field { margin-bottom: 0.6rem; }
  .floor-field label {
    font-size: 0.8rem;
    font-weight: 600;
    color: var(--teal-deep);
    display: block;
    margin-bottom: 0.3rem;
  }
  .floor-field .input-row { display: flex; gap: 0.5rem; align-items: center; }
  .floor-field select {
    padding: 0.55rem 0.6rem;
    border: 2px solid rgba(42,157,143,0.3);
    border-radius: 10px;
    background: rgba(255,255,255,0.8);
    font-family: 'DM Sans', sans-serif;
    font-size: 0.85rem;
    color: var(--teal-deep);
    outline: none;
    cursor: pointer;
  }
  .ashrae-result {
    margin-top: 0.65rem;
    background: rgba(87,204,153,0.12);
    border: 1px solid rgba(87,204,153,0.4);
    border-radius: 10px;
    padding: 0.55rem 0.75rem;
    font-size: 0.8rem;
    color: #1a6b4a;
    display: none;
  }
  .ashrae-result.visible { display: block; }
  .ashrae-result strong { font-size: 1rem; }

  /* Q input panel (known Q) */
  .q-known-panel { }
  .q-known-panel.hidden { display: none; }

  /* Converted Q note */
  .q-converted {
    font-size: 0.75rem;
    color: var(--teal-mid);
    margin-top: 0.3rem;
    opacity: 0.8;
    min-height: 1.1em;
  }

  @media (max-width: 400px) {
    .card { padding: 1.5rem 1.25rem; }
    .assumptions { grid-template-columns: 1fr; }
    .q-tab { font-size: 0.72rem; padding: 0.38rem 0.35rem; }
  }
</style>
</head>
<body>

<div class="particles" id="particles"></div>

<div class="wrapper">
  <header>
    <span class="logo-icon"></span> <!-- 🌿 -->
    <h1>Classroom <span>CO₂</span> Calculator</h1>
    <p class="subtitle">Steady-State Mass Balance Model</p>
  </header>

  <div class="card">
    <div class="section-label">Input Variables</div>

    <!-- Q -->
    <div class="input-group">
      <div class="input-label" style="margin-bottom:0.6rem;">
        <span class="input-name"><span class="var">Q</span> — Ventilation Rate</span>
      </div>

      <!-- Unit tabs -->
      <div class="q-mode-tabs">
        <button class="q-tab active" data-mode="m3hr" onclick="setQMode('m3hr')">
          m³/hr<span class="q-tab-label">cubic meters/hour</span>
        </button>
        <button class="q-tab" data-mode="cfm" onclick="setQMode('cfm')">
          CFM<span class="q-tab-label">cubic feet/min</span>
        </button>
        <button class="q-tab" data-mode="ach" onclick="setQMode('ach')">
          ACH<span class="q-tab-label">air changes/hour</span>
        </button>
      </div>

      <!-- Don't-know toggle -->
      <label class="dontknow-toggle">
        <input type="checkbox" id="dontknow-chk" onchange="toggleDontKnow()">
        <span class="toggle-track"></span>
        <span class="toggle-label">I don't know my ventilation rate — use ASHRAE default</span>
      </label>

      <!-- ASHRAE default panel -->
      <div class="dontknow-panel" id="dontknow-panel">
        <div class="dontknow-info">
          ASHRAE 62.1 sets the minimum at <strong>15 CFM per person</strong> OR
          <strong>0.15 CFM per ft²</strong> — whichever is greater.
          Enter your floor area and the calculator picks the correct value automatically.
        </div>
        <div class="floor-field">
          <label>Floor Area</label>
          <div class="input-row">
            <input class="num-input" type="number" id="floor-area" placeholder="e.g. 900" min="1" style="flex:1" oninput="updateAshrae()">
            <select id="floor-unit" onchange="updateAshrae()">
              <option value="ft2">ft²</option>
              <option value="m2">m²</option>
            </select>
          </div>
        </div>
        <div class="ashrae-result" id="ashrae-result"></div>
      </div>

      <!-- Known-Q input -->
      <div class="q-known-panel" id="q-known-panel">
        <div class="input-wrap">
          <input class="num-input" type="number" id="Q-raw" value="2180" min="1" placeholder="e.g. 2180" oninput="syncQSlider()">
        </div>
        <div class="slider-row">
          <input type="range" id="Q-slider" min="100" max="8000" value="2180" step="10" oninput="syncQInput()">
          <span class="slider-val" id="Q-display">2180</span>
        </div>
        <div class="q-converted" id="q-converted"></div>
      </div>
    </div>

    <!-- V -->
    <div class="input-group">
      <div class="input-label">
        <span class="input-name"><span class="var">V</span> — Classroom Volume</span>
        <span class="input-unit">m³</span>
      </div>
      <div class="input-wrap">
        <input class="num-input" type="number" id="V" value="545" min="1" placeholder="e.g. 545">
      </div>
      <div class="slider-row">
        <input type="range" id="V-slider" min="50" max="2000" value="545" step="5">
        <span class="slider-val" id="V-display">545</span>
      </div>
    </div>

    <!-- Occupancy -->
    <div class="input-group">
      <div class="input-label">
        <span class="input-name">Room Occupancy</span>
        <span class="input-unit">people</span>
      </div>
      <div class="input-wrap">
        <input class="num-input" type="number" id="occ" value="30" min="1" max="300" placeholder="e.g. 30">
      </div>
      <div class="slider-row">
        <input type="range" id="occ-slider" min="1" max="150" value="30" step="1">
        <span class="slider-val" id="occ-display">30</span>
      </div>
    </div>

    <!-- Assumptions -->
    <div class="assumptions">
      <div class="assum-item">12 breaths/min</div>
      <div class="assum-item">0.45 L/exhale</div>
      <div class="assum-item">4.5% CO₂ exhaled</div>
      <div class="assum-item">420 ppm outdoor</div>
    </div>

    <button class="calc-btn" onclick="calculate()">
      <span class="btn-icon">🔬</span> Calculate Indoor CO₂
    </button>

    <div class="error-msg" id="error-msg">⚠️ Please enter valid positive numbers for all fields.</div>

    <!-- Results -->
    <div class="results" id="results">
      <div class="section-label" style="margin-top:1rem;">Results</div>

      <div class="co2-display" id="co2-display">
        <div class="co2-val" id="co2-val">—</div>
        <div class="co2-unit">PPM — Indoor CO₂ Concentration</div>
        <div class="co2-badge" id="co2-badge">—</div>
      </div>

      <div class="gauge-wrap">
        <div class="gauge-bar">
          <div class="gauge-marker" id="gauge-marker" style="left: 0%"></div>
        </div>
        <div class="gauge-labels">
          <span>400</span><span>Good &lt;1000</span><span>Elevated</span><span>High</span><span>2500+ ppm</span>
        </div>
      </div>

      <div class="stats-row">
        <div class="stat-box">
          <div class="stat-label">ACH</div>
          <div class="stat-value" id="stat-ach">—</div>
          <div class="stat-sub">air changes/hr</div>
        </div>
        <div class="stat-box">
          <div class="stat-label">Emission Rate</div>
          <div class="stat-value" id="stat-e">—</div>
          <div class="stat-sub">ppm·m³/hr</div>
        </div>
        <div class="stat-box">
          <div class="stat-label">ΔCO₂ Above</div>
          <div class="stat-value" id="stat-delta">—</div>
          <div class="stat-sub">ppm over outdoor</div>
        </div>
      </div>

      <div class="recommendation" id="recommendation"></div>
    </div>
  </div>

  <div class="formula-footer">
    <strong>Formula:</strong> C<sub>indoor</sub> = C<sub>outdoor</sub> + (E / Q) × 10⁶<br>
    E = occupants × 12 × 0.45 × 60 × 0.045 / 1000 &nbsp;(m³ CO₂/hr)<br>
    Based on ASHRAE Steady-State Mass Balance · Outdoor CO₂ = 420 ppm
  </div>

  <footer>Built for educators · Based on Steady-State Mass Balance Excel model</footer>
</div>

<script>
  // ─── Floating particles ──────────────────────────────────────────
  (function spawnParticles() {
    const container = document.getElementById('particles');
    for (let i = 0; i < 30; i++) {
      const el = document.createElement('div');
      el.className = 'particle';
      const size = Math.random() * 14 + 4;
      el.style.cssText = `width:${size}px;height:${size}px;left:${Math.random()*100}%;bottom:-${size}px;animation-duration:${Math.random()*14+8}s;animation-delay:${Math.random()*12}s;opacity:${Math.random()*0.4+0.1};`;
      container.appendChild(el);
    }
  })();

  // ─── Slider sync helpers ─────────────────────────────────────────
  function updateSliderBg(sld) {
    const pct = ((+sld.value - +sld.min) / (+sld.max - +sld.min)) * 100;
    sld.style.setProperty('--val', pct + '%');
  }
  function syncSlider(inputId, sliderId, displayId) {
    const inp = document.getElementById(inputId);
    const sld = document.getElementById(sliderId);
    const dsp = document.getElementById(displayId);
    inp.addEventListener('input', () => { sld.value = inp.value; dsp.textContent = inp.value; updateSliderBg(sld); });
    sld.addEventListener('input', () => { inp.value = sld.value; dsp.textContent = sld.value; updateSliderBg(sld); });
    updateSliderBg(sld);
  }
  syncSlider('V', 'V-slider', 'V-display');
  syncSlider('occ', 'occ-slider', 'occ-display');

  // ─── Q MODE STATE ────────────────────────────────────────────────
  let qMode = 'm3hr'; // 'm3hr' | 'cfm' | 'ach'
  const sliderCfg = {
    m3hr: { min: 50,  max: 15000, step: 10,  def: 2180  },
    cfm:  { min: 50,  max: 8800,  step: 5,   def: 1285  },
    ach:  { min: 0.1, max: 20,    step: 0.1, def: 4     },
  };

  function setQMode(mode) {
    qMode = mode;
    // Update tab styles
    document.querySelectorAll('.q-tab').forEach(t => t.classList.toggle('active', t.dataset.mode === mode));
    // Update slider range
    const cfg = sliderCfg[mode];
    const sld = document.getElementById('Q-slider');
    sld.min = cfg.min; sld.max = cfg.max; sld.step = cfg.step;
    // Set sensible default value if field is empty or first switch
    const inp = document.getElementById('Q-raw');
    inp.value = cfg.def;
    sld.value = cfg.def;
    document.getElementById('Q-display').textContent = cfg.def;
    updateSliderBg(sld);
    updateQConvertedNote();
  }

  function syncQSlider() {
    const inp = document.getElementById('Q-raw');
    const sld = document.getElementById('Q-slider');
    sld.value = inp.value;
    document.getElementById('Q-display').textContent = inp.value;
    updateSliderBg(sld);
    updateQConvertedNote();
  }
  function syncQInput() {
    const sld = document.getElementById('Q-slider');
    const inp = document.getElementById('Q-raw');
    inp.value = sld.value;
    document.getElementById('Q-display').textContent = sld.value;
    updateSliderBg(sld);
    updateQConvertedNote();
  }

  // Show equivalent in other units under the Q input
  function updateQConvertedNote() {
    const raw = parseFloat(document.getElementById('Q-raw').value);
    if (!raw || isNaN(raw)) { document.getElementById('q-converted').textContent = ''; return; }
    const q_m3hr = toM3Hr(raw, qMode);
    const q_cfm  = q_m3hr / 0.028316847 / 60;
    let parts = [];
    if (qMode !== 'm3hr') parts.push(q_m3hr.toFixed(1) + ' m³/hr');
    if (qMode !== 'cfm')  parts.push(q_cfm.toFixed(1) + ' CFM');
    // ACH shown in results always
    document.getElementById('q-converted').textContent = parts.length ? '≈ ' + parts.join('  ·  ') : '';
  }

  // Convert raw Q input → m³/hr (all calcs use m³/hr internally)
  function toM3Hr(val, mode, V_m3) {
    if (mode === 'm3hr') return val;
    if (mode === 'cfm')  return val * 0.028316847 * 60; // CFM → m³/hr
    if (mode === 'ach')  return val * (V_m3 || 1);      // ACH × Volume
    return val;
  }

  // ─── DON'T-KNOW Q TOGGLE ────────────────────────────────────────
  function toggleDontKnow() {
    const checked = document.getElementById('dontknow-chk').checked;
    document.getElementById('dontknow-panel').classList.toggle('visible', checked);
    document.getElementById('q-known-panel').classList.toggle('hidden', checked);
    if (checked) updateAshrae();
  }

  function updateAshrae() {
    const area = parseFloat(document.getElementById('floor-area').value);
    const unit = document.getElementById('floor-unit').value;
    const occ  = parseFloat(document.getElementById('occ').value) || 1;
    const el   = document.getElementById('ashrae-result');
    if (!area || isNaN(area)) { el.classList.remove('visible'); return; }

    // Convert area to ft² for ASHRAE calc
    const area_ft2 = unit === 'm2' ? area * 10.7639 : area;

    const cfm_per_person = 15 * occ;          // 15 CFM × occupants
    const cfm_per_ft2    = 0.15 * area_ft2;   // 0.15 CFM × ft²
    const Q_cfm = Math.max(cfm_per_person, cfm_per_ft2);
    const Q_m3hr = Q_cfm * 0.028316847 * 60;
    const winner = cfm_per_person >= cfm_per_ft2 ? '15 CFM/person' : '0.15 CFM/ft²';

    el.innerHTML = `
      <strong>${Q_cfm.toFixed(1)} CFM</strong> &nbsp;·&nbsp; ${Q_m3hr.toFixed(1)} m³/hr<br>
      <span style="font-size:0.72rem;opacity:0.8">Governed by ${winner} rule · Used directly in calculation</span>`;
    el.classList.add('visible');

    // Store for calculate()
    el.dataset.q_m3hr = Q_m3hr;
  }

  // Init slider bg on load
  updateSliderBg(document.getElementById('Q-slider'));

  // ─── Constants ──────────────────────────────────────────────────
  const BREATHS = 12, L_EXHALE = 0.45, CO2_FRAC = 0.045, MIN_HR = 60, C_OUT = 420;

  // ─── Calculate ──────────────────────────────────────────────────
  function calculate() {
    const err = document.getElementById('error-msg');
    const res = document.getElementById('results');
    const V   = parseFloat(document.getElementById('V').value);
    const occ = parseFloat(document.getElementById('occ').value);

    // Get Q in m³/hr
    let Q_m3hr;
    const dontknow = document.getElementById('dontknow-chk').checked;
    if (dontknow) {
      const ar = document.getElementById('ashrae-result');
      Q_m3hr = parseFloat(ar.dataset.q_m3hr);
      if (!Q_m3hr || isNaN(Q_m3hr)) {
        err.textContent = 'Please enter a floor area to use the ASHRAE default.';
        err.classList.add('visible'); res.classList.remove('visible'); return;
      }
    } else {
      const raw = parseFloat(document.getElementById('Q-raw').value);
      if (!raw || isNaN(raw) || raw <= 0) {
        err.textContent = 'Please enter a valid ventilation rate Q.';
        err.classList.add('visible'); res.classList.remove('visible'); return;
      }
      Q_m3hr = toM3Hr(raw, qMode, V);
    }

    if (!V || !occ || V <= 0 || occ <= 0 || isNaN(V) || isNaN(occ)) {
      err.textContent = 'Please enter valid positive numbers for all fields.';
      err.classList.add('visible'); res.classList.remove('visible'); return;
    }
    err.classList.remove('visible');

    // Core mass-balance calculation
    const E_m3   = (occ * BREATHS * L_EXHALE * MIN_HR * CO2_FRAC) / 1000; // m³ CO₂/hr
    const E_ppm  = E_m3 * 1e6;
    const ACH    = Q_m3hr / V;
    const Q_cfm  = Q_m3hr / (0.028316847 * 60);
    const C_indoor = C_OUT + (E_m3 / Q_m3hr) * 1e6;
    const delta    = C_indoor - C_OUT;

    // Level
    let level, cls, rec;
    if (C_indoor < 1000) {
      level = 'Good'; cls = 'level-good';
      rec = `<strong> Air quality is good.</strong> CO₂ levels are within healthy range. Ventilation is adequate for the current occupancy.`;
    } else if (C_indoor < 1500) {
      level = 'Elevated'; cls = 'level-elevated';
      rec = `<strong> Slightly elevated.</strong> Consider increasing ventilation or reducing occupancy. Opening windows or boosting HVAC flow will help.`;
    } else if (C_indoor < 2000) {
      level = 'High'; cls = 'level-high';
      rec = `<strong> High CO₂ levels.</strong> Students may feel drowsy and lose focus. Increase ventilation immediately — open doors, windows, or boost HVAC airflow.`;
    } else {
      level = 'Hazardous'; cls = 'level-hazardous';
      rec = `<strong> Hazardous levels.</strong> CO₂ this high significantly impairs cognitive performance. Reduce occupancy, maximize ventilation, and consider evacuating until levels drop.`;
    }

    const gaugePos = Math.min(100, Math.max(0, ((C_indoor - 400) / 2100) * 100));

    document.getElementById('co2-display').className = 'co2-display ' + cls;
    document.getElementById('co2-val').textContent   = C_indoor.toFixed(1);
    document.getElementById('co2-badge').textContent = level;
    document.getElementById('stat-ach').textContent  = ACH.toFixed(2);
    document.getElementById('stat-e').textContent    = E_ppm.toFixed(0);
    document.getElementById('stat-delta').textContent = '+' + delta.toFixed(1);
    document.getElementById('gauge-marker').style.left = gaugePos + '%';
    document.getElementById('recommendation').innerHTML = rec;

    res.classList.add('visible');
    res.scrollIntoView({ behavior: 'smooth', block: 'nearest' });
  }

  document.addEventListener('keydown', e => { if (e.key === 'Enter') calculate(); });
</script>
</body>
</html>
