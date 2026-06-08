<!DOCTYPE html>
<html lang="vi">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>PromptVault — AI Roleplay Hub</title>
<link href="https://fonts.googleapis.com/css2?family=Cinzel+Decorative:wght@400;700&family=Crimson+Pro:ital,wght@0,300;0,400;0,600;1,300;1,400&family=JetBrains+Mono:wght@300;400&display=swap" rel="stylesheet">
<style>
  :root {
    --bg: #0a0a0f;
    --bg2: #111118;
    --bg3: #18181f;
    --card: #1a1a24;
    --card2: #20202e;
    --border: #2a2a3a;
    --border2: #35354a;
    --gold: #c9a84c;
    --gold2: #e8c96a;
    --gold3: #f5dea0;
    --accent: #7b5ea7;
    --accent2: #9b7ec8;
    --rose: #c96a7b;
    --teal: #4a9b8e;
    --text: #e8e0d0;
    --text2: #a09888;
    --text3: #6a6258;
    --radius: 12px;
    --glow: 0 0 20px rgba(201,168,76,0.15);
    --glow2: 0 0 40px rgba(201,168,76,0.08);
  }

  * { box-sizing: border-box; margin: 0; padding: 0; }
  
  body {
    background: var(--bg);
    color: var(--text);
    font-family: 'Crimson Pro', Georgia, serif;
    font-size: 16px;
    min-height: 100vh;
    overflow-x: hidden;
  }

  /* ── NOISE TEXTURE OVERLAY ── */
  body::before {
    content: '';
    position: fixed;
    inset: 0;
    background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)' opacity='0.035'/%3E%3C/svg%3E");
    pointer-events: none;
    z-index: 0;
    opacity: 0.4;
  }

  /* ── ANIMATED BG ORBS ── */
  .bg-orbs {
    position: fixed;
    inset: 0;
    pointer-events: none;
    z-index: 0;
    overflow: hidden;
  }
  .orb {
    position: absolute;
    border-radius: 50%;
    filter: blur(80px);
    opacity: 0.06;
    animation: drift 20s ease-in-out infinite;
  }
  .orb1 { width: 600px; height: 600px; background: var(--gold); top: -200px; left: -100px; animation-delay: 0s; }
  .orb2 { width: 400px; height: 400px; background: var(--accent); bottom: -100px; right: -100px; animation-delay: -7s; }
  .orb3 { width: 300px; height: 300px; background: var(--teal); top: 40%; left: 60%; animation-delay: -13s; }
  @keyframes drift {
    0%, 100% { transform: translate(0, 0) scale(1); }
    33% { transform: translate(30px, -20px) scale(1.05); }
    66% { transform: translate(-20px, 30px) scale(0.95); }
  }

  /* ── LAYOUT ── */
  #app { position: relative; z-index: 1; }

  /* ── HEADER ── */
  header {
    padding: 32px 40px 24px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    border-bottom: 1px solid var(--border);
    backdrop-filter: blur(10px);
    background: rgba(10,10,15,0.7);
    position: sticky;
    top: 0;
    z-index: 100;
  }
  .logo-wrap { display: flex; align-items: center; gap: 14px; }
  .logo-icon {
    width: 40px; height: 40px;
    background: linear-gradient(135deg, var(--gold), var(--accent));
    border-radius: 10px;
    display: flex; align-items: center; justify-content: center;
    font-size: 20px;
    box-shadow: 0 0 20px rgba(201,168,76,0.3);
  }
  .logo-text {
    font-family: 'Cinzel Decorative', serif;
    font-size: 22px;
    font-weight: 700;
    background: linear-gradient(135deg, var(--gold2), var(--gold3));
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
    letter-spacing: 1px;
  }
  .logo-sub { font-size: 11px; color: var(--text3); font-family: 'JetBrains Mono', monospace; letter-spacing: 2px; text-transform: uppercase; }
  
  .header-actions { display: flex; gap: 10px; align-items: center; }
  
  /* ── BUTTONS ── */
  .btn {
    display: inline-flex; align-items: center; gap: 7px;
    padding: 9px 18px;
    border-radius: 8px;
    border: 1px solid var(--border2);
    background: var(--card);
    color: var(--text);
    font-family: 'Crimson Pro', serif;
    font-size: 14px;
    cursor: pointer;
    transition: all 0.2s;
    text-decoration: none;
  }
  .btn:hover { background: var(--card2); border-color: var(--gold); color: var(--gold2); }
  .btn-gold {
    background: linear-gradient(135deg, rgba(201,168,76,0.2), rgba(201,168,76,0.1));
    border-color: var(--gold);
    color: var(--gold2);
  }
  .btn-gold:hover { background: linear-gradient(135deg, rgba(201,168,76,0.35), rgba(201,168,76,0.2)); box-shadow: var(--glow); }
  .btn-danger { border-color: #a03030; color: #e06060; background: rgba(160,48,48,0.1); }
  .btn-danger:hover { background: rgba(160,48,48,0.2); border-color: #c04040; }
  .btn-sm { padding: 6px 12px; font-size: 13px; }
  .btn-icon { padding: 9px; }

  /* ── MAIN CONTENT ── */
  main { padding: 32px 40px; max-width: 1400px; margin: 0 auto; }

  /* ── SEARCH & FILTER BAR ── */
  .filter-bar {
    display: flex; gap: 12px; align-items: center;
    margin-bottom: 24px;
    flex-wrap: wrap;
  }
  .search-wrap {
    position: relative; flex: 1; min-width: 200px;
  }
  .search-wrap .icon {
    position: absolute; left: 14px; top: 50%; transform: translateY(-50%);
    color: var(--text3); font-size: 15px; pointer-events: none;
  }
  input[type="text"], input[type="password"], textarea, select {
    background: var(--card);
    border: 1px solid var(--border2);
    border-radius: 8px;
    color: var(--text);
    font-family: 'Crimson Pro', serif;
    font-size: 15px;
    padding: 10px 14px;
    width: 100%;
    transition: border-color 0.2s, box-shadow 0.2s;
    outline: none;
  }
  input[type="text"]:focus, input[type="password"]:focus, textarea:focus, select:focus {
    border-color: var(--gold);
    box-shadow: 0 0 0 3px rgba(201,168,76,0.1);
  }
  .search-wrap input { padding-left: 42px; }
  select { appearance: none; cursor: pointer; }

  /* ── TAG PILLS ROW ── */
  .tags-row {
    display: flex; gap: 8px; flex-wrap: wrap;
    margin-bottom: 28px;
    align-items: center;
  }
  .tag-filter {
    padding: 5px 14px;
    border-radius: 20px;
    border: 1px solid var(--border2);
    background: transparent;
    color: var(--text2);
    font-family: 'Crimson Pro', serif;
    font-size: 13px;
    cursor: pointer;
    transition: all 0.18s;
    letter-spacing: 0.3px;
  }
  .tag-filter:hover { border-color: var(--gold); color: var(--gold2); }
  .tag-filter.active { background: rgba(201,168,76,0.15); border-color: var(--gold); color: var(--gold2); }
  .tag-filter.genre-tag.active { background: rgba(123,94,167,0.2); border-color: var(--accent2); color: var(--accent2); }

  /* ── SECTION HEADER ── */
  .section-header {
    display: flex; align-items: baseline; gap: 12px;
    margin-bottom: 20px;
  }
  .section-title {
    font-family: 'Cinzel Decorative', serif;
    font-size: 16px;
    color: var(--gold2);
    letter-spacing: 1px;
  }
  .count-badge {
    font-family: 'JetBrains Mono', monospace;
    font-size: 11px;
    color: var(--text3);
    background: var(--bg3);
    border: 1px solid var(--border);
    padding: 2px 8px;
    border-radius: 20px;
  }

  /* ── GRID ── */
  .prompts-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(320px, 1fr));
    gap: 16px;
  }

  /* ── PROMPT CARD ── */
  .prompt-card {
    background: var(--card);
    border: 1px solid var(--border);
    border-radius: var(--radius);
    padding: 20px;
    cursor: pointer;
    transition: all 0.25s;
    position: relative;
    overflow: hidden;
    animation: fadeUp 0.4s ease both;
  }
  .prompt-card::before {
    content: '';
    position: absolute;
    inset: 0;
    border-radius: var(--radius);
    opacity: 0;
    background: linear-gradient(135deg, rgba(201,168,76,0.06), transparent);
    transition: opacity 0.3s;
    pointer-events: none;
  }
  .prompt-card:hover { border-color: var(--border2); transform: translateY(-2px); box-shadow: 0 8px 30px rgba(0,0,0,0.3), var(--glow2); }
  .prompt-card:hover::before { opacity: 1; }
  .prompt-card.locked .card-content { filter: blur(3px); user-select: none; pointer-events: none; }

  .card-top {
    display: flex; align-items: flex-start; justify-content: space-between;
    margin-bottom: 10px; gap: 10px;
  }
  .card-name {
    font-family: 'Cinzel Decorative', serif;
    font-size: 14px;
    color: var(--gold3);
    font-weight: 400;
    line-height: 1.4;
    flex: 1;
  }
  .card-badges { display: flex; gap: 6px; align-items: center; flex-shrink: 0; }
  .lock-badge {
    width: 28px; height: 28px;
    border-radius: 6px;
    border: 1px solid var(--border2);
    display: flex; align-items: center; justify-content: center;
    font-size: 12px;
    background: var(--bg3);
    color: var(--text3);
  }
  .lock-badge.locked { border-color: var(--rose); color: var(--rose); background: rgba(201,106,123,0.1); }
  .lock-badge.unlocked { border-color: var(--teal); color: var(--teal); background: rgba(74,155,142,0.1); }

  .card-desc {
    font-size: 14px;
    color: var(--text2);
    line-height: 1.6;
    margin-bottom: 14px;
    display: -webkit-box;
    -webkit-line-clamp: 2;
    -webkit-box-orient: vertical;
    overflow: hidden;
    font-style: italic;
  }
  .card-tags { display: flex; gap: 6px; flex-wrap: wrap; margin-bottom: 14px; }
  .tag-pill {
    font-size: 11px;
    padding: 3px 10px;
    border-radius: 12px;
    font-family: 'JetBrains Mono', monospace;
    letter-spacing: 0.3px;
  }
  .tag-pill.genre { background: rgba(123,94,167,0.2); border: 1px solid rgba(123,94,167,0.4); color: var(--accent2); }
  .tag-pill.tag { background: rgba(201,168,76,0.1); border: 1px solid rgba(201,168,76,0.25); color: #a08840; }

  .card-footer {
    display: flex; align-items: center; justify-content: space-between;
    border-top: 1px solid var(--border);
    padding-top: 12px;
    margin-top: 4px;
  }
  .card-link {
    font-family: 'JetBrains Mono', monospace;
    font-size: 11px;
    color: var(--text3);
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
    max-width: 160px;
  }
  .card-actions { display: flex; gap: 6px; }

  /* ── OVERLAY LOCK ── */
  .lock-overlay {
    position: absolute;
    inset: 0;
    background: rgba(10,10,15,0.75);
    border-radius: var(--radius);
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    gap: 10px;
    backdrop-filter: blur(2px);
    z-index: 5;
  }
  .lock-overlay .lock-icon { font-size: 28px; }
  .lock-overlay p { font-size: 13px; color: var(--text3); font-style: italic; text-align: center; max-width: 200px; }
  .lock-overlay .hint {
    font-size: 12px;
    color: var(--rose);
    font-family: 'JetBrains Mono', monospace;
    background: rgba(201,106,123,0.1);
    border: 1px solid rgba(201,106,123,0.3);
    padding: 4px 12px;
    border-radius: 20px;
    text-align: center;
  }

  /* ── MODAL ── */
  .modal-bg {
    position: fixed; inset: 0;
    background: rgba(0,0,0,0.75);
    backdrop-filter: blur(6px);
    z-index: 200;
    display: flex; align-items: center; justify-content: center;
    padding: 20px;
    animation: fadeIn 0.2s ease;
  }
  .modal-bg.hidden { display: none; }
  .modal {
    background: var(--bg2);
    border: 1px solid var(--border2);
    border-radius: 16px;
    width: 100%;
    max-width: 560px;
    max-height: 90vh;
    overflow-y: auto;
    box-shadow: 0 20px 80px rgba(0,0,0,0.6), var(--glow);
    animation: scaleIn 0.25s cubic-bezier(0.175, 0.885, 0.32, 1.275);
  }
  .modal-lg { max-width: 720px; }
  .modal-header {
    padding: 24px 28px 20px;
    border-bottom: 1px solid var(--border);
    display: flex; align-items: center; justify-content: space-between;
  }
  .modal-title {
    font-family: 'Cinzel Decorative', serif;
    font-size: 17px;
    color: var(--gold2);
  }
  .modal-body { padding: 24px 28px; }
  .modal-footer {
    padding: 16px 28px 24px;
    display: flex; gap: 10px; justify-content: flex-end;
    border-top: 1px solid var(--border);
  }
  .close-btn {
    width: 32px; height: 32px; border-radius: 8px;
    background: transparent; border: 1px solid var(--border);
    color: var(--text3); cursor: pointer; font-size: 16px;
    display: flex; align-items: center; justify-content: center;
    transition: all 0.2s;
  }
  .close-btn:hover { background: var(--card); color: var(--text); border-color: var(--border2); }

  /* ── FORM ── */
  .form-group { margin-bottom: 18px; }
  .form-label {
    display: block;
    font-size: 12px;
    text-transform: uppercase;
    letter-spacing: 1.5px;
    color: var(--text3);
    font-family: 'JetBrains Mono', monospace;
    margin-bottom: 7px;
  }
  .form-hint { font-size: 12px; color: var(--text3); margin-top: 5px; font-style: italic; }
  textarea { resize: vertical; min-height: 90px; line-height: 1.6; }
  .form-row { display: grid; grid-template-columns: 1fr 1fr; gap: 14px; }
  
  .tags-input-wrap { display: flex; gap: 8px; }
  .tags-input-wrap input { flex: 1; }
  .tags-preview { display: flex; gap: 6px; flex-wrap: wrap; margin-top: 8px; }
  .tag-removable {
    display: flex; align-items: center; gap: 5px;
    font-size: 11px; padding: 3px 10px; border-radius: 12px;
    font-family: 'JetBrains Mono', monospace;
    cursor: default;
  }
  .tag-removable .rm { cursor: pointer; opacity: 0.6; font-size: 13px; line-height: 1; }
  .tag-removable .rm:hover { opacity: 1; }
  .tag-removable.genre { background: rgba(123,94,167,0.2); border: 1px solid rgba(123,94,167,0.4); color: var(--accent2); }
  .tag-removable.tag { background: rgba(201,168,76,0.1); border: 1px solid rgba(201,168,76,0.25); color: #a08840; }

  /* ── PASSWORD SECTION ── */
  .pw-section {
    background: var(--bg3);
    border: 1px solid var(--border);
    border-radius: 10px;
    padding: 16px;
    margin-top: 16px;
  }
  .pw-toggle {
    display: flex; align-items: center; gap: 10px; cursor: pointer;
    color: var(--text2); font-size: 14px;
    user-select: none;
  }
  .pw-toggle input[type="checkbox"] { width: auto; accent-color: var(--gold); cursor: pointer; }
  .pw-fields { margin-top: 14px; display: flex; flex-direction: column; gap: 12px; }
  .pw-fields.hidden { display: none; }

  /* ── UNLOCK MODAL ── */
  .unlock-modal .lock-big { font-size: 48px; text-align: center; margin-bottom: 12px; }
  .unlock-modal .hint-text {
    text-align: center;
    font-size: 13px;
    color: var(--rose);
    font-family: 'JetBrains Mono', monospace;
    background: rgba(201,106,123,0.08);
    border: 1px solid rgba(201,106,123,0.2);
    padding: 8px 16px;
    border-radius: 8px;
    margin-bottom: 16px;
    font-style: normal;
  }
  .pw-input-wrap { position: relative; }
  .pw-input-wrap input { padding-right: 44px; letter-spacing: 2px; font-family: 'JetBrains Mono', monospace; }
  .pw-toggle-vis {
    position: absolute; right: 12px; top: 50%; transform: translateY(-50%);
    background: none; border: none; color: var(--text3); cursor: pointer;
    font-size: 15px; padding: 4px;
  }
  .pw-error { color: var(--rose); font-size: 13px; margin-top: 8px; font-style: italic; display: none; }
  .pw-error.show { display: block; }

  /* ── VIEW MODAL ── */
  .prompt-text-display {
    background: var(--bg3);
    border: 1px solid var(--border);
    border-radius: 8px;
    padding: 16px;
    font-family: 'JetBrains Mono', monospace;
    font-size: 13px;
    line-height: 1.7;
    color: var(--text);
    white-space: pre-wrap;
    word-break: break-word;
    max-height: 300px;
    overflow-y: auto;
  }
  .copy-success { color: var(--teal); font-size: 13px; font-style: italic; display: none; }
  .copy-success.show { display: inline; }

  /* ── EMPTY STATE ── */
  .empty-state {
    text-align: center;
    padding: 80px 20px;
    color: var(--text3);
  }
  .empty-state .empty-icon { font-size: 52px; margin-bottom: 16px; opacity: 0.4; }
  .empty-state h3 { font-family: 'Cinzel Decorative', serif; font-size: 16px; color: var(--text2); margin-bottom: 8px; }
  .empty-state p { font-size: 14px; font-style: italic; }

  /* ── TOAST ── */
  .toast {
    position: fixed;
    bottom: 30px; right: 30px;
    background: var(--card);
    border: 1px solid var(--border2);
    border-radius: 10px;
    padding: 12px 18px;
    font-size: 14px;
    color: var(--text);
    z-index: 999;
    box-shadow: 0 8px 30px rgba(0,0,0,0.4);
    animation: slideUp 0.3s ease;
    display: flex; align-items: center; gap: 10px;
    max-width: 300px;
  }
  .toast.success { border-color: var(--teal); }
  .toast.error { border-color: var(--rose); }
  .toast-icon { font-size: 18px; }
  .toast.fade-out { animation: fadeOut 0.3s ease forwards; }

  /* ── DIVIDER ── */
  .divider {
    border: none;
    border-top: 1px solid var(--border);
    margin: 24px 0;
  }

  /* ── SCROLLBAR ── */
  ::-webkit-scrollbar { width: 6px; }
  ::-webkit-scrollbar-track { background: var(--bg); }
  ::-webkit-scrollbar-thumb { background: var(--border2); border-radius: 3px; }
  ::-webkit-scrollbar-thumb:hover { background: var(--gold); }

  /* ── ANIMATIONS ── */
  @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }
  @keyframes fadeUp { from { opacity: 0; transform: translateY(12px); } to { opacity: 1; transform: translateY(0); } }
  @keyframes scaleIn { from { opacity: 0; transform: scale(0.93); } to { opacity: 1; transform: scale(1); } }
  @keyframes slideUp { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
  @keyframes fadeOut { to { opacity: 0; transform: translateY(-6px); } }

  /* ── RESPONSIVE ── */
  @media (max-width: 640px) {
    header { padding: 16px 20px; }
    main { padding: 20px; }
    .form-row { grid-template-columns: 1fr; }
    .logo-text { font-size: 17px; }
  }
</style>
</head>
<body>
<div class="bg-orbs">
  <div class="orb orb1"></div>
  <div class="orb orb2"></div>
  <div class="orb orb3"></div>
</div>

<div id="app">
  <!-- HEADER -->
  <header>
    <div class="logo-wrap">
      <div class="logo-icon">📜</div>
      <div>
        <div class="logo-text">PromptVault</div>
        <div class="logo-sub">AI Roleplay Hub</div>
      </div>
    </div>
    <div class="header-actions">
      <button class="btn btn-gold" onclick="openAddModal()">
        <span>＋</span> Thêm Prompt
      </button>
      <button class="btn btn-sm" onclick="exportData()" title="Export JSON">⬇ Export</button>
      <label class="btn btn-sm" style="cursor:pointer" title="Import JSON">⬆ Import<input type="file" accept=".json" style="display:none" onchange="importData(event)"></label>
    </div>
  </header>

  <main>
    <!-- FILTER BAR -->
    <div class="filter-bar">
      <div class="search-wrap">
        <span class="icon">🔍</span>
        <input type="text" id="searchInput" placeholder="Tìm kiếm prompt..." oninput="renderPrompts()">
      </div>
      <select id="sortSelect" onchange="renderPrompts()" style="width:auto;min-width:150px">
        <option value="newest">Mới nhất</option>
        <option value="oldest">Cũ nhất</option>
        <option value="name">Tên A–Z</option>
      </select>
    </div>

    <!-- TAGS ROW -->
    <div id="tagsRow" class="tags-row"></div>

    <!-- SECTION HEADER -->
    <div class="section-header">
      <span class="section-title">Thư Viện Prompt</span>
      <span class="count-badge" id="countBadge">0</span>
    </div>

    <!-- GRID -->
    <div class="prompts-grid" id="promptsGrid"></div>
  </main>
</div>

<!-- ══════════════════════════════════
     ADD / EDIT MODAL
══════════════════════════════════ -->
<div class="modal-bg hidden" id="addModal">
  <div class="modal modal-lg">
    <div class="modal-header">
      <span class="modal-title" id="addModalTitle">✦ Thêm Prompt Mới</span>
      <button class="close-btn" onclick="closeAddModal()">✕</button>
    </div>
    <div class="modal-body">
      <div class="form-row">
        <div class="form-group">
          <label class="form-label">Tên Bot / Nhân vật *</label>
          <input type="text" id="fName" placeholder="Ví dụ: Elara the Mage">
        </div>
        <div class="form-group">
          <label class="form-label">Link hoặc Text Prompt *</label>
          <input type="text" id="fLink" placeholder="https://... hoặc paste prompt text">
        </div>
      </div>

      <div class="form-group">
        <label class="form-label">Mô tả ngắn</label>
        <textarea id="fDesc" placeholder="Tóm tắt nhân vật, bối cảnh, phong cách..."></textarea>
      </div>

      <div class="form-row">
        <div class="form-group">
          <label class="form-label">Genre (thể loại)</label>
          <div class="tags-input-wrap">
            <input type="text" id="fGenreInput" placeholder="Fantasy, Romance..." onkeydown="handleTagKey(event,'genre')">
            <button class="btn btn-sm" onclick="addTag('genre')">＋</button>
          </div>
          <div class="tags-preview" id="fGenrePrev"></div>
          <div class="form-hint">Nhấn Enter hoặc ＋ để thêm</div>
        </div>
        <div class="form-group">
          <label class="form-label">Tags</label>
          <div class="tags-input-wrap">
            <input type="text" id="fTagInput" placeholder="NSFW, Yandere, Isekai..." onkeydown="handleTagKey(event,'tag')">
            <button class="btn btn-sm" onclick="addTag('tag')">＋</button>
          </div>
          <div class="tags-preview" id="fTagPrev"></div>
          <div class="form-hint">Nhấn Enter hoặc ＋ để thêm</div>
        </div>
      </div>

      <!-- PASSWORD SECTION -->
      <div class="pw-section">
        <label class="pw-toggle">
          <input type="checkbox" id="fPwEnabled" onchange="togglePwFields()">
          <span>🔒 Bảo vệ bằng mật khẩu</span>
        </label>
        <div class="pw-fields hidden" id="pwFields">
          <div class="form-group" style="margin:0">
            <label class="form-label">Mật khẩu</label>
            <input type="text" id="fPassword" placeholder="Nhập mật khẩu...">
          </div>
          <div class="form-group" style="margin:0">
            <label class="form-label">Gợi ý (Hint) — hiển thị cho bạn bè</label>
            <input type="text" id="fHint" placeholder="Ví dụ: Tên con mèo nhà mình 🐱">
            <div class="form-hint">Hint giúp bạn bè nhớ mật khẩu mà không tiết lộ trực tiếp</div>
          </div>
        </div>
      </div>
    </div>
    <div class="modal-footer">
      <button class="btn" onclick="closeAddModal()">Hủy</button>
      <button class="btn btn-gold" onclick="savePrompt()">💾 Lưu Prompt</button>
    </div>
  </div>
</div>

<!-- ══════════════════════════════════
     UNLOCK MODAL
══════════════════════════════════ -->
<div class="modal-bg hidden" id="unlockModal">
  <div class="modal unlock-modal">
    <div class="modal-header">
      <span class="modal-title">🔐 Nhập Mật Khẩu</span>
      <button class="close-btn" onclick="closeUnlockModal()">✕</button>
    </div>
    <div class="modal-body">
      <div class="lock-big">🔒</div>
      <p style="text-align:center;color:var(--text2);margin-bottom:16px;font-style:italic" id="unlockBotName">...</p>
      <div id="unlockHintWrap" class="hint-text" style="display:none"></div>
      <div class="form-group">
        <label class="form-label">Mật khẩu</label>
        <div class="pw-input-wrap">
          <input type="password" id="unlockInput" placeholder="••••••••" onkeydown="if(event.key==='Enter')tryUnlock()">
          <button class="pw-toggle-vis" onclick="togglePwVis('unlockInput',this)">👁</button>
        </div>
        <div class="pw-error" id="unlockError">Mật khẩu không đúng. Thử lại nhé!</div>
      </div>
    </div>
    <div class="modal-footer">
      <button class="btn" onclick="closeUnlockModal()">Hủy</button>
      <button class="btn btn-gold" onclick="tryUnlock()">Mở khóa ✦</button>
    </div>
  </div>
</div>

<!-- ══════════════════════════════════
     VIEW PROMPT MODAL
══════════════════════════════════ -->
<div class="modal-bg hidden" id="viewModal">
  <div class="modal modal-lg">
    <div class="modal-header">
      <span class="modal-title" id="viewTitle">...</span>
      <button class="close-btn" onclick="closeViewModal()">✕</button>
    </div>
    <div class="modal-body">
      <div class="form-group">
        <label class="form-label">Mô tả</label>
        <p id="viewDesc" style="color:var(--text2);font-style:italic;line-height:1.7"></p>
      </div>
      <div class="form-group">
        <label class="form-label">Link / Prompt Text</label>
        <div id="viewLink" style="display:flex;gap:10px;align-items:flex-start"></div>
      </div>
      <div class="form-group" id="viewTagsWrap">
        <label class="form-label">Tags &amp; Genre</label>
        <div class="card-tags" id="viewTags"></div>
      </div>
    </div>
    <div class="modal-footer">
      <button class="btn" onclick="closeViewModal()">Đóng</button>
      <button class="btn btn-gold" id="viewEditBtn" onclick="">✏ Chỉnh sửa</button>
    </div>
  </div>
</div>

<!-- ══════════════════════════════════
     DELETE CONFIRM MODAL
══════════════════════════════════ -->
<div class="modal-bg hidden" id="deleteModal">
  <div class="modal" style="max-width:420px">
    <div class="modal-header">
      <span class="modal-title">⚠ Xác nhận xóa</span>
      <button class="close-btn" onclick="closeDeleteModal()">✕</button>
    </div>
    <div class="modal-body">
      <p style="color:var(--text2);line-height:1.7">Bạn có chắc muốn xóa prompt <strong id="deletePromptName" style="color:var(--gold3)"></strong>?<br>Hành động này không thể hoàn tác.</p>
    </div>
    <div class="modal-footer">
      <button class="btn" onclick="closeDeleteModal()">Hủy</button>
      <button class="btn btn-danger" id="confirmDeleteBtn">🗑 Xóa</button>
    </div>
  </div>
</div>

<script>
// ════════════════════════════════════════
//  DATA STORE
// ════════════════════════════════════════
const STORE_KEY = 'promptvault_data';
let prompts = [];
let editingId = null;
let unlockTargetId = null;
let viewingId = null;
let deletingId = null;
let sessionUnlocked = new Set(); // unlocked this session
let activeFilters = { genres: new Set(), tags: new Set() };

function save() {
  localStorage.setItem(STORE_KEY, JSON.stringify(prompts));
}
function load() {
  try { prompts = JSON.parse(localStorage.getItem(STORE_KEY) || '[]'); } catch { prompts = []; }
}
function genId() { return Date.now().toString(36) + Math.random().toString(36).slice(2,7); }

// ════════════════════════════════════════
//  TAG EDITOR STATE
// ════════════════════════════════════════
let editGenres = [];
let editTags = [];

function handleTagKey(e, type) {
  if (e.key === 'Enter') { e.preventDefault(); addTag(type); }
}
function addTag(type) {
  const inp = document.getElementById(type === 'genre' ? 'fGenreInput' : 'fTagInput');
  const val = inp.value.trim();
  if (!val) return;
  const arr = type === 'genre' ? editGenres : editTags;
  if (!arr.includes(val)) { arr.push(val); renderTagPrev(type); }
  inp.value = '';
  inp.focus();
}
function removeTag(type, val) {
  if (type === 'genre') { editGenres = editGenres.filter(t => t !== val); renderTagPrev('genre'); }
  else { editTags = editTags.filter(t => t !== val); renderTagPrev('tag'); }
}
function renderTagPrev(type) {
  const arr = type === 'genre' ? editGenres : editTags;
  const el = document.getElementById(type === 'genre' ? 'fGenrePrev' : 'fTagPrev');
  el.innerHTML = arr.map(t =>
    `<span class="tag-removable ${type}">${t}<span class="rm" onclick="removeTag('${type}','${t}')">×</span></span>`
  ).join('');
}

// ════════════════════════════════════════
//  RENDER
// ════════════════════════════════════════
function getAllTagsGenres() {
  const genres = new Set(), tags = new Set();
  prompts.forEach(p => {
    (p.genres||[]).forEach(g => genres.add(g));
    (p.tags||[]).forEach(t => tags.add(t));
  });
  return { genres: [...genres], tags: [...tags] };
}

function renderTagsRow() {
  const { genres, tags } = getAllTagsGenres();
  const el = document.getElementById('tagsRow');
  if (!genres.length && !tags.length) { el.innerHTML = ''; return; }
  let html = `<span style="font-size:12px;color:var(--text3);font-family:'JetBrains Mono',monospace;letter-spacing:1px">FILTER:</span>`;
  genres.forEach(g => {
    const active = activeFilters.genres.has(g) ? ' active genre-tag' : '';
    html += `<button class="tag-filter genre-tag${active}" onclick="toggleFilter('genre','${CSS.escape(g)}')">${g}</button>`;
  });
  tags.forEach(t => {
    const active = activeFilters.tags.has(t) ? ' active' : '';
    html += `<button class="tag-filter${active}" onclick="toggleFilter('tag','${CSS.escape(t)}')">${t}</button>`;
  });
  if (activeFilters.genres.size || activeFilters.tags.size) {
    html += `<button class="btn btn-sm" onclick="clearFilters()" style="font-size:12px;padding:4px 10px">✕ Xóa filter</button>`;
  }
  el.innerHTML = html;
}

function toggleFilter(type, val) {
  const set = type === 'genre' ? activeFilters.genres : activeFilters.tags;
  if (set.has(val)) set.delete(val); else set.add(val);
  renderTagsRow();
  renderPrompts();
}
function clearFilters() {
  activeFilters.genres.clear();
  activeFilters.tags.clear();
  renderTagsRow();
  renderPrompts();
}

function renderPrompts() {
  const search = document.getElementById('searchInput').value.toLowerCase();
  const sort = document.getElementById('sortSelect').value;

  let filtered = prompts.filter(p => {
    const matchSearch = !search ||
      p.name.toLowerCase().includes(search) ||
      (p.desc||'').toLowerCase().includes(search) ||
      (p.genres||[]).some(g => g.toLowerCase().includes(search)) ||
      (p.tags||[]).some(t => t.toLowerCase().includes(search));

    const matchGenre = !activeFilters.genres.size ||
      [...activeFilters.genres].some(g => (p.genres||[]).includes(g));
    const matchTag = !activeFilters.tags.size ||
      [...activeFilters.tags].some(t => (p.tags||[]).includes(t));

    return matchSearch && matchGenre && matchTag;
  });

  if (sort === 'newest') filtered.sort((a,b) => b.created - a.created);
  else if (sort === 'oldest') filtered.sort((a,b) => a.created - b.created);
  else if (sort === 'name') filtered.sort((a,b) => a.name.localeCompare(b.name));

  document.getElementById('countBadge').textContent = filtered.length;

  const grid = document.getElementById('promptsGrid');
  if (!filtered.length) {
    grid.innerHTML = `<div class="empty-state" style="grid-column:1/-1">
      <div class="empty-icon">📜</div>
      <h3>${prompts.length ? 'Không tìm thấy kết quả' : 'Chưa có prompt nào'}</h3>
      <p>${prompts.length ? 'Thử thay đổi bộ lọc hoặc từ khóa tìm kiếm' : 'Nhấn "+ Thêm Prompt" để bắt đầu tạo bộ sưu tập của bạn'}</p>
    </div>`;
    return;
  }

  grid.innerHTML = filtered.map((p, i) => buildCard(p, i)).join('');
  renderTagsRow();
}

function buildCard(p, i) {
  const isLocked = p.password && !sessionUnlocked.has(p.id);
  const lockBadge = p.password
    ? `<span class="lock-badge ${isLocked ? 'locked' : 'unlocked'}" title="${isLocked ? 'Đã khóa' : 'Đã mở khóa'}">${isLocked ? '🔒' : '🔓'}</span>`
    : '';

  const genres = (p.genres||[]).map(g => `<span class="tag-pill genre">${g}</span>`).join('');
  const tags = (p.tags||[]).map(t => `<span class="tag-pill tag">${t}</span>`).join('');

  const lockOverlay = isLocked ? `
    <div class="lock-overlay" onclick="openUnlockModal('${p.id}')">
      <span class="lock-icon">🔒</span>
      ${p.hint ? `<div class="hint">💡 ${escHtml(p.hint)}</div>` : ''}
      <p>Nhấn để nhập mật khẩu</p>
    </div>` : '';

  const linkPreview = isLocked ? '••••••••' :
    (p.link.startsWith('http') ? p.link : p.link.slice(0,40)+'...');

  return `<div class="prompt-card${isLocked ? ' locked' : ''}" style="animation-delay:${i*0.04}s" id="card-${p.id}">
    ${lockOverlay}
    <div class="card-content">
      <div class="card-top">
        <div class="card-name">${escHtml(p.name)}</div>
        <div class="card-badges">${lockBadge}</div>
      </div>
      <div class="card-desc">${escHtml(p.desc||'Không có mô tả.')}</div>
      <div class="card-tags">${genres}${tags}</div>
      <div class="card-footer">
        <span class="card-link" title="${isLocked ? '' : escHtml(p.link)}">${isLocked ? '••••••••' : escHtml(linkPreview)}</span>
        <div class="card-actions" onclick="event.stopPropagation()">
          <button class="btn btn-sm btn-icon" onclick="openViewModal('${p.id}')" title="Xem">👁</button>
          <button class="btn btn-sm btn-icon" onclick="openEditModal('${p.id}')" title="Sửa">✏</button>
          <button class="btn btn-sm btn-icon btn-danger" onclick="openDeleteModal('${p.id}')" title="Xóa">🗑</button>
        </div>
      </div>
    </div>
  </div>`;
}

// ════════════════════════════════════════
//  ADD / EDIT MODAL
// ════════════════════════════════════════
function openAddModal() {
  editingId = null;
  editGenres = []; editTags = [];
  document.getElementById('addModalTitle').textContent = '✦ Thêm Prompt Mới';
  document.getElementById('fName').value = '';
  document.getElementById('fLink').value = '';
  document.getElementById('fDesc').value = '';
  document.getElementById('fPassword').value = '';
  document.getElementById('fHint').value = '';
  document.getElementById('fPwEnabled').checked = false;
  document.getElementById('pwFields').classList.add('hidden');
  renderTagPrev('genre'); renderTagPrev('tag');
  document.getElementById('addModal').classList.remove('hidden');
}
function openEditModal(id) {
  const p = prompts.find(x => x.id === id);
  if (!p) return;
  editingId = id;
  editGenres = [...(p.genres||[])];
  editTags = [...(p.tags||[])];
  document.getElementById('addModalTitle').textContent = '✏ Chỉnh Sửa Prompt';
  document.getElementById('fName').value = p.name;
  document.getElementById('fLink').value = p.link;
  document.getElementById('fDesc').value = p.desc||'';
  document.getElementById('fPassword').value = p.password||'';
  document.getElementById('fHint').value = p.hint||'';
  const hasPw = !!p.password;
  document.getElementById('fPwEnabled').checked = hasPw;
  document.getElementById('pwFields').classList.toggle('hidden', !hasPw);
  renderTagPrev('genre'); renderTagPrev('tag');
  document.getElementById('addModal').classList.remove('hidden');
  closeViewModal();
}
function closeAddModal() { document.getElementById('addModal').classList.add('hidden'); }

function togglePwFields() {
  const checked = document.getElementById('fPwEnabled').checked;
  document.getElementById('pwFields').classList.toggle('hidden', !checked);
}

function savePrompt() {
  const name = document.getElementById('fName').value.trim();
  const link = document.getElementById('fLink').value.trim();
  if (!name || !link) { showToast('Vui lòng điền Tên và Link/Prompt!', 'error'); return; }
  const pwEnabled = document.getElementById('fPwEnabled').checked;
  const pw = pwEnabled ? document.getElementById('fPassword').value.trim() : '';
  const hint = pwEnabled ? document.getElementById('fHint').value.trim() : '';
  if (pwEnabled && !pw) { showToast('Vui lòng nhập mật khẩu!', 'error'); return; }

  if (editingId) {
    const idx = prompts.findIndex(x => x.id === editingId);
    if (idx !== -1) {
      prompts[idx] = { ...prompts[idx], name, link, desc: document.getElementById('fDesc').value.trim(), genres: [...editGenres], tags: [...editTags], password: pw, hint };
    }
    showToast('Đã cập nhật prompt!', 'success');
  } else {
    prompts.unshift({ id: genId(), name, link, desc: document.getElementById('fDesc').value.trim(), genres: [...editGenres], tags: [...editTags], password: pw, hint, created: Date.now() });
    showToast('Đã thêm prompt mới!', 'success');
  }
  save(); closeAddModal(); renderTagsRow(); renderPrompts();
}

// ════════════════════════════════════════
//  UNLOCK MODAL
// ════════════════════════════════════════
function openUnlockModal(id) {
  unlockTargetId = id;
  const p = prompts.find(x => x.id === id);
  if (!p) return;
  document.getElementById('unlockBotName').textContent = p.name;
  const hintWrap = document.getElementById('unlockHintWrap');
  if (p.hint) { hintWrap.textContent = '💡 ' + p.hint; hintWrap.style.display = 'block'; }
  else { hintWrap.style.display = 'none'; }
  document.getElementById('unlockInput').value = '';
  document.getElementById('unlockError').classList.remove('show');
  document.getElementById('unlockModal').classList.remove('hidden');
  setTimeout(() => document.getElementById('unlockInput').focus(), 100);
}
function closeUnlockModal() { document.getElementById('unlockModal').classList.add('hidden'); unlockTargetId = null; }

function tryUnlock() {
  const p = prompts.find(x => x.id === unlockTargetId);
  if (!p) return;
  const val = document.getElementById('unlockInput').value;
  if (val === p.password) {
    sessionUnlocked.add(p.id);
    closeUnlockModal();
    renderPrompts();
    showToast('🔓 Đã mở khóa!', 'success');
  } else {
    document.getElementById('unlockError').classList.add('show');
    document.getElementById('unlockInput').select();
  }
}

function togglePwVis(inputId, btn) {
  const inp = document.getElementById(inputId);
  if (inp.type === 'password') { inp.type = 'text'; btn.textContent = '🙈'; }
  else { inp.type = 'password'; btn.textContent = '👁'; }
}

// ════════════════════════════════════════
//  VIEW MODAL
// ════════════════════════════════════════
function openViewModal(id) {
  const p = prompts.find(x => x.id === id);
  if (!p) return;
  if (p.password && !sessionUnlocked.has(p.id)) { openUnlockModal(id); return; }
  viewingId = id;
  document.getElementById('viewTitle').textContent = p.name;
  document.getElementById('viewDesc').textContent = p.desc || 'Không có mô tả.';

  const linkEl = document.getElementById('viewLink');
  const isUrl = p.link.startsWith('http');
  if (isUrl) {
    linkEl.innerHTML = `
      <a href="${escHtml(p.link)}" target="_blank" class="btn btn-gold" style="font-size:13px">🔗 Mở Link</a>
      <button class="btn btn-sm" onclick="copyText('${escHtml(p.link).replace(/'/g,"\\'")}',this)">📋 Copy</button>`;
  } else {
    linkEl.innerHTML = `
      <div style="flex:1">
        <div class="prompt-text-display">${escHtml(p.link)}</div>
      </div>`;
    linkEl.innerHTML += `<button class="btn btn-sm" style="align-self:flex-start" onclick="copyText(document.querySelector('.prompt-text-display').textContent, this)">📋 Copy</button>`;
  }

  const tagsEl = document.getElementById('viewTags');
  tagsEl.innerHTML = (p.genres||[]).map(g => `<span class="tag-pill genre">${g}</span>`).join('') +
    (p.tags||[]).map(t => `<span class="tag-pill tag">${t}</span>`).join('');
  document.getElementById('viewTagsWrap').style.display = tagsEl.innerHTML ? '' : 'none';

  document.getElementById('viewEditBtn').onclick = () => openEditModal(id);
  document.getElementById('viewModal').classList.remove('hidden');
}
function closeViewModal() { document.getElementById('viewModal').classList.add('hidden'); viewingId = null; }

function copyText(text, btn) {
  navigator.clipboard.writeText(text).then(() => {
    const orig = btn.innerHTML;
    btn.innerHTML = '✅ Copied!';
    btn.style.borderColor = 'var(--teal)';
    btn.style.color = 'var(--teal)';
    setTimeout(() => { btn.innerHTML = orig; btn.style.borderColor = ''; btn.style.color = ''; }, 1500);
  });
}

// ════════════════════════════════════════
//  DELETE MODAL
// ════════════════════════════════════════
function openDeleteModal(id) {
  deletingId = id;
  const p = prompts.find(x => x.id === id);
  document.getElementById('deletePromptName').textContent = p ? p.name : '';
  document.getElementById('confirmDeleteBtn').onclick = confirmDelete;
  document.getElementById('deleteModal').classList.remove('hidden');
}
function closeDeleteModal() { document.getElementById('deleteModal').classList.add('hidden'); deletingId = null; }
function confirmDelete() {
  prompts = prompts.filter(x => x.id !== deletingId);
  sessionUnlocked.delete(deletingId);
  save(); closeDeleteModal(); renderTagsRow(); renderPrompts();
  showToast('Đã xóa prompt.', 'success');
}

// ════════════════════════════════════════
//  EXPORT / IMPORT
// ════════════════════════════════════════
function exportData() {
  const json = JSON.stringify(prompts, null, 2);
  const blob = new Blob([json], { type: 'application/json' });
  const url = URL.createObjectURL(blob);
  const a = document.createElement('a');
  a.href = url; a.download = 'promptvault_backup.json'; a.click();
  URL.revokeObjectURL(url);
  showToast('Đã export backup!', 'success');
}
function importData(e) {
  const file = e.target.files[0]; if (!file) return;
  const reader = new FileReader();
  reader.onload = ev => {
    try {
      const data = JSON.parse(ev.target.result);
      if (!Array.isArray(data)) throw new Error();
      const merged = [...data];
      const existingIds = new Set(prompts.map(p => p.id));
      data.forEach(p => { if (!existingIds.has(p.id)) merged.push(p); });
      prompts = data; // replace
      save(); renderTagsRow(); renderPrompts();
      showToast(`Đã import ${data.length} prompts!`, 'success');
    } catch { showToast('File JSON không hợp lệ!', 'error'); }
  };
  reader.readAsText(file);
  e.target.value = '';
}

// ════════════════════════════════════════
//  TOAST
// ════════════════════════════════════════
function showToast(msg, type = 'success') {
  const t = document.createElement('div');
  t.className = `toast ${type}`;
  t.innerHTML = `<span class="toast-icon">${type === 'success' ? '✓' : '✕'}</span>${escHtml(msg)}`;
  document.body.appendChild(t);
  setTimeout(() => {
    t.classList.add('fade-out');
    setTimeout(() => t.remove(), 350);
  }, 2200);
}

// ════════════════════════════════════════
//  UTILS
// ════════════════════════════════════════
function escHtml(s) {
  return String(s||'').replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;').replace(/"/g,'&quot;').replace(/'/g,'&#39;');
}

// Close modals on background click
document.querySelectorAll('.modal-bg').forEach(bg => {
  bg.addEventListener('click', e => {
    if (e.target === bg) {
      bg.classList.add('hidden');
      unlockTargetId = null; viewingId = null; deletingId = null; editingId = null;
    }
  });
});

// ESC to close
document.addEventListener('keydown', e => {
  if (e.key === 'Escape') {
    ['addModal','unlockModal','viewModal','deleteModal'].forEach(id => {
      document.getElementById(id).classList.add('hidden');
    });
  }
});

// ════════════════════════════════════════
//  INIT
// ════════════════════════════════════════
load();
renderTagsRow();
renderPrompts();
</script>
</body>
</html>
