
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<title>API Dashboard - Multi Server</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&family=JetBrains+Mono:wght@400;500&display=swap" rel="stylesheet">
<style>
  /* ============= RESET & VARIABLES ============= */
  * { box-sizing: border-box; margin: 0; padding: 0; }
  :root {
    --accent: #8b5cf6;
    --accent-2: #ec4899;
    --accent-3: #06b6d4;
    --success: #10b981;
    --danger: #ef4444;
    --warning: #f59e0b;
    --text: #f8fafc;
    --muted: #94a3b8;
    --card-bg: rgba(15, 15, 30, 0.65);
    --card-border: rgba(255, 255, 255, 0.08);
    --card-border-hover: rgba(139, 92, 246, 0.5);
    --shadow-lg: 0 20px 50px -12px rgba(0, 0, 0, 0.6);
    --glass-blur: blur(20px) saturate(180%);
  }
  
  html { scroll-behavior: smooth; }
  body {
    font-family: 'Inter', -apple-system, sans-serif;
    color: var(--text);
    min-height: 100vh;
    padding: 32px 20px;
    background:
      linear-gradient(135deg, rgba(15, 12, 41, 0.85), rgba(36, 36, 62, 0.85)),
      url('https://files.catbox.moe/vxmarw.jpg') center/cover no-repeat fixed;
    background-attachment: fixed;
  }

  body::before, body::after {
    content: ""; position: fixed; border-radius: 50%; filter: blur(120px); pointer-events: none; z-index: -1; opacity: 0.35;
  }
  body::before { width: 500px; height: 500px; background: var(--accent); top: -150px; left: -150px; animation: float1 12s ease-in-out infinite; }
  body::after { width: 400px; height: 400px; background: var(--accent-2); bottom: -150px; right: -150px; animation: float2 14s ease-in-out infinite; }
  @keyframes float1 { 0%,100% { transform: translate(0,0); } 50% { transform: translate(60px, 40px); } }
  @keyframes float2 { 0%,100% { transform: translate(0,0); } 50% { transform: translate(-50px, -30px); } }

  .wrap { max-width: 1200px; margin: 0 auto; position: relative; z-index: 1; }

  /* ============= HEADER ============= */
  header {
    display: flex; flex-wrap: wrap; gap: 20px; align-items: center; justify-content: space-between;
    margin-bottom: 24px; padding: 20px 24px;
    background: var(--card-bg); border: 1px solid var(--card-border);
    border-radius: 20px; backdrop-filter: var(--glass-blur); box-shadow: var(--shadow-lg);
  }
  .brand { display: flex; align-items: center; gap: 14px; }
  .brand-icon {
    width: 48px; height: 48px; border-radius: 14px;
    background: linear-gradient(135deg, var(--accent), var(--accent-2));
    display: flex; align-items: center; justify-content: center; color: #fff; box-shadow: 0 8px 20px rgba(139, 92, 246, 0.4);
  }
  .brand-icon svg { width: 26px; height: 26px; }
  .brand-text h1 {
    font-size: 1.4rem; font-weight: 800; letter-spacing: -0.02em;
    background: linear-gradient(135deg, #fff, #cbd5e1); -webkit-background-clip: text; -webkit-text-fill-color: transparent;
  }
  .brand-text p { font-size: 0.8rem; color: var(--muted); margin-top: 2px; }
  .header-actions { display: flex; gap: 10px; flex-wrap: wrap; }

  /* ============= SERVER BAR (MULTI SERVER) ============= */
  .server-bar {
    display: flex; flex-direction: column; gap: 12px;
    padding: 16px 20px; margin-bottom: 24px;
    background: var(--card-bg); border: 1px solid var(--card-border);
    border-radius: 16px; backdrop-filter: var(--glass-blur); box-shadow: var(--shadow-lg);
  }
  .server-list { display: flex; flex-wrap: wrap; gap: 8px; }
  .server-tag {
    display: inline-flex; align-items: center; gap: 6px;
    padding: 6px 12px; border-radius: 8px; font-size: 0.75rem; font-weight: 600;
    background: rgba(6, 182, 212, 0.15); color: #67e8f9; border: 1px solid rgba(6, 182, 212, 0.3);
  }
  .server-tag .close-btn { cursor: pointer; opacity: 0.7; transition: 0.2s; }
  .server-tag .close-btn:hover { opacity: 1; color: #fff; }
  
  .server-input-wrap { display: flex; gap: 8px; align-items: center; }
  .server-input-wrap svg { width: 16px; height: 16px; color: var(--muted); }
  .server-input {
    flex: 1; padding: 10px 14px; border-radius: 10px; border: 1px solid var(--card-border);
    background: rgba(0,0,0,0.3); color: var(--text); font-family: 'JetBrains Mono', monospace; font-size: 0.82rem; outline: none; transition: all 0.2s;
  }
  .server-input:focus { border-color: var(--accent); box-shadow: 0 0 0 4px rgba(139, 92, 246, 0.15); }

  /* ============= BUTTONS ============= */
  .btn {
    padding: 11px 18px; border-radius: 12px; border: 1px solid var(--card-border); background: rgba(255,255,255,0.05);
    color: var(--text); font-family: inherit; font-weight: 600; font-size: 0.85rem;
    cursor: pointer; display: inline-flex; align-items: center; justify-content: center; gap: 8px;
    transition: all 0.2s; white-space: nowrap;
  }
  .btn svg { width: 16px; height: 16px; flex-shrink: 0; }
  .btn:hover { background: rgba(255,255,255,0.1); border-color: rgba(255,255,255,0.2); transform: translateY(-1px); }
  .btn:active { transform: translateY(0); }
  .btn:disabled { opacity: 0.5; cursor: not-allowed; transform: none; }
  .btn.primary { background: linear-gradient(135deg, var(--accent), var(--accent-2)); border: none; box-shadow: 0 8px 24px rgba(139, 92, 246, 0.35); }
  .btn.primary:hover { box-shadow: 0 12px 28px rgba(139, 92, 246, 0.5); filter: brightness(1.1); }
  .btn.danger { background: rgba(239, 68, 68, 0.12); border-color: rgba(239, 68, 68, 0.3); color: #fca5a5; }
  .btn.danger:hover { background: rgba(239, 68, 68, 0.25); }
  .btn.sync { background: rgba(6, 182, 212, 0.12); border-color: rgba(6, 182, 212, 0.3); color: #67e8f9; }
  .btn.sync:hover { background: rgba(6, 182, 212, 0.25); }
  .btn.sm { padding: 8px 12px; font-size: 0.78rem; border-radius: 10px; }

  /* ============= STATS ============= */
  .stats { display: grid; grid-template-columns: repeat(auto-fit, minmax(180px, 1fr)); gap: 16px; margin-bottom: 28px; }
  .stat-card {
    background: var(--card-bg); border: 1px solid var(--card-border); border-radius: 16px; padding: 20px;
    backdrop-filter: var(--glass-blur); box-shadow: var(--shadow-lg); display: flex; align-items: center; gap: 16px;
    transition: all 0.25s; position: relative; overflow: hidden;
  }
  .stat-card::before { content: ""; position: absolute; top: 0; left: 0; width: 4px; height: 100%; background: linear-gradient(180deg, var(--accent), var(--accent-2)); }
  .stat-card:hover { border-color: var(--card-border-hover); transform: translateY(-2px); }
  .stat-icon {
    width: 44px; height: 44px; border-radius: 12px; background: rgba(139, 92, 246, 0.15); border: 1px solid rgba(139, 92, 246, 0.3);
    display: flex; align-items: center; justify-content: center; color: var(--accent); flex-shrink: 0;
  }
  .stat-icon svg { width: 22px; height: 22px; }
  .stat-icon.green { background: rgba(16, 185, 129, 0.15); border-color: rgba(16, 185, 129, 0.3); color: var(--success); }
  .stat-icon.cyan { background: rgba(6, 182, 212, 0.15); border-color: rgba(6, 182, 212, 0.3); color: var(--accent-3); }
  .stat-label { font-size: 0.72rem; color: var(--muted); text-transform: uppercase; letter-spacing: 0.06em; font-weight: 600; }
  .stat-value { font-size: 1.7rem; font-weight: 800; margin-top: 2px; line-height: 1; letter-spacing: -0.02em; }

  /* ============= TOOLBAR ============= */
  .toolbar { display: flex; gap: 12px; margin-bottom: 24px; flex-wrap: wrap; }
  .search-wrap { flex: 1; min-width: 220px; position: relative; }
  .search-wrap svg { position: absolute; left: 16px; top: 50%; transform: translateY(-50%); width: 18px; height: 18px; color: var(--muted); pointer-events: none; }
  .search-input {
    width: 100%; padding: 13px 16px 13px 46px; border-radius: 12px; border: 1px solid var(--card-border);
    background: var(--card-bg); backdrop-filter: var(--glass-blur); color: var(--text);
    font-family: inherit; font-size: 0.9rem; outline: none; transition: all 0.2s;
  }
  .search-input::placeholder { color: var(--muted); }
  .search-input:focus { border-color: var(--accent); box-shadow: 0 0 0 4px rgba(139, 92, 246, 0.15); }
  .filter-wrap { position: relative; }
  .filter-wrap svg { position: absolute; right: 14px; top: 50%; transform: translateY(-50%); width: 16px; height: 16px; color: var(--muted); pointer-events: none; }
  .filter-select {
    padding: 13px 42px 13px 16px; border-radius: 12px; border: 1px solid var(--card-border);
    background: var(--card-bg); backdrop-filter: var(--glass-blur); color: var(--text);
    font-family: inherit; font-size: 0.9rem; cursor: pointer; outline: none; appearance: none; min-width: 180px; transition: all 0.2s;
  }
  .filter-select:focus { border-color: var(--accent); }
  .filter-select option { background: #1a1830; color: var(--text); }

  /* ============= GRID / CARDS ============= */
  .grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(340px, 1fr)); gap: 20px; }
  .card {
    background: var(--card-bg); border: 1px solid var(--card-border); border-radius: 18px; padding: 22px;
    backdrop-filter: var(--glass-blur); box-shadow: var(--shadow-lg); transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
    position: relative; overflow: hidden; display: flex; flex-direction: column; gap: 14px;
  }
  .card::before { content: ""; position: absolute; top: 0; left: 0; height: 100%; width: 4px; background: linear-gradient(180deg, var(--accent), var(--accent-2)); opacity: 0; transition: opacity 0.3s; }
  .card:hover { border-color: var(--card-border-hover); transform: translateY(-4px); box-shadow: 0 24px 60px -12px rgba(139, 92, 246, 0.3); }
  .card:hover::before { opacity: 1; }
  .card.inactive { opacity: 0.65; }

  .card-head { display: flex; justify-content: space-between; align-items: flex-start; gap: 12px; }
  .card-title { font-size: 1.02rem; font-weight: 700; letter-spacing: -0.01em; line-height: 1.3; word-break: break-word; flex: 1; }
  .badge {
    font-size: 0.68rem; padding: 4px 10px; border-radius: 8px; background: rgba(139, 92, 246, 0.15); color: #c4b5fd; border: 1px solid rgba(139, 92, 246, 0.3);
    font-weight: 700; text-transform: uppercase; letter-spacing: 0.05em; white-space: nowrap; flex-shrink: 0;
  }
  .badge.synced { background: rgba(16, 185, 129, 0.15); color: #6ee7b7; border-color: rgba(16, 185, 129, 0.3); }
  .badge.local { background: rgba(245, 158, 11, 0.15); color: #fbbf24; border-color: rgba(245, 158, 11, 0.3); }

  .endpoint-box {
    font-family: 'JetBrains Mono', monospace; font-size: 0.78rem; color: #a5b4fc; background: rgba(0,0,0,0.35); padding: 12px 14px; border-radius: 10px;
    border: 1px solid rgba(139, 92, 246, 0.15); word-break: break-all; line-height: 1.55; display: flex; flex-direction: column; gap: 6px;
  }
  .endpoint-row { display: flex; gap: 8px; align-items: flex-start; }
  .endpoint-row svg { width: 14px; height: 14px; flex-shrink: 0; margin-top: 2px; color: var(--muted); }
  .endpoint-row .key-text { color: #f9a8d4; }

  .meta-row { display: flex; flex-wrap: wrap; gap: 8px; font-size: 0.72rem; }
  .meta-chip { display: inline-flex; align-items: center; gap: 5px; background: rgba(255,255,255,0.05); border: 1px solid var(--card-border); padding: 4px 10px; border-radius: 8px; color: var(--muted); font-weight: 500; }
  .meta-chip svg { width: 12px; height: 12px; }
  .meta-chip.active { color: var(--success); border-color: rgba(16,185,129,0.3); background: rgba(16,185,129,0.08); }
  .meta-chip.inactive { color: var(--danger); border-color: rgba(239,68,68,0.3); background: rgba(239,68,68,0.08); }

  .card-desc { font-size: 0.82rem; color: #cbd5e1; line-height: 1.55; word-break: break-word; }
  .card-actions { display: flex; gap: 8px; margin-top: auto; padding-top: 4px; }
  .card-actions .btn { flex: 1; padding: 9px 12px; font-size: 0.78rem; }

  /* ============= EMPTY ============= */
  .empty {
    text-align: center; padding: 80px 20px; grid-column: 1 / -1; background: var(--card-bg); border: 1px dashed var(--card-border);
    border-radius: 18px; backdrop-filter: var(--glass-blur);
  }
  .empty svg { width: 56px; height: 56px; color: var(--muted); margin-bottom: 16px; opacity: 0.5; }
  .empty h3 { font-size: 1.1rem; margin-bottom: 6px; }
  .empty p { font-size: 0.85rem; color: var(--muted); margin-bottom: 20px; }
  .empty .btn { margin: 0 auto; }

  /* ============= MODAL (PERSIS GAMBAR & ANTI TERPOTONG) ============= */
  .modal-overlay {
    display: none; position: fixed; inset: 0; background: rgba(5, 5, 15, 0.75); backdrop-filter: blur(8px);
    z-index: 999; justify-content: center; align-items: center; padding: 16px;
  }
  .modal-overlay.active { display: flex; animation: fadeIn 0.2s ease; }
  @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }
  
  .modal {
    background: #121022; border: 1px solid var(--card-border); border-radius: 20px; max-width: 520px; width: 100%; 
    max-height: 90vh; max-height: 90dvh; display: grid; grid-template-rows: auto 1fr auto;
    box-shadow: 0 40px 100px -20px rgba(0,0,0,0.8); animation: pop 0.28s cubic-bezier(0.34, 1.56, 0.64, 1); overflow: hidden;
  }
  @keyframes pop { from { transform: scale(0.92); opacity: 0; } to { transform: scale(1); opacity: 1; } }

  .modal-head { display: flex; align-items: center; gap: 14px; padding: 24px 24px 16px 24px; background: #121022; z-index: 2; }
  .modal-head-icon {
    width: 44px; height: 44px; border-radius: 12px; background: linear-gradient(135deg, #c026d3, #db2777);
    display: flex; align-items: center; justify-content: center; color: #fff; flex-shrink: 0;
  }
  .modal-head-icon svg { width: 22px; height: 22px; }
  .modal h2 { font-size: 1.25rem; font-weight: 700; }

  .modal-body { padding: 0 24px 20px 24px; overflow-y: auto; -webkit-overflow-scrolling: touch; }
  .modal-body::-webkit-scrollbar { width: 6px; }
  .modal-body::-webkit-scrollbar-thumb { background: var(--accent); border-radius: 3px; }

  .form-group { margin-bottom: 18px; }
  .form-group label { display: block; font-size: 0.75rem; color: #94a3b8; margin-bottom: 8px; font-weight: 700; text-transform: uppercase; letter-spacing: 0.05em; }
  .form-group input, .form-group textarea, .form-group select {
    width: 100%; padding: 12px 16px; border-radius: 12px; border: 1px solid #2d2a45; background: #1a1730;
    color: #fff; font-family: inherit; font-size: 0.95rem; outline: none; transition: all 0.2s;
  }
  .form-group textarea { resize: vertical; min-height: 80px; }
  .form-group input:focus, .form-group textarea:focus, .form-group select:focus { border-color: #a855f7; box-shadow: 0 0 0 3px rgba(168, 85, 247, 0.2); background: #1a1730; }
  .form-group select { appearance: none; background-image: url("data:image/svg+xml;charset=US-ASCII,%3Csvg%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%20width%3D%22292.4%22%20height%3D%22292.4%22%3E%3Cpath%20fill%3D%22%2394a3b8%22%20d%3D%22M287%2069.4a17.6%2017.6%200%200%200-13-5.4H18.4c-5%200-9.3%201.8-12.9%205.4A17.6%2017.6%200%200%200%200%2082.2c0%205%201.8%209.3%205.4%2012.9l128%20127.9c3.6%203.6%207.8%205.4%2012.8%205.4s9.2-1.8%2012.8-5.4L287%2095c3.5-3.5%205.4-7.8%205.4-12.8%200-5-1.9-9.2-5.5-12.8z%22%2F%3E%3C%2Fsvg%3E"); background-repeat: no-repeat; background-position: right 16px top 50%; background-size: 12px auto; }
  .form-group select option { background: #1a1730; color: #fff; }

  .checkbox-group { display: flex; align-items: center; gap: 12px; margin-top: 10px; }
  .checkbox-group input[type="checkbox"] { width: 20px; height: 20px; accent-color: #a855f7; cursor: pointer; margin: 0; }
  .checkbox-group label { margin: 0; text-transform: none; letter-spacing: normal; font-size: 0.9rem; color: #fff; font-weight: 500; cursor: pointer; }

  .modal-actions {
    display: flex; gap: 12px; justify-content: flex-end; padding: 16px 24px; background: #121022;
    border-top: 1px solid #2d2a45; padding-bottom: calc(16px + env(safe-area-inset-bottom));
  }
  
  .btn-batal { background: #2d2a45; color: #fff; border: none; padding: 12px 20px; border-radius: 10px; font-weight: 600; cursor: pointer; transition: 0.2s; }
  .btn-batal:hover { background: #3f3a5e; }
  .btn-tambah { background: linear-gradient(90deg, #c026d3, #db2777); color: #fff; border: none; padding: 12px 24px; border-radius: 10px; font-weight: 700; cursor: pointer; display: flex; align-items: center; gap: 8px; transition: 0.2s; box-shadow: 0 4px 15px rgba(192, 38, 211, 0.3); }
  .btn-tambah:hover { transform: translateY(-2px); box-shadow: 0 6px 20px rgba(192, 38, 211, 0.5); }
  .btn-tambah:disabled { opacity: 0.7; cursor: not-allowed; transform: none; }

  /* ============= TOAST ============= */
  .toast {
    position: fixed; bottom: 32px; right: 32px; background: rgba(20, 18, 40, 0.95); border: 1px solid var(--accent);
    border-radius: 14px; padding: 14px 20px; color: #fff; font-size: 0.88rem; font-weight: 500; box-shadow: 0 20px 50px rgba(0,0,0,0.6);
    transform: translateY(150px); opacity: 0; transition: all 0.35s cubic-bezier(0.34, 1.56, 0.64, 1);
    z-index: 1000; display: flex; align-items: center; gap: 10px; backdrop-filter: var(--glass-blur); max-width: 340px;
  }
  .toast svg { width: 18px; height: 18px; flex-shrink: 0; }
  .toast.show { transform: translateY(0); opacity: 1; }
  .toast.success { border-color: var(--success); }
  .toast.success svg { color: var(--success); }
  .toast.error { border-color: var(--danger); }
  .toast.error svg { color: var(--danger); }
  .toast.info svg { color: var(--accent); }
  .toast.warning { border-color: var(--warning); }
  .toast.warning svg { color: var(--warning); }

  /* ============= SCROLLBAR ============= */
  ::-webkit-scrollbar { width: 8px; height: 8px; }
  ::-webkit-scrollbar-track { background: rgba(0,0,0,0.2); }
  ::-webkit-scrollbar-thumb { background: linear-gradient(180deg, var(--accent), var(--accent-2)); border-radius: 4px; }

  .spinner-sm { display: inline-block; width: 14px; height: 14px; border: 2px solid rgba(255,255,255,0.3); border-top-color: #fff; border-radius: 50%; animation: spin 0.8s linear infinite; }
  @keyframes spin { to { transform: rotate(360deg); } }

  /* ============= RESPONSIVE ============= */
  @media (max-width: 640px) {
    body { padding: 16px 12px; }
    header { padding: 16px; flex-direction: column; align-items: flex-start; }
    .header-actions { width: 100%; }
    .header-actions .btn { flex: 1; }
    .grid { grid-template-columns: 1fr; }
    .stat-card { padding: 16px; }
    .toast { left: 12px; right: 12px; bottom: 12px; max-width: none; }
    .server-bar { padding: 12px; }
    .modal-overlay { padding: 10px; align-items: flex-end; }
    .modal { max-height: 92dvh; } 
    .modal-head { padding: 20px 20px 10px 20px; }
    .modal-body { padding: 10px 20px 20px 20px; }
    .modal-actions { padding: 14px 20px; padding-bottom: calc(14px + env(safe-area-inset-bottom)); }
    .server-input-wrap { flex-direction: column; align-items: stretch; }
  }
</style>
</head>
<body>
<div class="wrap">
  <!-- HEADER -->
  <header>
    <div class="brand">
      <div class="brand-icon">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round" stroke-linejoin="round">
          <rect x="3" y="3" width="7" height="7" rx="1.5"/><rect x="14" y="3" width="7" height="7" rx="1.5"/>
          <rect x="3" y="14" width="7" height="7" rx="1.5"/><rect x="14" y="14" width="7" height="7" rx="1.5"/>
        </svg>
      </div>
      <div class="brand-text">
        <h1>API Dashboard</h1>
        <p>Upload JSON & Sinkronisasi ke Multi-Server Publik</p>
      </div>
    </div>
    <div class="header-actions">
      <button class="btn" id="btnUpload" style="color: var(--accent-3); border-color: rgba(6,182,212,0.3);">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4"/><polyline points="17 8 12 3 7 8"/><line x1="12" y1="3" x2="12" y2="15"/></svg>
        Upload JSON
      </button>
      <input type="file" id="fileUpload" accept=".json" style="display: none;">

      <button class="btn" id="btnExport">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4"/><polyline points="17 8 12 3 7 8"/><line x1="12" y1="3" x2="12" y2="15"/></svg>
        Export
      </button>
      <button class="btn primary" id="btnAdd">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><line x1="12" y1="5" x2="12" y2="19"/><line x1="5" y1="12" x2="19" y2="12"/></svg>
        Tambah API
      </button>
    </div>
  </header>

  <!-- MULTI SERVER CONFIG BAR -->
  <div class="server-bar">
    <div style="display:flex; justify-content:space-between; align-items:center; flex-wrap:wrap; gap:10px;">
      <div style="font-size:0.85rem; font-weight:600; color:var(--text); display:flex; align-items:center; gap:8px;">
        <svg viewBox="0 0 24 24" fill="none" stroke="var(--accent-3)" stroke-width="2" width="18" height="18"><rect x="2" y="2" width="20" height="8" rx="2" ry="2"/><rect x="2" y="14" width="20" height="8" rx="2" ry="2"/><line x1="6" y1="6" x2="6.01" y2="6"/><line x1="6" y1="18" x2="6.01" y2="18"/></svg>
        Server Tujuan (HTTP/HTTPS)
      </div>
      <button class="btn sm sync" id="btnPushAll">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><polyline points="16 7 12 3 8 7"/><line x1="12" y1="3" x2="12" y2="12"/><path d="M20.88 18.09A5 5 0 0 0 18 9h-1.26A8 8 0 1 0 3 16.29"/></svg>
        Kirim Semua API ke Semua Server
      </button>
    </div>
    
    <div class="server-list" id="serverList">
      <!-- Tag server akan muncul di sini via JS -->
      <span style="font-size:0.8rem; color:var(--muted); font-style:italic;">Belum ada server yang ditambahkan.</span>
    </div>

    <div class="server-input-wrap">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect x="2" y="3" width="20" height="14" rx="2" ry="2"/><line x1="8" y1="21" x2="16" y2="21"/><line x1="12" y1="17" x2="12" y2="21"/></svg>
      <input class="server-input" id="newServerUrl" placeholder="Contoh: localhost:3000 atau https://api.domain.com">
      <button class="btn sync" id="btnAddServer">Tambah Server</button>
    </div>
  </div>

  <!-- STATS -->
  <div class="stats">
    <div class="stat-card">
      <div class="stat-icon">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><ellipse cx="12" cy="5" rx="9" ry="3"/><path d="M21 12c0 1.66-4 3-9 3s-9-1.34-9-3"/><path d="M3 5v14c0 1.66 4 3 9 3s9-1.34 9-3V5"/></svg>
      </div>
      <div><div class="stat-label">Total API</div><div class="stat-value" id="statTotal">0</div></div>
    </div>
    <div class="stat-card">
      <div class="stat-icon cyan">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M22 19a2 2 0 0 1-2 2H4a2 2 0 0 1-2-2V5a2 2 0 0 1 2-2h5l2 3h9a2 2 0 0 1 2 2z"/></svg>
      </div>
      <div><div class="stat-label">Kategori</div><div class="stat-value" id="statCat">0</div></div>
    </div>
    <div class="stat-card">
      <div class="stat-icon green">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><polyline points="20 6 9 17 4 12"/></svg>
      </div>
      <div><div class="stat-label">Aktif</div><div class="stat-value" id="statActive">0</div></div>
    </div>
  </div>

  <!-- TOOLBAR -->
  <div class="toolbar">
    <div class="search-wrap">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="11" cy="11" r="8"/><line x1="21" y1="21" x2="16.65" y2="16.65"/></svg>
      <input class="search-input" id="searchInput" placeholder="Cari nama, endpoint, atau deskripsi...">
    </div>
    <div class="filter-wrap">
      <select class="filter-select" id="filterCat"><option value="">Semua Kategori</option></select>
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><polyline points="6 9 12 15 18 9"/></svg>
    </div>
  </div>

  <!-- GRID -->
  <div class="grid" id="grid"></div>
</div>

<!-- MODAL ADD/EDIT -->
<div class="modal-overlay" id="modal">
  <div class="modal">
    <div class="modal-head">
      <div class="modal-head-icon">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><line x1="12" y1="5" x2="12" y2="19"/><line x1="5" y1="12" x2="19" y2="12"/></svg>
      </div>
      <h2 id="modalTitle">Tambah API</h2>
    </div>
    <div class="modal-body">
      <input type="hidden" id="editId">
      <div class="form-group"><label>NAMA API</label><input type="text" id="fName" placeholder="Contoh: Ryzz Gantenk API"></div>
      <div class="form-group"><label>ENDPOINT / URL</label><input type="text" id="fEndpoint" placeholder="http://ryzzgantenk.ryuuuprvt.my.id:2821"></div>
      <div class="form-group"><label>API KEY (OPSIONAL)</label><input type="text" id="fKey" placeholder="freeApikey / sk-xxx..."></div>
      <div class="form-group">
        <label>KATEGORI</label>
        <input type="text" id="fCat" placeholder="Downloader / Lyrics / AI / dll" list="catList">
        <datalist id="catList"><option value="Downloader"><option value="Lyrics"><option value="Music"><option value="AI"><option value="Image"><option value="Tools"><option value="Other"></datalist>
      </div>
      <div class="form-group"><label>METODE</label><select id="fMethod"><option value="GET">GET</option><option value="POST">POST</option><option value="PUT">PUT</option><option value="DELETE">DELETE</option></select></div>
      <div class="form-group"><label>DESKRIPSI</label><textarea id="fDesc" placeholder="Fungsi endpoint ini..."></textarea></div>
      <div class="form-group"><label>STATUS</label><select id="fStatus"><option value="active">Aktif</option><option value="inactive">Nonaktif</option></select></div>
      <div class="checkbox-group">
        <input type="checkbox" id="fSync">
        <label for="fSync">Kirim ke server setelah disimpan</label>
      </div>
    </div>
    <div class="modal-actions">
      <button class="btn-batal" id="btnCancel">Batal</button>
      <button class="btn-tambah" id="btnSave">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><path d="M19 21H5a2 2 0 0 1-2-2V5a2 2 0 0 1 2-2h11l5 5v11a2 2 0 0 1-2 2z"/><polyline points="17 21 17 13 7 13 7 21"/><polyline points="7 3 7 8 15 8"/></svg>
        Tambahkan
      </button>
    </div>
  </div>
</div>

<!-- TOAST -->
<div class="toast" id="toast"></div>

<script>
/* =========================================
   SVG ICON HELPER
   ========================================= */
const ICONS = {
  copy: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect x="9" y="9" width="13" height="13" rx="2" ry="2"/><path d="M5 15H4a2 2 0 0 1-2-2V4a2 2 0 0 1 2-2h9a2 2 0 0 1 2 2v1"/></svg>`,
  test: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><polyline points="22 12 18 12 15 21 9 3 6 12 2 12"/></svg>`,
  edit: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M11 4H4a2 2 0 0 0-2 2v14a2 2 0 0 0 2 2h14a2 2 0 0 0 2-2v-7"/><path d="M18.5 2.5a2.121 2.121 0 0 1 3 3L12 15l-4 1 1-4 9.5-9.5z"/></svg>`,
  trash: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><polyline points="3 6 5 6 21 6"/><path d="M19 6v14a2 2 0 0 1-2 2H7a2 2 0 0 1-2-2V6m3 0V4a2 2 0 0 1 2-2h4a2 2 0 0 1 2 2v2"/></svg>`,
  link: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"/><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"/></svg>`,
  key: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M21 2l-2 2m-7.61 7.61a5.5 5.5 0 1 1-7.778 7.778 5.5 5.5 0 0 1 7.777-7.777zm0 0L15.5 7.5m0 0l3 3L22 7l-3-3m-3.5 3.5L19 4"/></svg>`,
  method: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><polyline points="16 18 22 12 16 6"/><polyline points="8 6 2 12 8 18"/></svg>`,
  check: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><polyline points="20 6 9 17 4 12"/></svg>`,
  x: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><line x1="18" y1="6" x2="6" y2="18"/><line x1="6" y1="6" x2="18" y2="18"/></svg>`,
  search: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="11" cy="11" r="8"/><line x1="21" y1="21" x2="16.65" y2="16.65"/></svg>`,
  cloud: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M18 10h-1.26A8 8 0 1 0 9 20h9a5 5 0 0 0 0-10z"/></svg>`,
  warning: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M10.29 3.86L1.82 18a2 2 0 0 0 1.71 3h16.94a2 2 0 0 0 1.71-3L13.71 3.86a2 2 0 0 0-3.42 0z"/><line x1="12" y1="9" x2="12" y2="13"/><line x1="12" y1="17" x2="12.01" y2="17"/></svg>`,
};

/* =========================================
   STORAGE KEYS & STATE
   ========================================= */
const STORAGE_KEY = "api_dashboard_data_v4";
const SERVERS_KEY = "api_dashboard_servers";

let data = [];
let servers = []; // Array of URLs
let editingId = null;

const $ = id => document.getElementById(id);
const grid = $("grid");
const searchInput = $("searchInput");
const filterCat = $("filterCat");
const serverListEl = $("serverList");

/* =========================================
   LOAD / SAVE DATA
   ========================================= */
function loadData() {
  try {
    const raw = localStorage.getItem(STORAGE_KEY);
    data = raw ? JSON.parse(raw) : [];
    if (!Array.isArray(data)) data = [];
    
    const rawServers = localStorage.getItem(SERVERS_KEY);
    servers = rawServers ? JSON.parse(rawServers) : [];
    if (!Array.isArray(servers)) servers = [];
  } catch { data = []; servers = []; }
}
function saveData() { localStorage.setItem(STORAGE_KEY, JSON.stringify(data)); }
function saveServers() { localStorage.setItem(SERVERS_KEY, JSON.stringify(servers)); }

/* =========================================
   SERVER MANAGEMENT (MULTI-SERVER)
   ========================================= */
function normalizeUrl(url) {
  let cleanUrl = url.trim().replace(/\/+$/, "");
  if (!cleanUrl) return "";
  // Auto prefix http:// jika belum ada http/https
  if (!/^https?:\/\//i.test(cleanUrl)) {
    cleanUrl = "http://" + cleanUrl;
  }
  return cleanUrl;
}

function renderServers() {
  serverListEl.innerHTML = "";
  if (servers.length === 0) {
    serverListEl.innerHTML = `<span style="font-size:0.8rem; color:var(--muted); font-style:italic;">Belum ada server yang ditambahkan.</span>`;
    return;
  }
  servers.forEach((srv, idx) => {
    const tag = document.createElement("div");
    tag.className = "server-tag";
    tag.innerHTML = `
      ${ICONS.cloud} ${escapeHtml(srv)}
      <span class="close-btn" data-idx="${idx}" title="Hapus Server">${ICONS.x}</span>
    `;
    tag.querySelector(".close-btn").onclick = () => {
      if (confirm(`Hapus server ${srv} dari daftar?`)) {
        servers.splice(idx, 1);
        saveServers();
        renderServers();
        toast("Server dihapus dari daftar", "info");
      }
    };
    serverListEl.appendChild(tag);
  });
}

$("btnAddServer").onclick = () => {
  const input = $("newServerUrl").value;
  const normalized = normalizeUrl(input);
  if (!normalized) return toast("URL server tidak boleh kosong", "error");
  if (servers.includes(normalized)) return toast("Server sudah ada di daftar", "warning");
  
  servers.push(normalized);
  saveServers();
  renderServers();
  $("newServerUrl").value = "";
  toast("Server berhasil ditambahkan", "success");
};

/* =========================================
   SERVER API CLIENT
   ========================================= */
async function serverRequest(baseUrl, path, options = {}) {
  const url = baseUrl + path;
  const res = await fetch(url, {
    headers: { "Content-Type": "application/json", ...(options.headers || {}) },
    ...options,
  });
  if (!res.ok) {
    const text = await res.text().catch(() => "");
    throw new Error(`HTTP ${res.status}: ${text || res.statusText}`);
  }
  return res.json().catch(() => ({}));
}

async function pushToAllServers(item) {
  if (servers.length === 0) return { ok: 0, fail: 0, skipped: true };
  
  let ok = 0, fail = 0;
  const body = {
    id: item.id, name: item.name, endpoint: item.endpoint, key: item.key,
    category: item.category, method: item.method, desc: item.desc, status: item.status,
  };

  // Kirim ke semua server secara paralel
  await Promise.all(servers.map(async (baseUrl) => {
    try {
      // Coba push (POST/PUT) - Asumsi backend handle id
      await serverRequest(baseUrl, `/api/endpoints`, {
        method: "POST",
        body: JSON.stringify(body),
      });
      ok++;
    } catch (e) {
      console.warn(`Gagal kirim ke ${baseUrl}:`, e.message);
      fail++;
    }
  }));

  return { ok, fail };
}

async function deleteFromAllServers(item) {
  if (servers.length === 0) return;
  await Promise.all(servers.map(async (baseUrl) => {
    try {
      await serverRequest(baseUrl, `/api/endpoints/${item.id}`, { method: "DELETE" });
    } catch (e) {
      console.warn(`Gagal hapus dari ${baseUrl}:`, e.message);
    }
  }));
}

/* =========================================
   RENDER UI
   ========================================= */
function render() {
  const search = searchInput.value.toLowerCase().trim();
  const cat = filterCat.value;

  let filtered = data.filter(d => {
    if (cat && d.category !== cat) return false;
    if (!search) return true;
    return (
      d.name.toLowerCase().includes(search) ||
      d.endpoint.toLowerCase().includes(search) ||
      (d.desc || "").toLowerCase().includes(search) ||
      (d.key || "").toLowerCase().includes(search)
    );
  });

  $("statTotal").textContent = data.length;
  $("statCat").textContent = new Set(data.map(d => d.category)).size;
  $("statActive").textContent = data.filter(d => d.status === "active").length;

  const cats = [...new Set(data.map(d => d.category))];
  const currentVal = filterCat.value;
  filterCat.innerHTML = '<option value="">Semua Kategori</option>' +
    cats.map(c => `<option value="${escapeHtml(c)}">${escapeHtml(c)}</option>`).join("");
  filterCat.value = currentVal;

  grid.innerHTML = "";

  if (data.length === 0) {
    grid.innerHTML = `<div class="empty">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"><rect x="3" y="3" width="7" height="7" rx="1.5"/><rect x="14" y="3" width="7" height="7" rx="1.5"/><rect x="3" y="14" width="7" height="7" rx="1.5"/><rect x="14" y="14" width="7" height="7" rx="1.5"/></svg>
      <h3>Belum ada API</h3>
      <p>Klik tombol di bawah untuk menambahkan endpoint pertamamu, atau Upload JSON.</p>
      <button class="btn primary" onclick="openAdd()">${ICONS.check} Tambah API</button>
    </div>`;
    return;
  }

  if (filtered.length === 0) {
    grid.innerHTML = `<div class="empty">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"><circle cx="11" cy="11" r="8"/><line x1="21" y1="21" x2="16.65" y2="16.65"/></svg>
      <h3>Tidak ada hasil</h3>
      <p>Coba ubah kata kunci atau filter kategori</p>
    </div>`;
    return;
  }

  filtered.forEach(item => {
    const card = document.createElement("div");
    card.className = "card" + (item.status === "inactive" ? " inactive" : "");
    const statusClass = item.status === "active" ? "active" : "inactive";
    const statusText = item.status === "active" ? "Aktif" : "Nonaktif";
    const statusIcon = item.status === "active" ? ICONS.check : ICONS.x;
    
    // Cek apakah item ini sudah pernah di-push (ada flag synced)
    const isSynced = item.synced === true;

    card.innerHTML = `
      <div class="card-head">
        <div class="card-title">${escapeHtml(item.name)}</div>
        <div class="badge ${isSynced ? 'synced' : 'local'}">${isSynced ? 'Synced' : 'Local'}</div>
      </div>
      <div class="endpoint-box">
        <div class="endpoint-row">${ICONS.link} <span>${escapeHtml(item.endpoint)}</span></div>
        ${item.key ? `<div class="endpoint-row">${ICONS.key} <span class="key-text">${escapeHtml(item.key)}</span></div>` : ""}
      </div>
      <div class="meta-row">
        <span class="meta-chip">${ICONS.method}${escapeHtml(item.method)}</span>
        <span class="meta-chip ${statusClass}">${statusIcon}${statusText}</span>
        <span class="meta-chip">${ICONS.cloud}${escapeHtml(item.category)}</span>
      </div>
      ${item.desc ? `<div class="card-desc">${escapeHtml(item.desc)}</div>` : ""}
      <div class="card-actions">
        <button class="btn sm" data-copy="${item.id}">${ICONS.copy}Copy</button>
        <button class="btn sm" data-test="${item.id}">${ICONS.test}Test</button>
        <button class="btn sm" data-edit="${item.id}">${ICONS.edit}Edit</button>
        <button class="btn sm danger" data-del="${item.id}">${ICONS.trash}</button>
      </div>
    `;
    grid.appendChild(card);
  });

  grid.querySelectorAll("[data-copy]").forEach(b => b.onclick = () => copyEndpoint(b.dataset.copy));
  grid.querySelectorAll("[data-test]").forEach(b => b.onclick = () => testEndpoint(b.dataset.test));
  grid.querySelectorAll("[data-edit]").forEach(b => b.onclick = () => openEdit(b.dataset.edit));
  grid.querySelectorAll("[data-del]").forEach(b => b.onclick = () => deleteItem(b.dataset.del));
}

function escapeHtml(s) {
  return String(s ?? "").replace(/[&<>"']/g, c => ({ "&":"&amp;","<":"&lt;",">":"&gt;",'"':"&quot;","'":"&#39;" }[c]));
}

/* =========================================
   ACTIONS
   ========================================= */
function copyEndpoint(id) {
  const item = data.find(d => d.id === id);
  if (!item) return;
  let text = item.endpoint;
  if (item.key) text += (text.includes("?") ? "&" : "?") + `apikey=${item.key}`;
  navigator.clipboard.writeText(text).then(() => toast("Endpoint dicopy!", "success")).catch(() => toast("Gagal copy", "error"));
}

function testEndpoint(id) {
  const item = data.find(d => d.id === id);
  if (!item) return;
  let url = item.endpoint;
  if (item.key && !url.includes("apikey")) url += (url.includes("?") ? "&" : "?") + `apikey=${item.key}`;
  toast("Membuka endpoint...", "info");
  window.open(url, "_blank");
}

async function deleteItem(id) {
  const item = data.find(d => d.id === id);
  if (!item) return;
  if (!confirm(`Hapus "${item.name}"?`)) return;

  // Hapus dari semua server
  if (servers.length > 0) {
    toast("Menghapus dari server...", "info");
    await deleteFromAllServers(item);
  }

  data = data.filter(d => d.id !== id);
  saveData();
  render();
  toast("API dihapus", "success");
}

/* =========================================
   MODAL ADD / EDIT
   ========================================= */
function openAdd() {
  editingId = null;
  $("modalTitle").textContent = "Tambah API";
  $("editId").value = "";
  $("fName").value = "";
  $("fEndpoint").value = "";
  $("fKey").value = "";
  $("fCat").value = "";
  $("fMethod").value = "GET";
  $("fDesc").value = "";
  $("fStatus").value = "active";
  $("fSync").checked = servers.length > 0;
  $("fSync").disabled = servers.length === 0;
  $("modal").classList.add("active");
  $("fName").focus();
}

function openEdit(id) {
  const item = data.find(d => d.id === id);
  if (!item) return;
  editingId = id;
  $("modalTitle").textContent = "Edit API";
  $("editId").value = id;
  $("fName").value = item.name;
  $("fEndpoint").value = item.endpoint;
  $("fKey").value = item.key || "";
  $("fCat").value = item.category;
  $("fMethod").value = item.method || "GET";
  $("fDesc").value = item.desc || "";
  $("fStatus").value = item.status || "active";
  $("fSync").checked = servers.length > 0;
  $("fSync").disabled = servers.length === 0;
  $("modal").classList.add("active");
}

function closeModal() {
  $("modal").classList.remove("active");
  editingId = null;
}

async function saveItem() {
  const name = $("fName").value.trim();
  const endpoint = $("fEndpoint").value.trim();
  if (!name || !endpoint) return toast("Nama & endpoint wajib diisi", "error");

  const isEdit = !!editingId;
  const payload = {
    id: editingId || Date.now().toString(36) + Math.random().toString(36).slice(2, 6),
    name, endpoint,
    key: $("fKey").value.trim(),
    category: $("fCat").value.trim() || "Other",
    method: $("fMethod").value,
    desc: $("fDesc").value.trim(),
    status: $("fStatus").value,
    synced: false
  };

  const wantSync = $("fSync").checked && servers.length > 0;

  if (isEdit) {
    const idx = data.findIndex(d => d.id === editingId);
    if (idx !== -1) data[idx] = payload;
  } else {
    data.push(payload);
  }
  saveData();

  if (wantSync) {
    const btn = $("btnSave");
    const originalHtml = btn.innerHTML;
    btn.disabled = true;
    btn.innerHTML = `<span class="spinner-sm"></span> Mengirim...`;
    
    const res = await pushToAllServers(payload);
    
    if (res.ok > 0) {
      const idx = data.findIndex(d => d.id === payload.id);
      if (idx !== -1) { data[idx].synced = true; saveData(); }
      if (res.fail === 0) toast(`Berhasil dikirim ke ${res.ok} server`, "success");
      else toast(`Sukses ke ${res.ok} server, gagal di ${res.fail}`, "warning");
    } else {
      toast(`Tersimpan lokal, gagal ke server`, "warning");
    }
    
    btn.disabled = false;
    btn.innerHTML = originalHtml;
  } else {
    toast("API berhasil disimpan lokal", "success");
  }

  render();
  closeModal();
}

/* =========================================
   SYNC — PUSH SEMUA API KE SEMUA SERVER
   ========================================= */
$("btnPushAll").onclick = async () => {
  if (servers.length === 0) return toast("Tambahkan URL server dulu", "error");
  if (data.length === 0) return toast("Tidak ada API untuk dikirim", "warning");
  if (!confirm(`Kirim ${data.length} API ke ${servers.length} server publik?`)) return;

  const btn = $("btnPushAll");
  const orig = btn.innerHTML;
  btn.disabled = true;
  let okCount = 0, failCount = 0;

  for (let i = 0; i < data.length; i++) {
    const item = data[i];
    btn.innerHTML = `<span class="spinner-sm"></span> Memproses API ${i+1}/${data.length}...`;
    
    const res = await pushToAllServers(item);
    if (res.ok > 0) {
      data[i].synced = true;
      okCount++;
    } else {
      failCount++;
    }
  }

  saveData();
  render();
  btn.disabled = false;
  btn.innerHTML = orig;

  if (failCount === 0) toast(`Semua ${data.length} API terkirim ke semua server!`, "success");
  else toast(`${okCount} API sukses, ${failCount} gagal`, "warning");
};

/* =========================================
   UPLOAD JSON & AUTO PUSH KE SERVER PUBLIK
   ========================================= */
$("btnUpload").onclick = () => $("fileUpload").click();
$("fileUpload").onchange = (e) => {
  const file = e.target.files[0];
  if (!file) return;

  const reader = new FileReader();
  reader.onload = async (ev) => {
    try {
      const parsed = JSON.parse(ev.target.result);
      if (!Array.isArray(parsed)) throw new Error("File JSON harus berisi Array [...]");

      const doSync = servers.length > 0 && confirm(`Ditemukan ${parsed.length} API di file.\n\nUpload ke lokal & kirim ke ${servers.length} server publik sekaligus?`);
      if (!doSync && !confirm(`Tambahkan ${parsed.length} API ke dashboard lokal?`)) return;

      const newItems = parsed.map(d => ({
        id: Date.now().toString(36) + Math.random().toString(36).slice(2, 6),
        name: d.name || d.title || "Untitled",
        endpoint: d.endpoint || d.url || "",
        key: d.key || d.apikey || "",
        category: d.category || "Other",
        method: d.method || "GET",
        desc: d.desc || d.description || "",
        status: d.status || "active",
        synced: false
      }));

      data.push(...newItems);
      saveData();
      render();
      toast(`${newItems.length} API ditambahkan ke lokal`, "success");

      // Jika user setuju kirim ke server
      if (doSync) {
        let ok = 0, fail = 0;
        toast("Mulai mengirim ke server publik...", "info");
        
        for (let i = 0; i < newItems.length; i++) {
          const item = newItems[i];
          const res = await pushToAllServers(item);
          if (res.ok > 0) {
            const idx = data.findIndex(d => d.id === item.id);
            if (idx !== -1) data[idx].synced = true;
            ok++;
          } else {
            fail++;
          }
        }
        saveData();
        render();
        
        if (fail === 0) toast(`Upload sukses! ${ok} API terkirim ke semua server.`, "success");
        else toast(`${ok} API sukses, ${fail} gagal.`, "warning");
      }
    } catch (err) {
      toast("Gagal membaca file: " + err.message, "error");
    }
    e.target.value = "";
  };
  reader.readAsText(file);
};

/* =========================================
   EXPORT JSON
   ========================================= */
$("btnExport").onclick = () => {
  if (data.length === 0) return toast("Tidak ada data untuk diexport", "warning");
  const blob = new Blob([JSON.stringify(data, null, 2)], { type: "application/json" });
  const a = document.createElement("a");
  a.href = URL.createObjectURL(blob);
  a.download = `api-dashboard-${new Date().toISOString().slice(0, 10)}.json`;
  a.click();
  URL.revokeObjectURL(a.href);
  toast("File JSON berhasil diexport", "success");
};

/* =========================================
   TOAST
   ========================================= */
let toastTimer = null;
function toast(msg, type = "info") {
  const t = $("toast");
  const icon = type === "success" ? ICONS.check : type === "error" ? ICONS.x : type === "warning" ? ICONS.warning : ICONS.search;
  t.innerHTML = icon + `<span>${escapeHtml(msg)}</span>`;
  t.className = "toast show " + type;
  clearTimeout(toastTimer);
  toastTimer = setTimeout(() => { t.className = "toast"; }, 3500);
}

/* =========================================
   INIT
   ========================================= */
document.addEventListener("DOMContentLoaded", () => {
  loadData();
  render();
  renderServers();

  $("btnAdd").onclick = openAdd;
  $("btnCancel").onclick = closeModal;
  $("btnSave").onclick = saveItem;

  searchInput.addEventListener("input", render);
  filterCat.addEventListener("change", render);

  $("modal").addEventListener("click", e => { if (e.target === $("modal")) closeModal(); });
  document.addEventListener("keydown", e => { if (e.key === "Escape") closeModal(); });
});
</script>
</body>
</html>
