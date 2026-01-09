再婚活相談所セレネス
<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover" />
  <title>オリジナルおみくじ</title>
  <style>
    :root{
      --maxw: 860px;
      --pad: 14px;
      --safe-b: env(safe-area-inset-bottom, 0px);
      --safe-t: env(safe-area-inset-top, 0px);

      --btn-h: 56px;
      --btn-r: 14px;

      --shadow: 0 10px 18px rgba(0,0,0,.14);
      --border: 1px solid #ddd;
    }

    *{ box-sizing: border-box; -webkit-tap-highlight-color: transparent; }
    html,body{ height:100%; }
    body{
      font-family: system-ui,-apple-system,BlinkMacSystemFont,"Segoe UI",sans-serif;
      margin: 0;
      background: #f6f6f6;
      color:#111;
    }

    /* アプリ枠 */
    .app{
      max-width: var(--maxw);
      margin: 0 auto;
      min-height: 100vh;
      padding: calc(var(--pad) + var(--safe-t)) var(--pad) calc(var(--pad) + var(--safe-b)) var(--pad);
      display: flex;
      flex-direction: column;
      gap: 12px;
    }

    /* ヘッダー */
    .top-bar{
      display:flex;
      align-items:flex-start;
      justify-content:space-between;
      gap: 8px;
      background:#fff;
      border: var(--border);
      border-radius: 14px;
      padding: 12px 12px;
      box-shadow: 0 4px 12px rgba(0,0,0,.06);
    }
    h1{
      font-size: clamp(18px, 3.6vw, 24px);
      margin:0 0 4px 0;
      line-height: 1.2;
    }
    #subtitle{
      font-size: 12px;
      color:#666;
    }
    .settings-button{
      border:none;
      background: #fff;
      width: 44px;
      height: 44px;
      border-radius: 12px;
      box-shadow: 0 4px 10px rgba(0,0,0,.08);
      cursor:pointer;
      font-size: 20px;
      display:flex;
      align-items:center;
      justify-content:center;
    }

    /* メインカード */
    .main-card{
      background:#fff;
      border: var(--border);
      border-radius: 18px;
      padding: 14px;
      box-shadow: 0 6px 16px rgba(0,0,0,.06);
      display:flex;
      flex-direction: column;
      align-items: center;
      gap: 12px;
    }

    /* おみくじ筒 */
    .tube-area{
      width: 100%;
      display:flex;
      justify-content:center;
      position:relative;
      padding-top: 4px;
    }
    .omikuji-tube{
      position:relative;
      width: min(44vw, 170px);
      aspect-ratio: 13 / 17;
      border-radius: 18px 18px 34px 34px;
      background: linear-gradient(180deg,#f7f7f7,#dedede);
      box-shadow: var(--shadow);
      overflow:hidden;
      transform-origin: 50% 85%;
    }
    .tube-top{
      position:absolute; top: 8%;
      left:50%; transform:translateX(-50%);
      width: 70%;
      height: 10%;
      border-radius: 999px;
      background: linear-gradient(180deg,#cfcfcf,#f0f0f0);
      box-shadow: inset 0 1px 2px rgba(0,0,0,.15);
    }
    .tube-label{
      position:absolute; bottom: 8%;
      left:50%; transform:translateX(-50%);
      padding: 6px 12px;
      background:#b71c1c;
      color:#fff;
      font-size: clamp(12px, 3.4vw, 14px);
      border-radius: 999px;
      letter-spacing: .12em;
      user-select:none;
    }

    /* 光リング */
    .glow-ring{
      position:absolute;
      top: -8px;
      left:50%;
      transform:translateX(-50%);
      width: min(74vw, 320px);
      height: min(22vw, 90px);
      border-radius: 999px;
      border: 2px solid rgba(245,180,0,.55);
      opacity: 0;
      pointer-events:none;
    }
    .glow-ring.active{
      animation: ring-pulse .9s ease-in-out infinite;
      opacity:1;
    }
    @keyframes ring-pulse{
      0%{transform:translateX(-50%) scale(.94); opacity:.22;}
      50%{transform:translateX(-50%) scale(1.02); opacity:.65;}
      100%{transform:translateX(-50%) scale(.94); opacity:.22;}
    }

    /* 筒の揺れ */
    .shake{ animation: tube-shake .55s ease-in-out; }
    @keyframes tube-shake{
      0%{transform:rotate(0deg) translateY(0);}
      15%{transform:rotate(-4deg) translateY(1px);}
      30%{transform:rotate(4deg) translateY(0);}
      45%{transform:rotate(-3deg) translateY(1px);}
      60%{transform:rotate(3deg) translateY(0);}
      75%{transform:rotate(-2deg) translateY(1px);}
      100%{transform:rotate(0deg) translateY(0);}
    }

    /* おみくじ棒 */
    .omikuji-stick{
      position:absolute;
      left:50%;
      bottom:-95%;
      transform:translateX(-50%);
      width: 70%;
      height: 92%;
      background:#fdfaf0;
      border-radius: 12px;
      border: 1px solid #d7ccc8;
      box-shadow: 0 3px 8px rgba(0,0,0,.16);
      display:flex;
      align-items:center;
      justify-content:center;
      padding: 10px;
      text-align:center;
      font-size: clamp(16px, 4.2vw, 18px);
      writing-mode: vertical-rl;
      text-orientation: upright;
      user-select:none;
    }
    .omikuji-stick.stick-pop{
      animation: stick-pop .95s cubic-bezier(.16,1.15,.22,1) forwards;
    }
    @keyframes stick-pop{
      0%{bottom:-95%; transform:translateX(-50%) scale(.98);}
      55%{bottom:30%; transform:translateX(-50%) scale(1.03);}
      78%{bottom:10%; transform:translateX(-50%) scale(1.00);}
      100%{bottom:14%; transform:translateX(-50%) scale(1.00);}
    }

    /* 結果カード */
    #result{
      width: 100%;
      text-align:center;
      font-size: clamp(15px, 3.9vw, 18px);
      padding: 12px;
      border-radius: 14px;
      border: 1px solid #ccc;
      background:#fafafa;
      min-height: 2.8em;
      display:flex;
      align-items:center;
      justify-content:center;
      line-height: 1.4;
    }

    /* ランク色（棒＋本文） */
    .rank-daikichi{border-color:#f5b400;background:#fff9e0; box-shadow:0 0 10px rgba(245,180,0,.55), 0 3px 8px rgba(0,0,0,.16);}
    .rank-chukichi{border-color:#4caf50;background:#f1f8e9;}
    .rank-shokichi{border-color:#1976d2;background:#e3f2fd;}
    .rank-kichi{border-color:#9c27b0;background:#f3e5f5;}
    .rank-suekichi{border-color:#ff7043;background:#fbe9e7;}
    .rank-kyo{border-color:#9e9e9e;background:#eeeeee;}

    /* 操作ボタン（固定フッター風） */
    .bottom-actions{
      position: sticky;
      bottom: 0;
      padding: 10px 0 calc(10px + var(--safe-b)) 0;
      background: linear-gradient(180deg, rgba(246,246,246,0), rgba(246,246,246,1) 40%);
      display:flex;
      justify-content:center;
    }
    #drawButton{
      width: 100%;
      max-width: 520px;
      height: var(--btn-h);
      border-radius: var(--btn-r);
      border: none;
      font-size: 18px;
      font-weight: 700;
      background:#f5b400;
      cursor:pointer;
      box-shadow: var(--shadow);
    }
    #drawButton:active{ transform: translateY(1px); }
    #drawButton:disabled{ opacity:.7; cursor:default; }

    /* ===== 設定パネル ===== */
    #settingsPanel{
      display:none;
      background:#fff;
      border: var(--border);
      border-radius: 18px;
      padding: 14px;
      box-shadow: 0 10px 18px rgba(0,0,0,.10);
    }
    .section{ margin-top: 14px; }
    .section-title{ font-weight: 800; margin-bottom: 6px; font-size: 14px; }
    small{ color:#666; display:block; line-height:1.35; }

    .row{ display:flex; flex-wrap:wrap; align-items:center; gap:8px; }
    textarea{
      width:100%;
      height: 180px;
      margin-top: 8px;
      font-family: inherit;
      font-size: 16px; /* iOSズーム防止 */
      border-radius: 12px;
      border: 1px solid #ccc;
      padding: 10px;
    }
    input[type="text"], input[type="number"], input[type="password"]{
      padding: 10px 10px;
      font-size: 16px; /* iOSズーム防止 */
      border-radius: 12px;
      border: 1px solid #ccc;
      width: 100%;
      max-width: 420px;
    }
    select{
      padding: 10px 10px;
      font-size: 16px;
      border-radius: 12px;
      border: 1px solid #ccc;
    }

    .btn{
      height: 44px;
      padding: 0 12px;
      border-radius: 12px;
      border: none;
      font-size: 14px;
      font-weight: 700;
      cursor:pointer;
    }
    .btn.green{ background:#4caf50; color:#fff; }
    .btn.blue{ background:#1976d2; color:#fff; }
    .btn.red{ background:#e53935; color:#fff; }
    .btn.purple{ background:#9c27b0; color:#fff; }

    .prob-grid{
      display:grid;
      grid-template-columns: repeat(2, minmax(0, 1fr));
      gap: 10px;
      margin-top: 10px;
    }
    @media (min-width: 640px){
      .prob-grid{ grid-template-columns: repeat(3, minmax(0, 1fr)); }
    }
    .prob-item{ display:flex; align-items:center; gap:6px; }
    .prob-item label{ min-width: 42px; font-weight: 700; }

    /* トグル */
    .toggle{
      display:flex; align-items:center; gap:10px; margin-top: 10px;
      padding: 10px; border: 1px solid #ddd; border-radius: 14px;
      background:#fafafa;
    }
    .toggle input{ width: 20px; height: 20px; }

    /* キラ粒子・フラッシュ・紙吹雪 */
    .sparkle{
      position:fixed; width:6px; height:6px; border-radius:999px;
      background:rgba(245,180,0,.9); pointer-events:none; z-index:9998;
      animation: sparkle-float 1.1s ease-out forwards;
    }
    @keyframes sparkle-float{
      0%{transform:translate3d(0,0,0) scale(1); opacity:0;}
      15%{opacity:1;}
      100%{transform:translate3d(var(--dx), var(--dy), 0) scale(.2); opacity:0;}
    }
    .flash{
      position:fixed; inset:0;
      background:radial-gradient(circle at 50% 35%, rgba(255,215,64,.65), rgba(255,215,64,0) 55%);
      opacity:0; pointer-events:none; z-index:9997;
    }
    .flash.active{ animation: flash-pop .55s ease-out; }
    @keyframes flash-pop{
      0%{opacity:0;} 25%{opacity:.95;} 100%{opacity:0;}
    }
    .confetti-piece{
      position:fixed; top:-10px; width:7px; height:14px; opacity:.95;
      animation:confetti-fall 1.7s linear forwards;
      z-index:9999; pointer-events:none;
    }
    @keyframes confetti-fall{
      0%{transform:translate3d(0,0,0) rotateZ(0deg); opacity:1;}
      100%{transform:translate3d(var(--drift),110vh,0) rotateZ(420deg); opacity:0;}
    }
  </style>
</head>

<body>
  <div class="app">
    <div class="top-bar">
      <div class="title-block">
        <h1 id="appTitle">オリジナルおみくじ</h1>
        <div id="subtitle">右上の歯車は運営用です</div>
      </div>
      <button id="settingsToggleButton" class="settings-button" title="運営設定">⚙️</button>
    </div>

    <div class="main-card">
      <div class="tube-area">
        <div id="glowRing" class="glow-ring"></div>
        <div id="tube" class="omikuji-tube" aria-label="おみくじ筒">
          <div class="tube-top"></div>
          <div class="tube-label">おみくじ</div>
          <div id="omikujiStick" class="omikuji-stick"><span id="stickText">おみくじ</span></div>
        </div>
      </div>

      <div id="result" role="status" aria-live="polite">ここに結果が表示されます</div>
    </div>

    <!-- 運営設定（パスコード制） -->
    <div id="settingsPanel">
      <div class="section">
        <div class="section-title">運営ログイン</div>
        <small>パスコードを入力すると設定が開きます（ユーザーに見せない用）</small>
        <div class="row" style="margin-top:8px;">
          <input id="adminPassInput" type="password" placeholder="パスコード（初期：1234）" />
          <button id="adminUnlockBtn" class="btn blue">開く</button>
          <button id="adminLockBtn" class="btn red" style="display:none;">閉じる</button>
        </div>
        <small style="margin-top:6px;">※ 初期パスは 1234（後で変更できます）</small>
      </div>

      <div id="adminArea" style="display:none;">
        <div class="toggle">
          <input type="checkbox" id="soundToggle" />
          <div>
            <div style="font-weight:800;">効果音を鳴らす</div>
            <small>スマホでもボタンを押した時のみ再生されます</small>
          </div>
        </div>

        <div class="section">
          <div class="section-title">おみくじセットの管理</div>
          <div class="row">
            <label for="setSelect" style="font-weight:800;">セット：</label>
            <select id="setSelect"></select>
            <button id="deleteSetButton" class="btn red">削除</button>
          </div>
          <div class="row" style="margin-top:8px;">
            <input type="text" id="newSetNameInput" placeholder="新しいセット名" />
            <button id="addSetButton" class="btn blue">追加</button>
          </div>
          <small>セットごとに中身と確率を別保存できます</small>
        </div>

        <div class="section">
          <div class="section-title">このセットのおみくじの中身</div>
          <small>1行=1結果。先頭に「大吉：」「中吉：」などを付けると確率が反映されます。</small>
          <textarea id="itemsInput"></textarea>
          <div class="row">
            <button id="saveItemsButton" class="btn green">内容を保存</button>
          </div>
        </div>

        <div class="section">
          <div class="section-title">このセットの確率</div>
          <small>合計100%にしてください</small>
          <div class="prob-grid">
            <div class="prob-item"><label>大吉</label><input type="number" id="prob-daikichi" min="0" max="100">%</div>
            <div class="prob-item"><label>中吉</label><input type="number" id="prob-chukichi" min="0" max="100">%</div>
            <div class="prob-item"><label>小吉</label><input type="number" id="prob-shokichi" min="0" max="100">%</div>
            <div class="prob-item"><label>吉</label><input type="number" id="prob-kichi" min="0" max="100">%</div>
            <div class="prob-item"><label>末吉</label><input type="number" id="prob-suekichi" min="0" max="100">%</div>
            <div class="prob-item"><label>凶</label><input type="number" id="prob-kyo" min="0" max="100">%</div>
          </div>
          <small style="margin-top:6px;">合計：<span id="probTotal">0</span>%</small>
          <div class="row" style="margin-top:8px;">
            <button id="saveProbButton" class="btn purple">確率を保存</button>
          </div>
        </div>

        <div class="section">
          <div class="section-title">表示設定</div>
          <div class="row">
            <input type="text" id="titleInput" placeholder="タイトル" />
            <button id="saveSettingsButton" class="btn blue">保存</button>
          </div>
        </div>

        <div class="section">
          <div class="section-title">パスコード変更</div>
          <small>次回からこのパスで設定が開きます</small>
          <div class="row" style="margin-top:8px;">
            <input id="newPassInput" type="password" placeholder="新しいパスコード" />
            <button id="savePassBtn" class="btn blue">変更</button>
          </div>
        </div>
      </div>
    </div>

    <div class="bottom-actions">
      <button id="drawButton">おみくじを引く</button>
    </div>
  </div>

  <!-- 効果音（同フォルダに置く） -->
  <audio id="soundShake" src="shake.mp3" preload="auto"></audio>
  <audio id="soundPop" src="pop.mp3" preload="auto"></audio>
  <audio id="soundDaikichi" src="daikichi.mp3" preload="auto"></audio>

  <div id="flash" class="flash"></div>

  <script>
    // ===== 保存キー =====
    const STORAGE_KEY_SETS = "custom_omikuji_sets_v4_mobile";
    const STORAGE_KEY_SETTINGS = "custom_omikuji_settings_v4_mobile";
    const STORAGE_KEY_ADMIN = "custom_omikuji_admin_v4_mobile";

    const RANKS = [
      { key: "大吉", inputId: "prob-daikichi" },
      { key: "中吉", inputId: "prob-chukichi" },
      { key: "小吉", inputId: "prob-shokichi" },
      { key: "吉",   inputId: "prob-kichi" },
      { key: "末吉", inputId: "prob-suekichi" },
      { key: "凶",   inputId: "prob-kyo" }
    ];

    const defaultSetName = "基本おみくじ";
    const defaultItems = [
      "大吉：今年は新しい出会いがたくさん！",
      "中吉：肩の力をぬくと、思わぬチャンスが",
      "小吉：少しずつ進めば、ちゃんと前に進んでる",
      "吉：自分を大事にすると、いいご縁が近づく",
      "末吉：焦らず、休みながらいきましょう",
      "凶：今日はゆっくり休むのが吉"
    ];
    const defaultWeights = { "大吉":10,"中吉":30,"小吉":30,"吉":20,"末吉":10,"凶":0 };

    // ===== DOM =====
    const resultDiv = document.getElementById("result");
    const drawButton = document.getElementById("drawButton");

    const tube = document.getElementById("tube");
    const glowRing = document.getElementById("glowRing");
    const stickElement = document.getElementById("omikujiStick");
    const stickText = document.getElementById("stickText");
    const flash = document.getElementById("flash");

    const settingsToggleButton = document.getElementById("settingsToggleButton");
    const settingsPanel = document.getElementById("settingsPanel");

    // admin login
    const adminPassInput = document.getElementById("adminPassInput");
    const adminUnlockBtn = document.getElementById("adminUnlockBtn");
    const adminLockBtn = document.getElementById("adminLockBtn");
    const adminArea = document.getElementById("adminArea");

    const newPassInput = document.getElementById("newPassInput");
    const savePassBtn = document.getElementById("savePassBtn");
    const soundToggle = document.getElementById("soundToggle");

    // settings ui
    const appTitle = document.getElementById("appTitle");
    const titleInput = document.getElementById("titleInput");
    const saveSettingsButton = document.getElementById("saveSettingsButton");

    const setSelect = document.getElementById("setSelect");
    const addSetButton = document.getElementById("addSetButton");
    const deleteSetButton = document.getElementById("deleteSetButton");
    const newSetNameInput = document.getElementById("newSetNameInput");

    const itemsInput = document.getElementById("itemsInput");
    const saveItemsButton = document.getElementById("saveItemsButton");

    const saveProbButton = document.getElementById("saveProbButton");
    const probTotalSpan = document.getElementById("probTotal");

    // sounds
    const soundShake = document.getElementById("soundShake");
    const soundPop = document.getElementById("soundPop");
    const soundDaikichi = document.getElementById("soundDaikichi");

    function playSound(audio){
      if(!audio) return;
      if(!admin.soundOn) return;
      audio.currentTime = 0;
      audio.play().catch(()=>{});
    }

    // ===== データ =====
    let sets = {};
    let settings = { title:"オリジナルおみくじ", currentSet: defaultSetName };
    let admin = { pass:"1234", unlocked:false, soundOn:true };

    function loadAdmin(){
      const saved = localStorage.getItem(STORAGE_KEY_ADMIN);
      if(saved){
        try{ admin = Object.assign(admin, JSON.parse(saved)); }catch(e){}
      }
      soundToggle.checked = !!admin.soundOn;
    }
    function saveAdmin(){
      localStorage.setItem(STORAGE_KEY_ADMIN, JSON.stringify({
        pass: admin.pass,
        soundOn: admin.soundOn
      }));
    }

    function loadSettings(){
      const saved = localStorage.getItem(STORAGE_KEY_SETTINGS);
      if(saved){
        try{ settings = Object.assign(settings, JSON.parse(saved)); }catch(e){}
      }
      appTitle.textContent = settings.title;
      titleInput.value = settings.title;
    }
    function saveSettings(){
      localStorage.setItem(STORAGE_KEY_SETTINGS, JSON.stringify(settings));
    }

    function ensureSetData(name){
      let data = sets[name];
      if(!data){
        data = { items: [], weights: Object.assign({}, defaultWeights) };
        sets[name] = data;
      }else if(Array.isArray(data)){
        data = { items: data, weights: Object.assign({}, defaultWeights) };
        sets[name] = data;
      }else{
        if(!Array.isArray(data.items)) data.items = [];
        if(!data.weights) data.weights = Object.assign({}, defaultWeights);
      }
      return data;
    }

    function loadSets(){
      const saved = localStorage.getItem(STORAGE_KEY_SETS);
      if(saved){
        try{
          const obj = JSON.parse(saved);
          if(obj && typeof obj === "object") sets = obj;
        }catch(e){}
      }
      if(Object.keys(sets).length === 0){
        sets[defaultSetName] = { items: defaultItems.slice(), weights: Object.assign({}, defaultWeights) };
      }
      Object.keys(sets).forEach(n=>ensureSetData(n));
      if(!settings.currentSet || !sets[settings.currentSet]) settings.currentSet = Object.keys(sets)[0];
    }
    function saveSets(){ localStorage.setItem(STORAGE_KEY_SETS, JSON.stringify(sets)); }

    function refreshSetSelect(){
      setSelect.innerHTML = "";
      Object.keys(sets).forEach(name=>{
        const opt = document.createElement("option");
        opt.value = name;
        opt.textContent = name;
        setSelect.appendChild(opt);
      });
      setSelect.value = settings.currentSet;
      loadItemsToTextarea();
      loadProbabilitiesUI();
    }

    function loadItemsToTextarea(){
      const data = ensureSetData(settings.currentSet);
      itemsInput.value = (data.items || []).join("\n");
    }
    function getItemsFromTextarea(){
      return itemsInput.value.split("\n").map(s=>s.trim()).filter(s=>s.length>0);
    }

    function loadProbabilitiesUI(){
      const data = ensureSetData(settings.currentSet);
      const weights = data.weights || defaultWeights;
      let total = 0;
      RANKS.forEach(r=>{
        const input = document.getElementById(r.inputId);
        const val = (weights[r.key] != null) ? weights[r.key] : defaultWeights[r.key];
        input.value = val;
        total += Number(val) || 0;
      });
      probTotalSpan.textContent = total;
    }
    function updateProbTotalLabel(){
      let total = 0;
      RANKS.forEach(r=>{
        total += Number(document.getElementById(r.inputId).value) || 0;
      });
      probTotalSpan.textContent = total;
    }
    RANKS.forEach(r=>{
      document.getElementById(r.inputId).addEventListener("input", updateProbTotalLabel);
    });

    // ===== 演出 =====
    function resetEffects(){
      resultDiv.className = "";
      stickElement.className = "omikuji-stick";
      stickElement.style.bottom = "-95%";
      stickElement.classList.remove("stick-pop");
      glowRing.classList.remove("active");
      tube.classList.remove("shake");
      flash.classList.remove("active");
    }

    function classForRank(rank){
      if(rank==="大吉") return "rank-daikichi";
      if(rank==="中吉") return "rank-chukichi";
      if(rank==="小吉") return "rank-shokichi";
      if(rank==="吉") return "rank-kichi";
      if(rank==="末吉") return "rank-suekichi";
      if(rank==="凶") return "rank-kyo";
      return "";
    }

    function launchSparkles(centerX, centerY, power){
      const count = power ? 34 : 18;
      for(let i=0;i<count;i++){
        const s=document.createElement("div");
        s.className="sparkle";
        const ang=Math.random()*Math.PI*2;
        const dist=(power?160:110)*(0.25+Math.random()*0.75);
        const dx=Math.cos(ang)*dist;
        const dy=Math.sin(ang)*dist - (power?40:20);
        s.style.left = (centerX + (Math.random()*20-10)) + "px";
        s.style.top  = (centerY + (Math.random()*20-10)) + "px";
        s.style.setProperty("--dx", dx.toFixed(0)+"px");
        s.style.setProperty("--dy", dy.toFixed(0)+"px");
        document.body.appendChild(s);
        setTimeout(()=>s.remove(), 1200);
      }
    }

    function doFlash(){
      flash.classList.remove("active");
      void flash.offsetWidth;
      flash.classList.add("active");
    }

    function launchConfetti(power){
      const colors=["#f5b400","#ff7043","#4caf50","#42a5f5","#ab47bc","#ffeb3b"];
      const count = power ? 120 : 70;
      for(let i=0;i<count;i++){
        const p=document.createElement("div");
        p.className="confetti-piece";
        const drift = (Math.random()*120 - 60).toFixed(0) + "px";
        p.style.left = (Math.random()*100) + "vw";
        p.style.background = colors[Math.floor(Math.random()*colors.length)];
        p.style.animationDelay = (Math.random()*0.35) + "s";
        p.style.setProperty("--drift", drift);
        document.body.appendChild(p);
        setTimeout(()=>p.remove(), 1900);
      }
    }

    function showResult(chosenResult, chosenRank){
      let bodyText = chosenResult;
      const idx = bodyText.indexOf("：")>=0 ? bodyText.indexOf("：") : bodyText.indexOf(":");
      if(idx>0) bodyText = bodyText.slice(idx+1).trim();

      const rankClass = classForRank(chosenRank);
      if(rankClass){
        resultDiv.classList.add(rankClass);
        stickElement.classList.add(rankClass);
      }

      stickText.textContent = chosenRank || "おみくじ";
      resultDiv.textContent = chosenRank ? (chosenRank + "： " + bodyText) : bodyText;

      // 棒ポップ
      stickElement.style.bottom = "-95%";
      void stickElement.offsetWidth;
      stickElement.classList.add("stick-pop");

      // 音
      playSound(soundPop);

      // 粒子
      const rect = tube.getBoundingClientRect();
      const cx = rect.left + rect.width/2;
      const cy = rect.top + rect.height*0.25;

      const isDaikichi = chosenRank === "大吉";
      launchSparkles(cx, cy, isDaikichi);

      if(isDaikichi){
        doFlash();
        launchConfetti(true);
        playSound(soundDaikichi);
      }else if(chosenRank === "中吉"){
        launchConfetti(false);
      }
    }

    function pickResult(items, weights){
      const groups={};
      RANKS.forEach(r=>groups[r.key]=[]);
      const others=[];

      items.forEach(line=>{
        let idx=line.indexOf("：");
        if(idx===-1) idx=line.indexOf(":");
        if(idx>0){
          const prefix=line.slice(0,idx).trim();
          if(groups[prefix]) groups[prefix].push(line);
          else others.push(line);
        }else{
          others.push(line);
        }
      });

      const active=[];
      RANKS.forEach(r=>{
        const w=(typeof weights[r.key]==="number")?weights[r.key]:defaultWeights[r.key];
        if(w>0 && groups[r.key].length>0) active.push({rank:r.key, weight:w});
      });

      if(active.length===0){
        const i=Math.floor(Math.random()*items.length);
        return {rank:null, text:items[i]};
      }

      const total=active.reduce((s,a)=>s+a.weight,0);
      let rnd=Math.random()*total;
      let chosen=active[0].rank;
      for(const a of active){
        if(rnd < a.weight){ chosen=a.rank; break; }
        rnd -= a.weight;
      }
      const options=groups[chosen];
      const i=Math.floor(Math.random()*options.length);
      return {rank:chosen, text:options[i]};
    }

    // ===== ボタン =====
    drawButton.addEventListener("click", ()=>{
      const data = ensureSetData(settings.currentSet);
      const items = getItemsFromTextarea();
      if(items.length===0){
        alert("中身が空です。設定から入力してください。");
        return;
      }
      data.items = items;
      saveSets();

      drawButton.disabled = true;
      resetEffects();

      // 抽選中演出
      tube.classList.add("shake");
      glowRing.classList.add("active");
      resultDiv.textContent = "…抽選中…";
      playSound(soundShake);

      const picked = pickResult(items, data.weights || defaultWeights);

      setTimeout(()=>{
        glowRing.classList.remove("active");
        showResult(picked.text, picked.rank);
        drawButton.disabled = false;
      }, 650);
    });

    // ===== 設定画面開閉（外側はパスが必要） =====
    let panelVisible = false;
    settingsToggleButton.addEventListener("click", ()=>{
      panelVisible = !panelVisible;
      settingsPanel.style.display = panelVisible ? "block" : "none";
      if(!panelVisible){
        admin.unlocked = false;
        adminArea.style.display = "none";
        adminLockBtn.style.display = "none";
        adminPassInput.value = "";
      }
    });

    // admin unlock
    adminUnlockBtn.addEventListener("click", ()=>{
      const pass = adminPassInput.value.trim();
      if(pass && pass === admin.pass){
        admin.unlocked = true;
        adminArea.style.display = "block";
        adminLockBtn.style.display = "inline-flex";
        adminPassInput.value = "";
      }else{
        alert("パスコードが違います");
      }
    });
    adminLockBtn.addEventListener("click", ()=>{
      admin.unlocked = false;
      adminArea.style.display = "none";
      adminLockBtn.style.display = "none";
      adminPassInput.value = "";
    });

    // sound toggle
    soundToggle.addEventListener("change", ()=>{
      admin.soundOn = !!soundToggle.checked;
      saveAdmin();
    });

    // change pass
    savePassBtn.addEventListener("click", ()=>{
      const np = newPassInput.value.trim();
      if(!np){
        alert("新しいパスコードを入力してください");
        return;
      }
      admin.pass = np;
      newPassInput.value = "";
      saveAdmin();
      alert("パスコードを変更しました");
    });

    // set mgmt
    saveItemsButton.addEventListener("click", ()=>{
      const name=settings.currentSet;
      const data=ensureSetData(name);
      data.items=getItemsFromTextarea();
      saveSets();
      alert("内容を保存しました");
    });
    addSetButton.addEventListener("click", ()=>{
      const raw=newSetNameInput.value.trim();
      if(!raw){ alert("セット名を入力してください"); return; }
      if(sets[raw]){ alert("同名セットがあります"); return; }
      sets[raw]={ items:[], weights:Object.assign({}, defaultWeights) };
      settings.currentSet=raw;
      saveSets(); saveSettings();
      newSetNameInput.value="";
      refreshSetSelect();
      alert("セットを作成しました");
    });
    deleteSetButton.addEventListener("click", ()=>{
      const keys=Object.keys(sets);
      if(keys.length<=1){ alert("最後の1つは削除できません"); return; }
      if(!confirm("このセットを削除しますか？")) return;
      delete sets[settings.currentSet];
      settings.currentSet = Object.keys(sets)[0];
      saveSets(); saveSettings();
      refreshSetSelect();
      resetEffects();
      stickText.textContent="おみくじ";
      resultDiv.textContent="ここに結果が表示されます";
    });
    setSelect.addEventListener("change", ()=>{
      settings.currentSet = setSelect.value;
      saveSettings();
      loadItemsToTextarea();
      loadProbabilitiesUI();
    });

    saveSettingsButton.addEventListener("click", ()=>{
      const t=titleInput.value.trim() || "オリジナルおみくじ";
      settings.title=t;
      appTitle.textContent=t;
      saveSettings();
      alert("保存しました");
    });

    saveProbButton.addEventListener("click", ()=>{
      const data=ensureSetData(settings.currentSet);
      const newW={}; let total=0;
      RANKS.forEach(r=>{
        const input=document.getElementById(r.inputId);
        let v=parseInt(input.value,10);
        if(isNaN(v)||v<0) v=0;
        if(v>100) v=100;
        input.value=v;
        newW[r.key]=v;
        total += v;
      });
      probTotalSpan.textContent=total;
      if(total!==100){
        alert("合計を100%にしてください（現在："+total+"%）");
        return;
      }
      data.weights=newW;
      saveSets();
      alert("確率を保存しました");
    });

    // ===== 初期化 =====
    loadAdmin();
    loadSettings();
    loadSets();
    refreshSetSelect();
    updateProbTotalLabel();
  </script>
</body>
</html>
