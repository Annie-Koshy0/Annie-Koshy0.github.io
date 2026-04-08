<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Photobooth</title>
  <link href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@400;600;700&family=Quicksand:wght@300;400;500;600&display=swap" rel="stylesheet">
  <style>
    * { box-sizing: border-box; margin: 0; padding: 0; }
    body {
      font-family: 'Quicksand', sans-serif;
      background: linear-gradient(135deg, #722F37 0%, #5C1A25 50%, #4A1220 100%);
      color: #f5f0e8;
      min-height: 100vh;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      padding: 20px;
      text-align: center;
    }
    
    .main-wrapper {
      max-width: 680px;
      width: 100%;
      margin: 0 auto;
      padding: 20px;
    }
    
    h1, h2, h3 { 
      font-family: 'Playfair Display', serif;
      font-weight: 600;
      color: #f5f0e8;
    }
    h1 { font-size: 2.6rem; margin-bottom: 0.8rem; letter-spacing: 2px; font-style: italic; text-shadow: 0 2px 8px rgba(0,0,0,0.4); }
    h2 { font-size: 1.5rem; margin: 2rem 0 1.2rem; letter-spacing: 1px; }
    h3 { font-size: 1.2rem; margin-bottom: 0.8rem; }
    
    .step { display: none; width: 100%; animation: fadeIn 0.5s ease; }
    .step.active { display: block; }
    @keyframes fadeIn { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: translateY(0); } }

    .subtitle { font-size: 1rem; color: #e8dfd0; margin-bottom: 2.5rem; font-weight: 500; letter-spacing: 3px; text-transform: uppercase; }

    .btn {
      display: block; width: 100%; padding: 18px; margin: 12px 0;
      background: #f5f0e8; color: #5C1A25; border: 2px solid #f5f0e8;
      font-size: 1rem; font-family: 'Quicksand', sans-serif; font-weight: 600;
      cursor: pointer; transition: all 0.3s; letter-spacing: 2px; text-transform: uppercase;
    }
    .btn:hover { background: #e8dfd0; transform: translateY(-2px); box-shadow: 0 6px 15px rgba(0,0,0,0.3); }
    .btn.secondary { background: transparent; color: #f5f0e8; border-color: rgba(245,240,232,0.5); }
    .btn.secondary:hover { background: rgba(245,240,232,0.15); border-color: #f5f0e8; }
    .btn:disabled { opacity: 0.4; cursor: not-allowed; transform: none; box-shadow: none; }
    .btn.small { padding: 8px 14px; font-size: 0.85rem; width: auto; display: inline-block; margin: 5px; }

    /* Number Selection */
    .number-selection {
      background: rgba(245,240,232,0.1);
      padding: 20px;
      margin: 20px 0;
      border-radius: 8px;
      border: 2px solid rgba(201,184,164,0.3);
    }
    .number-grid {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 15px;
      margin-top: 15px;
    }
    .number-option {
      padding: 25px;
      border: 3px solid rgba(201,184,164,0.5);
      border-radius: 8px;
      cursor: pointer;
      transition: all 0.3s;
      background: rgba(245,240,232,0.08);
      font-family: 'Quicksand', sans-serif;
    }
    .number-option:hover, .number-option.selected {
      border-color: #f5f0e8;
      background: rgba(245,240,232,0.2);
      transform: translateY(-2px);
    }
    .number-option h4 {
      font-size: 1.8rem;
      margin-bottom: 5px;
      color: #f5f0e8;
      font-family: 'Playfair Display', serif;
    }
    .number-option p {
      font-size: 0.9rem;
      color: #d4c9b8;
    }

    /* Layout Grid Selection */
    .layout-selection {
      background: rgba(245,240,232,0.1);
      padding: 20px;
      margin: 20px 0;
      border-radius: 8px;
      border: 2px solid rgba(201,184,164,0.3);
    }
    .layout-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(140px, 1fr));
      gap: 15px;
      margin-top: 15px;
    }
    .layout-option {
      background: rgba(245,240,232,0.12);
      padding: 12px;
      border-radius: 8px;
      cursor: pointer;
      transition: all 0.3s;
      border: 3px solid transparent;
    }
    .layout-option:hover { transform: translateY(-3px); }
    .layout-option.selected { border-color: #f5f0e8; background: rgba(245,240,232,0.2); }
    
    .layout-visual {
      background: #1a1a1a;
      border-radius: 4px;
      overflow: hidden;
      margin-bottom: 8px;
      height: 120px;
    }
    .layout-photo {
      background: linear-gradient(135deg, #8b4553 0%, #6b2f3a 100%);
      margin: 2px;
      border-radius: 2px;
      border: 2px solid #2a2a2a;
    }
    
    /* Vertical Strip: 1 column × N rows */
    .vertical-strip-4 { display: grid; grid-template-rows: repeat(4, 1fr); grid-template-columns: 1fr; gap: 2px; height: 100%; padding: 2px; }
    .vertical-strip-6 { display: grid; grid-template-rows: repeat(6, 1fr); grid-template-columns: 1fr; gap: 2px; height: 100%; padding: 2px; }
    
    /* Landscape: 1 row × N columns */
    .landscape-row-4 { display: grid; grid-template-rows: 1fr; grid-template-columns: repeat(4, 1fr); gap: 2px; height: 100%; padding: 2px; }
    .landscape-row-6 { display: grid; grid-template-rows: 1fr; grid-template-columns: repeat(6, 1fr); gap: 2px; height: 100%; padding: 2px; }
    
    /* Square Grids */
    .grid-2x2 { display: grid; grid-template-columns: repeat(2, 1fr); grid-template-rows: repeat(2, 1fr); gap: 2px; height: 100%; padding: 2px; }
    .grid-3x3 { display: grid; grid-template-columns: repeat(3, 1fr); grid-template-rows: repeat(2, 1fr); gap: 2px; height: 100%; padding: 2px; }
    
    /* Separate Photos - Staggered Layout */
    .separate-photos-4 { position: relative; height: 100%; padding: 4px; }
    .separate-photos-4 .sep-photo { position: absolute; width: 40%; height: 35%; background: linear-gradient(135deg, #8b4553 0%, #6b2f3a 100%); border: 2px solid #2a2a2a; border-radius: 3px; }
    .separate-photos-4 .sep-photo:nth-child(1) { top: 5%; left: 30%; }
    .separate-photos-4 .sep-photo:nth-child(2) { top: 35%; left: 5%; }
    .separate-photos-4 .sep-photo:nth-child(3) { top: 35%; right: 5%; }
    .separate-photos-4 .sep-photo:nth-child(4) { bottom: 5%; left: 30%; }
    
    .separate-photos-6 { position: relative; height: 100%; padding: 4px; }
    .separate-photos-6 .sep-photo { position: absolute; width: 35%; height: 30%; background: linear-gradient(135deg, #8b4553 0%, #6b2f3a 100%); border: 2px solid #2a2a2a; border-radius: 3px; }
    .separate-photos-6 .sep-photo:nth-child(1) { top: 5%; left: 32.5%; }
    .separate-photos-6 .sep-photo:nth-child(2) { top: 30%; left: 5%; }
    .separate-photos-6 .sep-photo:nth-child(3) { top: 30%; right: 5%; }
    .separate-photos-6 .sep-photo:nth-child(4) { top: 55%; left: 15%; }
    .separate-photos-6 .sep-photo:nth-child(5) { top: 55%; right: 15%; }
    .separate-photos-6 .sep-photo:nth-child(6) { bottom: 5%; left: 32.5%; }
    
    .layout-title { font-family: 'Playfair Display', serif; font-size: 0.95rem; font-weight: 600; margin-bottom: 4px; color: #f5f0e8; }
    .layout-desc { font-size: 0.75rem; color: #d4c9b8; line-height: 1.3; }

    .video-wrapper { position: relative; width: 100%; background: #1a1a1a; margin: 20px 0; box-shadow: 0 8px 30px rgba(0,0,0,0.4); border: 6px solid #f5f0e8; }
    #video { width: 100%; display: none; }
    #video.active { display: block; }

    .countdown { font-size: 4rem; font-weight: 700; margin: 20px 0; color: #f5f0e8; animation: pulse 1s ease; text-shadow: 0 2px 10px rgba(0,0,0,0.5); }
    @keyframes pulse { 0%, 100% { transform: scale(1); opacity: 1; } 50% { transform: scale(1.1); opacity: 0.8; } }
    .pose-msg { background: rgba(245,240,232,0.15); padding: 15px; margin: 15px 0; font-style: italic; color: #f5f0e8; font-size: 1.1rem; border-left: 3px solid #f5f0e8; border-right: 3px solid #f5f0e8; }

    .photo-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(110px, 1fr)); gap: 15px; margin: 20px 0; }
    .grid-item { position: relative; cursor: pointer; border-radius: 8px; overflow: hidden; border: 3px solid rgba(245,240,232,0.3); transition: all 0.3s; }
    .grid-item:hover { transform: scale(1.03); border-color: #f5f0e8; }
    .grid-item.edited::after { content: "✓"; position: absolute; top: 6px; right: 6px; background: #5C1A25; color: #f5f0e8; width: 22px; height: 22px; border-radius: 50%; display: flex; align-items: center; justify-content: center; font-size: 0.8rem; font-weight: bold; }
    .grid-item img { width: 100%; height: 100%; object-fit: cover; display: block; }

    /* PHOTO STRIP WITH WHITE BORDERS */
    .photo-strip { background: #f5f0e8; padding: 20px; margin: 18px 0; box-shadow: 0 8px 30px rgba(0,0,0,0.3); position: relative; color: #2c241b; border: 12px solid #ffffff; border-radius: 4px; }
    .photo-strip::before { content: ""; position: absolute; top: -12px; left: -12px; right: -12px; bottom: -12px; border: 3px solid #e0d8cc; pointer-events: none; border-radius: 6px; }
    .strip-content { position: relative; display: grid; gap: 10px; filter: none; transition: filter 0.3s; overflow: hidden; background: #ffffff; padding: 8px; border-radius: 2px; }
    .strip-content.vintage-glare { filter: brightness(1.08) contrast(1.12) sepia(25%) saturate(1.2) drop-shadow(0 0 2px rgba(255,255,200,0.3)); }
    .strip-content.bw { filter: brightness(1.1) contrast(1.25) grayscale(100%); }
    .strip-content.color { filter: brightness(1.05) contrast(1.1) saturate(1.1); }
    .strip-content.grain::after {
      content: ""; position: absolute; top: 0; left: 0; right: 0; bottom: 0;
      background-image: url("image/svg+xml,%3Csvg viewBox='0 0 400 400' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noiseFilter'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23noiseFilter)' opacity='0.08'/%3E%3C/svg%3E");
      pointer-events: none; z-index: 10; mix-blend-mode: overlay;
    }
    .strip-img { width: 100%; display: block; object-fit: cover; border: 4px solid #ffffff; border-radius: 2px; box-shadow: 0 2px 8px rgba(0,0,0,0.15); }
    .strip-4 { grid-template-rows: repeat(4, 1fr); }
    .strip-6 { grid-template-rows: repeat(6, 1fr); }
    .strip-landscape-row-4 { grid-template-columns: repeat(4, 1fr); grid-template-rows: 1fr; }
    .strip-landscape-row-6 { grid-template-columns: repeat(6, 1fr); grid-template-rows: 1fr; }
    .strip-2x2 { grid-template-columns: repeat(2, 1fr); grid-template-rows: repeat(2, 1fr); }
    .strip-3x3 { grid-template-columns: repeat(3, 1fr); grid-template-rows: repeat(3, 1fr); }
    
    /* Separate photo with white border */
    .separate-photo-strip { background: #f5f0e8; padding: 15px; margin: 15px 0; box-shadow: 0 6px 20px rgba(0,0,0,0.2); position: relative; color: #2c241b; border: 10px solid #ffffff; border-radius: 4px; }
    .separate-photo-content { background: #ffffff; padding: 6px; border-radius: 2px; }
    .separate-photo-content img { width: 100%; display: block; object-fit: cover; border: 3px solid #ffffff; border-radius: 2px; box-shadow: 0 2px 6px rgba(0,0,0,0.15); }
    
    .filter-toggle { display: flex; justify-content: center; gap: 12px; margin: 20px 0; }
    .filter-toggle button {
      padding: 10px 22px; border: 2px solid rgba(201,184,164,0.5); background: transparent;
      cursor: pointer; transition: all 0.3s; font-family: 'Quicksand', sans-serif; font-size: 0.95rem; font-weight: 500; color: #f5f0e8;
    }
    .filter-toggle button:hover { background: rgba(245,240,232,0.1); }
    .filter-toggle button.active { border-color: #f5f0e8; background: #f5f0e8; color: #5C1A25; }

    .customization-section { background: rgba(245,240,232,0.12); padding: 20px; margin: 20px 0; border: 2px solid rgba(201,184,164,0.3); }
    .customization-section label { display: block; margin-bottom: 8px; color: #f5f0e8; font-size: 1rem; font-weight: 500; }
    .customization-section input[type="text"] { width: 100%; padding: 10px 12px; border: 2px solid rgba(201,184,164,0.4); background: rgba(245,240,232,0.1); font-family: 'Quicksand', sans-serif; font-size: 0.95rem; color: #f5f0e8; margin-bottom: 15px; }
    .customization-section input:focus { outline: none; border-color: #f5f0e8; background: rgba(245,240,232,0.15); }
    .customization-section input::placeholder { color: rgba(245,240,232,0.5); }
    
    .toggle-container { display: flex; align-items: center; justify-content: center; gap: 12px; margin: 15px 0; }
    .toggle-label { color: #f5f0e8; font-size: 0.95rem; font-weight: 500; }
    .toggle-switch { position: relative; width: 50px; height: 26px; }
    .toggle-switch input { opacity: 0; width: 0; height: 0; }
    .toggle-slider { position: absolute; cursor: pointer; top: 0; left: 0; right: 0; bottom: 0; background-color: rgba(201,184,164,0.4); transition: 0.3s; border-radius: 26px; }
    .toggle-slider:before { position: absolute; content: ""; height: 18px; width: 18px; left: 4px; bottom: 4px; background-color: #f5f0e8; transition: 0.3s; border-radius: 50%; }
    input:checked + .toggle-slider { background-color: #f5f0e8; }
    input:checked + .toggle-slider:before { transform: translateX(24px); background-color: #5C1A25; }
    
    .date-input-container { margin-top: 15px; max-height: 0; overflow: hidden; transition: max-height 0.3s ease, opacity 0.3s ease; opacity: 0; }
    .date-input-container.show { max-height: 80px; opacity: 1; }
    .date-input-container input[type="date"] { width: 100%; padding: 10px 12px; border: 2px solid rgba(201,184,164,0.4); background: rgba(245,240,232,0.1); font-family: 'Quicksand', sans-serif; font-size: 0.95rem; color: #f5f0e8; }

    #errorMsg { color: #ff9e9e; margin: 15px 0; font-size: 1rem; padding: 12px; background: rgba(139,35,35,0.2); border: 1px solid rgba(255,158,158,0.3); }
    .hidden { display: none !important; }

    input[type="file"] { display: none; }
    .upload-trigger { display: inline-block; padding: 18px; margin: 12px 0; background: #f5f0e8; color: #5C1A25; cursor: pointer; width: 100%; font-size: 1rem; font-family: 'Quicksand', sans-serif; font-weight: 600; transition: all 0.3s; letter-spacing: 2px; text-transform: uppercase; border: 2px solid #f5f0e8; }
    .upload-trigger:hover { background: #e8dfd0; transform: translateY(-2px); }
    
    .preview-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(80px, 1fr)); gap: 12px; margin-top: 15px; }
    .preview-grid img { width: 100%; border: 3px solid #f5f0e8; box-shadow: 0 3px 10px rgba(0,0,0,0.3); }

    .loading { display: inline-block; width: 18px; height: 18px; border: 3px solid rgba(245,240,232,0.3); border-top-color: #f5f0e8; border-radius: 50%; animation: spin 1s ease-in-out infinite; margin-left: 10px; vertical-align: middle; }
    @keyframes spin { to { transform: rotate(360deg); } }

    .modal-overlay {
      position: fixed; top: 0; left: 0; width: 100%; height: 100%;
      background: rgba(20,5,8,0.9); z-index: 2000;
      display: none; align-items: center; justify-content: center;
      padding: 20px;
    }
    .modal-overlay.active { display: flex; }
    .modal-content {
      background: #f5f0e8; padding: 25px; border-radius: 12px;
      max-width: 550px; width: 100%; text-align: center;
      box-shadow: 0 20px 50px rgba(0,0,0,0.6); color: #2c241b;
    }
    .modal-content h3 { color: #4a3728; margin-bottom: 15px; }
    .editor-preview { width: 100%; max-height: 220px; object-fit: contain; background: #fff; margin: 15px 0; border: 4px solid #1a1a1a; }
    .editor-controls { display: flex; flex-wrap: wrap; justify-content: center; gap: 8px; margin: 12px 0; }
    .editor-controls button {
      padding: 7px 14px; background: #5C1A25; color: #f5f0e8; border: none;
      cursor: pointer; font-family: 'Quicksand', sans-serif; font-weight: 500;
      transition: 0.3s;
    }
    .editor-controls button:hover { background: #4A1220; }
    .slider-group { margin: 8px 0; text-align: left; }
    .slider-group label { display: block; margin-bottom: 4px; font-size: 0.85rem; color: #4a3728; }
    .slider-group input[type="range"] { width: 100%; accent-color: #5C1A25; }
    
    .crop-container {
      position: relative; width: 100%; height: 240px;
      background: #fff; margin: 12px 0; overflow: hidden;
      border: 4px solid #1a1a1a; touch-action: none;
    }
    .crop-image {
      position: absolute; top: 0; left: 0;
      transform-origin: center center;
      cursor: grab; max-width: none;
      user-select: none; -webkit-user-drag: none;
    }
    .crop-image:active { cursor: grabbing; }
    .crop-frame {
      position: absolute; top: 50%; left: 50%;
      transform: translate(-50%, -50%);
      width: 180px; height: 180px;
      border: 3px solid #5C1A25;
      box-shadow: 0 0 0 9999px rgba(0,0,0,0.6);
      pointer-events: none; z-index: 10;
    }
    .crop-frame::before, .crop-frame::after {
      content: ""; position: absolute; background: rgba(92,26,37,0.3);
    }
    .crop-frame::before { top: 33.33%; left: 0; right: 0; height: 1px; box-shadow: 0 60px 0 rgba(92,26,37,0.3); }
    .crop-frame::after { left: 33.33%; top: 0; bottom: 0; width: 1px; box-shadow: 60px 0 0 rgba(92,26,37,0.3); }
    
    .crop-controls { display: flex; justify-content: center; gap: 8px; margin: 8px 0; }
    .crop-controls button {
      padding: 6px 12px; background: #5C1A25; color: #f5f0e8; border: none;
      cursor: pointer; font-family: 'Quicksand', sans-serif; font-weight: 500;
    }
    .crop-controls button:hover { background: #4A1220; }
    .crop-hint { font-size: 0.8rem; color: #6b5b4f; margin: 6px 0; font-style: italic; }

    @media (max-width: 480px) {
      .main-wrapper { padding: 15px; }
      .number-grid { grid-template-columns: 1fr; }
      .layout-grid { grid-template-columns: 1fr; }
      h1 { font-size: 2rem; }
      h2 { font-size: 1.3rem; }
      .photo-strip { padding: 12px; }
      .photo-strip::before { border-width: 8px; }
      .crop-container { height: 200px; }
      .crop-frame { width: 150px; height: 150px; }
    }
  </style>
</head>
<body>

  <div class="main-wrapper">
    <!-- STEP 1: Landing -->
    <div id="step1" class="step active">
      <h1>Photobooth</h1>
      <p class="subtitle">Capture Your Moment</p>
      <button class="btn" id="btnCamera">Use Camera</button>
      <label class="upload-trigger" for="fileInput">Upload Photos</label>
      <input type="file" id="fileInput" accept="image/*" multiple>
      <p id="errorMsg"></p>
    </div>

    <!-- STEP 2: Layout Selection -->
    <div id="step2" class="step">
      <h1>Choose Your Layout</h1>
      
      <div class="number-selection">
        <h3>1. Number of Photos</h3>
        <p style="color:#d4c9b8; font-size:0.9rem; margin-bottom:10px;">How many photos would you like?</p>
        <div class="number-grid">
          <div class="number-option" data-count="4">
            <h4>4 Photos</h4>
            <p>Classic photobooth strip</p>
          </div>
          <div class="number-option" data-count="6">
            <h4>6 Photos</h4>
            <p>Extended memories</p>
          </div>
        </div>
      </div>

      <div class="layout-selection">
        <h3>2. Choose Layout Style</h3>
        <p style="color:#d4c9b8; font-size:0.9rem; margin-bottom:10px;">Select how your photos will be arranged:</p>
        <div class="layout-grid" id="layoutGrid"></div>
      </div>

      <div id="uploadSection" class="hidden">
        <h3>Upload Your Photos</h3>
        <p style="margin-bottom:10px; color:#d4c9b8; font-size:0.9rem;">Select up to <span id="uploadLimit">6</span> photos</p>
        <label class="upload-trigger" for="fileInputStep2">Choose Photos</label>
        <input type="file" id="fileInputStep2" accept="image/*" multiple>
        <div id="previewContainer" class="preview-grid hidden"></div>
        <p id="uploadStatus" style="margin:8px 0; font-style:italic; color:#d4c9b8; font-size:0.85rem;"></p>
      </div>

      <button class="btn" id="btnStart" disabled>Continue</button>
      <button class="btn secondary" id="btnBack1">Back</button>
    </div>

    <!-- STEP 3: Capture -->
    <div id="step3" class="step">
      <h1 id="captureTitle">Prepare</h1>
      <div class="video-wrapper">
        <video id="video" autoplay playsinline muted></video>
      </div>
      <div id="countdown" class="countdown hidden"></div>
      <div id="poseMsg" class="pose-msg hidden">Adjust your pose and get ready</div>
      <button class="btn secondary" id="btnBack2">Back</button>
    </div>

    <!-- STEP 3.5: Review & Edit Photos -->
    <div id="step3_5" class="step">
      <h1>Review & Edit</h1>
      <p style="color:#d4c9b8; margin-bottom:15px; font-size:0.95rem;">Tap any photo to edit it. Click "Continue" when ready.</p>
      <div id="reviewGrid" class="photo-grid"></div>
      <button class="btn" id="btnContinueFromGrid">Continue</button>
      <button class="btn secondary" id="btnBackToCapture">Back</button>
    </div>

    <!-- STEP 4: Results -->
    <div id="step4" class="step">
      <h1>Your Photobooth</h1>
      <div id="resultsContainer"></div>
      <div class="filter-toggle">
        <button id="filterBW">B&W</button>
        <button id="filterColor" class="active">Color</button>
      </div>
      <div class="customization-section">
        <label for="customNote">Add a Note (Optional)</label>
        <input type="text" id="customNote" placeholder="Enter your message here..." maxlength="50">
        <div class="toggle-container">
          <span class="toggle-label">Date Stamp</span>
          <label class="toggle-switch">
            <input type="checkbox" id="dateStampToggle" checked>
            <span class="toggle-slider"></span>
          </label>
        </div>
        <div class="date-input-container show" id="dateInputContainer">
          <input type="date" id="customDate">
        </div>
      </div>
      <button class="btn" id="btnDownload">Download</button>
      <button class="btn secondary" id="btnRestart">Start Over</button>
    </div>
  </div>

  <!-- LIGHTING TIP MODAL -->
  <div id="lightingModal" class="modal-overlay">
    <div class="modal-content">
      <h3>💡 Lighting Tip</h3>
      <p style="margin:15px 0; line-height:1.6; color:#4a3728;">For the best results, face a soft light source like a window or lamp. Keep bright lights behind you to avoid harsh shadows on your face. Good lighting makes all the difference!</p>
      <button class="btn small" id="btnStartCamera" style="margin-top:10px;">Got it, Start Camera</button>
      <button class="btn secondary small" id="btnCancelLighting" style="margin-top:5px;">Cancel</button>
    </div>
  </div>

  <!-- EDITOR MODAL -->
  <div id="editorModal" class="modal-overlay">
    <div class="modal-content">
      <h3>Edit Photo <span id="editPhotoNum">1</span></h3>
      <div id="normalPreview">
        <img id="editorPreview" class="editor-preview" src="" alt="Preview">
      </div>
      <div id="cropPreview" class="hidden">
        <div class="crop-container" id="cropContainer">
          <img id="cropImage" class="crop-image" src="" alt="Crop">
          <div class="crop-frame"></div>
        </div>
        <div class="crop-controls">
          <button id="btnZoomOut">− Zoom</button>
          <button id="btnZoomIn">Zoom +</button>
          <button id="btnResetCrop">Reset</button>
        </div>
        <p class="crop-hint">Drag to move • Scroll/Pinch to zoom</p>
      </div>
      <div class="editor-controls">
        <button id="btnRotate">↻ Rotate</button>
        <button id="btnFlipH">↔ Flip H</button>
        <button id="btnFlipV">↕ Flip V</button>
        <button id="btnCrop" style="background:#7a4a55;">✂ Crop</button>
      </div>
      <div class="slider-group">
        <label>Brightness</label>
        <input type="range" id="editBrightness" min="50" max="150" value="100">
      </div>
      <div class="slider-group">
        <label>Contrast</label>
        <input type="range" id="editContrast" min="50" max="150" value="100">
      </div>
      <div style="margin-top:12px;">
        <button class="btn small" id="btnApplyCrop" style="display:none; background:#7a4a55; margin-bottom:8px;">Apply Crop</button>
        <button class="btn small" id="btnApplyExit">Apply & Exit</button>
      </div>
    </div>
  </div>

  <script>
    const state = {
      mode: null,
      count: null,
      orientation: null,
      photos: [],
      editedPhotos: [],
      editingIndex: null,
      currentFilter: 'vintage-glare',
      stream: null
    };

    const steps = {
      1: document.getElementById('step1'),
      2: document.getElementById('step2'),
      3: document.getElementById('step3'),
      3.5: document.getElementById('step3_5'),
      4: document.getElementById('step4')
    };
    const video = document.getElementById('video');
    const errorMsg = document.getElementById('errorMsg');
    const dateStampToggle = document.getElementById('dateStampToggle');
    const dateInputContainer = document.getElementById('dateInputContainer');
    const customDateInput = document.getElementById('customDate');

    let cropState = { scale: 1, translateX: 0, translateY: 0, isDragging: false, startX: 0, startY: 0, startTranslateX: 0, startTranslateY: 0 };

    document.addEventListener('DOMContentLoaded', () => {
      customDateInput.value = new Date().toISOString().split('T')[0];
    });

    dateStampToggle.addEventListener('change', () => {
      dateInputContainer.classList.toggle('show', dateStampToggle.checked);
      if (dateStampToggle.checked && !customDateInput.value) customDateInput.value = new Date().toISOString().split('T')[0];
    });

    function showStep(n) {
      Object.values(steps).forEach(s => s.classList.remove('active'));
      steps[n].classList.add('active');
      if (n === 1) resetAll();
    }

    // Lighting Modal
    document.getElementById('btnCamera').addEventListener('click', () => {
      document.getElementById('lightingModal').classList.add('active');
    });
    document.getElementById('btnStartCamera').addEventListener('click', async () => {
      document.getElementById('lightingModal').classList.remove('active');
      errorMsg.textContent = '';
      try {
        const devices = await navigator.mediaDevices.enumerateDevices();
        if (devices.filter(d => d.kind === 'videoinput').length === 0) {
          errorMsg.innerHTML = 'No camera detected. Please use upload.';
          return;
        }
        const testStream = await navigator.mediaDevices.getUserMedia({ video: { width: 640, height: 480 }, audio: false });
        testStream.getTracks().forEach(t => t.stop());
        state.mode = 'camera';
        showStep(2);
      } catch (err) {
        errorMsg.innerHTML = 'Camera access failed. Please check permissions.';
      }
    });
    document.getElementById('btnCancelLighting').addEventListener('click', () => {
      document.getElementById('lightingModal').classList.remove('active');
    });

    document.getElementById('fileInput').addEventListener('click', () => {
      state.mode = 'upload';
      showStep(2);
      document.getElementById('uploadSection').classList.remove('hidden');
    });

    document.getElementById('btnBack1').addEventListener('click', () => showStep(1));
    document.getElementById('btnBack2').addEventListener('click', () => {
      stopCamera();
      showStep(2);
    });
    document.getElementById('btnBackToCapture').addEventListener('click', () => showStep(3));

    // Number Selection
    document.querySelectorAll('.number-option').forEach(opt => {
      opt.addEventListener('click', () => {
        document.querySelectorAll('.number-option').forEach(o => o.classList.remove('selected'));
        opt.classList.add('selected');
        state.count = parseInt(opt.dataset.count);
        document.getElementById('uploadLimit').textContent = state.count;
        document.getElementById('fileInputStep2').max = state.count;
        updateLayoutOptions();
        checkContinueBtn();
      });
    });

    // Dynamic Layout Options
    function updateLayoutOptions() {
      const grid = document.getElementById('layoutGrid');
      if (!state.count) return;
      
      let html = '';
      
      if (state.count === 4) {
        html = `
          <div class="layout-option" data-layout="vertical-strip">
            <div class="layout-visual">
              <div class="vertical-strip-4">
                <div class="layout-photo"></div><div class="layout-photo"></div><div class="layout-photo"></div><div class="layout-photo"></div>
              </div>
            </div>
            <div class="layout-title">Vertical Strip</div>
            <div class="layout-desc">4 photos in 1 column (4×1)</div>
          </div>

          <div class="layout-option" data-layout="landscape">
            <div class="layout-visual">
              <div class="landscape-row-4">
                <div class="layout-photo"></div><div class="layout-photo"></div><div class="layout-photo"></div><div class="layout-photo"></div>
              </div>
            </div>
            <div class="layout-title">Landscape</div>
            <div class="layout-desc">4 photos in 1 row (1×4)</div>
          </div>

          <div class="layout-option" data-layout="grid-2x2">
            <div class="layout-visual">
              <div class="grid-2x2">
                <div class="layout-photo"></div><div class="layout-photo"></div><div class="layout-photo"></div><div class="layout-photo"></div>
              </div>
            </div>
            <div class="layout-title">2×2 Grid</div>
            <div class="layout-desc">4 photos in square grid</div>
          </div>

          <div class="layout-option" data-layout="separate">
            <div class="layout-visual">
              <div class="separate-photos-4">
                <div class="sep-photo"></div><div class="sep-photo"></div><div class="sep-photo"></div><div class="sep-photo"></div>
              </div>
            </div>
            <div class="layout-title">Separate Files</div>
            <div class="layout-desc">4 individual photos with borders</div>
          </div>
        `;
      } else if (state.count === 6) {
        html = `
          <div class="layout-option" data-layout="vertical-strip">
            <div class="layout-visual">
              <div class="vertical-strip-6">
                <div class="layout-photo"></div><div class="layout-photo"></div><div class="layout-photo"></div><div class="layout-photo"></div><div class="layout-photo"></div><div class="layout-photo"></div>
              </div>
            </div>
            <div class="layout-title">Vertical Strip</div>
            <div class="layout-desc">6 photos in 1 column (6×1)</div>
          </div>

          <div class="layout-option" data-layout="landscape">
            <div class="layout-visual">
              <div class="landscape-row-6">
                <div class="layout-photo"></div><div class="layout-photo"></div><div class="layout-photo"></div><div class="layout-photo"></div><div class="layout-photo"></div><div class="layout-photo"></div>
              </div>
            </div>
            <div class="layout-title">Landscape</div>
            <div class="layout-desc">6 photos in 1 row (1×6)</div>
          </div>

          <div class="layout-option" data-layout="grid-3x3">
            <div class="layout-visual">
              <div class="grid-3x3">
                <div class="layout-photo"></div><div class="layout-photo"></div><div class="layout-photo"></div><div class="layout-photo"></div><div class="layout-photo"></div><div class="layout-photo"></div>
              </div>
            </div>
            <div class="layout-title">3×3 Grid</div>
            <div class="layout-desc">6 photos in 3×2 grid</div>
          </div>

          <div class="layout-option" data-layout="separate">
            <div class="layout-visual">
              <div class="separate-photos-6">
                <div class="sep-photo"></div><div class="sep-photo"></div><div class="sep-photo"></div><div class="sep-photo"></div><div class="sep-photo"></div><div class="sep-photo"></div>
              </div>
            </div>
            <div class="layout-title">Separate Files</div>
            <div class="layout-desc">6 individual photos with borders</div>
          </div>
        `;
      }
      
      grid.innerHTML = html;
      
      grid.querySelectorAll('.layout-option').forEach(opt => {
        opt.addEventListener('click', () => {
          grid.querySelectorAll('.layout-option').forEach(o => o.classList.remove('selected'));
          opt.classList.add('selected');
          state.orientation = opt.dataset.layout;
          checkContinueBtn();
        });
      });
    }

    function checkContinueBtn() {
      const valid = state.count && state.orientation && 
        (state.mode === 'camera' || (state.mode === 'upload' && state.photos.length === state.count));
      document.getElementById('btnStart').disabled = !valid;
    }

    document.getElementById('fileInputStep2').addEventListener('change', (e) => {
      const files = Array.from(e.target.files);
      if (files.length === 0) return;
      const limit = state.count;
      if (files.length > limit) errorMsg.textContent = `Only first ${limit} photos used.`;
      else errorMsg.textContent = '';

      state.photos = [];
      const pc = document.getElementById('previewContainer');
      pc.classList.remove('hidden'); pc.innerHTML = '';
      let loaded = 0;
      files.slice(0, limit).forEach(file => {
        const reader = new FileReader();
        reader.onload = (ev) => {
          const img = new Image();
          img.onload = () => {
            const c = document.createElement('canvas');
            c.width = img.width; c.height = img.height;
            const ctx = c.getContext('2d');
            ctx.filter = 'brightness(1.08) contrast(1.12) sepia(25%) saturate(1.2)';
            ctx.drawImage(img, 0, 0);
            state.photos.push(c.toDataURL('image/jpeg', 0.9));
            const pImg = document.createElement('img');
            pImg.src = state.photos[state.photos.length-1];
            pc.appendChild(pImg);
            loaded++;
            document.getElementById('uploadStatus').textContent = `Loaded ${loaded}/${limit}`;
            if (loaded === limit) checkContinueBtn();
          };
          img.src = ev.target.result;
        };
        reader.readAsDataURL(file);
      });
    });

    document.getElementById('btnStart').addEventListener('click', async () => {
      if (!state.count || !state.orientation) { errorMsg.textContent = 'Select layout first.'; return; }
      showStep(3);
      if (state.mode === 'camera') await initCamera();
      else goToReviewGrid();
    });

    async function initCamera() {
      document.getElementById('captureTitle').textContent = 'Camera Preview';
      errorMsg.textContent = '';
      try {
        state.stream = await navigator.mediaDevices.getUserMedia({ video: { facingMode: 'user', width: 1280, height: 720 }, audio: false });
        video.srcObject = state.stream;
        video.classList.add('active');
        video.onloadedmetadata = () => {
          document.getElementById('captureTitle').textContent = `Photo 1 of ${state.count}`;
          setTimeout(captureSeq, 2000);
        };
      } catch (err) {
        errorMsg.innerHTML = 'Camera error. Switching to upload.';
        setTimeout(() => { state.mode = 'upload'; showStep(2); }, 1500);
      }
    }

    function stopCamera() {
      if (state.stream) {
        state.stream.getTracks().forEach(track => track.stop());
        state.stream = null;
      }
      video.srcObject = null;
      video.classList.remove('active');
    }

    let currentCap = 0;
    function captureSeq() {
      if (currentCap >= state.count) { 
        stopCamera();
        goToReviewGrid(); 
        return; 
      }
      const cd = document.getElementById('countdown');
      const pm = document.getElementById('poseMsg');
      cd.classList.remove('hidden');
      let c = 3; cd.textContent = c;
      const t = setInterval(() => {
        c--;
        if (c > 0) cd.textContent = c;
        else {
          clearInterval(t);
          cd.classList.add('hidden');
          const cvs = document.createElement('canvas');
          cvs.width = video.videoWidth; cvs.height = video.videoHeight;
          const ctx = cvs.getContext('2d');
          ctx.filter = 'brightness(1.08) contrast(1.12) sepia(25%) saturate(1.2)';
          ctx.drawImage(video, 0, 0);
          state.photos.push(cvs.toDataURL('image/jpeg', 0.95));
          currentCap++;
          if (currentCap < state.count) {
            pm.classList.remove('hidden');
            setTimeout(() => {
              pm.classList.add('hidden');
              document.getElementById('captureTitle').textContent = `Photo ${currentCap+1} of ${state.count}`;
              captureSeq();
            }, 2000);
          } else {
            stopCamera();
            goToReviewGrid();
          }
        }
      }, 1000);
    }

    function goToReviewGrid() {
      currentCap = 0;
      state.editedPhotos = state.photos.map(() => false);
      renderReviewGrid();
      showStep(3.5);
    }

    function renderReviewGrid() {
      const grid = document.getElementById('reviewGrid');
      grid.innerHTML = '';
      state.photos.forEach((src, i) => {
        const item = document.createElement('div');
        item.className = `grid-item ${state.editedPhotos[i] ? 'edited' : ''}`;
        item.innerHTML = `<img src="${src}" alt="Photo ${i+1}">`;
        item.addEventListener('click', () => openEditor(i));
        grid.appendChild(item);
      });
    }

    function openEditor(index) {
      state.editingIndex = index;
      document.getElementById('editorModal').classList.add('active');
      document.getElementById('editPhotoNum').textContent = index + 1;
      state.isCropping = false;
      document.getElementById('editBrightness').value = 100;
      document.getElementById('editContrast').value = 100;
      updateEditorPreview();
    }

    function updateEditorPreview() {
      const np = document.getElementById('normalPreview');
      const cp = document.getElementById('cropPreview');
      const img = document.getElementById('editorPreview');
      if (state.isCropping) {
        np.classList.add('hidden'); cp.classList.remove('hidden');
        document.getElementById('btnApplyCrop').style.display = 'inline-block';
        initCrop();
      } else {
        np.classList.remove('hidden'); cp.classList.add('hidden');
        document.getElementById('btnApplyCrop').style.display = 'none';
        img.src = state.photos[state.editingIndex];
        img.style.transform = `translate(0,0) scale(1)`;
        img.style.filter = `brightness(${document.getElementById('editBrightness').value}%) contrast(${document.getElementById('editContrast').value}%)`;
      }
    }

    function initCrop() {
      const container = document.getElementById('cropContainer');
      const cropImg = document.getElementById('cropImage');
      cropState = { scale: 1, translateX: 0, translateY: 0, isDragging: false, startX: 0, startY: 0, startTranslateX: 0, startTranslateY: 0 };
      cropImg.src = state.photos[state.editingIndex];
      cropImg.onload = () => {
        const cRect = container.getBoundingClientRect();
        const iRect = cropImg.getBoundingClientRect();
        cropState.translateX = (cRect.width - iRect.width) / 2;
        cropState.translateY = (cRect.height - iRect.height) / 2;
        updateCropTransform();
      };
      const newCropImg = cropImg.cloneNode(true);
      cropImg.parentNode.replaceChild(newCropImg, cropImg);
      attachCropListeners(newCropImg, container);
    }

    function attachCropListeners(cropImg, container) {
      const startDrag = (e) => { e.preventDefault(); cropState.isDragging = true; const p = e.touches ? e.touches[0] : e; cropState.startX = p.clientX; cropState.startY = p.clientY; cropState.startTranslateX = cropState.translateX; cropState.startTranslateY = cropState.translateY; cropImg.style.cursor = 'grabbing'; };
      const drag = (e) => { if (!cropState.isDragging) return; e.preventDefault(); const p = e.touches ? e.touches[0] : e; cropState.translateX = cropState.startTranslateX + (p.clientX - cropState.startX); cropState.translateY = cropState.startTranslateY + (p.clientY - cropState.startY); updateCropTransform(); };
      const endDrag = () => { cropState.isDragging = false; cropImg.style.cursor = 'grab'; };
      cropImg.addEventListener('mousedown', startDrag);
      cropImg.addEventListener('touchstart', startDrag, { passive: false });
      document.addEventListener('mousemove', drag);
      document.addEventListener('touchmove', drag, { passive: false });
      document.addEventListener('mouseup', endDrag);
      document.addEventListener('touchend', endDrag);
      container.addEventListener('wheel', (e) => { e.preventDefault(); adjustZoom(e.deltaY > 0 ? -0.1 : 0.1); }, { passive: false });
    }

    function updateCropTransform() {
      document.getElementById('cropImage').style.transform = `translate(${cropState.translateX}px, ${cropState.translateY}px) scale(${cropState.scale})`;
    }
    function adjustZoom(d) { cropState.scale = Math.max(0.5, Math.min(5, cropState.scale + d)); updateCropTransform(); }

    document.getElementById('btnZoomIn').addEventListener('click', () => adjustZoom(0.15));
    document.getElementById('btnZoomOut').addEventListener('click', () => adjustZoom(-0.15));
    document.getElementById('btnResetCrop').addEventListener('click', () => { cropState.scale = 1; cropState.translateX = 0; cropState.translateY = 0; updateCropTransform(); });
    document.getElementById('btnCrop').addEventListener('click', () => { state.isCropping = !state.isCropping; document.getElementById('btnCrop').style.background = state.isCropping ? '#4A1220' : '#7a4a55'; updateEditorPreview(); });
    
    document.getElementById('btnApplyCrop').addEventListener('click', () => {
      const container = document.getElementById('cropContainer');
      const cropImg = document.getElementById('cropImage');
      const cRect = container.getBoundingClientRect();
      const fw = window.innerWidth <= 480 ? 150 : 180;
      const fh = fw;
      const fx = (cRect.width - fw) / 2;
      const fy = (cRect.height - fh) / 2;
      const srcX = (fx - cropState.translateX) / cropState.scale;
      const srcY = (fy - cropState.translateY) / cropState.scale;
      const srcW = fw / cropState.scale;
      const srcH = fh / cropState.scale;
      const c = document.createElement('canvas');
      c.width = srcW; c.height = srcH;
      const ctx = c.getContext('2d');
      const tempImg = new Image();
      tempImg.onload = () => {
        ctx.drawImage(tempImg, srcX, srcY, srcW, srcH, 0, 0, srcW, srcH);
        state.photos[state.editingIndex] = c.toDataURL('image/jpeg', 0.95);
        state.isCropping = false;
        document.getElementById('btnCrop').style.background = '#7a4a55';
        updateEditorPreview();
      };
      tempImg.src = state.photos[state.editingIndex];
    });

    let editorTransform = { rot: 0, sx: 1, sy: 1 };
    function updateEditorTransformUI() {
      const img = document.getElementById('editorPreview');
      img.style.transform = `rotate(${editorTransform.rot}deg) scale(${editorTransform.sx}, ${editorTransform.sy})`;
    }
    document.getElementById('btnRotate').addEventListener('click', () => { editorTransform.rot = (editorTransform.rot + 90) % 360; updateEditorTransformUI(); });
    document.getElementById('btnFlipH').addEventListener('click', () => { editorTransform.sx *= -1; updateEditorTransformUI(); });
    document.getElementById('btnFlipV').addEventListener('click', () => { editorTransform.sy *= -1; updateEditorTransformUI(); });
    document.getElementById('editBrightness').addEventListener('input', () => {
      document.getElementById('editorPreview').style.filter = `brightness(${document.getElementById('editBrightness').value}%) contrast(${document.getElementById('editContrast').value}%)`;
    });
    document.getElementById('editContrast').addEventListener('input', () => {
      document.getElementById('editorPreview').style.filter = `brightness(${document.getElementById('editBrightness').value}%) contrast(${document.getElementById('editContrast').value}%)`;
    });

    document.getElementById('btnApplyExit').addEventListener('click', () => {
      const img = document.getElementById('editorPreview');
      const tempImg = new Image();
      tempImg.onload = () => {
        const c = document.createElement('canvas');
        const isRotated = editorTransform.rot === 90 || editorTransform.rot === 270;
        c.width = isRotated ? tempImg.height : tempImg.width;
        c.height = isRotated ? tempImg.width : tempImg.height;
        const ctx = c.getContext('2d');
        ctx.translate(c.width/2, c.height/2);
        ctx.rotate(editorTransform.rot * Math.PI / 180);
        ctx.scale(editorTransform.sx, editorTransform.sy);
        ctx.filter = `brightness(${document.getElementById('editBrightness').value}%) contrast(${document.getElementById('editContrast').value}%)`;
        ctx.drawImage(tempImg, -tempImg.width/2, -tempImg.height/2);
        state.photos[state.editingIndex] = c.toDataURL('image/jpeg', 0.95);
        state.editedPhotos[state.editingIndex] = true;
        document.getElementById('editorModal').classList.remove('active');
        editorTransform = { rot: 0, sx: 1, sy: 1 };
        renderReviewGrid();
      };
      tempImg.src = state.photos[state.editingIndex];
    });

    document.getElementById('btnContinueFromGrid').addEventListener('click', () => {
      renderResults();
      showStep(4);
    });

    function renderResults() {
      const container = document.getElementById('resultsContainer');
      container.innerHTML = '';
      
      if (state.orientation === 'separate') {
        state.photos.forEach((src, i) => {
          const wrapper = document.createElement('div');
          wrapper.className = 'separate-photo-strip';
          
          let dateHTML = '';
          if (dateStampToggle.checked && customDateInput.value) {
            const d = new Date(customDateInput.value).toLocaleDateString('en-US', {month:'long', day:'numeric', year:'numeric'});
            dateHTML = `<p style="text-align:center; margin-top:8px; font-style:italic; font-size:0.95rem; color:#5C1A25; font-family:'Playfair Display', serif;">${d}</p>`;
          }
          
          wrapper.innerHTML = `
            <div class="separate-photo-content">
              <img src="${src}" class="strip-img" style="filter: brightness(1.08) contrast(1.12) sepia(25%) saturate(1.2) drop-shadow(0 0 2px rgba(255,255,200,0.3));" data-filter="vintage-glare">
            </div>
            ${dateHTML}
          `;
          container.appendChild(wrapper);
        });
        return;
      }
      
      const stripWrapper = document.createElement('div');
      stripWrapper.className = 'photo-strip';
      
      let dateHTML = '';
      if (dateStampToggle.checked && customDateInput.value) {
        const d = new Date(customDateInput.value).toLocaleDateString('en-US', {month:'long', day:'numeric', year:'numeric'});
        dateHTML = `<p style="text-align:center; margin-top:12px; font-style:italic; font-size:1.05rem; color:#5C1A25; font-family:'Playfair Display', serif;">${d}</p>`;
      }
      
      let stripClass = 'strip-content grain vintage-glare';
      if (state.count === 4) stripClass += ' strip-4';
      else stripClass += ' strip-6';
      
      if (state.orientation === 'landscape') {
        stripClass += (state.count === 4 ? ' strip-landscape-row-4' : ' strip-landscape-row-6');
      } else if (state.orientation === 'grid-2x2') {
        stripClass += ' strip-2x2';
      } else if (state.orientation === 'grid-3x3') {
        stripClass += ' strip-3x3';
      }
      
      let photosHTML = '';
      state.photos.forEach(src => {
        photosHTML += `<img src="${src}" class="strip-img">`;
      });
      
      stripWrapper.innerHTML = `
        <div class="${stripClass}">
          ${photosHTML}
        </div>
        ${dateHTML}
      `;
      
      container.appendChild(stripWrapper);
    }

    // Filter toggle
    document.getElementById('filterBW').addEventListener('click', () => {
      state.currentFilter = 'bw';
      document.getElementById('filterBW').classList.add('active');
      document.getElementById('filterColor').classList.remove('active');
      applyFilter();
    });
    
    document.getElementById('filterColor').addEventListener('click', () => {
      state.currentFilter = 'color';
      document.getElementById('filterColor').classList.add('active');
      document.getElementById('filterBW').classList.remove('active');
      applyFilter();
    });
    
    function applyFilter() {
      const strips = document.querySelectorAll('.strip-content');
      const separateImgs = document.querySelectorAll('.separate-photo-content img');
      
      strips.forEach(strip => {
        strip.classList.remove('vintage-glare', 'bw', 'color');
        strip.classList.add(state.currentFilter);
      });
      
      separateImgs.forEach(img => {
        img.removeAttribute('style');
        if (state.currentFilter === 'bw') {
          img.style.filter = 'brightness(1.1) contrast(1.25) grayscale(100%)';
        } else if (state.currentFilter === 'color') {
          img.style.filter = 'brightness(1.05) contrast(1.1) saturate(1.1)';
        } else {
          img.style.filter = 'brightness(1.08) contrast(1.12) sepia(25%) saturate(1.2) drop-shadow(0 0 2px rgba(255,255,200,0.3))';
        }
      });
    }

    document.getElementById('btnDownload').addEventListener('click', async () => {
      const btn = document.getElementById('btnDownload');
      const orig = btn.innerHTML;
      btn.innerHTML = 'Preparing... <span class="loading"></span>';
      btn.disabled = true;
      const note = document.getElementById('customNote').value.trim();
      const addDate = dateStampToggle.checked;
      const dVal = customDateInput.value;
      const fClass = state.currentFilter;

      try {
        if (state.orientation === 'separate') {
          for (let i = 0; i < state.count; i++) {
            if (!state.photos[i]) continue;
            const img = new Image();
            img.src = state.photos[i];
            await new Promise(res => img.onload = res);
            
            const c = document.createElement('canvas');
            const border = 50;
            const textArea = (note || (addDate && dVal)) ? 100 : 0;
            c.width = img.width + border * 2;
            c.height = img.height + border * 2 + textArea;
            const ctx = c.getContext('2d');
            
            ctx.fillStyle = '#ffffff';
            ctx.fillRect(0, 0, c.width, c.height);
            
            if (fClass === 'bw') ctx.filter = 'brightness(1.1) contrast(1.25) grayscale(100%)';
            else if (fClass === 'color') ctx.filter = 'brightness(1.05) contrast(1.1) saturate(1.1)';
            else ctx.filter = 'brightness(1.08) contrast(1.12) sepia(25%) saturate(1.2)';
            
            ctx.drawImage(img, border, border);
            
            if (note || (addDate && dVal)) {
              ctx.filter = 'none';
              ctx.fillStyle = '#5C1A25';
              ctx.textAlign = 'center';
              
              let yPos = c.height - 30;
              
              if (addDate && dVal) {
                const d = new Date(dVal).toLocaleDateString('en-US', {month:'long', day:'numeric', year:'numeric'});
                ctx.font = 'italic 32px "Playfair Display"';
                ctx.fillText(d, c.width/2, yPos);
                yPos -= 50;
              }
              
              if (note) {
                ctx.font = 'italic 48px "Playfair Display"';
                ctx.fillText(note, c.width/2, yPos);
              }
            }
            
            const blob = await new Promise(res => c.toBlob(res, 'image/jpeg', 0.95));
            const url = URL.createObjectURL(blob);
            const a = document.createElement('a'); a.href = url; a.download = `photobooth-${i+1}.jpg`;
            document.body.appendChild(a); a.click(); document.body.removeChild(a);
            URL.revokeObjectURL(url);
            await new Promise(r => setTimeout(r, 200));
          }
        } else {
          const tc = document.createElement('canvas');
          const tCtx = tc.getContext('2d');
          const pW = 800, pH = 800, gap = 30, border = 120, r = 8;
          
          // Larger bottom margin for readable text
          const bottomMargin = 200;
          
          let cW, cH;
          
          if (state.orientation === 'vertical-strip') {
            cW = pW + border*2;
            cH = (pH * state.count) + (gap * (state.count-1)) + border*2 + bottomMargin;
          } else if (state.orientation === 'landscape') {
            cW = (pW * state.count) + (gap * (state.count-1)) + border*2;
            cH = pH + border*2 + bottomMargin;
          } else {
            const size = state.count === 4 ? 2 : 3;
            cW = (pW * size) + (gap * (size-1)) + border*2;
            cH = (pH * size) + (gap * (size-1)) + border*2 + bottomMargin;
          }
          
          tc.width = cW; tc.height = cH;
          
          tCtx.fillStyle = '#ffffff';
          tCtx.fillRect(0, 0, cW, cH);
          
          const innerBorder = 20;
          tCtx.fillStyle = '#f5f0e8';
          tCtx.fillRect(innerBorder, innerBorder, cW - innerBorder*2, cH - innerBorder*2);
          
          if (fClass === 'bw') tCtx.filter = 'brightness(1.1) contrast(1.25) grayscale(100%)';
          else if (fClass === 'color') tCtx.filter = 'brightness(1.05) contrast(1.1) saturate(1.1)';
          else tCtx.filter = 'brightness(1.08) contrast(1.12) sepia(25%) saturate(1.2)';
          
          const imgs = await Promise.all(state.photos.map(src => new Promise(res => { const i = new Image(); i.onload = () => res(i); i.src = src; })));
          imgs.forEach((img, i) => {
            let x, y;
            if (state.orientation === 'vertical-strip') {
              x = border; y = border + i*(pH+gap);
            } else if (state.orientation === 'landscape') {
              x = border + i*(pW+gap); y = border;
            } else {
              const cols = state.orientation === 'grid-3x3' ? 3 : 2;
              const row = Math.floor(i/cols), col = i%cols;
              x = border + col*(pW+gap); y = border + row*(pH+gap);
            }
            tCtx.beginPath(); tCtx.moveTo(x+r, y); tCtx.lineTo(x+pW-r, y); tCtx.quadraticCurveTo(x+pW, y, x+pW, y+r);
            tCtx.lineTo(x+pW, y+pH-r); tCtx.quadraticCurveTo(x+pW, y+pH, x+pW-r, y+pH);
            tCtx.lineTo(x+r, y+pH); tCtx.quadraticCurveTo(x, y+pH, x, y+pH-r);
            tCtx.lineTo(x, y+r); tCtx.quadraticCurveTo(x, y, x+r, y); tCtx.closePath();
            tCtx.save(); tCtx.clip(); tCtx.drawImage(img, x, y, pW, pH); tCtx.restore();
          });
          
          tCtx.filter = 'none';
          
          // Film grain
          const gc = document.createElement('canvas'); gc.width=cW; gc.height=cH-bottomMargin;
          const gctx = gc.getContext('2d');
          const id = gctx.createImageData(cW, cH-bottomMargin);
          for(let i=0; i<id.data.length; i+=4) { const v = (Math.random()-0.5)*30; id.data[i]=v; id.data[i+1]=v; id.data[i+2]=v; id.data[i+3]=25; }
          gctx.putImageData(id, 0, 0);
          tCtx.globalCompositeOperation = 'overlay'; tCtx.drawImage(gc, 0, 0); tCtx.globalCompositeOperation = 'source-over';
          
          // Text positioning - centered, note above date
          const textStartY = cH - bottomMargin + 60;
          tCtx.fillStyle = '#5C1A25';
          tCtx.textAlign = 'center';
          
          let currentY = textStartY;
          
          // Note first (above date)
          if (note) {
            tCtx.font = 'italic 56px "Playfair Display"';
            tCtx.fillText(note, cW/2, currentY);
            currentY += 70;
          }
          
          // Date below note
          if (addDate && dVal) {
            const d = new Date(dVal).toLocaleDateString('en-US', {month:'long', day:'numeric', year:'numeric'});
            tCtx.font = 'italic 40px "Playfair Display"';
            tCtx.fillText(d, cW/2, currentY);
          }
          
          const blob = await new Promise(res => tc.toBlob(res, 'image/jpeg', 0.95));
          const url = URL.createObjectURL(blob);
          const a = document.createElement('a'); a.href = url; a.download = `photobooth-${Date.now()}.jpg`;
          document.body.appendChild(a); a.click(); document.body.removeChild(a);
          URL.revokeObjectURL(url);
        }
        
        // Success popup
        const html = `<div style="position:fixed;bottom:20px;left:50%;transform:translateX(-50%);background:#5C1A25;color:#f5f0e8;padding:20px 30px;z-index:3000;max-width:450px;text-align:center;animation:slideUp 0.5s ease;font-family:'Quicksand',sans-serif;box-shadow:0 10px 30px rgba(0,0,0,0.4);border-radius:8px;">
          <strong style="font-size:1.2rem;display:block;margin-bottom:8px;">Download Complete</strong>
          <p style="font-size:0.95rem;line-height:1.5;margin:0;">Your moment has been saved to your device.</p>
          <button onclick="this.parentElement.remove()" style="margin-top:12px;padding:6px 16px;background:rgba(245,240,232,0.2);border:1px solid rgba(245,240,232,0.4);color:#f5f0e8;cursor:pointer;font-family:'Quicksand',sans-serif;border-radius:15px;">Close</button>
        </div>`;
        const style = document.createElement('style');
        style.textContent = `@keyframes slideUp { from{transform:translate(-50%,100px);opacity:0;} to{transform:translate(-50%,0);opacity:1;} }`;
        document.head.appendChild(style);
        document.body.insertAdjacentHTML('beforeend', html);
        setTimeout(() => { const el = document.querySelector('[style*="position:fixed"]'); if(el) el.remove(); }, 7000);
      } catch (err) { console.error(err); errorMsg.textContent = 'Download failed.'; }
      btn.innerHTML = orig; btn.disabled = false;
    });

    document.getElementById('btnRestart').addEventListener('click', () => showStep(1));

    function resetAll() {
      stopCamera();
      state.photos = []; state.editedPhotos = []; state.editingIndex = null; state.mode = null; state.currentFilter = 'vintage-glare';
      state.count = null; state.orientation = null;
      document.querySelectorAll('.number-option').forEach(o => o.classList.remove('selected'));
      document.getElementById('layoutGrid').innerHTML = '';
      document.getElementById('btnStart').disabled = true;
      document.getElementById('uploadSection').classList.add('hidden');
      document.getElementById('previewContainer').classList.add('hidden');
      document.getElementById('previewContainer').innerHTML = '';
      document.getElementById('uploadStatus').textContent = '';
      document.getElementById('fileInputStep2').value = '';
      document.getElementById('customNote').value = '';
      dateStampToggle.checked = true; dateInputContainer.classList.add('show');
      customDateInput.value = new Date().toISOString().split('T')[0];
      document.getElementById('filterColor').classList.add('active');
      document.getElementById('filterBW').classList.remove('active');
    }
  </script>
</body>
</html>
