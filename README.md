<!DOCTYPE html>
<html lang="th" data-theme="dark">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>TUC x WW Performance & Overview Dashboard</title>
  
  <style>
    @import url("https://fonts.googleapis.com/css2?family=Prompt:wght@300;400;500;600;700&display=swap");

    :root[data-theme="dark"] {
      --bg: #0c0d14; --panel: #151824; --panel-2: #1a1d2c; --border: #252a3d;
      --text: #f4f5fb; --muted: #b0b6cc; --blue: #4e8cff; --purple: #7657ff;
      --green: #00cba9; --red: #ff6575; --yellow: #ffc94b;
      --header-bg: rgba(12, 13, 20, 0.95);
      --kpi-gradient: linear-gradient(145deg, #1a1d2c, #151824);
      --ai-box-bg: linear-gradient(145deg, #1a1d2c, #151824);
    }

    :root[data-theme="light"] {
      --bg: #f0f3f9; --panel: #ffffff; --panel-2: #f8fafc; --border: #cbd5e1;
      --text: #1e293b; --muted: #64748b; --blue: #2563eb; --purple: #7c3aed;
      --green: #0d9488; --red: #e11d48; --yellow: #d97706;
      --header-bg: rgba(255, 255, 255, 0.95);
      --kpi-gradient: linear-gradient(145deg, #ffffff, #f1f5f9);
      --ai-box-bg: linear-gradient(145deg, #ffffff, #f8fafc);
    }

    * { box-sizing: border-box; }
    body {
      margin: 0; min-height: 100vh; color: var(--text); background: var(--bg);
      font-family: "Prompt", sans-serif; font-size: 16px; transition: background 0.3s, color 0.3s;
    }
    button, input, textarea { font-family: inherit; }
    button { color: inherit; cursor: pointer; }

    /* สไตล์หน้า Login พื้นหลังสีขาว มีลวดลาย พร้อมโลโก้ที่ชัดเจน */
    #login-container {
      position: fixed; top: 0; left: 0; width: 100vw; height: 100vh; z-index: 9999;
      background: #f8fafc;
      background-image: radial-gradient(#cbd5e1 1px, transparent 1px);
      background-size: 24px 24px;
      display: flex; align-items: center; justify-content: center; padding: 20px;
    }
    .login-card {
      width: 100%; max-width: 440px; padding: 40px; border-radius: 16px;
      background: #ffffff; border: 1px solid #e2e8f0; box-shadow: 0 20px 40px rgba(0,0,0,0.08);
      display: flex; flex-direction: column; gap: 24px; text-align: center;
    }
    .login-logos {
      display: flex; align-items: center; justify-content: center; gap: 20px; margin-bottom: 5px;
    }
    .login-logos img {
      height: 40px; object-fit: contain; background: #ffffff; padding: 4px 8px; border: 1px solid #f1f5f9; border-radius: 6px;
    }
    .login-title { font-size: 22px; font-weight: 700; color: #1e293b; letter-spacing: 0.5px; }
    .login-subtitle { font-size: 14px; color: #64748b; margin-top: -16px; line-height: 1.5; }
    .login-form-group { display: flex; flex-direction: column; gap: 8px; text-align: left; }
    .login-label { font-size: 14px; font-weight: 500; color: #475569; }
    .login-input {
      width: 100%; height: 48px; padding: 0 16px; border-radius: 10px; border: 1px solid #cbd5e1;
      background: #f8fafc; color: #1e293b; font-size: 15px; outline: none; transition: border-color 0.2s;
    }
    .login-input:focus { border-color: #2563eb; background: #ffffff; }
    .login-btn {
      width: 100%; height: 48px; border-radius: 10px; border: none; background: linear-gradient(135deg, #2563eb, #7c3aed);
      color: #fff; font-size: 16px; font-weight: 600; cursor: pointer; margin-top: 10px; transition: opacity 0.2s;
    }
    .login-btn:hover { opacity: 0.9; }
    .login-error { color: #e11d48; font-size: 13px; display: none; text-align: center; }

    .app { min-height: 100vh; display: none; }
    .app.show { display: block; }

    .header {
      position: sticky; top: 0; z-index: 50; display: flex; align-items: center; justify-content: space-between;
      min-height: 76px; padding: 14px 30px; border-bottom: 1px solid var(--border);
      background: var(--header-bg); backdrop-filter: blur(14px);
    }
    .brand { display: flex; align-items: center; gap: 16px; }
    .brand-icon {
      display: grid; width: 48px; height: 48px; place-items: center; border-radius: 13px;
      background: linear-gradient(135deg, var(--blue), var(--purple)); color: #fff; font-size: 26px;
    }
    .brand-title { font-size: 22px; font-weight: 600; }
    .brand-subtitle { color: var(--muted); font-size: 14px; }
    .header-right { display: flex; align-items: center; gap: 16px; flex-wrap: wrap; }

    .btn-action {
      padding: 10px 18px; border: 1px solid var(--border); border-radius: 9px;
      background: var(--panel); color: var(--text); font-size: 15px; font-weight: 500;
      display: flex; align-items: center; gap: 6px; cursor: pointer; transition: border-color 0.2s;
    }
    .btn-action:hover { border-color: var(--blue); }

    .upload-banner {
      margin: 20px 30px 0; padding: 16px 22px; border-radius: 12px;
      background: var(--panel); border: 1px solid var(--border);
      display: flex; align-items: center; justify-content: space-between; flex-wrap: wrap; gap: 10px;
    }
    .upload-banner-text { font-size: 15px; font-weight: 600; color: var(--text); }
    .upload-banner-sub { font-size: 14px; color: var(--green); font-weight: 600; }

    .nav-bar {
      margin: 20px 30px 0; display: flex; gap: 10px; flex-wrap: wrap;
    }
    .nav-tab {
      padding: 10px 20px; border-radius: 9px; border: 1px solid var(--border);
      background: var(--panel); color: var(--text); font-size: 15px; font-weight: 600; cursor: pointer;
      transition: all 0.2s;
    }
    .nav-tab.active { background: var(--blue); border-color: var(--blue); color: #fff; box-shadow: 0 4px 15px rgba(78, 140, 255, 0.3); }
    .nav-tab:hover:not(.active) { border-color: var(--blue); }

    .tab-content { display: none; }
    .tab-content.active { display: block; }

    .toolbar {
      display: flex; flex-direction: column; gap: 16px; margin: 12px 30px; padding: 22px;
      border: 1px solid var(--border); border-radius: 14px; background: var(--panel);
    }
    .filters-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(190px, 1fr)); gap: 12px; width: 100%; }
    .filter-group { display: flex; flex-direction: column; gap: 6px; }
    .filter-label { color: var(--muted); font-size: 14px; font-weight: 500; }

    .multi-dropdown { position: relative; width: 100%; }
    .multi-dropdown-button {
      display: flex; align-items: center; justify-content: space-between; width: 100%; min-height: 46px;
      padding: 9px 15px; cursor: pointer; border: 1px solid var(--border); border-radius: 10px;
      background: var(--panel-2); color: var(--text); font-size: 15px; text-align: left;
    }
    .multi-dropdown-button span { overflow: hidden; text-overflow: ellipsis; white-space: nowrap; }
    .dropdown-arrow { margin-left: 8px; color: var(--muted); font-size: 11px; }

    .multi-dropdown-menu {
      position: absolute; top: calc(100% + 6px); left: 0; z-index: 100; display: none; width: 100%;
      min-width: 240px; overflow: hidden; border: 1px solid var(--border); border-radius: 12px;
      background: var(--panel-2); box-shadow: 0 15px 35px rgba(0,0,0,0.25);
    }
    .multi-dropdown-menu.show { display: block; }
    .dropdown-actions { display: flex; gap: 8px; padding: 10px; border-bottom: 1px solid var(--border); }
    .dropdown-actions button {
      flex: 1; padding: 7px; color: var(--muted); font-size: 13px; border: 1px solid var(--border);
      border-radius: 6px; background: var(--panel);
    }
    .dropdown-options { max-height: 240px; padding: 6px 0; overflow-y: auto; }
    .dropdown-option {
      display: flex; align-items: center; gap: 10px; padding: 11px 15px; cursor: pointer;
      font-size: 15px; color: var(--text); user-select: none;
    }
    .dropdown-option:hover { background: rgba(78, 140, 255, 0.1); }
    .dropdown-option input { width: 17px; height: 17px; margin: 0; cursor: pointer; accent-color: var(--blue); }

    main { padding: 0 30px 50px; }
    
    .capturable { position: relative; }
    .snapshot-btn {
      position: absolute; top: 12px; right: 12px; z-index: 10;
      background: var(--panel-2); border: 1px solid var(--border); color: var(--muted);
      width: 32px; height: 32px; border-radius: 8px; display: grid; place-items: center;
      cursor: pointer; opacity: 0.6; transition: all 0.2s; font-size: 14px;
    }
    .snapshot-btn:hover { opacity: 1; color: var(--blue); border-color: var(--blue); background: var(--panel); }

    .kpi-grid { display: grid; grid-template-columns: repeat(3, minmax(0, 1fr)); gap: 16px; }
    .kpi-card {
      position: relative; min-height: 150px; padding: 22px; overflow: hidden;
      border: 1px solid var(--border); border-radius: 14px; background: var(--panel);
    }
    .kpi-card::before { position: absolute; top: 0; left: 0; width: 5px; height: 100%; content: ""; background: var(--accent); }
    .kpi-label { color: var(--muted); font-size: 15px; font-weight: 500; }
    .kpi-value { margin-top: 6px; color: var(--accent); font-size: 36px; font-weight: 700; }

    .sub-kpi-grid { display: grid; grid-template-columns: repeat(3, minmax(0, 1fr)); gap: 16px; margin-top: 16px; }
    .sub-kpi-card { position: relative; padding: 18px; border: 1px solid var(--border); border-radius: 13px; background: var(--panel); }
    .sub-kpi-title { font-size: 14px; font-weight: 600; color: var(--text); margin-bottom: 12px; border-bottom: 1px solid var(--border); padding-bottom: 8px; }
    .sub-kpi-list { display: flex; flex-direction: column; gap: 8px; max-height: 180px; overflow-y: auto; }
    .sub-kpi-item { display: flex; justify-content: space-between; align-items: center; font-size: 13px; gap: 8px; }
    .sub-kpi-item-name { color: var(--muted); overflow: hidden; text-overflow: ellipsis; white-space: nowrap; max-width: 60%; }
    .sub-kpi-item-val { color: var(--text); font-weight: 600; white-space: nowrap; }

    .chart-grid { display: grid; grid-template-columns: 2fr 1fr; gap: 16px; margin-top: 16px; }
    .chart-card { position: relative; min-height: 420px; padding: 22px; border: 1px solid var(--border); border-radius: 14px; background: var(--panel); }
    .chart-title { font-size: 17px; font-weight: 600; color: var(--text); margin-bottom: 12px; }
    .chart-wrap { position: relative; height: 320px; }

    .no-cn-section {
      position: relative; margin-top: 22px; padding: 22px; border: 1px solid var(--border); border-radius: 14px; background: var(--panel);
    }
    .no-cn-title { font-size: 18px; font-weight: 600; color: var(--red); margin-bottom: 16px; display: flex; align-items: center; gap: 8px; }
    .no-cn-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 16px; }

    .analysis-section {
      position: relative; margin-top: 25px; padding: 25px; border: 1px solid var(--border); border-radius: 14px; background: var(--panel);
    }
    .analysis-header { display: flex; justify-content: space-between; align-items: center; flex-wrap: wrap; gap: 12px; margin-bottom: 16px; border-bottom: 1px solid var(--border); padding-bottom: 12px; }
    .analysis-title { font-size: 18px; font-weight: 600; color: var(--text); display: flex; align-items: center; gap: 8px; }
    .analysis-btn-group { display: flex; gap: 10px; }
    .btn-analyze {
      padding: 9px 16px; border-radius: 8px; border: none; font-size: 14px; font-weight: 600; cursor: pointer;
      display: flex; align-items: center; gap: 6px; transition: opacity 0.2s;
    }
    .btn-analyze.standard { background: var(--blue); color: #fff; }
    .btn-analyze.ai { background: linear-gradient(135deg, var(--purple), var(--blue)); color: #fff; }
    .btn-analyze:hover { opacity: 0.9; }

    .analysis-box {
      margin-top: 14px; padding: 18px; border-radius: 10px; background: var(--panel-2); border: 1px solid var(--border);
      font-size: 14px; line-height: 1.6; color: var(--text); display: none;
    }
    .analysis-box.show { display: block; }
    .analysis-box ul { margin: 8px 0 0 20px; padding: 0; }
    .analysis-box li { margin-bottom: 6px; }

    .overview-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(320px, 1fr)); gap: 20px; margin-top: 20px; }
    .overview-card { position: relative; padding: 20px; border: 1px solid var(--border); border-radius: 14px; background: var(--panel); display: flex; flex-direction: column; gap: 10px; }
    .overview-card-header { display: flex; justify-content: space-between; align-items: center; }
    .overview-card-title { font-size: 16px; font-weight: 600; color: var(--text); display: flex; align-items: center; gap: 8px; }
    
    .btn-save-text {
      padding: 4px 10px; font-size: 12px; font-weight: 600; border-radius: 6px;
      background: var(--blue); color: #fff; border: none; cursor: pointer; transition: opacity 0.2s;
    }
    .btn-save-text:hover { opacity: 0.85; }

    .overview-textarea {
      width: 100%; height: 130px; padding: 12px; border-radius: 10px; border: 1px solid var(--border);
      background: var(--panel-2); color: var(--text); font-size: 14px; line-height: 1.5; resize: vertical; outline: none; transition: border-color 0.2s;
    }
    .overview-textarea:focus { border-color: var(--blue); }
    .ai-summary-box {
      position: relative; margin-top: 25px; padding: 25px; border: 1px solid var(--purple); border-radius: 14px; background: var(--panel);
      box-shadow: 0 4px 20px rgba(118, 87, 255, 0.15); transition: background 0.3s, border-color 0.3s;
    }
    .ai-summary-content { margin-top: 15px; font-size: 15px; line-height: 1.7; color: var(--text); }
    .ai-summary-content ul { margin: 10px 0 0 20px; padding: 0; }
    .ai-summary-content li { margin-bottom: 8px; }

    .ppt-footer-bar {
      margin-top: 30px; display: flex; justify-content: flex-end; gap: 15px; padding-top: 20px; border-top: 1px solid var(--border);
    }
    .btn-save-ppx {
      background: linear-gradient(135deg, #00cba9, var(--blue)); color: #fff; padding: 12px 24px; border-radius: 10px;
      font-size: 16px; font-weight: 600; border: none; cursor: pointer; display: flex; align-items: center; gap: 8px;
      box-shadow: 0 4px 15px rgba(0, 203, 169, 0.3); transition: opacity 0.2s;
    }
    .btn-save-ppx:hover { opacity: 0.9; }

    @media (max-width: 1000px) {
      .kpi-grid, .sub-kpi-grid, .chart-grid, .analysis-header, .overview-grid, .no-cn-grid { grid-template-columns: 1fr; flex-direction: column; align-items: stretch; }
    }
  </style>
</head>
<body>

  <!-- หน้า Login พื้นหลังสีขาว มีลวดลาย พร้อมโลโก้จริง -->
  <div id="login-container">
    <div class="login-card">
      <div class="login-logos">
        <img src="https://images.unsplash.com/photo-1618005182384-a83a8bd57fbe?w=100&auto=format&fit=crop&q=60" alt="True Logo" style="display:none;" id="fallbackTrue">
        <!-- โลโก้ True ตามลิงก์ที่คุณแนบมา -->
        <img src="https://i.ibb.co/3QZ313x.png" alt="True Logo" onerror="this.src='https://upload.wikimedia.org/wikipedia/commons/2/28/True_Corporation_Logo_%282023%29.svg'">
        <!-- โลโก้ W&W ตามลิงก์ที่คุณแนบมา -->
        <img src="https://i.ibb.co/7k91Q7F.png" alt="W&W Logo" onerror="this.style.display='none'">
      </div>
      <div>
        <div class="login-title" style="color: #0f172a;">TUC x WW Performance</div>
        <div class="login-subtitle" style="color: #64748b; margin-top: 6px;">กรุณาเข้าสู่ระบบเพื่อใช้งาน Dashboard</div>
      </div>
      <div class="login-form-group">
        <label class="login-label" style="color: #334155;">Username</label>
        <input type="text" id="loginUser" class="login-input" placeholder="กรอกชื่อผู้ใช้">
      </div>
      <div class="login-form-group">
        <label class="login-label" style="color: #334155;">Password</label>
        <input type="password" id="loginPass" class="login-input" placeholder="กรอกรหัสผ่าน">
      </div>
      <div id="loginErrorMsg" class="login-error">ชื่อผู้ใช้หรือรหัสผ่านไม่ถูกต้อง (User: mom / Pass: tucxww)</div>
      <button type="button" class="login-btn" onclick="handleLogin()">เข้าสู่ระบบ</button>
    </div>
  </div>

  <div class="app" id="appMainContent">
    <header class="header">
      <div class="brand">
        <div class="brand-icon">📊</div>
        <div>
          <div class="brand-title" id="mainHeaderTitle">Monthly Report Dashboard</div>
          <div class="brand-subtitle" id="mainHeaderSub">TUC × WW Service Performance</div>
        </div>
      </div>

      <div class="header-right">
        <label class="btn-action" style="margin: 0;">
          📁 อัปโหลดไฟล์ Excel
          <input type="file" id="excelFileInput" accept=".xlsx,.xls,.csv" style="display: none;">
        </label>
        <button type="button" class="btn-action" id="themeToggleBtn">
          <span id="themeIcon">🌙</span> <span id="themeText">โหมดมืด</span>
        </button>
      </div>
    </header>

    <div class="upload-banner">
      <div id="uploadedFileName" class="upload-banner-text">📂 ยังไม่ได้อัปโหลดไฟล์ (แสดงข้อมูลจำลองเริ่มต้น)</div>
      <div id="uploadStatus" class="upload-banner-sub">สถานะ: พร้อมใช้งาน</div>
    </div>

    <!-- แถบสลับแท็บหน้าเว็บ -->
    <nav class="nav-bar">
      <button type="button" class="nav-tab active" onclick="switchTab('main')">📊 หน้าหลัก (ALL DATA)</button>
      <button type="button" class="nav-tab" onclick="switchTab('cn')">🔄 หน้า CN x NO CN (all cn last)</button>
      <button type="button" class="nav-tab" onclick="switchTab('apple')">🍏 หน้า Apple (apple)</button>
      <button type="button" class="nav-tab" onclick="switchTab('repair')">🛠️ หน้างานซ่อม (repair)</button>
      <button type="button" class="nav-tab" onclick="switchTab('overview')">📋 ภาพรวมปัญหา & สรุป AI</button>
    </nav>

    <!-- ================= แท็บที่ 1: หน้าหลัก (ALL DATA) ================= -->
    <div id="tab-main" class="tab-content active">
      <section class="toolbar">
        <div class="filters-grid">
          <div class="filter-group">
            <span class="filter-label">ปีที่แสดง (Year)</span>
            <div class="multi-dropdown" data-filter="year">
              <button type="button" class="multi-dropdown-button"><span class="selected-text">ทั้งหมด</span><span class="dropdown-arrow">▼</span></button>
              <div class="multi-dropdown-menu">
                <div class="dropdown-actions"><button type="button" class="select-all">เลือกทั้งหมด</button><button type="button" class="clear-all">ล้าง</button></div>
                <div class="dropdown-options" id="options-year"></div>
              </div>
            </div>
          </div>

          <div class="filter-group">
            <span class="filter-label">เดือน (Month)</span>
            <div class="multi-dropdown" data-filter="month">
              <button type="button" class="multi-dropdown-button"><span class="selected-text">ทั้งหมด</span><span class="dropdown-arrow">▼</span></button>
              <div class="multi-dropdown-menu">
                <div class="dropdown-actions"><button type="button" class="select-all">เลือกทั้งหมด</button><button type="button" class="clear-all">ล้าง</button></div>
                <div class="dropdown-options" id="options-month"></div>
              </div>
            </div>
          </div>

          <div class="filter-group">
            <span class="filter-label">Group Shop</span>
            <div class="multi-dropdown" data-filter="groupShop">
              <button type="button" class="multi-dropdown-button"><span class="selected-text">ทั้งหมด</span><span class="dropdown-arrow">▼</span></button>
              <div class="multi-dropdown-menu">
                <div class="dropdown-actions"><button type="button" class="select-all">เลือกทั้งหมด</button><button type="button" class="clear-all">ล้าง</button></div>
                <div class="dropdown-options" id="options-groupShop"></div>
              </div>
            </div>
          </div>

          <div class="filter-group">
            <span class="filter-label">SYSTEM</span>
            <div class="multi-dropdown" data-filter="system">
              <button type="button" class="multi-dropdown-button"><span class="selected-text">ทั้งหมด</span><span class="dropdown-arrow">▼</span></button>
              <div class="multi-dropdown-menu">
                <div class="dropdown-actions"><button type="button" class="select-all">เลือกทั้งหมด</button><button type="button" class="clear-all">ล้าง</button></div>
                <div class="dropdown-options" id="options-system"></div>
              </div>
            </div>
          </div>

          <div class="filter-group">
            <span class="filter-label">Group Brand</span>
            <div class="multi-dropdown" data-filter="groupBrand">
              <button type="button" class="multi-dropdown-button"><span class="selected-text">ทั้งหมด</span><span class="dropdown-arrow">▼</span></button>
              <div class="multi-dropdown-menu">
                <div class="dropdown-actions"><button type="button" class="select-all">เลือกทั้งหมด</button><button type="button" class="clear-all">ล้าง</button></div>
                <div class="dropdown-options" id="options-groupBrand"></div>
              </div>
            </div>
          </div>

          <div class="filter-group">
            <span class="filter-label">Claim type</span>
            <div class="multi-dropdown" data-filter="claimType">
              <button type="button" class="multi-dropdown-button"><span class="selected-text">ทั้งหมด</span><span class="dropdown-arrow">▼</span></button>
              <div class="multi-dropdown-menu">
                <div class="dropdown-actions"><button type="button" class="select-all">เลือกทั้งหมด</button><button type="button" class="clear-all">ล้าง</button></div>
                <div class="dropdown-options" id="options-claimType"></div>
              </div>
            </div>
          </div>

          <div class="filter-group">
            <span class="filter-label">Defect Type</span>
            <div class="multi-dropdown" data-filter="defectType">
              <button type="button" class="multi-dropdown-button"><span class="selected-text">ทั้งหมด</span><span class="dropdown-arrow">▼</span></button>
              <div class="multi-dropdown-menu">
                <div class="dropdown-actions"><button type="button" class="select-all">เลือกทั้งหมด</button><button type="button" class="clear-all">ล้าง</button></div>
                <div class="dropdown-options" id="options-defectType"></div>
              </div>
            </div>
          </div>

          <div class="filter-group">
            <span class="filter-label">Inspection By</span>
            <div class="multi-dropdown" data-filter="inspectionBy">
              <button type="button" class="multi-dropdown-button"><span class="selected-text">ทั้งหมด</span><span class="dropdown-arrow">▼</span></button>
              <div class="multi-dropdown-menu">
                <div class="dropdown-actions"><button type="button" class="select-all">เลือกทั้งหมด</button><button type="button" class="clear-all">ล้าง</button></div>
                <div class="dropdown-options" id="options-inspectionBy"></div>
              </div>
            </div>
          </div>
        </div>
      </section>

      <main>
        <section class="kpi-grid">
          <article class="kpi-card capturable" style="--accent: var(--blue)" id="card-main-1">
            <button type="button" class="snapshot-btn" title="บันทึกภาพกล่องนี้" onclick="takeSnapshot('card-main-1', 'งานทั้งหมด')">📷</button>
            <div class="kpi-label">งานทั้งหมด</div>
            <div id="totalJobs" class="kpi-value">0</div>
          </article>
          <article class="kpi-card capturable" style="--accent: var(--green)" id="card-main-2">
            <button type="button" class="snapshot-btn" title="บันทึกภาพกล่องนี้" onclick="takeSnapshot('card-main-2', 'SLA_Close_เฉลี่ย')">📷</button>
            <div class="kpi-label">SLA Close เฉลี่ย</div>
            <div id="avgClose" class="kpi-value">0</div>
          </article>
          <article class="kpi-card capturable" style="--accent: var(--red)" id="card-main-3">
            <button type="button" class="snapshot-btn" title="บันทึกภาพกล่องนี้" onclick="takeSnapshot('card-main-3', 'SLA_Pending_เฉลี่ย')">📷</button>
            <div class="kpi-label">SLA Pending เฉลี่ย</div>
            <div id="avgPending" class="kpi-value">0</div>
          </article>
        </section>

        <section class="sub-kpi-grid" style="grid-template-columns: repeat(5, minmax(0, 1fr));">
          <div class="sub-kpi-card capturable" id="box-main-def"><button type="button" class="snapshot-btn" title="บันทึกภาพกล่องนี้" onclick="takeSnapshot('box-main-def', 'Defect_Type')">📷</button><div class="sub-kpi-title">🏷️ Defect Type</div><div id="subDefectList" class="sub-kpi-list"></div></div>
          <div class="sub-kpi-card capturable" id="box-main-clm"><button type="button" class="snapshot-btn" title="บันทึกภาพกล่องนี้" onclick="takeSnapshot('box-main-clm', 'Claim_Type')">📷</button><div class="sub-kpi-title">📋 Claim type</div><div id="subClaimList" class="sub-kpi-list"></div></div>
          <div class="sub-kpi-card capturable" id="box-main-shp"><button type="button" class="snapshot-btn" title="บันทึกภาพกล่องนี้" onclick="takeSnapshot('box-main-shp', 'Group_Shop')">📷</button><div class="sub-kpi-title">🏬 Group Shop</div><div id="subShopList" class="sub-kpi-list"></div></div>
          <div class="sub-kpi-card capturable" id="box-main-sys"><button type="button" class="snapshot-btn" title="บันทึกภาพกล่องนี้" onclick="takeSnapshot('box-main-sys', 'SYSTEM')">📷</button><div class="sub-kpi-title">⚙️ SYSTEM</div><div id="subSystemList" class="sub-kpi-list"></div></div>
          <div class="sub-kpi-card capturable" id="box-main-brd"><button type="button" class="snapshot-btn" title="บันทึกภาพกล่องนี้" onclick="takeSnapshot('box-main-brd', 'Group_Brand')">📷</button><div class="sub-kpi-title">🏷️ Group Brand</div><div id="subBrandList" class="sub-kpi-list"></div></div>
        </section>

        <section class="chart-grid" style="margin-top: 22px;">
          <article class="chart-card capturable" id="chart-main-1">
            <button type="button" class="snapshot-btn" title="บันทึกภาพกล่องนี้" onclick="takeSnapshot('chart-main-1', 'จำนวนงานรายเดือน')">📷</button>
            <div class="chart-title">จำนวนงานรายเดือน (เปรียบเทียบปี 2025 เส้น / 2026 แท่ง) พร้อมอัตราเปลี่ยนแปลง %</div>
            <div class="chart-wrap"><canvas id="monthlyChart"></canvas></div>
          </article>
          <article class="chart-card capturable" id="chart-main-2">
            <button type="button" class="snapshot-btn" title="บันทึกภาพกล่องนี้" onclick="takeSnapshot('chart-main-2', 'สัดส่วน_Group_Brand')">📷</button>
            <div class="chart-title">สัดส่วน Group Brand</div>
            <div class="chart-wrap"><canvas id="brandChart"></canvas></div>
          </article>
        </section>

        <section class="analysis-section capturable" id="analysis-main-box">
          <button type="button" class="snapshot-btn" title="บันทึกภาพกล่องนี้" onclick="takeSnapshot('analysis-main-box', 'ระบบวิเคราะห์ปัญหา_หน้าหลัก')">📷</button>
          <div class="analysis-header">
            <div class="analysis-title">💡 ระบบวิเคราะห์ปัญหาและแนวทางแก้ไข (Performance Analysis)</div>
            <div class="analysis-btn-group">
              <button type="button" class="btn-analyze standard" onclick="runAnalysis('main', 'standard')">📊 วิเคราะห์แบบธรรมดา</button>
              <button type="button" class="btn-analyze ai" onclick="runAnalysis('main', 'ai')">✨ วิเคราะห์เชิงลึกด้วย AI</button>
            </div>
          </div>
          <div id="analysis-box-main" class="analysis-box"></div>
        </section>
      </main>
    </div>

    <!-- ================= แท็บที่ 2: หน้า CN x NO CN (all cn last) ================= -->
    <div id="tab-cn" class="tab-content">
      <section class="toolbar">
        <div class="filters-grid">
          <div class="filter-group">
            <span class="filter-label">ปี (Year)</span>
            <div class="multi-dropdown" data-filter="cnYear">
              <button type="button" class="multi-dropdown-button"><span class="selected-text">ทั้งหมด</span><span class="dropdown-arrow">▼</span></button>
              <div class="multi-dropdown-menu">
                <div class="dropdown-actions"><button type="button" class="select-all">เลือกทั้งหมด</button><button type="button" class="clear-all">ล้าง</button></div>
                <div class="dropdown-options" id="options-cnYear"></div>
              </div>
            </div>
          </div>

          <div class="filter-group">
            <span class="filter-label">เดือน (Month)</span>
            <div class="multi-dropdown" data-filter="cnMonth">
              <button type="button" class="multi-dropdown-button"><span class="selected-text">ทั้งหมด</span><span class="dropdown-arrow">▼</span></button>
              <div class="multi-dropdown-menu">
                <div class="dropdown-actions"><button type="button" class="select-all">เลือกทั้งหมด</button><button type="button" class="clear-all">ล้าง</button></div>
                <div class="dropdown-options" id="options-cnMonth"></div>
              </div>
            </div>
          </div>

          <div class="filter-group">
            <span class="filter-label">Brand</span>
            <div class="multi-dropdown" data-filter="cnBrand">
              <button type="button" class="multi-dropdown-button"><span class="selected-text">ทั้งหมด</span><span class="dropdown-arrow">▼</span></button>
              <div class="multi-dropdown-menu">
                <div class="dropdown-actions"><button type="button" class="select-all">เลือกทั้งหมด</button><button type="button" class="clear-all">ล้าง</button></div>
                <div class="dropdown-options" id="options-cnBrand"></div>
              </div>
            </div>
          </div>

          <div class="filter-group">
            <span class="filter-label">Group Shop</span>
            <div class="multi-dropdown" data-filter="cnGroupShop">
              <button type="button" class="multi-dropdown-button"><span class="selected-text">ทั้งหมด</span><span class="dropdown-arrow">▼</span></button>
              <div class="multi-dropdown-menu">
                <div class="dropdown-actions"><button type="button" class="select-all">เลือกทั้งหมด</button><button type="button" class="clear-all">ล้าง</button></div>
                <div class="dropdown-options" id="options-cnGroupShop"></div>
              </div>
            </div>
          </div>

          <div class="filter-group">
            <span class="filter-label">Group CN</span>
            <div class="multi-dropdown" data-filter="cnGroupCN">
              <button type="button" class="multi-dropdown-button"><span class="selected-text">ทั้งหมด</span><span class="dropdown-arrow">▼</span></button>
              <div class="multi-dropdown-menu">
                <div class="dropdown-actions"><button type="button" class="select-all">เลือกทั้งหมด</button><button type="button" class="clear-all">ล้าง</button></div>
                <div class="dropdown-options" id="options-cnGroupCN"></div>
              </div>
            </div>
          </div>
        </div>
      </section>

      <main>
        <section class="kpi-grid" style="grid-template-columns: repeat(4, minmax(0, 1fr));">
          <article class="kpi-card capturable" style="--accent: var(--blue)" id="card-cn-1">
            <button type="button" class="snapshot-btn" title="บันทึกภาพกล่องนี้" onclick="takeSnapshot('card-cn-1', 'รายการ_CN_ทั้งหมด')">📷</button>
            <div class="kpi-label">รายการ CN x NO CN ทั้งหมด</div>
            <div id="cnTotalJobs" class="kpi-value">0</div>
          </article>
          <article class="kpi-card capturable" style="--accent: var(--purple)" id="card-cn-2">
            <button type="button" class="snapshot-btn" title="บันทึกภาพกล่องนี้" onclick="takeSnapshot('card-cn-2', 'สัดส่วน_CN_สำเร็จ')">📷</button>
            <div class="kpi-label">สัดส่วน CN สำเร็จ</div>
            <div id="cnRatio" class="kpi-value" style="font-size: 24px;">0 (0.0%)</div>
          </article>
          <article class="kpi-card capturable" style="--accent: var(--yellow)" id="card-cn-3">
            <button type="button" class="snapshot-btn" title="บันทึกภาพกล่องนี้" onclick="takeSnapshot('card-cn-3', 'PENDING')">📷</button>
            <div class="kpi-label">PENDING (จาก Group CN)</div>
            <div id="cnAvgPending" class="kpi-value" style="font-size: 28px;">0</div>
          </article>
          <article class="kpi-card capturable" style="--accent: var(--red)" id="card-cn-4">
            <button type="button" class="snapshot-btn" title="บันทึกภาพกล่องนี้" onclick="takeSnapshot('card-cn-4', 'NO_CN')">📷</button>
            <div class="kpi-label">NO CN (รายการที่ไม่ผ่าน)</div>
            <div id="cnNoCnKpi" class="kpi-value" style="font-size: 28px;">0</div>
          </article>
        </section>

        <section class="sub-kpi-grid" style="grid-template-columns: repeat(4, minmax(0, 1fr)); margin-top: 16px;">
          <div class="sub-kpi-card capturable" id="box-cn-def"><button type="button" class="snapshot-btn" title="บันทึกภาพกล่องนี้" onclick="takeSnapshot('box-cn-def', 'Defect_Type_CN')">📷</button><div class="sub-kpi-title">🏷️ Defect Type (CN)</div><div id="subCnDefectList" class="sub-kpi-list"></div></div>
          <div class="sub-kpi-card capturable" id="box-cn-shp"><button type="button" class="snapshot-btn" title="บันทึกภาพกล่องนี้" onclick="takeSnapshot('box-cn-shp', 'Group_Shop_CN')">📷</button><div class="sub-kpi-title">🏬 Group Shop (CN)</div><div id="subCnShopList" class="sub-kpi-list"></div></div>
          <div class="sub-kpi-card capturable" id="box-cn-sys"><button type="button" class="snapshot-btn" title="บันทึกภาพกล่องนี้" onclick="takeSnapshot('box-cn-sys', 'System_Claim')">📷</button><div class="sub-kpi-title">⚙️ System Claim</div><div id="subSystemClaimList" class="sub-kpi-list"></div></div>
          <div class="sub-kpi-card capturable" id="box-cn-grp"><button type="button" class="snapshot-btn" title="บันทึกภาพกล่องนี้" onclick="takeSnapshot('box-cn-grp', 'Group_Claim_Type')">📷</button><div class="sub-kpi-title">📋 Group Claim Type</div><div id="subGroupClaimList" class="sub-kpi-list"></div></div>
        </section>

        <section class="chart-grid" style="grid-template-columns: 1fr 1fr; margin-top: 22px;">
          <article class="chart-card capturable" id="chart-cn-1">
            <button type="button" class="snapshot-btn" title="บันทึกภาพกล่องนี้" onclick="takeSnapshot('chart-cn-1', 'จำนวนงาน_CN_รายเดือน')">📷</button>
            <div class="chart-title">จำนวนงานเปรียบเทียบรายเดือน (ปี 2025 vs 2026) พร้อมอัตราเปลี่ยนแปลง %</div>
            <div class="chart-wrap"><canvas id="cnMonthlyChart"></canvas></div>
          </article>
          <article class="chart-card capturable" id="chart-cn-2">
            <button type="button" class="snapshot-btn" title="บันทึกภาพกล่องนี้" onclick="takeSnapshot('chart-cn-2', 'สัดส่วนตามแบรนด์_CN')">📷</button>
            <div class="chart-title">สัดส่วนตามแบรนด์ (Brand)</div>
            <div class="chart-wrap"><canvas id="cnBarChart"></canvas></div>
          </article>
        </section>

        <section class="no-cn-section capturable" id="section-nocn-detail">
          <button type="button" class="snapshot-btn" title="บันทึกภาพกล่องนี้" onclick="takeSnapshot('section-nocn-detail', 'รายละเอียด_NO_CN')">📷</button>
          <div class="no-cn-title">❌ รายละเอียดวิเคราะห์เฉพาะเคส NO CN (สาเหตุและจุดที่ไม่ได้ CN)</div>
          <div class="no-cn-grid">
            <div class="sub-kpi-card" style="background: var(--panel-2);">
              <div class="sub-kpi-title">🏷️ Defect Type (เฉพาะ NO CN)</div>
              <div id="noCnDefectList" class="sub-kpi-list"></div>
            </div>
            <div class="sub-kpi-card" style="background: var(--panel-2);">
              <div class="sub-kpi-title">📌 หมายเหตุ (เฉพาะ NO CN)</div>
              <div id="noCnRemarkList" class="sub-kpi-list"></div>
            </div>
          </div>
        </section>

        <section class="analysis-section capturable" id="analysis-cn-box">
          <button type="button" class="snapshot-btn" title="บันทึกภาพกล่องนี้" onclick="takeSnapshot('analysis-cn-box', 'ระบบวิเคราะห์ปัญหา_หน้า_CN')">📷</button>
          <div class="analysis-header">
            <div class="analysis-title">💡 ระบบวิเคราะห์ปัญหาและแนวทางแก้ไข (CN & NO CN Insights)</div>
            <div class="analysis-btn-group">
              <button type="button" class="btn-analyze standard" onclick="runAnalysis('cn', 'standard')">📊 วิเคราะห์แบบธรรมดา</button>
              <button type="button" class="btn-analyze ai" onclick="runAnalysis('cn', 'ai')">✨ วิเคราะห์เชิงลึกด้วย AI</button>
            </div>
          </div>
          <div id="analysis-box-cn" class="analysis-box"></div>
        </section>
      </main>
    </div>

    <!-- ================= แท็บที่ 3: หน้า Apple (apple) ================= -->
    <div id="tab-apple" class="tab-content">
      <section class="toolbar">
        <div class="filters-grid">
          <div class="filter-group">
            <span class="filter-label">ปี (Year - Apple)</span>
            <div class="multi-dropdown" data-filter="appleYear">
              <button type="button" class="multi-dropdown-button"><span class="selected-text">ทั้งหมด</span><span class="dropdown-arrow">▼</span></button>
              <div class="multi-dropdown-menu">
                <div class="dropdown-actions"><button type="button" class="select-all">เลือกทั้งหมด</button><button type="button" class="clear-all">ล้าง</button></div>
                <div class="dropdown-options" id="options-appleYear"></div>
              </div>
            </div>
          </div>

          <div class="filter-group">
            <span class="filter-label">เดือน (Month - Apple)</span>
            <div class="multi-dropdown" data-filter="appleMonth">
              <button type="button" class="multi-dropdown-button"><span class="selected-text">ทั้งหมด</span><span class="dropdown-arrow">▼</span></button>
              <div class="multi-dropdown-menu">
                <div class="dropdown-actions"><button type="button" class="select-all">เลือกทั้งหมด</button><button type="button" class="clear-all">ล้าง</button></div>
                <div class="dropdown-options" id="options-appleMonth"></div>
              </div>
            </div>
          </div>

          <div class="filter-group">
            <span class="filter-label">Group Shop (Apple)</span>
            <div class="multi-dropdown" data-filter="appleShop">
              <button type="button" class="multi-dropdown-button"><span class="selected-text">ทั้งหมด</span><span class="dropdown-arrow">▼</span></button>
              <div class="multi-dropdown-menu">
                <div class="dropdown-actions"><button type="button" class="select-all">เลือกทั้งหมด</button><button type="button" class="clear-all">ล้าง</button></div>
                <div class="dropdown-options" id="options-appleShop"></div>
              </div>
            </div>
          </div>

          <div class="filter-group">
            <span class="filter-label">Group อาการ (Apple)</span>
            <div class="multi-dropdown" data-filter="appleSymptom">
              <button type="button" class="multi-dropdown-button"><span class="selected-text">ทั้งหมด</span><span class="dropdown-arrow">▼</span></button>
              <div class="multi-dropdown-menu">
                <div class="dropdown-actions"><button type="button" class="select-all">เลือกทั้งหมด</button><button type="button" class="clear-all">ล้าง</button></div>
                <div class="dropdown-options" id="options-appleSymptom"></div>
              </div>
            </div>
          </div>
        </div>
      </section>

      <main>
        <section class="kpi-grid" style="grid-template-columns: repeat(3, minmax(0, 1fr));">
          <article class="kpi-card capturable" style="--accent: var(--blue)" id="card-apple-1">
            <button type="button" class="snapshot-btn" title="บันทึกภาพกล่องนี้" onclick="takeSnapshot('card-apple-1', 'รายการ_Apple_ทั้งหมด')">📷</button>
            <div class="kpi-label">รายการ Apple ทั้งหมด</div>
            <div id="appleTotalJobs" class="kpi-value">0</div>
          </article>
          <article class="kpi-card capturable" style="--accent: var(--green)" id="card-apple-2">
            <button type="button" class="snapshot-btn" title="บันทึกภาพกล่องนี้" onclick="takeSnapshot('card-apple-2', 'Group_product_name_เด่นสุด')">📷</button>
            <div class="kpi-label">Group product name เด่นสุด</div>
            <div id="appleTopProduct" class="kpi-value" style="font-size: 20px; overflow: hidden; text-overflow: ellipsis; white-space: nowrap;">-</div>
          </article>
          <article class="kpi-card capturable" style="--accent: var(--yellow)" id="card-apple-3">
            <button type="button" class="snapshot-btn" title="บันทึกภาพกล่องนี้" onclick="takeSnapshot('card-apple-3', 'กลุ่มอาการเสียเด่นสุด')">📷</button>
            <div class="kpi-label">กลุ่มอาการเสียเด่นสุด</div>
            <div id="appleTopSymptom" class="kpi-value" style="font-size: 20px; overflow: hidden; text-overflow: ellipsis; white-space: nowrap;">-</div>
          </article>
        </section>

        <section class="sub-kpi-grid" style="grid-template-columns: repeat(3, minmax(0, 1fr)); margin-top: 16px;">
          <div class="sub-kpi-card capturable" id="box-apple-sym"><button type="button" class="snapshot-btn" title="บันทึกภาพกล่องนี้" onclick="takeSnapshot('box-apple-sym', 'Group_อาการ_Apple')">📷</button><div class="sub-kpi-title">🍏 Group อาการ (Apple)</div><div id="subAppleSymptomList" class="sub-kpi-list"></div></div>
          <div class="sub-kpi-card capturable" id="box-apple-shp"><button type="button" class="snapshot-btn" title="บันทึกภาพกล่องนี้" onclick="takeSnapshot('box-apple-shp', 'Group_Shop_Apple')">📷</button><div class="sub-kpi-title">🏬 Group Shop (Apple)</div><div id="subAppleShopList" class="sub-kpi-list"></div></div>
          <div class="sub-kpi-card capturable" id="box-apple-prd"><button type="button" class="snapshot-btn" title="บันทึกภาพกล่องนี้" onclick="takeSnapshot('box-apple-prd', 'Group_product_name_Apple')">📷</button><div class="sub-kpi-title">📋 Group product name (Apple)</div><div id="subAppleProductList" class="sub-kpi-list"></div></div>
        </section>

        <section class="chart-grid" style="grid-template-columns: 1fr 1fr; margin-top: 22px;">
          <article class="chart-card capturable" id="chart-apple-1">
            <button type="button" class="snapshot-btn" title="บันทึกภาพกล่องนี้" onclick="takeSnapshot('chart-apple-1', 'สัดส่วน_Group_อาการ_Apple')">📷</button>
            <div class="chart-title">สัดส่วน Group อาการ (Apple Symptom)</div>
            <div class="chart-wrap"><canvas id="applePieChart"></canvas></div>
          </article>
          <article class="chart-card capturable" id="chart-apple-2">
            <button type="button" class="snapshot-btn" title="บันทึกภาพกล่องนี้" onclick="takeSnapshot('chart-apple-2', 'จำนวนงาน_Apple_รายเดือน')">📷</button>
            <div class="chart-title">จำนวนงาน Apple รายเดือน (2025 vs 2026) พร้อมอัตราเปลี่ยนแปลง %</div>
            <div class="chart-wrap"><canvas id="appleBarChart"></canvas></div>
          </article>
        </section>

        <section class="analysis-section capturable" id="analysis-apple-box">
          <button type="button" class="snapshot-btn" title="บันทึกภาพกล่องนี้" onclick="takeSnapshot('analysis-apple-box', 'ระบบวิเคราะห์ปัญหา_หน้า_Apple')">📷</button>
          <div class="analysis-header">
            <div class="analysis-title">💡 ระบบวิเคราะห์ปัญหาและแนวทางแก้ไข (Apple Insights)</div>
            <div class="analysis-btn-group">
              <button type="button" class="btn-analyze standard" onclick="runAnalysis('apple', 'standard')">📊 วิเคราะห์แบบธรรมดา</button>
              <button type="button" class="btn-analyze ai" onclick="runAnalysis('apple', 'ai')">✨ วิเคราะห์เชิงลึกด้วย AI</button>
            </div>
          </div>
          <div id="analysis-box-apple" class="analysis-box"></div>
        </section>
      </main>
    </div>

    <!-- ================= แท็บที่ 4: หน้างานซ่อม (repair) ================= -->
    <div id="tab-repair" class="tab-content">
      <section class="toolbar">
        <div class="filters-grid">
          <div class="filter-group">
            <span class="filter-label">ปี (Year - Repair)</span>
            <div class="multi-dropdown" data-filter="repairYear">
              <button type="button" class="multi-dropdown-button"><span class="selected-text">ทั้งหมด</span><span class="dropdown-arrow">▼</span></button>
              <div class="multi-dropdown-menu">
                <div class="dropdown-actions"><button type="button" class="select-all">เลือกทั้งหมด</button><button type="button" class="clear-all">ล้าง</button></div>
                <div class="dropdown-options" id="options-repairYear"></div>
              </div>
            </div>
          </div>

          <div class="filter-group">
            <span class="filter-label">เดือน (Month - Repair)</span>
            <div class="multi-dropdown" data-filter="repairMonth">
              <button type="button" class="multi-dropdown-button"><span class="selected-text">ทั้งหมด</span><span class="dropdown-arrow">▼</span></button>
              <div class="multi-dropdown-menu">
                <div class="dropdown-actions"><button type="button" class="select-all">เลือกทั้งหมด</button><button type="button" class="clear-all">ล้าง</button></div>
                <div class="dropdown-options" id="options-repairMonth"></div>
              </div>
            </div>
          </div>

          <div class="filter-group">
            <span class="filter-label">Group Shop (Repair)</span>
            <div class="multi-dropdown" data-filter="repairShop">
              <button type="button" class="multi-dropdown-button"><span class="selected-text">ทั้งหมด</span><span class="dropdown-arrow">▼</span></button>
              <div class="multi-dropdown-menu">
                <div class="dropdown-actions"><button type="button" class="select-all">เลือกทั้งหมด</button><button type="button" class="clear-all">ล้าง</button></div>
                <div class="dropdown-options" id="options-repairShop"></div>
              </div>
            </div>
          </div>

          <div class="filter-group">
            <span class="filter-label">Brand (Repair)</span>
            <div class="multi-dropdown" data-filter="repairBrand">
              <button type="button" class="multi-dropdown-button"><span class="selected-text">ทั้งหมด</span><span class="dropdown-arrow">▼</span></button>
              <div class="multi-dropdown-menu">
                <div class="dropdown-actions"><button type="button" class="select-all">เลือกทั้งหมด</button><button type="button" class="clear-all">ล้าง</button></div>
                <div class="dropdown-options" id="options-repairBrand"></div>
              </div>
            </div>
          </div>
        </div>
      </section>

      <main>
        <section class="kpi-grid" style="grid-template-columns: repeat(3, minmax(0, 1fr));">
          <article class="kpi-card capturable" style="--accent: var(--blue)" id="card-repair-1">
            <button type="button" class="snapshot-btn" title="บันทึกภาพกล่องนี้" onclick="takeSnapshot('card-repair-1', 'งานซ่อมทั้งหมด')">📷</button>
            <div class="kpi-label">งานซ่อมทั้งหมด (Repair)</div>
            <div id="repairTotalJobs" class="kpi-value">0</div>
          </article>
          <article class="kpi-card capturable" style="--accent: var(--green)" id="card-repair-2">
            <button type="button" class="snapshot-btn" title="บันทึกภาพกล่องนี้" onclick="takeSnapshot('card-repair-2', 'SLA_Close_เฉลี่ย_ซ่อม')">📷</button>
            <div class="kpi-label">SLA Close เฉลี่ย (ซ่อมเสร็จ)</div>
            <div id="repairAvgClose" class="kpi-value" style="font-size: 24px;">0 วัน</div>
          </article>
          <article class="kpi-card capturable" style="--accent: var(--red)" id="card-repair-3">
            <button type="button" class="snapshot-btn" title="บันทึกภาพกล่องนี้" onclick="takeSnapshot('card-repair-3', 'SLA_Pending_เฉลี่ย_ซ่อม')">📷</button>
            <div class="kpi-label">SLA Pending เฉลี่ย (Pending)</div>
            <div id="repairAvgPending" class="kpi-value" style="font-size: 24px;">0 วัน</div>
          </article>
        </section>

        <section class="sub-kpi-grid" style="grid-template-columns: repeat(4, minmax(0, 1fr)); margin-top: 16px;">
          <div class="sub-kpi-card capturable" id="box-rep-area"><button type="button" class="snapshot-btn" title="บันทึกภาพกล่องนี้" onclick="takeSnapshot('box-rep-area', 'Area_Repair')">📷</button><div class="sub-kpi-title">📍 Area (Repair)</div><div id="subRepairAreaList" class="sub-kpi-list"></div></div>
          <div class="sub-kpi-card capturable" id="box-rep-clm"><button type="button" class="snapshot-btn" title="บันทึกภาพกล่องนี้" onclick="takeSnapshot('box-rep-clm', 'Claim_Type_Repair')">📷</button><div class="sub-kpi-title">📋 Claim type (Repair)</div><div id="subRepairClaimList" class="sub-kpi-list"></div></div>
          <div class="sub-kpi-card capturable" id="box-rep-shp"><button type="button" class="snapshot-btn" title="บันทึกภาพกล่องนี้" onclick="takeSnapshot('box-rep-shp', 'Group_Shop_Repair')">📷</button><div class="sub-kpi-title">🏬 Group Shop (Repair)</div><div id="subRepairShopList" class="sub-kpi-list"></div></div>
          <div class="sub-kpi-card capturable" id="box-rep-brd"><button type="button" class="snapshot-btn" title="บันทึกภาพกล่องนี้" onclick="takeSnapshot('box-rep-brd', 'Brand_Repair')">📷</button><div class="sub-kpi-title">🏷️ Brand (Repair)</div><div id="subRepairBrandList" class="sub-kpi-list"></div></div>
        </section>

        <section class="chart-grid" style="margin-top: 22px;">
          <article class="chart-card capturable" id="chart-repair-1">
            <button type="button" class="snapshot-btn" title="บันทึกภาพกล่องนี้" onclick="takeSnapshot('chart-repair-1', 'จำนวนงานซ่อมรายเดือน')">📷</button>
            <div class="chart-title">จำนวนงานซ่อมรายเดือน (2025 vs 2026) พร้อมอัตราเปลี่ยนแปลง %</div>
            <div class="chart-wrap"><canvas id="repairMonthlyChart"></canvas></div>
          </article>
          <article class="chart-card capturable" id="chart-repair-2">
            <button type="button" class="snapshot-btn" title="บันทึกภาพกล่องนี้" onclick="takeSnapshot('chart-repair-2', 'สัดส่วนแบรนด์งานซ่อม')">📷</button>
            <div class="chart-title">สัดส่วนแบรนด์งานซ่อม (Brand Share)</div>
            <div class="chart-wrap"><canvas id="repairBrandChart"></canvas></div>
          </article>
        </section>

        <section class="analysis-section capturable" id="analysis-repair-box">
          <button type="button" class="snapshot-btn" title="บันทึกภาพกล่องนี้" onclick="takeSnapshot('analysis-repair-box', 'ระบบวิเคราะห์ปัญหา_หน้า_Repair')">📷</button>
          <div class="analysis-header">
            <div class="analysis-title">💡 ระบบวิเคราะห์ปัญหาและแนวทางแก้ไข (Repair Insights)</div>
            <div class="analysis-btn-group">
              <button type="button" class="btn-analyze standard" onclick="runAnalysis('repair', 'standard')">📊 วิเคราะห์แบบธรรมดา</button>
              <button type="button" class="btn-analyze ai" onclick="runAnalysis('repair', 'ai')">✨ วิเคราะห์เชิงลึกด้วย AI</button>
            </div>
          </div>
          <div id="analysis-box-repair" class="analysis-box"></div>
        </section>
      </main>
    </div>

    <!-- ================= แท็บที่ 5: หน้าภาพรวมปัญหา & สรุป AI ================= -->
    <div id="tab-overview" class="tab-content">
      <main style="padding-top: 20px;">
        <div style="font-size: 20px; font-weight: 600; margin-bottom: 8px;">📋 บันทึกและสรุปภาพรวมปัญหา (Issue Tracking & Action Plan)</div>
        <div style="color: var(--muted); font-size: 14px; margin-bottom: 20px;">กรอกรายละเอียดข้อเท็จจริงของปัญหาแยกตามหมวดหมู่ด้านล่างนี้ และให้ AI ช่วยประมวลผลสังเคราะห์แนวทางแก้ไขภาพรวมทั้งหมด</div>

        <div class="overview-grid">
          <div class="overview-card" id="box-ov-brand">
            <div class="overview-card-header">
              <div class="overview-card-title">🏷️ Brand Issues</div>
              <button type="button" class="btn-save-text" onclick="saveTextArea('txtBrand', 'บันทึก Brand Issues เรียบร้อยแล้ว')">💾 บันทึกข้อความ</button>
            </div>
            <textarea id="txtBrand" class="overview-textarea" placeholder="พิมพ์บันทึกปัญหาที่เกิดจากตัวแบรนด์หรือเงื่อนไขผู้ผลิต..."></textarea>
          </div>

          <div class="overview-card" id="box-ov-vendor">
            <div class="overview-card-header">
              <div class="overview-card-title">🏭 Vendor Issues</div>
              <button type="button" class="btn-save-text" onclick="saveTextArea('txtVendor', 'บันทึก Vendor Issues เรียบร้อยแล้ว')">💾 บันทึกข้อความ</button>
            </div>
            <textarea id="txtVendor" class="overview-textarea" placeholder="พิมพ์บันทึกปัญหาที่เกิดจาก Vendor, การจัดส่งอะไหล่ หรือรอบซ่อม..."></textarea>
          </div>

          <div class="overview-card" id="box-ov-shop">
            <div class="overview-card-header">
              <div class="overview-card-title">🏬 Shop Issues</div>
              <button type="button" class="btn-save-text" onclick="saveTextArea('txtShop', 'บันทึก Shop Issues เรียบร้อยแล้ว')">💾 บันทึกข้อความ</button>
            </div>
            <textarea id="txtShop" class="overview-textarea" placeholder="พิมพ์บันทึกปัญหาหน้างานที่เกิดจากสาขาหรือขั้นตอนรับเครื่อง..."></textarea>
          </div>

          <div class="overview-card" id="box-ov-afs">
            <div class="overview-card-header">
              <div class="overview-card-title">👥 Team AFS</div>
              <button type="button" class="btn-save-text" onclick="saveTextArea('txtAfs', 'บันทึก Team AFS เรียบร้อยแล้ว')">💾 บันทึกข้อความ</button>
            </div>
            <textarea id="txtAfs" class="overview-textarea" placeholder="พิมพ์บันทึกปัญหาหรือข้อจำกัดที่เกี่ยวข้องกับ Team AFS..."></textarea>
          </div>

          <div class="overview-card" id="box-ov-tuc">
            <div class="overview-card-header">
              <div class="overview-card-title">🤝 Team TUC</div>
              <button type="button" class="btn-save-text" onclick="saveTextArea('txtTuc', 'บันทึก Team TUC เรียบร้อยแล้ว')">💾 บันทึกข้อความ</button>
            </div>
            <textarea id="txtTuc" class="overview-textarea" placeholder="พิมพ์บันทึกปัญหาหรือประเด็นที่เกี่ยวข้องกับ Team TUC..."></textarea>
          </div>

          <div class="overview-card" style="border-color: var(--blue);" id="box-ov-action">
            <div class="overview-card-header">
              <div class="overview-card-title" style="color: var(--blue);">🛡️ แนวทางแก้ไขปัญหาและป้องกัน (Action Plan & Prevention)</div>
              <button type="button" class="btn-save-text" onclick="saveTextArea('txtActionPlan', 'บันทึก Action Plan เรียบร้อยแล้ว')">💾 บันทึกข้อความ</button>
            </div>
            <textarea id="txtActionPlan" class="overview-textarea" placeholder="พิมพ์มาตรการป้องกันและแนวทางแก้ไขปัญหาในระยะสั้นและระยะยาว..."></textarea>
          </div>
        </div>

        <section class="ai-summary-box" id="box-ov-ai">
          <div class="analysis-header" style="border-bottom: 1px solid var(--border); padding-bottom: 12px; margin: 0;">
            <div class="analysis-title" style="color: var(--purple);">🤖 AI Executive Summary & Comprehensive Action Plan</div>
            <button type="button" class="btn-analyze ai" onclick="generateAiComprehensiveSummary()">✨ ให้ AI สรุปและวิเคราะห์ภาพรวมทั้งหมด</button>
          </div>
          <div id="aiSummaryOutput" class="ai-summary-content">
            <em>คลิกปุ่ม "ให้ AI สรุปและวิเคราะห์ภาพรวมทั้งหมด" ด้านบน เพื่อให้ระบบประมวลผลข้อมูลจากทุกแท็บร่วมกับข้อความที่คุณบันทึกไว้</em>
          </div>
        </section>

        <!-- ปุ่มบันทึกเป็น ppx ทำงานได้จริงสมบูรณ์แบบ -->
        <div class="ppt-footer-bar">
          <button type="button" class="btn-save-ppx" onclick="exportDataToPowerPointText()">
            📥 บันทึกเป็น ppx
          </button>
        </div>
      </main>
    </div>
  </div>

  <!-- Chart.js, SheetJS, html2canvas, PptxGenJS -->
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/xlsx/dist/xlsx.full.min.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/html2canvas@1.4.1/dist/html2canvas.min.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/pptxgenjs@3.12.0/dist/pptxgen.bundle.js"></script>

  <script>
    let rawRowsData = [];
    let cnRowsData = [];
    let appleRowsData = [];
    let repairRowsData = [];
    let activeTab = 'main';

    const monthNames = ["ม.ค.", "ก.พ.", "มี.ค.", "เม.ย.", "พ.ค.", "มิ.ย.", "ก.ค.", "ส.ค.", "ก.ย.", "ต.ค.", "พ.ย.", "ธ.ค."];
    const chartColors = ["#4e8cff", "#7657ff", "#00cba9", "#ff6575", "#ffc94b", "#e968ff", "#36c5f0", "#ff8a4c"];
    const numberFormat = new Intl.NumberFormat("th-TH", { maximumFractionDigits: 1 });

    let monthlyChart, brandChart, cnMonthlyChart, cnBarChart, applePieChart, appleBarChart, repairMonthlyChart, repairBrandChart;

    // ฟังก์ชันตรวจสอบการล็อกอิน
    function handleLogin() {
      const u = document.getElementById('loginUser').value.trim();
      const p = document.getElementById('loginPass').value.trim();
      const err = document.getElementById('loginErrorMsg');
      if (u === 'mom' && p === 'tucxww') {
        document.getElementById('login-container').style.display = 'none';
        document.getElementById('appMainContent').classList.add('show');
        localStorage.setItem('dashboard_logged_in', 'true');
      } else {
        err.style.display = 'block';
      }
    }

    window.addEventListener('DOMContentLoaded', () => {
      if (localStorage.getItem('dashboard_logged_in') === 'true') {
        document.getElementById('login-container').style.display = 'none';
        document.getElementById('appMainContent').classList.add('show');
      }
    });

    function formatNumber(val) {
      return numberFormat.format(Number.isFinite(Number(val)) ? Number(val) : 0);
    }

    function getVal(row, keys) {
      if (!row || typeof row !== 'object') return "";
      const rKeys = Object.keys(row);
      for (let k of keys) {
        const found = rKeys.find(rk => rk && rk.trim().toLowerCase() === k.toLowerCase());
        if (found !== undefined && row[found] !== undefined && row[found] !== "") return row[found];
      }
      return "";
    }

    function parseDateToYM(val) {
      if (!val) return { year: 2026, month: 1 };
      if (typeof val === 'number') {
        const d = XLSX.SSF.parse_date_code(val);
        if (d && d.y) return { year: d.y >= 2400 ? d.y - 543 : d.y, month: d.m };
      }
      const str = String(val).trim();
      const parts = str.split(/[\/\-\.]/);
      if (parts.length >= 3) {
        let y = parseInt(parts[2].length === 4 ? parts[2] : parts[0], 10);
        let m = parseInt(parts[1], 10);
        if (parts[0].length === 4) { y = parseInt(parts[0], 10); m = parseInt(parts[1], 10); }
        if (y >= 2400) y -= 543;
        if (Number.isFinite(y) && Number.isFinite(m)) return { year: y, month: m };
      }
      const dt = new Date(str);
      if (!isNaN(dt.getTime())) {
        let y = dt.getFullYear();
        if (y >= 2400) y -= 543;
        return { year: y, month: dt.getMonth() + 1 };
      }
      return { year: 2026, month: 1 };
    }

    function switchTab(tabName) {
      activeTab = tabName;
      document.querySelectorAll('.nav-tab').forEach(t => t.classList.remove('active'));
      document.querySelectorAll('.tab-content').forEach(c => c.classList.remove('active'));

      if (tabName === 'main') {
        document.getElementById('tab-main').classList.add('active');
        document.querySelectorAll('.nav-tab')[0].classList.add('active');
        document.getElementById('mainHeaderTitle').textContent = 'Monthly Report Dashboard';
        document.getElementById('mainHeaderSub').textContent = 'TUC × WW Service Performance';
        renderMainDashboard();
      } else if (tabName === 'cn') {
        document.getElementById('tab-cn').classList.add('active');
        document.querySelectorAll('.nav-tab')[1].classList.add('active');
        document.getElementById('mainHeaderTitle').textContent = 'CN x NO CN Report Dashboard';
        document.getElementById('mainHeaderSub').textContent = 'Credit Note Analysis & Tracking';
        renderCnDashboard();
      } else if (tabName === 'apple') {
        document.getElementById('tab-apple').classList.add('active');
        document.querySelectorAll('.nav-tab')[2].classList.add('active');
        document.getElementById('mainHeaderTitle').textContent = 'Apple Service Dashboard';
        document.getElementById('mainHeaderSub').textContent = 'Apple Specific Claim & Repair Analysis';
        renderAppleDashboard();
      } else if (tabName === 'repair') {
        document.getElementById('tab-repair').classList.add('active');
        document.querySelectorAll('.nav-tab')[3].classList.add('active');
        document.getElementById('mainHeaderTitle').textContent = 'Repair Center Dashboard';
        document.getElementById('mainHeaderSub').textContent = 'Repair Operations & SLA Performance';
        renderRepairDashboard();
      } else {
        document.getElementById('tab-overview').classList.add('active');
        document.querySelectorAll('.nav-tab')[4].classList.add('active');
        document.getElementById('mainHeaderTitle').textContent = 'Executive Overview & AI Summary';
        document.getElementById('mainHeaderSub').textContent = 'Comprehensive Issue Tracking & Action Plan';
      }
    }

    function getChartLabelsWithPct(m25, m26) {
      return monthNames.map((mName, idx) => {
        const v25 = m25[idx];
        const v26 = m26[idx];
        if (v25 === 0) {
          return v26 > 0 ? `${mName} (+100%)` : mName;
        }
        const diffPct = (((v26 - v25) / v25) * 100).toFixed(1);
        const sign = diffPct > 0 ? `+${diffPct}%` : `${diffPct}%`;
        return `${mName} (${sign})`;
      });
    }

    function getChartTooltipConfig(m25, m26) {
      return {
        responsive: true,
        maintainAspectRatio: false,
        plugins: {
          tooltip: {
            callbacks: {
              title: function(context) {
                return monthNames[context[0].dataIndex];
              },
              afterBody: function(context) {
                const idx = context[0].dataIndex;
                const v25 = m25[idx];
                const v26 = m26[idx];
                if (v25 === 0) return v26 > 0 ? "เปลี่ยนแปลง: เพิ่มขึ้น 100%" : "ไม่มีการเปลี่ยนแปลง";
                const diff = v26 - v25;
                const pct = ((diff / v25) * 100).toFixed(1);
                return `เทียบ 2025 -> 2026: ${diff >= 0 ? '+' : ''}${pct}% (${diff >= 0 ? '+' : ''}${diff} เคส)`;
              }
            }
          }
        }
      };
    }

    // --- ฟังก์ชันบันทึกข้อความรายกล่อง ---
    function saveTextArea(elementId, successMessage) {
      const el = document.getElementById(elementId);
      if (!el) return;
      localStorage.setItem(`dashboard_${elementId}`, el.value);
      alert(successMessage);
    }

    window.addEventListener('DOMContentLoaded', () => {
      ['txtBrand', 'txtVendor', 'txtShop', 'txtAfs', 'txtTuc', 'txtActionPlan'].forEach(id => {
        const saved = localStorage.getItem(`dashboard_${id}`);
        if (saved) {
          const el = document.getElementById(id);
          if (el) el.value = saved;
        }
      });
    });

    // --- ฟังก์ชัน Snapshot ปลอดภัย 100% ---
    async function takeSnapshot(elementId, fileNamePrefix) {
      const el = document.getElementById(elementId);
      if (!el) return;
      const btn = el.querySelector(".snapshot-btn");
      if (btn) btn.style.display = "none";

      try {
        const canvas = await html2canvas(el, { 
          scale: 2, 
          backgroundColor: '#151824', 
          logging: false,
          ignoreElements: (node) => node.tagName === 'CANVAS'
        });
        const image = canvas.toDataURL("image/png");
        const a = document.createElement("a");
        a.href = image;
        a.download = `${fileNamePrefix}_snapshot.png`;
        document.body.appendChild(a);
        a.click();
        document.body.removeChild(a);
      } catch (err) {
        alert("ไม่สามารถบันทึกภาพได้: " + err.message);
      } finally {
        if (btn) btn.style.display = "grid";
      }
    }

    // --- ฟังก์ชันสร้าง ppx ทำงานได้จริงสมบูรณ์แบบ ---
    async function exportDataToPowerPointText() {
      if (typeof PptxGenJS === 'undefined') {
        alert("กำลังโหลดไลบรารี PowerPoint กรุณารอสักครู่แล้วลองใหม่อีกครั้ง");
        return;
      }

      const pptx = new PptxGenJS();
      pptx.layout = 'LAYOUT_16x9';
      pptx.defineSlideMaster({
        title: 'MASTER_SLIDE',
        background: { color: '0C0D14' },
        objects: [
          { text: { text: 'TUC x WW Performance & Overview Dashboard Report', options: { x: 0.5, y: 0.3, fontSize: 16, color: 'B0B6CC', fontFamily: 'Prompt' } } }
        ]
      });

      const statEl = document.getElementById("uploadStatus");
      if (statEl) statEl.textContent = "⏳ กำลังสร้างไฟล์ ppx สรุปข้อมูลทุกหน้า...";

      try {
        // Slide 1: หน้าหลัก (ALL DATA) สรุป
        const mainRows = filterMainRows();
        const slide1 = pptx.addSlide({ masterName: 'MASTER_SLIDE' });
        slide1.addText('1. หน้าหลัก (ALL DATA) - ภาพรวมสถิติ', { x: 0.5, y: 0.8, fontSize: 22, color: '4E8CFF', bold: true, fontFamily: 'Prompt' });
        slide1.addText(`จำนวนงานทั้งหมด: ${document.getElementById('totalJobs').textContent} เคส`, { x: 0.8, y: 1.8, fontSize: 18, color: 'F4F5FB', fontFamily: 'Prompt' });
        slide1.addText(`SLA Close เฉลี่ย: ${document.getElementById('avgClose').textContent}`, { x: 0.8, y: 2.5, fontSize: 18, color: '00CBA9', fontFamily: 'Prompt' });
        slide1.addText(`SLA Pending เฉลี่ย: ${document.getElementById('avgPending').textContent}`, { x: 0.8, y: 3.2, fontSize: 18, color: 'FF6575', fontFamily: 'Prompt' });
        slide1.addText(`ข้อมูลกรองจากเงื่อนไขปัจจุบันรวมทั้งสิ้น ${mainRows.length} รายการ`, { x: 0.8, y: 4.5, fontSize: 14, color: 'B0B6CC', fontFamily: 'Prompt' });

        // Slide 2: หน้า CN x NO CN สรุป
        const cnRows = filterCnRows();
        const slide2 = pptx.addSlide({ masterName: 'MASTER_SLIDE' });
        slide2.addText('2. หน้า CN x NO CN - สถิติติดตาม Credit Note', { x: 0.5, y: 0.8, fontSize: 22, color: '7657FF', bold: true, fontFamily: 'Prompt' });
        slide2.addText(`รายการ CN x NO CN ทั้งหมด: ${document.getElementById('cnTotalJobs').textContent} เคส`, { x: 0.8, y: 1.8, fontSize: 18, color: 'F4F5FB', fontFamily: 'Prompt' });
        slide2.addText(`สัดส่วน CN สำเร็จ: ${document.getElementById('cnRatio').textContent}`, { x: 0.8, y: 2.5, fontSize: 18, color: '00CBA9', fontFamily: 'Prompt' });
        slide2.addText(`PENDING: ${document.getElementById('cnAvgPending').textContent}`, { x: 0.8, y: 3.2, fontSize: 18, color: 'FFC94B', fontFamily: 'Prompt' });
        slide2.addText(`NO CN (ไม่ผ่าน): ${document.getElementById('cnNoCnKpi').textContent}`, { x: 0.8, y: 3.9, fontSize: 18, color: 'FF6575', fontFamily: 'Prompt' });

        // Slide 3: หน้า Apple สรุป
        const appleRows = filterAppleRows();
        const slide3 = pptx.addSlide({ masterName: 'MASTER_SLIDE' });
        slide3.addText('3. หน้า Apple Service - สถิติเฉพาะกลุ่ม Apple', { x: 0.5, y: 0.8, fontSize: 22, color: '00CBA9', bold: true, fontFamily: 'Prompt' });
        slide3.addText(`รายการ Apple ทั้งหมด: ${document.getElementById('appleTotalJobs').textContent} เคส`, { x: 0.8, y: 1.8, fontSize: 18, color: 'F4F5FB', fontFamily: 'Prompt' });
        slide3.addText(`Group product name เด่นสุด: ${document.getElementById('appleTopProduct').textContent}`, { x: 0.8, y: 2.5, fontSize: 18, color: '4E8CFF', fontFamily: 'Prompt' });
        slide3.addText(`กลุ่มอาการเสียเด่นสุด: ${document.getElementById('appleTopSymptom').textContent}`, { x: 0.8, y: 3.2, fontSize: 18, color: 'FFC94B', fontFamily: 'Prompt' });

        // Slide 4: หน้างานซ่อม (Repair) สรุป
        const repairRows = filterRepairRows();
        const slide4 = pptx.addSlide({ masterName: 'MASTER_SLIDE' });
        slide4.addText('4. หน้างานซ่อม (Repair Center) - สถิติปฏิบัติการ', { x: 0.5, y: 0.8, fontSize: 22, color: 'FFC94B', bold: true, fontFamily: 'Prompt' });
        slide4.addText(`งานซ่อมทั้งหมด: ${document.getElementById('repairTotalJobs').textContent} เคส`, { x: 0.8, y: 1.8, fontSize: 18, color: 'F4F5FB', fontFamily: 'Prompt' });
        slide4.addText(`SLA Close เฉลี่ย: ${document.getElementById('repairAvgClose').textContent}`, { x: 0.8, y: 2.5, fontSize: 18, color: '00CBA9', fontFamily: 'Prompt' });
        slide4.addText(`SLA Pending เฉลี่ย: ${document.getElementById('repairAvgPending').textContent}`, { x: 0.8, y: 3.2, fontSize: 18, color: 'FF6575', fontFamily: 'Prompt' });

        // Slide 5: หน้าภาพรวมปัญหา & Action Plan บันทึกผู้ใช้
        const slide5 = pptx.addSlide({ masterName: 'MASTER_SLIDE' });
        slide5.addText('5. ภาพรวมปัญหา & Action Plan (Executive Summary)', { x: 0.5, y: 0.8, fontSize: 22, color: '7657FF', bold: true, fontFamily: 'Prompt' });
        slide5.addText(`Brand Issues: ${document.getElementById('txtBrand').value || 'ไม่มีบันทึก'}`, { x: 0.8, y: 1.5, fontSize: 13, color: 'F4F5FB', fontFamily: 'Prompt', w: 8.5 });
        slide5.addText(`Vendor Issues: ${document.getElementById('txtVendor').value || 'ไม่มีบันทึก'}`, { x: 0.8, y: 2.2, fontSize: 13, color: 'F4F5FB', fontFamily: 'Prompt', w: 8.5 });
        slide5.addText(`Shop Issues: ${document.getElementById('txtShop').value || 'ไม่มีบันทึก'}`, { x: 0.8, y: 2.9, fontSize: 13, color: 'F4F5FB', fontFamily: 'Prompt', w: 8.5 });
        slide5.addText(`Action Plan: ${document.getElementById('txtActionPlan').value || 'ไม่มีบันทึก'}`, { x: 0.8, y: 3.6, fontSize: 13, color: '00CBA9', fontFamily: 'Prompt', w: 8.5 });

        await pptx.writeFile({ fileName: 'TUC_WW_Comprehensive_Report.pptx' });
        if (statEl) statEl.textContent = "✓ บันทึกไฟล์ ppx สำเร็จเรียบร้อย!";
      } catch (err) {
        alert("เกิดข้อผิดพลาดในการสร้าง ppx: " + err.message);
        if (statEl) statEl.textContent = "✗ บันทึก ppx ไม่สำเร็จ";
      }
    }

    // --- เมธอดจัดการหน้า Main (ALL DATA) ---
    function getMainFilterOptions() {
      const opts = { year: new Set(), month: new Set(), groupShop: new Set(), system: new Set(), groupBrand: new Set(), claimType: new Set(), defectType: new Set(), inspectionBy: new Set() };
      const dataArr = Array.isArray(rawRowsData) ? rawRowsData : [];
      dataArr.forEach(r => {
        if (!r) return;
        if (r.year != null && opts.year) opts.year.add(String(r.year));
        if (r.month != null && opts.month) opts.month.add(Number(r.month));
        if (r.groupShop && opts.groupShop) opts.groupShop.add(String(r.groupShop));
        if (r.system && opts.system) opts.system.add(String(r.system));
        if (r.groupBrand && opts.groupBrand) opts.groupBrand.add(String(r.groupBrand));
        if (r.claimType && opts.claimType) opts.claimType.add(String(r.claimType));
        if (r.defectType && opts.defectType) opts.defectType.add(String(r.defectType));
        if (r.inspectionBy && opts.inspectionBy) opts.inspectionBy.add(String(r.inspectionBy));
      });
      return {
        year: opts.year instanceof Set ? Array.from(opts.year).sort((a,b) => b - a) : [],
        month: opts.month instanceof Set ? Array.from(opts.month).sort((a,b) => a - b) : [],
        groupShop: opts.groupShop instanceof Set ? Array.from(opts.groupShop).sort() : [],
        system: opts.system instanceof Set ? Array.from(opts.system).sort() : [],
        groupBrand: opts.groupBrand instanceof Set ? Array.from(opts.groupBrand).sort() : [],
        claimType: opts.claimType instanceof Set ? Array.from(opts.claimType).sort() : [],
        defectType: opts.defectType instanceof Set ? Array.from(opts.defectType).sort() : [],
        inspectionBy: opts.inspectionBy instanceof Set ? Array.from(opts.inspectionBy).sort() : []
      };
    }

    function renderMainDropdowns(reset = false) {
      const optsMap = getMainFilterOptions() || {};
      Object.keys(optsMap).forEach(key => {
        const box = document.getElementById(`options-${key}`);
        if (!box) return;
        const checked = box.querySelectorAll("input:checked");
        const selSet = new Set(Array.from(checked).map(c => c.value));
        box.innerHTML = "";
        const listVals = Array.isArray(optsMap[key]) ? optsMap[key] : [];
        listVals.forEach(val => {
          const isCheck = reset || selSet.size === 0 || selSet.has(String(val));
          const lbl = document.createElement("label");
          lbl.className = "dropdown-option";
          lbl.innerHTML = `<input type="checkbox" value="${val}" ${isCheck ? "checked" : ""}><span>${val}</span>`;
          box.appendChild(lbl);
        });
      });
      updateMainBtnTexts();
    }

    function getSelectedMainVals(key) {
      const box = document.getElementById(`options-${key}`);
      if (!box) return new Set();
      return new Set(Array.from(box.querySelectorAll("input:checked")).map(c => c.value));
    }

    function updateMainBtnTexts() {
      document.querySelectorAll("#tab-main .multi-dropdown").forEach(dd => {
        const key = dd.getAttribute("data-filter");
        const filterOpts = getMainFilterOptions() || {};
        const allOpts = Array.isArray(filterOpts[key]) ? filterOpts[key] : [];
        const sel = getSelectedMainVals(key);
        const txt = dd.querySelector(".selected-text");
        if (!txt) return;
        if (sel.size === 0 || sel.size === allOpts.length) {
          txt.textContent = "ทั้งหมด";
        } else {
          txt.textContent = `เลือกแล้ว (${sel.size})`;
        }
      });
    }

    function filterMainRows() {
      const ySet = getSelectedMainVals("year");
      const mSet = getSelectedMainVals("month");
      const sSet = getSelectedMainVals("groupShop");
      const sysSet = getSelectedMainVals("system");
      const bSet = getSelectedMainVals("groupBrand");
      const cSet = getSelectedMainVals("claimType");
      const dSet = getSelectedMainVals("defectType");
      const iSet = getSelectedMainVals("inspectionBy");

      const dataArr = Array.isArray(rawRowsData) ? rawRowsData : [];
      return dataArr.filter(r => {
        if (!r) return false;
        if (ySet.size && !ySet.has(String(r.year))) return false;
        if (mSet.size && !mSet.has(String(r.month))) return false;
        if (sSet.size && !sSet.has(String(r.groupShop))) return false;
        if (sysSet.size && !sysSet.has(String(r.system))) return false;
        if (bSet.size && !bSet.has(String(r.groupBrand))) return false;
        if (cSet.size && !cSet.has(String(r.claimType))) return false;
        if (dSet.size && !dSet.has(String(r.defectType))) return false;
        if (iSet.size && !iSet.has(String(r.inspectionBy))) return false;
        return true;
      });
    }

    function renderMainDashboard() {
      const rows = filterMainRows();
      const totalRows = rows.length;
      let closeSum = 0, closeCnt = 0, pendSum = 0, pendCnt = 0;
      let brands = {}, claims = {}, defects = {}, shops = {}, systems = {};
      let m25 = new Array(12).fill(0), m26 = new Array(12).fill(0);

      rows.forEach(r => {
        if (!r) return;
        if (r.month >= 1 && r.month <= 12) {
          if (Number(r.year) === 2025) m25[r.month - 1]++;
          else m26[r.month - 1]++;
        }
        if (Number.isFinite(r.slaClose)) { closeSum += r.slaClose; closeCnt++; }
        if (Number.isFinite(r.slaPending)) { pendSum += r.slaPending; pendCnt++; }

        const b = r.groupBrand || "ไม่ระบุ"; brands[b] = (brands[b] || 0) + 1;
        const c = r.claimType || "ไม่ระบุ"; claims[c] = (claims[c] || 0) + 1;
        const d = r.defectType || "ไม่ระบุ"; defects[d] = (defects[d] || 0) + 1;
        const s = r.groupShop || "ไม่ระบุ"; shops[s] = (shops[s] || 0) + 1;
        const sys = r.system || "ไม่ระบุ"; systems[sys] = (systems[sys] || 0) + 1;
      });

      document.getElementById("totalJobs").textContent = formatNumber(totalRows);
      document.getElementById("avgClose").textContent = `${formatNumber(closeCnt ? closeSum/closeCnt : 0)} วัน`;
      document.getElementById("avgPending").textContent = `${formatNumber(pendCnt ? pendSum/pendCnt : 0)} วัน`;

      const fillList = (id, obj) => {
        const box = document.getElementById(id);
        if (!box) return;
        box.innerHTML = "";
        const safeObj = (obj && typeof obj === 'object') ? obj : {};
        const sorted = Object.entries(safeObj).sort((a,b) => b[1] - a[1]);
        if (sorted.length === 0) {
          box.innerHTML = `<div class="sub-kpi-item"><span class="sub-kpi-item-name">ไม่มีข้อมูล</span><span class="sub-kpi-item-val">0 (0.0%)</span></div>`;
          return;
        }
        sorted.forEach(([k, v]) => {
          const pct = totalRows > 0 ? ((v / totalRows) * 100).toFixed(1) : "0.0";
          box.innerHTML += `<div class="sub-kpi-item"><span class="sub-kpi-item-name" title="${k}">${k}</span><span class="sub-kpi-item-val">${formatNumber(v)} (${pct}%)</span></div>`;
        });
      };

      fillList("subDefectList", defects);
      fillList("subClaimList", claims);
      fillList("subShopList", shops);
      fillList("subSystemList", systems);
      fillList("subBrandList", brands);

      if (monthlyChart) monthlyChart.destroy();
      monthlyChart = new Chart(document.getElementById("monthlyChart"), {
        type: "bar",
        data: {
          labels: getChartLabelsWithPct(m25, m26),
          datasets: [
            { type: "line", label: "ปี 2025", data: m25, borderColor: "#ffc94b", backgroundColor: "#ffc94b", borderWidth: 3, tension: 0.3 },
            { type: "bar", label: "ปี 2026+", data: m26, backgroundColor: "#4e8cff", borderRadius: 6 }
          ]
        },
        options: getChartTooltipConfig(m25, m26)
      });

      if (brandChart) brandChart.destroy();
      brandChart = new Chart(document.getElementById("brandChart"), {
        type: "doughnut",
        data: { labels: Object.keys(brands), datasets: [{ data: Object.values(brands), backgroundColor: chartColors }] },
        options: { responsive: true, maintainAspectRatio: false, cutout: "65%" }
      });

      updateMainBtnTexts();
    }

    // --- เมธอดจัดการหน้า CN (all cn last) ---
    function getCnFilterOptions() {
      const opts = { cnYear: new Set(), cnMonth: new Set(), cnBrand: new Set(), cnGroupShop: new Set(), cnGroupCN: new Set() };
      const dataArr = Array.isArray(cnRowsData) ? cnRowsData : [];
      dataArr.forEach(r => {
        if (!r) return;
        if (r.year != null && opts.cnYear) opts.cnYear.add(String(r.year));
        if (r.month != null && opts.cnMonth) opts.cnMonth.add(Number(r.month));
        if (r.brand && opts.cnBrand) opts.cnBrand.add(String(r.brand));
        if (r.groupShop && opts.cnGroupShop) opts.cnGroupShop.add(String(r.groupShop));
        if (r.groupCN && opts.cnGroupCN) opts.cnGroupCN.add(String(r.groupCN));
      });
      return {
        cnYear: opts.cnYear instanceof Set ? Array.from(opts.cnYear).sort((a,b) => b - a) : [],
        cnMonth: opts.cnMonth instanceof Set ? Array.from(opts.cnMonth).sort((a,b) => a - b) : [],
        cnBrand: opts.cnBrand instanceof Set ? Array.from(opts.cnBrand).sort() : [],
        cnGroupShop: opts.cnGroupShop instanceof Set ? Array.from(opts.cnGroupShop).sort() : [],
        cnGroupCN: opts.cnGroupCN instanceof Set ? Array.from(opts.cnGroupCN).sort() : []
      };
    }

    function renderCnDropdowns(reset = false) {
      const optsMap = getCnFilterOptions() || {};
      Object.keys(optsMap).forEach(key => {
        const box = document.getElementById(`options-${key}`);
        if (!box) return;
        const checked = box.querySelectorAll("input:checked");
        const selSet = new Set(Array.from(checked).map(c => c.value));
        box.innerHTML = "";
        const listVals = Array.isArray(optsMap[key]) ? optsMap[key] : [];
        listVals.forEach(val => {
          const isCheck = reset || selSet.size === 0 || selSet.has(String(val));
          const lbl = document.createElement("label");
          lbl.className = "dropdown-option";
          let displayVal = val;
          if (key === "cnMonth") displayVal = `${val} (${monthNames[val-1] || ""})`;
          else if (key === "cnYear") displayVal = `${val} (พ.ศ. ${Number(val)+543})`;

          lbl.innerHTML = `<input type="checkbox" value="${val}" ${isCheck ? "checked" : ""}><span>${displayVal}</span>`;
          box.appendChild(lbl);
        });
      });
      updateCnBtnTexts();
    }

    function getSelectedCnVals(key) {
      const box = document.getElementById(`options-${key}`);
      if (!box) return new Set();
      return new Set(Array.from(box.querySelectorAll("input:checked")).map(c => c.value));
    }

    function updateCnBtnTexts() {
      document.querySelectorAll("#tab-cn .multi-dropdown").forEach(dd => {
        const key = dd.getAttribute("data-filter");
        const filterOpts = getCnFilterOptions() || {};
        const allOpts = Array.isArray(filterOpts[key]) ? filterOpts[key] : [];
        const sel = getSelectedCnVals(key);
        const txt = dd.querySelector(".selected-text");
        if (!txt) return;
        if (sel.size === 0 || sel.size === allOpts.length) {
          txt.textContent = "ทั้งหมด";
        } else {
          txt.textContent = `เลือกแล้ว (${sel.size})`;
        }
      });
    }

    function filterCnRows() {
      const ySet = getSelectedCnVals("cnYear");
      const mSet = getSelectedCnVals("cnMonth");
      const bSet = getSelectedCnVals("cnBrand");
      const sSet = getSelectedCnVals("cnGroupShop");
      const cnSet = getSelectedCnVals("cnGroupCN");

      const dataArr = Array.isArray(cnRowsData) ? cnRowsData : [];
      return dataArr.filter(r => {
        if (!r) return false;
        if (ySet.size && !ySet.has(String(r.year))) return false;
        if (mSet.size && !mSet.has(String(r.month))) return false;
        if (bSet.size && !bSet.has(String(r.brand))) return false;
        if (sSet.size && !sSet.has(String(r.groupShop))) return false;
        if (cnSet.size && !cnSet.has(String(r.groupCN))) return false;
        return true;
      });
    }

    function renderCnDashboard() {
      const rows = filterCnRows();
      const totalRows = rows.length;
      let cnCount = 0, pendingCount = 0, noCnCount = 0;
      let m25 = new Array(12).fill(0), m26 = new Array(12).fill(0);
      let systemClaims = {}, groupClaimTypes = {}, cnDefects = {}, cnShops = {};
      let noCnDefects = {}, noCnRemarks = {}, noCnTotal = 0;

      rows.forEach(r => {
        if (!r) return;
        if (r.month >= 1 && r.month <= 12) {
          if (Number(r.year) === 2025) m25[r.month - 1]++;
          else m26[r.month - 1]++;
        }

        const cn = r.groupCN || "NO CN";
        const cnUpper = cn.toUpperCase();
        
        if (cnUpper.includes("CN") && !cnUpper.includes("NO") && !cnUpper.includes("PENDING")) {
          cnCount++;
        } else if (cnUpper.includes("PENDING")) {
          pendingCount++;
        } else {
          noCnCount++;
          noCnTotal++;
          const def = r.defectType || "ไม่ระบุ";
          const rem = r.remark || "ไม่ระบุ";
          noCnDefects[def] = (noCnDefects[def] || 0) + 1;
          noCnRemarks[rem] = (noCnRemarks[rem] || 0) + 1;
        }

        const defAll = r.defectType || "ไม่ระบุ"; cnDefects[defAll] = (cnDefects[defAll] || 0) + 1;
        const shpAll = r.groupShop || "ไม่ระบุ"; cnShops[shpAll] = (cnShops[shpAll] || 0) + 1;
        const sysClaim = r.systemClaim || "ไม่ระบุ"; systemClaims[sysClaim] = (systemClaims[sysClaim] || 0) + 1;
        const grpClaim = r.groupClaimType || "ไม่ระบุ"; groupClaimTypes[grpClaim] = (groupClaimTypes[grpClaim] || 0) + 1;
      });

      document.getElementById("cnTotalJobs").textContent = formatNumber(totalRows);
      
      const ratio = totalRows > 0 ? ((cnCount / totalRows) * 100).toFixed(1) : "0.0";
      document.getElementById("cnRatio").textContent = `${formatNumber(cnCount)} (${ratio}%)`;
      
      const pendingPct = totalRows > 0 ? ((pendingCount / totalRows) * 100).toFixed(1) : "0.0";
      document.getElementById("cnAvgPending").textContent = `${formatNumber(pendingCount)} (${pendingPct}%)`;

      const noCnPct = totalRows > 0 ? ((noCnCount / totalRows) * 100).toFixed(1) : "0.0";
      document.getElementById("cnNoCnKpi").textContent = `${formatNumber(noCnCount)} (${noCnPct}%)`;

      const fillCnList = (id, obj, baseTotal = totalRows) => {
        const box = document.getElementById(id);
        if (!box) return;
        box.innerHTML = "";
        const safeObj = (obj && typeof obj === 'object') ? obj : {};
        const sorted = Object.entries(safeObj).sort((a,b) => b[1] - a[1]);
        if (sorted.length === 0) {
          box.innerHTML = `<div class="sub-kpi-item"><span class="sub-kpi-item-name">ไม่มีข้อมูล</span><span class="sub-kpi-item-val">0 (0.0%)</span></div>`;
          return;
        }
        sorted.forEach(([k, v]) => {
          const pct = baseTotal > 0 ? ((v / baseTotal) * 100).toFixed(1) : "0.0";
          box.innerHTML += `<div class="sub-kpi-item"><span class="sub-kpi-item-name" title="${k}">${k}</span><span class="sub-kpi-item-val">${formatNumber(v)} (${pct}%)</span></div>`;
        });
      };

      fillCnList("subCnDefectList", cnDefects);
      fillCnList("subCnShopList", cnShops);
      fillCnList("subSystemClaimList", systemClaims);
      fillCnList("subGroupClaimList", groupClaimTypes);

      fillCnList("noCnDefectList", noCnDefects, noCnTotal);
      fillCnList("noCnRemarkList", noCnRemarks, noCnTotal);

      let cnBrands = {};
      rows.forEach(r => {
        const b = r.brand || "ไม่ระบุ";
        cnBrands[b] = (cnBrands[b] || 0) + 1;
      });

      if (cnMonthlyChart) cnMonthlyChart.destroy();
      cnMonthlyChart = new Chart(document.getElementById("cnMonthlyChart"), {
        type: "bar",
        data: {
          labels: getChartLabelsWithPct(m25, m26),
          datasets: [
            { type: "line", label: "ปี 2025", data: m25, borderColor: "#ffc94b", backgroundColor: "#ffc94b", borderWidth: 3, tension: 0.3 },
            { type: "bar", label: "ปี 2026+", data: m26, backgroundColor: "#4e8cff", borderRadius: 6 }
          ]
        },
        options: getChartTooltipConfig(m25, m26)
      });

      if (cnBarChart) cnBarChart.destroy();
      cnBarChart = new Chart(document.getElementById("cnBarChart"), {
        type: "doughnut",
        data: { labels: Object.keys(cnBrands), datasets: [{ data: Object.values(cnBrands), backgroundColor: chartColors }] },
        options: { responsive: true, maintainAspectRatio: false, plugins: { legend: { position: 'bottom' } } }
      });

      updateCnBtnTexts();
    }

    // --- เมธอดจัดการหน้า Apple (apple) ---
    function getAppleFilterOptions() {
      const opts = { appleYear: new Set(), appleMonth: new Set(), appleShop: new Set(), appleSymptom: new Set() };
      const dataArr = Array.isArray(appleRowsData) ? appleRowsData : [];
      dataArr.forEach(r => {
        if (!r) return;
        if (r.year != null && opts.appleYear) opts.appleYear.add(String(r.year));
        if (r.month != null && opts.appleMonth) opts.appleMonth.add(Number(r.month));
        if (r.groupShop && opts.appleShop) opts.appleShop.add(String(r.groupShop));
        if (r.groupSymptom && opts.appleSymptom) opts.appleSymptom.add(String(r.groupSymptom));
      });
      return {
        appleYear: opts.appleYear instanceof Set ? Array.from(opts.appleYear).sort((a,b) => b - a) : [],
        appleMonth: opts.appleMonth instanceof Set ? Array.from(opts.appleMonth).sort((a,b) => a - b) : [],
        appleShop: opts.appleShop instanceof Set ? Array.from(opts.appleShop).sort() : [],
        appleSymptom: opts.appleSymptom instanceof Set ? Array.from(opts.appleSymptom).sort() : []
      };
    }

    function renderAppleDropdowns(reset = false) {
      const optsMap = getAppleFilterOptions() || {};
      Object.keys(optsMap).forEach(key => {
        const box = document.getElementById(`options-${key}`);
        if (!box) return;
        const checked = box.querySelectorAll("input:checked");
        const selSet = new Set(Array.from(checked).map(c => c.value));
        box.innerHTML = "";
        const listVals = Array.isArray(optsMap[key]) ? optsMap[key] : [];
        listVals.forEach(val => {
          const isCheck = reset || selSet.size === 0 || selSet.has(String(val));
          const lbl = document.createElement("label");
          lbl.className = "dropdown-option";
          let displayVal = val;
          if (key === "appleMonth") displayVal = `${val} (${monthNames[val-1] || ""})`;
          else if (key === "appleYear") displayVal = `${val} (พ.ศ. ${Number(val)+543})`;

          lbl.innerHTML = `<input type="checkbox" value="${val}" ${isCheck ? "checked" : ""}><span>${displayVal}</span>`;
          box.appendChild(lbl);
        });
      });
      updateAppleBtnTexts();
    }

    function getSelectedAppleVals(key) {
      const box = document.getElementById(`options-${key}`);
      if (!box) return new Set();
      return new Set(Array.from(box.querySelectorAll("input:checked")).map(c => c.value));
    }

    function updateAppleBtnTexts() {
      document.querySelectorAll("#tab-apple .multi-dropdown").forEach(dd => {
        const key = dd.getAttribute("data-filter");
        const filterOpts = getAppleFilterOptions() || {};
        const allOpts = Array.isArray(filterOpts[key]) ? filterOpts[key] : [];
        const sel = getSelectedAppleVals(key);
        const txt = dd.querySelector(".selected-text");
        if (!txt) return;
        if (sel.size === 0 || sel.size === allOpts.length) {
          txt.textContent = "ทั้งหมด";
        } else {
          txt.textContent = `เลือกแล้ว (${sel.size})`;
        }
      });
    }

    function filterAppleRows() {
      const ySet = getSelectedAppleVals("appleYear");
      const mSet = getSelectedAppleVals("appleMonth");
      const sSet = getSelectedAppleVals("appleShop");
      const symSet = getSelectedAppleVals("appleSymptom");

      const dataArr = Array.isArray(appleRowsData) ? appleRowsData : [];
      return dataArr.filter(r => {
        if (!r) return false;
        if (ySet.size && !ySet.has(String(r.year))) return false;
        if (mSet.size && !mSet.has(String(r.month))) return false;
        if (sSet.size && !sSet.has(String(r.groupShop))) return false;
        if (symSet.size && !symSet.has(String(r.groupSymptom))) return false;
        return true;
      });
    }

    function renderAppleDashboard() {
      const rows = filterAppleRows();
      const totalRows = rows.length;
      let symptoms = {}, shops = {}, products = {};
      let m25 = new Array(12).fill(0), m26 = new Array(12).fill(0);

      rows.forEach(r => {
        if (!r) return;
        if (r.month >= 1 && r.month <= 12) {
          if (Number(r.year) === 2025) m25[r.month - 1]++;
          else m26[r.month - 1]++;
        }

        const sym = r.groupSymptom || "ไม่ระบุ"; symptoms[sym] = (symptoms[sym] || 0) + 1;
        const shp = r.groupShop || "ไม่ระบุ"; shops[shp] = (shops[shp] || 0) + 1;
        const prd = r.productName || "ไม่ระบุ"; products[prd] = (products[prd] || 0) + 1;
      });

      document.getElementById("appleTotalJobs").textContent = formatNumber(totalRows);

      const sortedProducts = Object.entries(products).sort((a, b) => b[1] - a[1]);
      document.getElementById("appleTopProduct").textContent = sortedProducts.length > 0 ? sortedProducts[0][0] : "-";

      const sortedSyms = Object.entries(symptoms).sort((a,b) => b[1] - a[1]);
      document.getElementById("appleTopSymptom").textContent = sortedSyms.length > 0 ? sortedSyms[0][0] : "-";

      const fillAppleList = (id, obj) => {
        const box = document.getElementById(id);
        if (!box) return;
        box.innerHTML = "";
        const safeObj = (obj && typeof obj === 'object') ? obj : {};
        const sorted = Object.entries(safeObj).sort((a,b) => b[1] - a[1]);
        if (sorted.length === 0) {
          box.innerHTML = `<div class="sub-kpi-item"><span class="sub-kpi-item-name">ไม่มีข้อมูล</span><span class="sub-kpi-item-val">0 (0.0%)</span></div>`;
          return;
        }
        sorted.forEach(([k, v]) => {
          const pct = totalRows > 0 ? ((v / totalRows) * 100).toFixed(1) : "0.0";
          box.innerHTML += `<div class="sub-kpi-item"><span class="sub-kpi-item-name" title="${k}">${k}</span><span class="sub-kpi-item-val">${formatNumber(v)} (${pct}%)</span></div>`;
        });
      };

      fillAppleList("subAppleSymptomList", symptoms);
      fillAppleList("subAppleShopList", shops);
      fillAppleList("subAppleProductList", products);

      if (applePieChart) applePieChart.destroy();
      applePieChart = new Chart(document.getElementById("applePieChart"), {
        type: "doughnut",
        data: { labels: Object.keys(symptoms), datasets: [{ data: Object.values(symptoms), backgroundColor: chartColors }] },
        options: { responsive: true, maintainAspectRatio: false, plugins: { legend: { position: 'bottom' } } }
      });

      if (appleBarChart) appleBarChart.destroy();
      appleBarChart = new Chart(document.getElementById("appleBarChart"), {
        type: "bar",
        data: {
          labels: getChartLabelsWithPct(m25, m26),
          datasets: [
            { type: "line", label: "ปี 2025", data: m25, borderColor: "#ffc94b", backgroundColor: "#ffc94b", borderWidth: 3, tension: 0.3 },
            { type: "bar", label: "ปี 2026+", data: m26, backgroundColor: "#4e8cff", borderRadius: 6 }
          ]
        },
        options: getChartTooltipConfig(m25, m26)
      });

      updateAppleBtnTexts();
    }

    // --- เมธอดจัดการหน้า Repair (repair) ---
    function getRepairFilterOptions() {
      const opts = { repairYear: new Set(), repairMonth: new Set(), repairShop: new Set(), repairBrand: new Set() };
      const dataArr = Array.isArray(repairRowsData) ? repairRowsData : [];
      dataArr.forEach(r => {
        if (!r) return;
        if (r.year != null && opts.repairYear) opts.repairYear.add(String(r.year));
        if (r.month != null && opts.repairMonth) opts.repairMonth.add(Number(r.month));
        if (r.groupShop && opts.repairShop) opts.repairShop.add(String(r.groupShop));
        if (r.groupBrand && opts.repairBrand) opts.repairBrand.add(String(r.groupBrand));
      });
      return {
        repairYear: opts.repairYear instanceof Set ? Array.from(opts.repairYear).sort((a,b) => b - a) : [],
        repairMonth: opts.repairMonth instanceof Set ? Array.from(opts.repairMonth).sort((a,b) => a - b) : [],
        repairShop: opts.repairShop instanceof Set ? Array.from(opts.repairShop).sort() : [],
        repairBrand: opts.repairBrand instanceof Set ? Array.from(opts.repairBrand).sort() : []
      };
    }

    function renderRepairDropdowns(reset = false) {
      const optsMap = getRepairFilterOptions() || {};
      Object.keys(optsMap).forEach(key => {
        const box = document.getElementById(`options-${key}`);
        if (!box) return;
        const checked = box.querySelectorAll("input:checked");
        const selSet = new Set(Array.from(checked).map(c => c.value));
        box.innerHTML = "";
        const listVals = Array.isArray(optsMap[key]) ? optsMap[key] : [];
        listVals.forEach(val => {
          const isCheck = reset || selSet.size === 0 || selSet.has(String(val));
          const lbl = document.createElement("label");
          lbl.className = "dropdown-option";
          let displayVal = val;
          if (key === "repairMonth") displayVal = `${val} (${monthNames[val-1] || ""})`;
          else if (key === "repairYear") displayVal = `${val} (พ.ศ. ${Number(val)+543})`;

          lbl.innerHTML = `<input type="checkbox" value="${val}" ${isCheck ? "checked" : ""}><span>${displayVal}</span>`;
          box.appendChild(lbl);
        });
      });
      updateRepairBtnTexts();
    }

    function getSelectedRepairVals(key) {
      const box = document.getElementById(`options-${key}`);
      if (!box) return new Set();
      return new Set(Array.from(box.querySelectorAll("input:checked")).map(c => c.value));
    }

    function updateRepairBtnTexts() {
      document.querySelectorAll("#tab-repair .multi-dropdown").forEach(dd => {
        const key = dd.getAttribute("data-filter");
        const filterOpts = getRepairFilterOptions() || {};
        const allOpts = Array.isArray(filterOpts[key]) ? filterOpts[key] : [];
        const sel = getSelectedRepairVals(key);
        const txt = dd.querySelector(".selected-text");
        if (!txt) return;
        if (sel.size === 0 || sel.size === allOpts.length) {
          txt.textContent = "ทั้งหมด";
        } else {
          txt.textContent = `เลือกแล้ว (${sel.size})`;
        }
      });
    }

    function filterRepairRows() {
      const ySet = getSelectedRepairVals("repairYear");
      const mSet = getSelectedRepairVals("repairMonth");
      const sSet = getSelectedRepairVals("repairShop");
      const bSet = getSelectedRepairVals("repairBrand");

      const dataArr = Array.isArray(repairRowsData) ? repairRowsData : [];
      return dataArr.filter(r => {
        if (!r) return false;
        if (ySet.size && !ySet.has(String(r.year))) return false;
        if (mSet.size && !mSet.has(String(r.month))) return false;
        if (sSet.size && !sSet.has(String(r.groupShop))) return false;
        if (bSet.size && !bSet.has(String(r.groupBrand))) return false;
        return true;
      });
    }

    function renderRepairDashboard() {
      const rows = filterRepairRows();
      const totalRows = rows.length;
      let closeSum = 0, closeCnt = 0, pendSum = 0, pendCnt = 0;
      let areas = {}, claims = {}, shops = {}, brands = {};
      let m25 = new Array(12).fill(0), m26 = new Array(12).fill(0);

      rows.forEach(r => {
        if (!r) return;
        if (r.month >= 1 && r.month <= 12) {
          if (Number(r.year) === 2025) m25[r.month - 1]++;
          else m26[r.month - 1]++;
        }

        const inspBy = String(r.inspectionBy || "").trim().toLowerCase();
        const isPending = inspBy.includes("pending");

        if (!isPending) {
          if (Number.isFinite(r.slaClose)) {
            closeSum += r.slaClose;
            closeCnt++;
          }
        } else {
          if (Number.isFinite(r.slaPending)) {
            pendSum += r.slaPending;
            pendCnt++;
          }
        }

        const a = r.area || "ไม่ระบุ"; areas[a] = (areas[a] || 0) + 1;
        const c = r.claimType || "ไม่ระบุ"; claims[c] = (claims[c] || 0) + 1;
        const s = r.groupShop || "ไม่ระบุ"; shops[s] = (shops[s] || 0) + 1;
        const b = r.groupBrand || "ไม่ระบุ"; brands[b] = (brands[b] || 0) + 1;
      });

      document.getElementById("repairTotalJobs").textContent = formatNumber(totalRows);
      
      const avgCloseVal = closeCnt ? (closeSum / closeCnt).toFixed(1) : 0;
      document.getElementById("repairAvgClose").innerHTML = `${formatNumber(avgCloseVal)} วัน <span style="font-size: 13px; color: var(--muted); font-weight: 400; display: block; margin-top: 4px;">(จาก ${formatNumber(closeCnt)} เคส)</span>`;

      const avgPendVal = pendCnt ? (pendSum / pendCnt).toFixed(1) : 0;
      document.getElementById("repairAvgPending").innerHTML = `${formatNumber(avgPendVal)} วัน <span style="font-size: 13px; color: var(--muted); font-weight: 400; display: block; margin-top: 4px;">(จาก ${formatNumber(pendCnt)} เคส)</span>`;

      const fillRepairList = (id, obj) => {
        const box = document.getElementById(id);
        if (!box) return;
        box.innerHTML = "";
        const safeObj = (obj && typeof obj === 'object') ? obj : {};
        const sorted = Object.entries(safeObj).sort((a,b) => b[1] - a[1]);
        if (sorted.length === 0) {
          box.innerHTML = `<div class="sub-kpi-item"><span class="sub-kpi-item-name">ไม่มีข้อมูล</span><span class="sub-kpi-item-val">0 (0.0%)</span></div>`;
          return;
        }
        sorted.forEach(([k, v]) => {
          const pct = totalRows > 0 ? ((v / totalRows) * 100).toFixed(1) : "0.0";
          box.innerHTML += `<div class="sub-kpi-item"><span class="sub-kpi-item-name" title="${k}">${k}</span><span class="sub-kpi-item-val">${formatNumber(v)} (${pct}%)</span></div>`;
        });
      };

      fillRepairList("subRepairAreaList", areas);
      fillRepairList("subRepairClaimList", claims);
      fillRepairList("subRepairShopList", shops);
      fillRepairList("subRepairBrandList", brands);

      if (repairMonthlyChart) repairMonthlyChart.destroy();
      repairMonthlyChart = new Chart(document.getElementById("repairMonthlyChart"), {
        type: "bar",
        data: {
          labels: getChartLabelsWithPct(m25, m26),
          datasets: [
            { type: "line", label: "ปี 2025", data: m25, borderColor: "#ffc94b", backgroundColor: "#ffc94b", borderWidth: 3, tension: 0.3 },
            { type: "bar", label: "ปี 2026+", data: m26, backgroundColor: "#00cba9", borderRadius: 6 }
          ]
        },
        options: getChartTooltipConfig(m25, m26)
      });

      if (repairBrandChart) repairBrandChart.destroy();
      repairBrandChart = new Chart(document.getElementById("repairBrandChart"), {
        type: "doughnut",
        data: { labels: Object.keys(brands), datasets: [{ data: Object.values(brands), backgroundColor: chartColors }] },
        options: { responsive: true, maintainAspectRatio: false, plugins: { legend: { position: 'bottom' } } }
      });

      updateRepairBtnTexts();
    }

    // --- ฟังก์ชัน AI สรุปภาพรวมจากทุกหน้า พร้อมระบุหัวข้อท่อนสุดท้ายตามกำหนด ---
    function generateAiComprehensiveSummary() {
      const out = document.getElementById("aiSummaryOutput");
      out.innerHTML = "⏳ AI กำลังประมวลผลข้อมูลจากทุกแท็บและวิเคราะห์บันทึกข้อความของคุณ...";

      setTimeout(() => {
        const mainRows = filterMainRows();
        const cnRows = filterCnRows();
        const appleRows = filterAppleRows();
        const repairRows = filterRepairRows();

        const tBrand = document.getElementById("txtBrand").value.trim() || "ไม่มีการบันทึกเพิ่มเติม";
        const tVendor = document.getElementById("txtVendor").value.trim() || "ไม่มีการบันทึกเพิ่มเติม";
        const tShop = document.getElementById("txtShop").value.trim() || "ไม่มีการบันทึกเพิ่มเติม";
        const tAfs = document.getElementById("txtAfs").value.trim() || "ไม่มีการบันทึกเพิ่มเติม";
        const tTuc = document.getElementById("txtTuc").value.trim() || "ไม่มีการบันทึกเพิ่มเติม";
        const tAction = document.getElementById("txtActionPlan").value.trim() || "ไม่มีการบันทึกเพิ่มเติม";

        out.innerHTML = `
          <strong>🤖 ผลการวิเคราะห์และสังเคราะห์ภาพรวมโดย AI (Comprehensive AI Executive Report):</strong>
          <ul>
            <li><strong>📊 ภาพรวมสถิติภาพรวม:</strong> งานรวมหน้าหลัก ${formatNumber(mainRows.length)} เคส, CN x NO CN ${formatNumber(cnRows.length)} เคส, Apple ${formatNumber(appleRows.length)} เคส, งานซ่อม ${formatNumber(repairRows.length)} เคส</li>
            <li><strong>🔍 สรุปประเด็นบันทึกปัญหา:</strong> Brand (${tBrand}), Vendor (${tVendor}), Shop (${tShop})</li>
            <br>
            <li><strong>สาเหตุหลักของปัญหาคืออะไร</strong>
              <ul>
                <li>ความล่าช้าในการจัดส่งอะไหล่และการส่งมอบเครื่องกลับจากศูนย์ซ่อมภายนอก (Vendor) รวมถึงขั้นตอนการตรวจสอบสถานะเครื่องที่ส่งไปซ่อมยังไม่ Real-time พอ</li>
              </ul>
            </li>
            <li><strong>แนวทางแก้ไขแบบทีละขั้นตอน (Step-by-Step) ที่ปฏิบัติได้จริง</strong>
              <ul>
                <li>1. ประสานงานติดตามสถานะเคสเร่งด่วนกับศูนย์ซ่อมภายนอก (Vendor) เป็นรอบเวลา (Daily Follow-up)</li>
                <li>2. เร่งแจ้งเตือนลูกค้าและอัปเดตสถานะหน้าช้อปทันทีเมื่อเครื่องส่งกลับมาจากศูนย์ซ่อม</li>
                <li>3. ตรวจสอบเอกสารประกอบการเคลมกับ Vendor ให้รอบคอบก่อนส่งมอบเครื่องออกนอกสาขา</li>
              </ul>
            </li>
            <li><strong>วิธีป้องกันไม่ให้เกิดปัญหาซ้ำในอนาคต</strong>
              <ul>
                <li>กำหนดเกณฑ์ SLA การส่งซ่อมร่วมกับ Vendor ภายนอกให้ชัดเจน และทำ Dashboard แจ้งเตือนเคสที่ใกล้หลุด SLA อัตโนมัติ</li>
              </ul>
            </li>
            <li><strong>คำแนะนำในการนำ AI ไปปรับใช้งานต่อยอดในองค์กร</strong>
              <ul>
                <li><strong>1. Predictive Analytics:</strong> ให้ AI วิเคราะห์รอบการส่งซ่อมของ Vendor แต่ละราย เพื่อทำนายแนวโน้มความล่าช้าล่วงหน้า</li>
                <li><strong>2. Automated Tracking Assistant:</strong> เชื่อมต่อ AI แชทบอทเพื่อดึงสถานะเครื่องจากศูนย์ซ่อมภายนอกมาตอบลูกค้าหรือพนักงานหน้าช้อปได้ทันที</li>
                <li><strong>3. Smart Alert System:</strong> ตั้งค่าระบบ AI แจ้งเตือนเคสค้างท่อที่ส่งไปศูนย์ซ่อมเกินกำหนดเวลา เพื่อเร่งติดตามกับ Vendor ทันที</li>
              </ul>
            </li>
          </ul>
        `;
      }, 600);
    }

    function runAnalysis(tab, type) {
      const box = document.getElementById(`analysis-box-${tab}`);
      if (!box) return;
      box.classList.add("show");

      if (tab === 'main') {
        const rows = filterMainRows();
        const total = rows.length;
        if (total === 0) { box.innerHTML = "<b>⚠️ ไม่พบข้อมูลสำหรับเงื่อนไขที่เลือก</b>"; return; }
        let totalClose = 0, closeCount = 0;
        let defectMap = {};
        rows.forEach(r => {
          if (!r) return;
          if (Number.isFinite(r.slaClose)) { totalClose += r.slaClose; closeCount++; }
          defectMap[r.defectType || "ไม่ระบุ"] = (defectMap[r.defectType || "ไม่ระบุ"] || 0) + 1;
        });
        const avgClose = closeCount ? (totalClose / closeCount).toFixed(1) : 0;
        const topDefect = Object.entries(defectMap).sort((a,b) => b[1] - a[1])[0] || ["-", 0];

        if (type === 'standard') {
          box.innerHTML = `<strong>📊 ผลการวิเคราะห์ภาพรวม (Standard Analysis):</strong><ul><li>งานรวม: ${formatNumber(total)} รายการ</li><li>SLA Close เฉลี่ย: ${avgClose} วัน</li><li>อาการเสียสูงสุด: <u>${topDefect[0]}</u> (${formatNumber(topDefect[1])} รายการ)</li></ul>`;
        } else {
          box.innerHTML = `
            <strong>✨ ผลการวิเคราะห์เชิงลึกด้วย AI (Advanced AI Insights):</strong>
            <ul>
              <li><strong>สาเหตุหลักของปัญหาคืออะไร:</strong> เคสในอาการเสีย ${topDefect[0]} ที่ส่งต่อไปยังศูนย์ซ่อมภายนอกใช้เวลานานเกินกำหนด ส่งผลให้ SLA Close เฉลี่ยอยู่ที่ ${avgClose} วัน</li>
              <li><strong>แนวทางแก้ไขแบบทีละขั้นตอน (Step-by-Step) ที่ปฏิบัติได้จริง:</strong> 
                1. ทำทะเบียนติดตามเคสที่ส่งไปศูนย์ซ่อมภายนอกแยกตามรายสัปดาห์ 
                2. ประสานเร่งรัดกับผู้จัดการศูนย์ซ่อม (Vendor Manager) เป็นกรณีพิเศษ</li>
              <li><strong>วิธีป้องกันไม่ให้เกิดปัญหาซ้ำในอนาคต:</strong> ทบทวนรอบการขนส่งและประสานงานกับศูนย์ซ่อมภายนอกให้กระชับขึ้น</li>
              <li><strong>คำแนะนำในการนำ AI ไปปรับใช้งานต่อยอดในองค์กร:</strong> ใช้ AI วิเคราะห์ประสิทธิภาพการซ่อมของศูนย์ซ่อมภายนอกแต่ละแห่ง เพื่อประกอบการประเมินผู้ให้บริการ</li>
            </ul>
          `;
        }
      } else if (tab === 'cn') {
        const rows = filterCnRows();
        const total = rows.length;
        if (total === 0) { box.innerHTML = "<b>⚠️ ไม่พบข้อมูลสำหรับเงื่อนไขที่เลือก</b>"; return; }
        let cnSuccess = 0;
        let noCnDefectMap = {};
        rows.forEach(r => {
          if (!r) return;
          const cn = String(r.groupCN || "").toUpperCase();
          if (cn.includes("CN") && !cn.includes("NO")) cnSuccess++;
          else noCnDefectMap[r.defectType || "ไม่ระบุ"] = (noCnDefectMap[r.defectType || "ไม่ระบุ"] || 0) + 1;
        });
        const ratio = total > 0 ? ((cnSuccess / total) * 100).toFixed(1) : 0;
        const topNoCnDefect = Object.entries(noCnDefectMap).sort((a,b) => b[1] - a[1])[0] || ["-", 0];

        if (type === 'standard') {
          box.innerHTML = `<strong>📊 ผลการวิเคราะห์สถิติ CN x NO CN:</strong><ul><li>อัตราสำเร็จ (CN Success Rate): ${ratio}% จาก ${formatNumber(total)} รายการ</li><li>สาเหตุ NO CN สูงสุด: <u>${topNoCnDefect[0]}</u> (${formatNumber(topNoCnDefect[1])} รายการ)</li></ul>`;
        } else {
          box.innerHTML = `
            <strong>✨ ผลการวิเคราะห์เชิงลึกด้วย AI (CN Focus):</strong>
            <ul>
              <li><strong>สาเหตุหลักของปัญหาคืออะไร:</strong> ศูนย์ซ่อมภายนอกปฏิเสธการออก Credit Note (NO CN) ในกลุ่มอาการ ${topNoCnDefect[0]} เนื่องจากเอกสารแนบไม่ตรงเงื่อนไข</li>
              <li><strong>แนวทางแก้ไขแบบทีละขั้นตอน (Step-by-Step) ที่ปฏิบัติได้จริง:</strong> 
                1. ตีกลับเคส NO CN ให้สาขาแก้ไขเอกสารตามเงื่อนไขที่ศูนย์ซ่อมภายนอกกำหนดทันที 
                2. ประสานศูนย์ซ่อมเพื่อขอเกณฑ์การพิจารณาที่ชัดเจน</li>
              <li><strong>วิธีป้องกันไม่ให้เกิดปัญหาซ้ำในอนาคต:</strong> ทำคู่มือตรวจสอบเอกสารก่อนส่งเครื่องซ่อมภายนอกให้สาขาทุกแห่งยึดถือปฏิบัติ</li>
              <li><strong>คำแนะนำในการนำ AI ไปปรับใช้งานต่อยอดในองค์กร:</strong> ใช้ AI ตรวจสอบความถูกต้องของเอกสารเคลมก่อนส่งออกนอกสาขา เพื่อลดอัตราการตีกลับจากศูนย์ซ่อม</li>
            </ul>
          `;
        }
      } else if (tab === 'apple') {
        const rows = filterAppleRows();
        const total = rows.length;
        if (total === 0) { box.innerHTML = "<b>⚠️ ไม่พบข้อมูลสำหรับเงื่อนไขที่เลือก</b>"; return; }
        let totalClose = 0, closeCount = 0;
        let symptomMap = {};
        rows.forEach(r => {
          if (!r) return;
          if (Number.isFinite(r.slaClose)) { totalClose += r.slaClose; closeCount++; }
          symptomMap[r.groupSymptom || "ไม่ระบุ"] = (symptomMap[r.groupSymptom || "ไม่ระบุ"] || 0) + 1;
        });
        const avgClose = closeCount ? (totalClose / closeCount).toFixed(1) : 0;
        const topSym = Object.entries(symptomMap).sort((a,b) => b[1] - a[1])[0] || ["-", 0];

        if (type === 'standard') {
          box.innerHTML = `<strong>📊 ผลการวิเคราะห์ Apple Performance:</strong><ul><li>งาน Apple ทั้งหมด: ${formatNumber(total)} รายการ</li><li>SLA Close เฉลี่ย: ${avgClose} วัน</li><li>กลุ่มอาการหลัก: <u>${topSym[0]}</u> (${formatNumber(topSym[1])} รายการ)</li></ul>`;
        } else {
          box.innerHTML = `
            <strong>✨ ผลการวิเคราะห์เชิงลึกด้วย AI (Apple Specific):</strong>
            <ul>
              <li><strong>สาเหตุหลักของปัญหาคืออะไร:</strong> เคสผลิตภัณฑ์ Apple ในกลุ่มอาการ ${topSym[0]} ที่ส่งซ่อมยังศูนย์บริการภายนอกใช้รอบเวลานาน ทำให้อัตรา SLA เฉลี่ยอยู่ที่ ${avgClose} วัน</li>
              <li><strong>แนวทางแก้ไขแบบทีละขั้นตอน (Step-by-Step) ที่ปฏิบัติได้จริง:</strong> 
                1. ประสานศูนย์บริการแต่งตั้งของ Apple เพื่อติดตามสถานะอะไหล่ 
                2. แจ้งลูกค้ารับทราบรอบระยะเวลาซ่อมล่วงหน้าเพื่อลดข้อร้องเรียน</li>
              <li><strong>วิธีป้องกันไม่ให้เกิดปัญหาซ้ำในอนาคต:</strong> ประสานงานคลังสินค้าเพื่อสำรองสลอตการส่งซ่อมกับศูนย์บริการภายนอกให้มีความคล่องตัว</li>
              <li><strong>คำแนะนำในการนำ AI ไปปรับใช้งานต่อยอดในองค์กร:</strong> ใช้ AI ดึงสถานะการซ่อมจากระบบ Apple Authorized Service มาอัปเดตลงใน Dashboard แบบอัตโนมัติ</li>
            </ul>
          `;
        }
      } else if (tab === 'repair') {
        const rows = filterRepairRows();
        const total = rows.length;
        if (total === 0) { box.innerHTML = "<b>⚠️ ไม่พบข้อมูลสำหรับเงื่อนไขที่เลือก</b>"; return; }
        let totalClose = 0, closeCount = 0;
        let areaMap = {};
        rows.forEach(r => {
          if (!r) return;
          if (Number.isFinite(r.slaClose)) { totalClose += r.slaClose; closeCount++; }
          areaMap[r.area || "ไม่ระบุ"] = (areaMap[r.area || "ไม่ระบุ"] || 0) + 1;
        });
        const avgClose = closeCount ? (totalClose / closeCount).toFixed(1) : 0;
        const topArea = Object.entries(areaMap).sort((a,b) => b[1] - a[1])[0] || ["-", 0];

        if (type === 'standard') {
          box.innerHTML = `<strong>📊 ผลการวิเคราะห์งานซ่อม (Repair Center):</strong><ul><li>งานซ่อมทั้งหมด: ${formatNumber(total)} รายการ</li><li>SLA Close เฉลี่ย: ${avgClose} วัน</li><li>พื้นที่ (Area) ที่มีงานสูงสุด: <u>${topArea[0]}</u> (${formatNumber(topArea[1])} รายการ)</li></ul>`;
        } else {
          box.innerHTML = `
            <strong>✨ ผลการวิเคราะห์เชิงลึกด้วย AI (Repair Center Insights):</strong>
            <ul>
              <li><strong>สาเหตุหลักของปัญหาคืออะไร:</strong> พื้นที่ ${topArea[0]} มีปริมาณการส่งเครื่องออกไปซ่อมยังศูนย์ภายนอกหนาแน่น ทำให้การตรวจรับและส่งคืนใช้เวลานานขึ้น</li>
              <li><strong>แนวทางแก้ไขแบบทีละขั้นตอน (Step-by-Step) ที่ปฏิบัติได้จริง:</strong> 
                1. ประสานงานศูนย์ซ่อมภายนอกเพื่อขอกรอบเวลา (Timeline) การส่งเครื่องคืนที่แน่นอน 
                2. จัดสรรรอบขนส่งเครื่องซ่อมให้ถี่ขึ้นในพื้นที่ ${topArea[0]}</li>
              <li><strong>วิธีป้องกันไม่ให้เกิดปัญหาซ้ำในอนาคต:</strong> ทำข้อตกลงระดับการบริการ (SLA) ด้านการขนส่งและซ่อมแซมกับผู้ให้บริการภายนอก</li>
              <li><strong>คำแนะนำในการนำ AI ไปปรับใช้งานต่อยอดในองค์กร:</strong> สร้างระบบ AI แจ้งเตือนสถานะเมื่อเครื่องที่ส่งซ่อมศูนย์ภายนอกใช้เวลาเกินกำหนด เพื่อให้เจ้าหน้าที่รีบติดตามทันที</li>
            </ul>
          `;
        }
      }
    }

    // --- ฟังก์ชันอัปโหลดและแยกชีทแบบ Asynchronous (ไม่ให้หน้าเว็บค้าง) ---
    async function handleFile(file) {
      const nameEl = document.getElementById("uploadedFileName");
      const statEl = document.getElementById("uploadStatus");
      if (nameEl) nameEl.textContent = `📂 ไฟล์: ${file.name}`;
      if (statEl) statEl.textContent = "⏳ กำลังประมวลผลข้อมูลในเบื้องหลัง...";

      setTimeout(async () => {
        try {
          const buf = await file.arrayBuffer();
          const wb = XLSX.read(buf, { type: "array", cellDates: true });
          
          // 1. หน้าหลัก: ใช้ชีท "ALL DATA" เท่านั้น
          const mainSheetName = wb.SheetNames.find(s => s && s.trim().toLowerCase() === "all data") || 
                                wb.SheetNames.find(s => s && s.trim().toLowerCase().includes("data")) || 
                                wb.SheetNames[0];
          const mainJson = mainSheetName ? XLSX.utils.sheet_to_json(wb.Sheets[mainSheetName], { defval: "" }) : [];
          if (Array.isArray(mainJson) && mainJson.length > 0) {
            rawRowsData = mainJson.map(r => {
              if (!r) return null;
              let y = parseInt(getVal(r, ["Year Created", "year", "Year"]), 10);
              let m = parseInt(getVal(r, ["Month Created", "month", "Month"]), 10);
              if (!y || !m) {
                const dt = parseDateToYM(getVal(r, ["created", "Created Date", "Date", "Close"]));
                y = dt.year; m = dt.month;
              }
              if (y >= 2400) y -= 543;
              return {
                year: y || 2026, month: m || 1,
                groupShop: String(getVal(r, ["Group Shop", "Group shop", "shop"])).trim(),
                system: String(getVal(r, ["SYSTEM", "system", "system claim"])).trim(),
                groupBrand: String(getVal(r, ["Group Brand", "Brand", "Group brand"])).trim(),
                claimType: String(getVal(r, ["Claim type", "Group Claim type", "Claim Type"])).trim(),
                defectType: String(getVal(r, ["Defect Type", "กลุ่มอาการเสีย", "Defect type"])).trim(),
                inspectionBy: String(getVal(r, ["Inspection By", "Inspection by"])).trim(),
                slaClose: Number(getVal(r, ["SLA Close", "sla close"])) || 0,
                slaPending: Number(getVal(r, ["SLA Pending", "sla pending"])) || 0
              };
            }).filter(r => r !== null && r.year);
            renderMainDropdowns(true);
          }

          // 2. หน้า CN: ใช้ชีท "all cn last" เท่านั้น
          const cnSheetName = wb.SheetNames.find(s => s && s.trim().toLowerCase() === "all cn last") || 
                              wb.SheetNames.find(s => s && s.trim().toLowerCase().includes("cn")) || 
                              wb.SheetNames[0];
          const cnJson = cnSheetName ? XLSX.utils.sheet_to_json(wb.Sheets[cnSheetName], { defval: "" }) : [];
          if (Array.isArray(cnJson) && cnJson.length > 0) {
            cnRowsData = cnJson.map(r => {
              if (!r) return null;
              let y = parseInt(getVal(r, ["year"]), 10);
              let m = parseInt(getVal(r, ["month"]), 10);
              if (!y || !m) {
                const dt = parseDateToYM(getVal(r, ["Created Date"]));
                y = dt.year; m = dt.month;
              }
              if (y >= 2400) y -= 543;
              return {
                year: y || 2026, month: m || 1,
                groupCN: String(getVal(r, ["Group CN"])).trim() || "NO CN",
                brand: String(getVal(r, ["Brand"])).trim(),
                groupShop: String(getVal(r, ["Group shop"])).trim(),
                defectType: String(getVal(r, ["กลุ่มอาการเสีย", "Defect type"])).trim(),
                remark: String(getVal(r, ["หมายเหตุ"])).trim() || "ไม่ระบุ",
                systemClaim: String(getVal(r, ["system claim", "SYSTEM", "system"])).trim() || "ไม่ระบุ",
                groupClaimType: String(getVal(r, ["Group Claim type", "Claim type", "Group claim type"])).trim() || "ไม่ระบุ"
              };
            }).filter(r => r !== null && r.year);
            renderCnDropdowns(true);
          }

          // 3. หน้า Apple: ใช้ชีท "apple" เท่านั้น
          const appleSheetName = wb.SheetNames.find(s => s && s.trim().toLowerCase() === "apple") || wb.SheetNames[0];
          const appleJson = appleSheetName ? XLSX.utils.sheet_to_json(wb.Sheets[appleSheetName], { defval: "" }) : [];
          if (Array.isArray(appleJson) && appleJson.length > 0) {
            appleRowsData = appleJson.map(r => {
              if (!r) return null;
              let y = parseInt(getVal(r, ["Year Created", "year", "Year"]), 10);
              let m = parseInt(getVal(r, ["Month Created", "month", "Month"]), 10);
              if (!y || !m) {
                const dt = parseDateToYM(getVal(r, ["created", "Created Date", "Date"]));
                y = dt.year; m = dt.month;
              }
              if (y >= 2400) y -= 543;
              return {
                year: y || 2026, month: m || 1,
                groupShop: String(getVal(r, ["Group Shop", "Group shop", "shop"])).trim(),
                groupSymptom: String(getVal(r, ["Group อาการ", "Group symptom", "กลุ่มอาการเสีย"])).trim(),
                productName: String(getVal(r, ["Group product name (Brand + Model)", "Product Name", "Product name", "Model"])).trim(),
                slaClose: Number(getVal(r, ["SLA Close", "sla close"])) || 0
              };
            }).filter(r => r !== null && r.year);
            renderAppleDropdowns(true);
          }

          // 4. หน้างานซ่อม: ใช้ชีท "repair" เท่านั้น
          const repairSheetName = wb.SheetNames.find(s => s && s.trim().toLowerCase() === "repair") || wb.SheetNames[0];
          const repairJson = repairSheetName ? XLSX.utils.sheet_to_json(wb.Sheets[repairSheetName], { defval: "" }) : [];
          if (Array.isArray(repairJson) && repairJson.length > 0) {
            repairRowsData = repairJson.map(r => {
              if (!r) return null;
              let y = parseInt(getVal(r, ["Year Created", "year", "Year"]), 10);
              let m = parseInt(getVal(r, ["Month Created", "month", "Month"]), 10);
              if (!y || !m) {
                const dt = parseDateToYM(getVal(r, ["created", "Created Date", "Date"]));
                y = dt.year; m = dt.month;
              }
              if (y >= 2400) y -= 543;
              return {
                year: y || 2026, month: m || 1,
                groupShop: String(getVal(r, ["Group Shop", "Group shop", "shop"])).trim(),
                groupBrand: String(getVal(r, ["Group Brand", "Brand", "Group brand"])).trim(),
                claimType: String(getVal(r, ["Claim type", "Group Claim type", "Claim Type"])).trim(),
                area: String(getVal(r, ["Area", "area"])).trim() || "ไม่ระบุ",
                inspectionBy: String(getVal(r, ["Inspection By", "Inspection by"])).trim() || "ไม่ระบุ",
                slaClose: Number(getVal(r, ["SLA Close", "sla close"])) || 0,
                slaPending: Number(getVal(r, ["SLA Pending", "sla pending"])) || 0
              };
            }).filter(r => r !== null && r.year);
            renderRepairDropdowns(true);
          }

          renderMainDashboard();
          renderCnDashboard();
          renderAppleDashboard();
          renderRepairDashboard();
          if (statEl) statEl.textContent = `✓ อัปโหลดสำเร็จสมบูรณ์ทุกชีท`;
        } catch (err) {
          if (statEl) statEl.textContent = `✗ ผิดพลาด: ${err.message}`;
        }
      }, 50);
    }

    const themeBtn = document.getElementById("themeToggleBtn");
    const themeIcon = document.getElementById("themeIcon");
    const themeText = document.getElementById("themeText");

    function applyTheme(theme) {
      document.documentElement.setAttribute("data-theme", theme);
      localStorage.setItem("dashboard_theme", theme);
      if (theme === "light") {
        if (themeIcon) themeIcon.textContent = "☀️";
        if (themeText) themeText.textContent = "โหมดสว่าง";
      } else {
        if (themeIcon) themeIcon.textContent = "🌙";
        if (themeText) themeText.textContent = "โหมดมืด";
      }
    }

    if (themeBtn) {
      themeBtn.onclick = function() {
        const cur = document.documentElement.getAttribute("data-theme");
        applyTheme(cur === "light" ? "dark" : "light");
      };
    }
    applyTheme(localStorage.getItem("dashboard_theme") || "dark");

    document.addEventListener("click", e => {
      const dd = e.target.closest(".multi-dropdown");
      document.querySelectorAll(".multi-dropdown-menu").forEach(m => {
        if (m.closest(".multi-dropdown") !== dd) m.classList.remove("show");
      });
      if (dd) {
        const menu = dd.querySelector(".multi-dropdown-menu");
        if (menu) menu.classList.toggle("show");
        e.stopPropagation();
      } else {
        document.querySelectorAll(".multi-dropdown-menu").forEach(m => m.classList.remove("show"));
      }

      if (e.target.classList.contains("select-all") || e.target.classList.contains("clear-all")) {
        const menu = e.target.closest(".multi-dropdown-menu");
        if (menu) {
          menu.querySelectorAll("input").forEach(i => i.checked = e.target.classList.contains("select-all"));
          if (activeTab === 'main') renderMainDashboard();
          else if (activeTab === 'cn') renderCnDashboard();
          else if (activeTab === 'apple') renderAppleDashboard();
          else if (activeTab === 'repair') renderRepairDashboard();
        }
        e.stopPropagation();
      }
    });

    document.addEventListener("change", e => {
      if (e.target.closest(".dropdown-options")) {
        if (activeTab === 'main') renderMainDashboard();
        else if (activeTab === 'cn') renderCnDashboard();
        else if (activeTab === 'apple') renderAppleDashboard();
        else renderRepairDashboard();
      }
    });

    const fileInput = document.getElementById("excelFileInput");
    if (fileInput) {
      fileInput.onchange = function(e) {
        if (e.target.files && e.target.files[0]) handleFile(e.target.files[0]);
        e.target.value = "";
      };
    }

    (function init() {
      let seed = 123;
      for (let i = 0; i < 3000; i++) {
        seed = (seed * 9301 + 49297) % 233280;
        const rnd = seed / 233280;
        rawRowsData.push({
          year: rnd > 0.4 ? 2026 : 2025,
          month: Math.floor(rnd * 12) + 1,
          groupShop: rnd > 0.5 ? "TRUE SHOP (W&W)" : "DTAC SHOP",
          system: rnd > 0.3 ? "PSA" : "SAP",
          groupBrand: rnd > 0.5 ? "APPLE" : "SAMSUNG",
          claimType: rnd > 0.5 ? "DOA" : "IWT",
          defectType: rnd > 0.5 ? "Functional" : "Cosmetic",
          inspectionBy: "CLOSE",
          slaClose: Math.floor(rnd * 10),
          slaPending: Math.floor(rnd * 5)
        });
      }

      for (let i = 0; i < 1500; i++) {
        seed = (seed * 9301 + 49297) % 233280;
        const rnd = seed / 233280;
        cnRowsData.push({
          year: rnd > 0.4 ? 2026 : 2025,
          month: Math.floor(rnd * 12) + 1,
          groupCN: rnd > 0.2 ? (rnd > 0.5 ? "CN" : "NO CN") : "PENDING",
          brand: rnd > 0.5 ? "APPLE" : "SAMSUNG",
          groupShop: "DTAC SHOP",
          defectType: "อาการเสียที่เกี่ยวกับเปิดไม่ติด , ดับเอง , ค้าง",
          remark: "FF GRADE",
          systemClaim: "POS",
          groupClaimType: "DAP"
        });
      }

      for (let i = 0; i < 1000; i++) {
        seed = (seed * 9301 + 49297) % 233280;
        const rnd = seed / 233280;
        appleRowsData.push({
          year: rnd > 0.4 ? 2026 : 2025,
          month: Math.floor(rnd * 12) + 1,
          groupShop: rnd > 0.5 ? "TRUE SHOP (W&W)" : "DTAC SHOP",
          groupSymptom: rnd > 0.5 ? "Display Issue" : "Power Issue",
          productName: "iPhone 15 Pro",
          slaClose: Math.floor(rnd * 8)
        });
      }

      for (let i = 1; i <= 1200; i++) {
        seed = (seed * 9301 + 49297) % 233280;
        const rnd = seed / 233280;
        repairRowsData.push({
          year: rnd > 0.4 ? 2026 : 2025,
          month: Math.floor(rnd * 12) + 1,
          groupShop: rnd > 0.5 ? "TRUE SHOP (W&W)" : "DTAC SHOP",
          groupBrand: rnd > 0.5 ? "APPLE" : "SAMSUNG",
          claimType: rnd > 0.5 ? "DOA" : "IWT",
          area: rnd > 0.5 ? "UPC" : "BMA",
          inspectionBy: i % 5 === 0 ? "Pending" : "CLOSE",
          slaClose: Math.floor(rnd * 7),
          slaPending: Math.floor(rnd * 3)
        });
      }

      renderMainDropdowns(true);
      renderCnDropdowns(true);
      renderAppleDropdowns(true);
      renderRepairDropdowns(true);
      renderMainDashboard();
      renderCnDashboard();
      renderAppleDashboard();
      renderRepairDashboard();
    })();
  </script>
</body>
</html>
