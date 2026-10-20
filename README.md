<!DOCTYPE html>
<html lang="hi">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  <title>A Special Day</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link href="https://fonts.googleapis.com/css2?family=Playfair+Display:ital,wght@0,600;1,400&family=Plus+Jakarta+Sans:wght@300;400;600;700&display=swap" rel="stylesheet">
  <script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.6.0/dist/confetti.browser.min.js"></script>

  <style>
    :root {
      --gold: #dfba73;
      --pink: #ff758f;
      --card-bg: rgba(25, 12, 17, 0.88);
      --card-border: rgba(223, 186, 115, 0.35);
    }
    * { box-sizing: border-box; margin: 0; padding: 0; user-select: none; -webkit-tap-highlight-color: transparent; }
    body {
      background: #000;
      color: #fff;
      font-family: 'Plus Jakarta Sans', sans-serif;
      overflow: hidden;
      height: 100vh;
      display: flex;
      justify-content: center;
      align-items: center;
    }
    
    .app-screen {
      width: 100%;
      max-width: 412px;
      height: 100vh;
      position: relative;
      background: radial-gradient(circle at 50% 25%, #2d050e 0%, #0d0205 65%, #050102 100%);
      overflow: hidden;
      display: flex;
      flex-direction: column;
      box-shadow: 0 0 50px rgba(255, 75, 110, 0.25);
    }

    .sparkle {
      position: absolute;
      width: 3px;
      height: 3px;
      background: rgba(255, 230, 180, 0.7);
      border-radius: 50%;
      animation: floatUp 6s linear infinite;
      pointer-events: none;
    }
    @keyframes floatUp {
      0% { transform: translateY(100vh) scale(0.5); opacity: 0; }
      50% { opacity: 0.8; }
      100% { transform: translateY(-10vh) scale(1.2); opacity: 0; }
    }

    .scene {
      display: none;
      flex-direction: column;
      align-items: center;
      justify-content: space-between;
      height: 100%;
      padding: 50px 24px 35px 24px;
      text-align: center;
      position: relative;
      z-index: 2;
    }
    .scene.active { display: flex; animation: fadeScene 0.5s cubic-bezier(0.2, 0.8, 0.2, 1) forwards; }
    @keyframes fadeScene {
      from { opacity: 0; transform: scale(0.97); }
      to { opacity: 1; transform: scale(1); }
    }

    .script-sub {
      font-family: 'Playfair Display', serif;
      font-style: italic;
      font-size: 1.15rem;
      color: #e8c49e;
      letter-spacing: 0.5px;
      margin-bottom: 6px;
    }
    .hero-title {
      font-size: 2.1rem;
      font-weight: 700;
      color: #fff;
      line-height: 1.25;
      text-shadow: 0 4px 18px rgba(255, 117, 143, 0.5);
    }
    .bottom-hint {
      font-size: 0.85rem;
      color: rgba(255, 255, 255, 0.55);
      letter-spacing: 1px;
    }

    /* Scene 1: Glowing Heart (Clean Beautiful SVG) */
    .s1-heart-wrap {
      margin: auto 0;
      cursor: pointer;
      display: flex;
      justify-content: center;
      align-items: center;
    }
    .glow-heart-svg {
      width: 105px;
      height: 105px;
      filter: drop-shadow(0 0 25px #ff4d6d) drop-shadow(0 0 50px rgba(255, 77, 109, 0.6));
      animation: heartPulse 1.3s infinite alternate ease-in-out;
    }
    @keyframes heartPulse {
      0% { transform: scale(0.93); filter: drop-shadow(0 0 15px #ff4d6d); }
      100% { transform: scale(1.08); filter: drop-shadow(0 0 35px #ff758f); }
    }

    /* Scene 2: Blooming Heart Tree */
    .tree-stage {
      position: relative;
      width: 260px;
      height: 300px;
      margin: 10px 0;
      cursor: pointer;
    }
    .tree-trunk {
      position: absolute;
      bottom: 0;
      left: 50%;
      width: 6px;
      height: 170px;
      background: linear-gradient(to top, #4a1910, #8d3a2b);
      transform: translateX(-50%);
      border-radius: 4px;
    }
    .tree-crown {
      position: absolute;
      top: 15px;
      left: 50%;
      transform: translateX(-50%) scale(0);
      width: 210px;
      height: 190px;
      background: radial-gradient(circle, #ffccd5 20%, #ff758f 70%, #d90429 100%);
      clip-path: path("M105,180 C105,180 15,120 15,60 C15,20 50,5 85,30 C105,45 105,45 105,45 C105,45 105,45 125,30 C160,5 195,20 195,60 C195,120 105,180 105,180 Z");
      box-shadow: 0 0 40px rgba(255, 117, 143, 0.7);
      transition: transform 1.2s cubic-bezier(0.18, 0.89, 0.32, 1.28);
    }
    .tree-crown.bloomed { transform: translateX(-50%) scale(1); }

    /* Scene 3: Multilayer Cake */
    .bakery-cake {
      position: relative;
      width: 200px;
      height: 180px;
      margin: 20px 0;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: flex-end;
      cursor: pointer;
    }
    .cake-base {
      width: 170px;
      height: 52px;
      background: #f7a8b8;
      border-radius: 14px;
      position: relative;
      box-shadow: 0 6px 0 #c25975, inset 0 6px 0 rgba(255,255,255,0.4);
    }
    .cake-top-layer {
      width: 130px;
      height: 42px;
      background: #fcd5ce;
      border-radius: 12px;
      margin-bottom: -6px;
      position: relative;
      box-shadow: 0 5px 0 #d88c9a, inset 0 5px 0 rgba(255,255,255,0.5);
      z-index: 2;
    }
    .cream-drip {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 16px;
      background: #fff;
      border-radius: 12px 12px 6px 6px;
      box-shadow: 0 3px 6px rgba(0,0,0,0.1);
    }
    .cake-candle {
      width: 10px;
      height: 38px;
      background: repeating-linear-gradient(45deg, #fff, #fff 5px, #ff477e 5px, #ff477e 10px);
      border-radius: 4px;
      margin-bottom: -4px;
      position: relative;
      z-index: 3;
    }
    .candle-fire {
      width: 12px;
      height: 18px;
      background: radial-gradient(circle, #fff 10%, #ffd000 60%, #ff5400 100%);
      border-radius: 50% 50% 20% 20%;
      position: absolute;
      top: -18px;
      left: -1px;
      box-shadow: 0 0 12px #ffb703, 0 0 25px #fb8500;
      display: none;
      animation: flameWobble 0.7s infinite alternate;
    }
    @keyframes flameWobble {
      0% { transform: scale(1) rotate(-3deg); }
      100% { transform: scale(1.15) rotate(3deg); }
    }

    /* Scene 4: Balloons */
    .balloons-flex {
      display: flex;
      justify-content: center;
      gap: 14px;
      margin: 15px 0;
    }
    .b-item {
      width: 46px;
      height: 60px;
      border-radius: 50% 50% 50% 50% / 40% 40% 60% 60%;
      cursor: pointer;
      position: relative;
      box-shadow: inset -4px -4px 10px rgba(0,0,0,0.35), inset 4px 4px 10px rgba(255,255,255,0.4);
      animation: bBob 2.5s ease-in-out infinite alternate;
      transition: transform 0.2s, opacity 0.3s;
    }
    @keyframes bBob {
      0% { transform: translateY(0); }
      100% { transform: translateY(-10deg); }
    }
    .reasons-scroll {
      width: 100%;
      max-height: 250px;
      overflow-y: auto;
      padding: 5px;
      display: flex;
      flex-direction: column;
      gap: 9px;
    }
    .reason-tile {
      background: var(--card-bg);
      border: 1px solid var(--card-border);
      border-radius: 14px;
      padding: 12px 16px;
      text-align: left;
      font-size: 0.9rem;
      color: #ffe5ec;
      backdrop-filter: blur(10px);
      box-shadow: 0 4px 15px rgba(0,0,0,0.3);
      animation: popIn 0.4s cubic-bezier(0.18, 0.89, 0.32, 1.28);
    }
    .reason-num {
      font-size: 0.75rem;
      letter-spacing: 1px;
      color: var(--gold);
      font-weight: 700;
      text-transform: uppercase;
      margin-bottom: 3px;
    }

    /* Scene 5: Polaroid Gallery */
    .polaroid-frame {
      background: #fdfdfd;
      padding: 12px 12px 30px 12px;
      border-radius: 4px;
      box-shadow: 0 15px 35px rgba(0,0,0,0.7), 0 0 15px rgba(255,255,255,0.1);
      transform: rotate(-2deg);
      cursor: pointer;
      margin: 15px 0;
      transition: transform 0.3s ease;
    }
    .polaroid-frame:active { transform: scale(0.98) rotate(0deg); }
    .polaroid-frame img {
      width: 240px;
      height: 270px;
      object-fit: cover;
      display: block;
      border-radius: 2px;
    }

    /* Scene 6: Envelope & Letter */
    .envelope-box {
      width: 220px;
      height: 140px;
      background: #cca062;
      border-radius: 10px;
      position: relative;
      box-shadow: 0 15px 35px rgba(0,0,0,0.5);
      cursor: pointer;
      display: flex;
      align-items: center;
      justify-content: center;
      margin: 30px 0;
    }
    .stamp-seal {
      width: 44px;
      height: 44px;
      background: #8b1e2f;
      border-radius: 50%;
      border: 2px solid #dfba73;
      display: flex;
      align-items: center;
      justify-content: center;
      color: #dfba73;
      font-weight: bold;
      font-size: 1.1rem;
      box-shadow: 0 4px 10px rgba(0,0,0,0.4);
    }
    .vintage-letter {
      display: none;
      background: rgba(18, 8, 12, 0.95);
      border: 1px solid var(--gold);
      border-radius: 16px;
      padding: 22px;
      text-align: left;
      max-height: 320px;
      overflow-y: auto;
      box-shadow: 0 0 30px rgba(223, 186, 115, 0.2);
    }
    .letter-body {
      font-family: 'Playfair Display', serif;
      font-size: 1rem;
      line-height: 1.7;
      color: #f7e1d7;
      white-space: pre-line;
    }

    .btn-glow {
      background: linear-gradient(135deg, #e63946, #b5179e);
      color: #fff;
      border: none;
      padding: 13px 30px;
      border-radius: 50px;
      font-weight: 700;
      font-size: 0.95rem;
      letter-spacing: 0.5px;
      cursor: pointer;
      box-shadow: 0 6px 20px rgba(230, 57, 70, 0.45);
      margin-top: 15px;
    }
    .btn-glow:active { transform: scale(0.96); }

    .admin-cog {
      position: absolute;
      top: 15px;
      right: 15px;
      background: rgba(255, 255, 255, 0.15);
      border: 1px solid rgba(255, 255, 255, 0.3);
      border-radius: 20px;
      padding: 6px 14px;
      font-size: 0.8rem;
      color: #fff;
      cursor: pointer;
      z-index: 100;
    }

    .admin-panel {
      display: none;
      position: fixed;
      inset: 0;
      background: rgba(0, 0, 0, 0.96);
      z-index: 300;
      overflow-y: auto;
      padding: 25px 20px;
      text-align: left;
    }
    .admin-panel.active { display: block; }
    .admin-panel h3 { color: var(--gold); margin-bottom: 16px; font-size: 1.2rem; }
    .fg { margin-bottom: 12px; }
    .fg label { display: block; font-size: 0.78rem; color: #aaa; margin-bottom: 4px; }
    .fg input, .fg textarea {
      width: 100%;
      padding: 9px;
      background: #191919;
      border: 1px solid #333;
      border-radius: 8px;
      color: #fff;
      font-size: 0.85rem;
    }
    .fg textarea { height: 90px; }
  </style>
</head>
<body>

  <audio id="bgSong" loop preload="none">
    <source src="https://cdn.pixabay.com/download/audio/2022/03/15/audio_c8c8a73467.mp3?filename=romantic-110825.mp3" type="audio/mp3">
  </audio>

  <div class="app-screen">
    <button class="admin-cog" id="adminBtn" onclick="openAdmin()">⚙️ Edit & Share</button>

    <div class="sparkle" style="left:15%; animation-delay:0s;"></div>
    <div class="sparkle" style="left:40%; animation-delay:2s;"></div>
    <div class="sparkle" style="left:75%; animation-delay:4s;"></div>
    <div class="sparkle" style="left:88%; animation-delay:1s;"></div>

    <!-- Scene 1: Tap Anywhere / Tap Heart Starts -->
    <div class="scene active" id="sc1" onclick="startStory(event)">
      <div style="margin-top: 30px;">
        <p class="bottom-hint">SEND THIS 🫶</p>
      </div>
      <div class="s1-heart-wrap">
        <svg class="glow-heart-svg" viewBox="0 0 24 24" fill="#ff4d6d">
          <path d="M12 21.35l-1.45-1.32C5.4 15.36 2 12.28 2 8.5 2 5.42 4.42 3 7.5 3c1.74 0 3.41.81 4.5 2.09C13.09 3.81 14.76 3 16.5 3 19.58 3 22 5.42 22 8.5c0 3.78-3.4 6.86-8.55 11.54L12 21.35z"/>
        </svg>
      </div>
      <p class="bottom-hint">tap anywhere to continue</p>
    </div>

    <!-- Scene 2: Blooming Tree -->
    <div class="scene" id="sc2">
      <div class="tree-stage" onclick="bloomTree()">
        <div class="tree-trunk"></div>
        <div class="tree-crown" id="treeCrown"></div>
      </div>
      <div>
        <p class="script-sub">it's officially your day</p>
        <h1 class="hero-title" id="txtTitleName">Happy Birthday, Tanisha!</h1>
        <p class="bottom-hint" id="txtAge" style="margin-top: 6px;">and just like that, you're turning 21 ✨</p>
      </div>
      <p class="bottom-hint" id="treeHint">tap tree to bloom 🌸</p>
      <button class="btn-glow" id="btnTreeNext" style="display:none;" onclick="goToScene(3)">Keep going 🎂</button>
    </div>

    <!-- Scene 3: Multilayer Cake -->
    <div class="scene" id="sc3">
      <div>
        <p class="script-sub">first things first 🎂</p>
        <h2 style="font-size: 1.4rem; color: #fff;">Baking something sweet...</h2>
      </div>
      <div class="bakery-cake" onclick="lightCake()">
        <div class="cake-candle"><div class="candle-fire" id="candleFlame"></div></div>
        <div class="cake-top-layer" id="cakeTop"><div class="cream-drip"></div></div>
        <div class="cake-base" id="cakeBase"><div class="cream-drip"></div></div>
      </div>
      <div>
        <p class="script-sub" id="cakeStatus">Make a wish & tap the cake 🕯️</p>
        <button class="btn-glow" id="btnCakeNext" style="display:none;" onclick="goToScene(4)">Keep going 🎈</button>
      </div>
      <p class="bottom-hint">tap to celebrate</p>
    </div>

    <!-- Scene 4: Balloons -->
    <div class="scene" id="sc4">
      <div>
        <h2 style="font-size: 1.4rem; color: #fff;">Pop the balloons 🎈</h2>
        <p class="bottom-hint" style="margin-top: 4px;">Each holds a reason you're loved. Pop them all</p>
      </div>
      <div class="balloons-flex">
        <div class="b-item" style="background: radial-gradient(circle at 30% 30%, #ff758f, #a4133c);" onclick="popB(0, this)"></div>
        <div class="b-item" style="background: radial-gradient(circle at 30% 30%, #ffd166, #e76f51);" onclick="popB(1, this)"></div>
        <div class="b-item" style="background: radial-gradient(circle at 30% 30%, #52b788, #1b4332);" onclick="popB(2, this)"></div>
        <div class="b-item" style="background: radial-gradient(circle at 30% 30%, #48cae4, #0077b6);" onclick="popB(3, this)"></div>
        <div class="b-item" style="background: radial-gradient(circle at 30% 30%, #b5179e, #47126b);" onclick="popB(4, this)"></div>
      </div>
      <div class="reasons-scroll" id="reasonsContainer"></div>
      <button class="btn-glow" onclick="goToScene(5)">Memory Lane 📸</button>
    </div>

    <!-- Scene 5: Polaroid Gallery -->
    <div class="scene" id="sc5">
      <div>
        <h2 style="font-size: 1.4rem; color: #fff;">A walk down memory lane</h2>
        <p class="bottom-hint" style="margin-top: 4px;">tap photo for next memory 📁</p>
      </div>
      <div class="polaroid-frame" onclick="nextSlidePhoto()">
        <img id="polaroidImg" src="" alt="Photo Memory">
        <p style="color:#444; font-size:0.8rem; margin-top:12px; font-weight:600;">Tap for next ❤️</p>
      </div>
      <button class="btn-glow" onclick="goToScene(6)">One last thing... 💌</button>
    </div>

    <!-- Scene 6: Envelope & Letter -->
    <div class="scene" id="sc6">
      <div>
        <h2 style="font-size: 1.4rem; color: #fff;" id="letterHeading">One last thing...</h2>
        <p class="script-sub" id="letterSenderTag" style="margin-top: 4px;">A letter for you</p>
      </div>

      <div class="envelope-box" id="envBox" onclick="openEnvelope()">
        <div class="stamp-seal" id="sealInitial">T</div>
      </div>

      <div class="vintage-letter" id="letterModal">
        <p class="letter-body" id="letterBodyText"></p>
        <p style="text-align:right; margin-top:18px; font-weight:700; color:var(--gold);" id="letterSignature"></p>
      </div>

      <button class="btn-glow" onclick="goToScene(1)">Replay ✨</button>
    </div>
  </div>

  <!-- Admin Modal -->
  <div class="admin-panel" id="adminModal">
    <h3>⚙️ Customise & Share Link</h3>
    
    <div class="fg">
      <label>Name:</label>
      <input type="text" id="inName">
    </div>
    <div class="fg">
      <label>Your Name (Sender):</label>
      <input type="text" id="inSender">
    </div>
    <div class="fg">
      <label>Age/Tagline:</label>
      <input type="text" id="inAge">
    </div>
    <div class="fg">
      <label>Cake Theme Color:</label>
      <input type="text" id="inCakeColor" placeholder="#f7a8b8">
    </div>
    <div class="fg">
      <label>Song Direct MP3 Link:</label>
      <input type="text" id="inSong">
    </div>
    <div class="fg">
      <label>Photo 1 URL:</label>
      <input type="text" id="inPhoto1">
    </div>
    <div class="fg">
      <label>Photo 2 URL:</label>
      <input type="text" id="inPhoto2">
    </div>
    <div class="fg">
      <label>Heartfelt Letter:</label>
      <textarea id="inLetter"></textarea>
    </div>

    <button class="btn-glow" style="width:100%; margin-top:10px;" onclick="saveAndGenerateShareLink()">🔗 Save & Copy Share Link</button>
    <button class="btn-glow" style="width:100%; background:#444; margin-top:8px;" onclick="closeAdmin()">Cancel</button>
  </div>

  <script>
    const DEFAULT_DATA = {
      name: "Tanisha",
      sender: "Mahi",
      ageText: "and just like that, you're turning 21 ✨",
      cakeColor: "#f7a8b8",
      songUrl: "https://cdn.pixabay.com/download/audio/2022/03/15/audio_c8c8a73467.mp3?filename=romantic-110825.mp3",
      photos: [
        "https://images.unsplash.com/photo-1534528741775-53994a69daeb?auto=format&fit=crop&w=600&q=80",
        "https://images.unsplash.com/photo-1517841905240-472988babdf9?auto=format&fit=crop&w=600&q=80"
      ],
      reasons: [
        "My worst days get shorter when you call ✨",
        "Your laugh is my favourite sound 💖",
        "The world is kinder with you in it 🌸",
        "You make ordinary days feel special 🍰",
        "You believed in me when I didn't 🌟"
      ],
      letter: "Dear Tanisha,\n\nI keep thinking about how lucky I got with you. You have seen me at my worst and stayed anyway, and I do not say thank you nearly enough for that.\n\nThis year, I hope life is gentle with you... Happy birthday, my favourite person! ❤️"
    };

    let APP_DATA = { ...DEFAULT_DATA };

    const urlParams = new URLSearchParams(window.location.search);
    const dataHash = urlParams.get("v");

    if (dataHash) {
      try {
        const decoded = JSON.parse(decodeURIComponent(escape(atob(dataHash))));
        APP_DATA = { ...DEFAULT_DATA, ...decoded };
        document.getElementById("adminBtn").style.display = "none";
      } catch (err) {}
    } else {
      const saved = localStorage.getItem("bday_store_data");
      if (saved) APP_DATA = { ...DEFAULT_DATA, ...JSON.parse(saved) };
    }

    function renderView() {
      document.getElementById("txtTitleName").innerText = `Happy Birthday, ${APP_DATA.name}!`;
      document.getElementById("txtAge").innerText = APP_DATA.ageText;
      document.getElementById("cakeBase").style.background = APP_DATA.cakeColor;
      document.getElementById("polaroidImg").src = APP_DATA.photos[0] || DEFAULT_DATA.photos[0];
      document.getElementById("letterHeading").innerText = `One last thing, ${APP_DATA.name}...`;
      document.getElementById("letterSenderTag").innerText = `${APP_DATA.sender} wrote you a letter`;
      document.getElementById("sealInitial").innerText = (APP_DATA.name[0] || 'T').toUpperCase();
      document.getElementById("letterBodyText").innerText = APP_DATA.letter;
      document.getElementById("letterSignature").innerText = `— With all my love, ${APP_DATA.sender}`;
      
      const audio = document.getElementById("bgSong");
      if (APP_DATA.songUrl) {
        audio.src = APP_DATA.songUrl;
      }
    }

    renderView();

    function goToScene(num) {
      document.querySelectorAll# _s1_ashish
