<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Dklinity - Clinical Research Database</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>
<style>
* { box-sizing: border-box; }
body { font-family: Segoe UI, Roboto, Arial, sans-serif; background: linear-gradient(135deg, #eff6ff 0%, #f0f9ff 100%); margin: 0; padding: 16px; min-height: 100vh; }
.container { max-width: 1400px; margin: 0 auto; background: #fff; border-radius: 14px; box-shadow: 0 8px 32px rgba(30, 58, 138, 0.12); overflow: hidden; }
.header { background: linear-gradient(135deg, #1e3a8a 0%, #2563eb 100%); color: #fff; padding: 16px 20px; text-align: center; box-shadow: 0 4px 20px rgba(30, 58, 138, 0.3); }
.header-content { display: flex; align-items: center; justify-content: center; gap: 12px; flex-wrap: wrap; }
.header-logo { display: none; }
.header-text h1 { margin: 0; font-size: 32px; font-weight: 800; letter-spacing: -0.8px; }
.header-text p { margin: 4px 0 0 0; font-size: 11px; opacity: 0.95; max-width: 600px; line-height: 1.3; }
.nav-tabs { display: flex; gap: 2px; padding: 0; background: #f8fafc; border-bottom: 3px solid #e6eef8; overflow-x: auto; }
.nav-tab { padding: 16px 20px; border: none; background: transparent; cursor: pointer; font-size: 14px; color: #64748b; border-bottom: 4px solid transparent; transition: all 0.2s; font-weight: 500; }
.nav-tab:hover { background: #f1f5f9; }
.nav-tab.active { color: #1e3a8a; border-bottom-color: #1e3a8a; background: #f1f5f9; font-weight: 600; }
.tab-content { display: none; padding: 24px; }
.tab-content.active { display: block; }
.toolbar { display: flex; justify-content: space-between; align-items: center; margin-bottom: 18px; flex-wrap: wrap; gap: 12px; }
.search-box { padding: 10px 14px; border: 2px solid #bfdbfe; border-radius: 8px; width: 240px; font-size: 14px; background: white; transition: all 0.2s; }
.search-box:focus { outline: none; border-color: #1e3a8a; box-shadow: 0 0 0 3px rgba(30, 58, 138, 0.1); }
.table-wrapper { overflow: auto; max-height: 520px; border: 1px solid #e6eef8; border-radius: 8px; }
table { width: 100%; border-collapse: collapse; }
thead { background: #f0f7ff; border-top: 2px solid #1e3a8a; }
th { position: sticky; top: 0; z-index: 2; padding: 16px 14px; text-align: left; font-weight: 700; color: #1e3a8a; border-bottom: 3px solid #1e3a8a; font-size: 13px; text-transform: uppercase; letter-spacing: 0.4px; background: #f0f7ff; }
td { padding: 14px 14px; border-bottom: 1px solid #e6eef8; font-size: 14px; color: #334155; }
tbody tr:hover { background: #f0f5fa; }
.btn { padding: 10px 18px; border: none; border-radius: 6px; cursor: pointer; font-size: 14px; font-weight: 600; transition: all 0.2s; }
.btn-primary { background: #1e3a8a; color: #fff; }
.btn-primary:hover { background: #1e40af; transform: translateY(-2px); box-shadow: 0 4px 12px rgba(30, 58, 138, 0.4); }
.btn-success { background: #10b981; color: #fff; }
.btn-success:hover { background: #059669; }
.btn-danger { background: #ef4444; color: #fff; }
.btn-danger:hover { background: #dc2626; }
.btn-secondary { background: #f1f5f9; color: #475569; }
.btn-secondary:hover { background: #e2e8f0; }
.btn-sm { padding: 8px 12px; font-size: 13px; }
.modal { display: none; position: fixed; inset: 0; background: rgba(0, 0, 0, 0.5); z-index: 1000; overflow-y: auto; }
.modal.show { display: flex; align-items: center; justify-content: center; }
.modal-content { background: #fff; border-radius: 0; padding: 0; width: 100%; max-width: 100vw; max-height: 100vh; overflow-y: auto; box-shadow: none; }
.modal-header { font-size: 20px; font-weight: 700; margin-bottom: 18px; color: #1e3a8a; }
.modal-header-bar { display: flex; align-items: center; gap: 16px; background: linear-gradient(135deg, #1e3a8a 0%, #2563eb 100%); color: white; padding: 20px 24px; margin: 0 0 20px 0; flex-wrap: wrap; }
.modal-header-bar button { background: rgba(255,255,255,0.2); border: 1px solid rgba(255,255,255,0.4); color: white; padding: 10px 16px; border-radius: 6px; cursor: pointer; font-size: 14px; font-weight: 600; transition: all 0.2s; }
.modal-header-bar button:hover { background: rgba(255,255,255,0.3); }
.modal-header-bar h2 { margin: 0; font-size: 24px; color: white; flex: 1; }
.modal-inner { padding: 24px; }
.form-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 14px; margin-bottom: 18px; }
.form-group { display: flex; flex-direction: column; }
.form-group label { font-weight: 600; margin-bottom: 8px; color: #1e293b; font-size: 14px; }
.form-group input, .form-group textarea, .form-group select { padding: 10px; border: 1px solid #d0d9e8; border-radius: 6px; font-size: 14px; font-family: inherit; }
.form-group textarea { resize: vertical; min-height: 80px; }
.form-actions { display: flex; gap: 8px; justify-content: flex-end; margin-top: 20px; }
.badge { display: inline-block; padding: 5px 12px; border-radius: 14px; font-size: 13px; font-weight: 600; background: #dbeafe; color: #082f49; }
.badge-success { background: #dcfce7; color: #166534; }
.badge-warning { background: #fef3c7; color: #92400e; }
.alert { padding: 12px; border-radius: 6px; margin-bottom: 12px; font-size: 13px; }
.alert-success { background: #d1fae5; color: #065f46; border: 1px solid #a7f3d0; }
.alert-error { background: #fee2e2; color: #991b1b; border: 1px solid #fecaca; }
.alert-info { background: #dbeafe; color: #082f49; border: 1px solid #93c5fd; }
.stats-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(140px, 1fr)); gap: 12px; margin-bottom: 18px; }
.stat-card { padding: 18px; background: linear-gradient(135deg, #eff6ff 0%, #f0f4ff 100%); border-radius: 12px; border: 2px solid #bfdbfe; box-shadow: 0 2px 8px rgba(30, 58, 138, 0.08); transition: all 0.3s; cursor: default; }
.stat-card:hover { transform: translateY(-3px); box-shadow: 0 6px 16px rgba(30, 58, 138, 0.12); border-color: #93c5fd; }
.stat-value { font-size: 26px; font-weight: 800; color: #1e3a8a; line-height: 1; }
.stat-label { font-size: 12px; color: #475569; margin-top: 8px; font-weight: 600; text-transform: uppercase; letter-spacing: 0.3px; }
.cohort-section { background: #f8fafc; padding: 12px; border-radius: 6px; margin-bottom: 12px; }
.cohort-item { background: #fff; padding: 10px; border: 1px solid #e6eef8; border-radius: 6px; margin-bottom: 8px; display: grid; grid-template-columns: 1fr auto; align-items: center; gap: 8px; }
.mapping-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; max-height: 400px; overflow-y: auto; border: 1px solid #e6eef8; padding: 12px; border-radius: 6px; }
.mapping-row { display: contents; }
.mapping-label { font-weight: 600; padding: 8px; color: #1e293b; }
.mapping-select { padding: 8px; border: 1px solid #e6eef8; border-radius: 6px; }
.sub-tabs { display: flex; gap: 4px; margin: 16px 0 12px 0; border-bottom: 2px solid #e6eef8; }
.sub-tab { padding: 10px 14px; border: none; background: transparent; cursor: pointer; font-size: 13px; color: #64748b; border-bottom: 3px solid transparent; transition: all 0.2s; }
.sub-tab:hover { background: #f1f5f9; }
.sub-tab.active { color: #667eea; border-bottom-color: #667eea; }
.tab-pane { display: none; }
.tab-pane.active { display: block; }
.multi-value-container { display: flex; flex-wrap: wrap; gap: 6px; margin-bottom: 8px; min-height: 28px; }
.multi-value-tag { display: inline-flex; align-items: center; gap: 6px; background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); color: white; padding: 5px 10px; border-radius: 16px; font-size: 12px; font-weight: 500; }
.multi-value-tag button { background: rgba(255,255,255,0.25); border: none; color: white; cursor: pointer; padding: 2px 5px; border-radius: 3px; font-size: 10px; }
.cohort-list { background: #f8fafc; padding: 12px; border-radius: 6px; margin: 12px 0; max-height: 300px; overflow-y: auto; }
.cohort-preview { background: #fff; border-left: 4px solid #10b981; padding: 10px; margin: 8px 0; border-radius: 4px; }
.cohort-preview.split { border-left-color: #f59e0b; }
.pagination { display: flex; gap: 8px; justify-content: center; align-items: center; margin-top: 16px; margin-bottom: 16px; }
.pagination button { padding: 8px 12px; border: 1px solid #e6eef8; background: #f8fafc; cursor: pointer; border-radius: 6px; font-size: 13px; }
.pagination button:hover { background: #e2e8f0; }
.pagination button.active { background: #1e3a8a; color: white; border-color: #1e3a8a; }
.pagination button:disabled { opacity: 0.5; cursor: not-allowed; }
.pagination span { color: #64748b; font-size: 13px; }
.details-section { border: 1px solid #e6eef8; border-radius: 8px; margin-bottom: 10px; background: #f8fafc; }
.details-section summary { cursor: pointer; padding: 10px 12px; font-weight: 700; color: #1e3a8a; list-style: none; }
.details-section summary::-webkit-details-marker { display: none; }
.details-body { padding: 10px 12px 14px 12px; }
.diff-cell { background: #fff7ed; border-left: 3px solid #f59e0b; }
.value-note { display: inline-block; margin-left: 6px; font-size: 11px; font-weight: 700; color: #92400e; background: #fef3c7; padding: 2px 6px; border-radius: 10px; }
.modal-content.fullpage { border-radius: 0; }
.trial-layout { display: grid; grid-template-columns: 230px 1fr; gap: 16px; }
.trial-nav { border: 1px solid #e6eef8; border-radius: 10px; background: #f8fafc; padding: 12px; height: fit-content; position: sticky; top: 12px; }
.trial-nav-title { font-size: 12px; font-weight: 800; color: #0f172a; text-transform: uppercase; letter-spacing: 0.3px; margin-bottom: 8px; }
.trial-nav-item { width: 100%; text-align: left; padding: 8px 10px; border: none; background: transparent; cursor: pointer; font-size: 13px; color: #475569; border-radius: 8px; margin-bottom: 4px; }
.trial-nav-item:hover { background: #e2e8f0; }
.trial-nav-item.active { background: #dbeafe; color: #1e3a8a; font-weight: 700; }
.trial-content { border: 1px solid #e6eef8; border-radius: 10px; padding: 16px; background: #fff; max-height: calc(100vh - 160px); overflow-y: auto; }
.trial-section { margin-bottom: 18px; padding-bottom: 12px; border-bottom: 1px solid #e6eef8; }
.trial-section:last-child { border-bottom: none; margin-bottom: 0; padding-bottom: 0; }
.trial-section h3 { margin: 0 0 10px 0; font-size: 16px; color: #1e3a8a; }
.summary-grid { grid-template-columns: repeat(2, minmax(240px, 1fr)); }
.multi-entry-list { display: grid; grid-template-columns: repeat(2, minmax(220px, 1fr)); gap: 8px; }
.multi-entry-item { display: grid; grid-template-columns: 1fr auto auto; gap: 4px; align-items: center; padding: 6px; border: 1px solid #e6eef8; border-radius: 8px; background: #f8fafc; }
.multi-entry-item input { width: 100%; padding: 8px; border: 1px solid #d0d9e8; border-radius: 6px; font-size: 13px; font-family: inherit; }
.multi-entry-item .btn { padding: 4px 6px; font-size: 12px; line-height: 1; min-width: 22px; height: 22px; }
.details-layout { display: grid; grid-template-columns: 220px 1fr; gap: 16px; }
.details-nav { border: 1px solid #e6eef8; border-radius: 10px; background: #f8fafc; padding: 12px; height: fit-content; position: sticky; top: 12px; }
.details-nav-title { font-size: 12px; font-weight: 800; color: #0f172a; text-transform: uppercase; letter-spacing: 0.3px; margin-bottom: 8px; }
.details-nav-item { width: 100%; text-align: left; padding: 8px 10px; border: none; background: transparent; cursor: pointer; font-size: 13px; color: #475569; border-radius: 8px; margin-bottom: 4px; }
.details-nav-item:hover { background: #e2e8f0; }
.details-nav-item.active { background: #dbeafe; color: #1e3a8a; font-weight: 700; }
.details-page { display: none; }
.details-page.active { display: block; }
.details-row-selector { display: flex; gap: 6px; flex-wrap: wrap; max-height: 140px; overflow: auto; border: 1px solid #e6eef8; padding: 8px; border-radius: 8px; background: #f8fafc; margin-bottom: 12px; }
.details-row-btn { padding: 6px 10px; border: 1px solid #e6eef8; background: #fff; cursor: pointer; border-radius: 6px; font-size: 12px; font-weight: 600; color: #1e293b; }
.details-row-btn:hover { background: #e2e8f0; }
.details-row-btn.active { background: #1e3a8a; color: #fff; border-color: #1e3a8a; }

</style>
</head>
<body>

<div class="container">
  <div class="header">
    <div class="header-content">
      <div class="header-text">
        <div style="display:flex;align-items:center;justify-content:center;gap:12px;flex-wrap:wrap;margin-bottom:2px">
          <span style="font-size:24px">🧬</span>
          <h1>Dklinity</h1>
          <span style="font-size:24px">🎗️</span>
        </div>
        <p style="margin:2px 0 0 0;font-size:11px;color:rgba(255,255,255,0.9);font-weight:500">Clinical Research & Oncology Management Platform</p>
      </div>
    </div>
  </div>

  <div class="nav-tabs">
    <div class="nav-tab active" onclick="switchTab(event, 'dashboard')">📊 Dashboard</div>
    <div class="nav-tab" onclick="switchTab(event, 'drugs')">💊 Drugs</div>
    <div class="nav-tab" onclick="switchTab(event, 'trials')">🧪 Trials</div>
    <div class="nav-tab" onclick="switchTab(event, 'companies')">🏢 Companies</div>
    <div class="nav-tab" onclick="switchTab(event, 'import')">📤 Import/Export</div>
  </div>

  <!-- Dashboard Tab -->
  <div id="dashboard" class="tab-content active">
    <div style="display:flex;align-items:center;gap:16px;margin-bottom:32px;padding-bottom:24px;border-bottom:3px solid #bfdbfe">
      <div style="flex:1">
        <h2 style="margin:0;color:#1e3a8a;font-size:26px;font-weight:800">📊 Dashboard</h2>
        <p style="margin:8px 0 0 0;font-size:13px;color:#475569;font-weight:500">Real-time insights on drugs, trials, and clinical research pipeline</p>
      </div>
    </div>
    
    <!-- Dashboard Filters -->
    <div style="display:flex;gap:16px;margin-bottom:24px;align-items:center;flex-wrap:wrap;background:linear-gradient(135deg, #eff6ff 0%, #f0f4ff 100%);padding:16px;border-radius:10px;border:2px solid #bfdbfe">
      <div style="display:flex;gap:10px;align-items:center">
        <label style="font-weight:700;color:#1e293b;font-size:13px">Filter by Sponsor:</label>
        <select id="dashboardFilterSponsor" onchange="updateDashboardStats()" style="padding:10px;border:2px solid #bfdbfe;border-radius:6px;background:white;font-size:14px;font-weight:500;cursor:pointer">
          <option value="">All Companies</option>
        </select>
      </div>
      <div style="display:flex;gap:10px;align-items:center">
        <label style="font-weight:700;color:#1e293b;font-size:13px">Filter by Condition:</label>
        <select id="dashboardFilterCondition" onchange="updateDashboardStats()" style="padding:10px;border:2px solid #bfdbfe;border-radius:6px;background:white;font-size:14px;font-weight:500;cursor:pointer">
          <option value="">All Conditions</option>
        </select>
      </div>
      <button class="btn btn-secondary btn-sm" onclick="clearDashboardFilters()">Clear Filters</button>
    </div>

    <!-- Dynamic Stats Grid -->
    <div class="stats-grid">
      <div class="stat-card">
        <div class="stat-value" id="statDrugs">0</div>
        <div class="stat-label">Total Drugs</div>
      </div>
      <div class="stat-card">
        <div class="stat-value" id="statDrugsApproved">0</div>
        <div class="stat-label">Approved Drugs</div>
      </div>
      <div class="stat-card">
        <div class="stat-value" id="statDrugsPhase1">0</div>
        <div class="stat-label">Phase 1 Drugs</div>
      </div>
      <div class="stat-card">
        <div class="stat-value" id="statDrugsPhase2">0</div>
        <div class="stat-label">Phase 2 Drugs</div>
      </div>
      <div class="stat-card">
        <div class="stat-value" id="statDrugsPhase3">0</div>
        <div class="stat-label">Phase 3 Drugs</div>
      </div>
      <div class="stat-card">
        <div class="stat-value" id="statTrials">0</div>
        <div class="stat-label">Total Trials</div>
      </div>
      <div class="stat-card">
        <div class="stat-value" id="statTrialsForCondition">0</div>
        <div class="stat-label">Trials (Condition)</div>
      </div>
      <div class="stat-card">
        <div class="stat-value" id="statCompanies">0</div>
        <div class="stat-label">Companies</div>
      </div>
      <div class="stat-card">
        <div class="stat-value" id="statCohorts">0</div>
        <div class="stat-label">Row Variants</div>
      </div>
      <div class="stat-card">
        <div class="stat-value" id="statLogs">0</div>
        <div class="stat-label">Log Entries</div>
      </div>
    </div>
    <h3 style="margin:32px 0 18px 0;font-size:22px;color:#1e293b;font-weight:700">Recent Activity</h3>
    <div id="recentLogs" style="max-height: 400px; overflow-y: auto; background: #f8fafc; padding: 16px; border-radius: 10px; border: 1px solid #e6eef8;"></div>
  </div>

  <!-- Drugs Tab -->
  <div id="drugs" class="tab-content">
    <h2 style="margin:0 0 24px 0;color:#1e3a8a;font-size:26px;font-weight:800">💊 Drugs</h2>
    <!-- Quick Filter Shortcuts -->
    <div style="display:flex;gap:8px;margin-bottom:16px;flex-wrap:wrap;align-items:center">
      <span style="font-size:13px;font-weight:700;color:#64748b">Quick Filters:</span>
      <button class="btn btn-sm" style="background:#eef2ff;color:#1e3a8a;border:1px solid #bfdbfe;font-size:13px" onclick="quickFilterDrug('Phase 1')">Phase 1</button>
      <button class="btn btn-sm" style="background:#eef2ff;color:#1e3a8a;border:1px solid #bfdbfe;font-size:13px" onclick="quickFilterDrug('Phase 2')">Phase 2</button>
      <button class="btn btn-sm" style="background:#eef2ff;color:#1e3a8a;border:1px solid #bfdbfe;font-size:13px" onclick="quickFilterDrug('Phase 3')">Phase 3</button>
      <button class="btn btn-sm" style="background:#eef2ff;color:#1e3a8a;border:1px solid #bfdbfe;font-size:13px" onclick="quickFilterDrug('Approved')">Approved</button>
      <button class="btn btn-sm" style="background:#f0fdf4;color:#166534;border:1px solid #a7f3d0;font-size:13px" onclick="quickFilterDrug('Active')">Active</button>
      <button class="btn btn-sm" style="background:#fef3c7;color:#92400e;border:1px solid #fcd34d;font-size:13px" onclick="quickFilterDrug('Inactive')">Inactive</button>
    </div>

    <div class="toolbar" style="align-items:center;">
      <div style="display:flex;gap:12px;align-items:center;flex-wrap:wrap;">
        <input id="drugSearch" class="search-box" placeholder="Search by name, synonyms, or ID..." oninput="refreshAllViews()" />
        <select id="drugFilterPhase" onchange="refreshAllViews()" style="padding:10px;border:2px solid #bfdbfe;border-radius:8px;background:white;font-size:14px;font-weight:500;cursor:pointer;transition:all 0.2s">
          <option value="">All Phases</option>
          <option>Discovery</option>
          <option>Preclinical</option>
          <option>IND</option>
          <option>Phase 1</option>
          <option>Phase 2</option>
          <option>Phase 3</option>
          <option>Phase 4</option>
          <option>Approved</option>
        </select>
        <select id="drugFilterSponsor" onchange="refreshAllViews()" style="padding:10px;border:2px solid #bfdbfe;border-radius:8px;background:white;font-size:14px;font-weight:500;cursor:pointer;transition:all 0.2s">
          <option value="">All Sponsors</option>
        </select>
        <select id="drugFilterStatus" onchange="refreshAllViews()" style="padding:10px;border:2px solid #bfdbfe;border-radius:8px;background:white;font-size:14px;font-weight:500;cursor:pointer;transition:all 0.2s">
          <option value="">All Status</option>
          <option>Active</option>
          <option>Inactive</option>
          <option>Approved</option>
        </select>
      </div>
      <div>
        <button class="btn btn-primary" onclick="openDrugModal()">+ Add Drug</button>
        <button class="btn btn-secondary" onclick="clearDrugFilters()">Clear Filters</button>
      </div>
    </div>
    <div id="drugsAlert"></div>
    <div class="table-wrapper">
      <table>
        <thead>
          <tr>
            <th>ID</th>
            <th>Product Name</th>
            <th>MOA</th>
            <th>Phase</th>
            <th>Sponsor</th>
            <th>Status</th>
          </tr>
        </thead>
        <tbody id="drugsList"></tbody>
      </table>
    </div>
    <div id="drugsPagination"></div>
  </div>

  <!-- Trials Tab -->
  <div id="trials" class="tab-content">
    <h2 style="margin:0 0 24px 0;color:#1e3a8a;font-size:26px;font-weight:800">🧪 Trials</h2>
    <!-- Quick Filter Shortcuts -->
    <div style="display:flex;gap:8px;margin-bottom:16px;flex-wrap:wrap;align-items:center">
      <span style="font-size:13px;font-weight:700;color:#64748b">Quick Filters:</span>
      <button class="btn btn-sm" style="background:#eef2ff;color:#1e3a8a;border:1px solid #bfdbfe;font-size:13px" onclick="quickFilterTrial('Phase 1')">Phase 1</button>
      <button class="btn btn-sm" style="background:#eef2ff;color:#1e3a8a;border:1px solid #bfdbfe;font-size:13px" onclick="quickFilterTrial('Phase 2')">Phase 2</button>
      <button class="btn btn-sm" style="background:#eef2ff;color:#1e3a8a;border:1px solid #bfdbfe;font-size:13px" onclick="quickFilterTrial('Phase 3')">Phase 3</button>
      <button class="btn btn-sm" style="background:#f0fdf4;color:#166534;border:1px solid #a7f3d0;font-size:13px" onclick="quickFilterTrial('Completed')">Completed</button>
      <button class="btn btn-sm" style="background:#fef3c7;color:#92400e;border:1px solid #fcd34d;font-size:13px" onclick="quickFilterTrial('Unknown')">Unknown Status</button>
    </div>

    <div class="toolbar" style="align-items:center;">
      <div style="display:flex;gap:12px;align-items:center;flex-wrap:wrap">
        <input id="trialSearch" class="search-box" placeholder="Search trials by ID or title..." oninput="refreshAllViews()" />
        <select id="trialFilterCondition" onchange="refreshAllViews()" style="padding:10px;border:2px solid #bfdbfe;border-radius:8px;background:white;font-size:14px;font-weight:500;cursor:pointer;transition:all 0.2s">
          <option value="">All Conditions</option>
        </select>
        <select id="trialFilterPhase" onchange="refreshAllViews()" style="padding:10px;border:2px solid #bfdbfe;border-radius:8px;background:white;font-size:14px;font-weight:500;cursor:pointer;transition:all 0.2s">
          <option value="">All Phases</option>
          <option>Phase 1</option>
          <option>Phase 2</option>
          <option>Phase 3</option>
        </select>
        <select id="trialFilterSponsor" onchange="refreshAllViews()" style="padding:10px;border:2px solid #bfdbfe;border-radius:8px;background:white;font-size:14px;font-weight:500;cursor:pointer;transition:all 0.2s">
          <option value="">All Sponsors</option>
        </select>
        <select id="trialFilterCollaborator" onchange="refreshAllViews()" style="padding:10px;border:2px solid #bfdbfe;border-radius:8px;background:white;font-size:14px;font-weight:500;cursor:pointer;transition:all 0.2s">
          <option value="">All Collaborators</option>
        </select>
        <select id="trialFilterStatus" onchange="refreshAllViews()" style="padding:10px;border:2px solid #bfdbfe;border-radius:8px;background:white;font-size:14px;font-weight:500;cursor:pointer;transition:all 0.2s">
          <option value="">All Status</option>
          <option>Completed</option>
          <option>Unknown</option>
        </select>
      </div>
      <div>
        <button class="btn btn-primary" onclick="openTrialModal()">+ Add Trial</button>
        <button class="btn btn-secondary" onclick="clearTrialFilters()">Clear Filters</button>
      </div>
    </div>
    <div id="trialsAlert"></div>
    <div class="table-wrapper">
      <table>
        <thead>
          <tr>
            <th>ID</th>
            <th>Trial Identifier</th>
            <th>Indication</th>
            <th>Drug</th>
            <th>Status</th>
            <th>Details</th>
          </tr>
        </thead>
        <tbody id="trialsList"></tbody>
      </table>
    </div>
    <div id="trialsPagination"></div>
  </div>

  <!-- Companies Tab -->
  <div id="companies" class="tab-content">
    <div class="toolbar" style="margin-bottom:24px">
      <h2 style="margin: 0;color:#1e3a8a;font-size:26px;font-weight:800">🏢 Companies</h2>
      <button class="btn btn-primary" onclick="openCompanyModal()">+ Add Company</button>
    </div>
    <div class="table-wrapper">
      <table>
        <thead>
          <tr>
            <th>ID</th>
            <th>Name</th>
            <th>Type</th>
          </tr>
        </thead>
        <tbody id="companiesList"></tbody>
      </table>
    </div>
    <div id="companiesPagination"></div>
  </div>

  <!-- Import/Export Tab -->
  <div id="import" class="tab-content">
    <h2 style="color:#1e3a8a;font-size:26px;font-weight:800">📤 Import / Export</h2>
    
    <div style="background: linear-gradient(135deg, #eff6ff 0%, #f0f4ff 100%); padding: 24px; border-radius: 12px; margin-bottom: 24px; border: 2px solid #bfdbfe;">
      <h3 style="margin-top: 0;font-size:20px;font-weight:700;color:#1e293b">Import Excel File</h3>
      <div style="display: grid; grid-template-columns: 1fr auto auto; gap: 12px; align-items: center; margin-bottom: 12px;">
        <input type="file" id="excelInput" accept=".xlsx,.xls" style="padding:10px;border:2px solid #bfdbfe;border-radius:6px;font-size:14px" />
        <button class="btn btn-primary" onclick="handleExcelUpload()">Upload & Map</button>
        <button class="btn btn-secondary" onclick="document.getElementById('excelInput').value=''">Clear</button>
      </div>
      <div id="importStatus"></div>
    </div>

    <div id="mappingPreview"></div>

    <div style="background: linear-gradient(135deg, #eff6ff 0%, #f0f4ff 100%); padding: 24px; border-radius: 12px; border: 2px solid #bfdbfe; margin-bottom: 24px;">
      <h3 style="margin-top: 0;font-size:20px;font-weight:700;color:#1e293b">Export Data</h3>
      <div style="display: flex; gap: 12px;">
        <button class="btn btn-success" onclick="exportToJSON()" style="background:#10b981;font-size:14px">Export as JSON</button>
        <button class="btn btn-success" onclick="exportToExcel()" style="background:#10b981;font-size:14px">Export as Excel</button>
      </div>
    </div>

    <div style="background: #fef2f2; padding: 24px; border-radius: 12px; border: 2px solid #ef4444; margin-top: 12px;">
      <h3 style="margin-top: 0; color: #991b1b;font-size:20px;font-weight:700">⚠️ Clear All Data</h3>
      <p style="color: #7f1d1d; margin: 8px 0;font-size:14px;font-weight:500">Remove all companies, drugs, and trials from the database. <strong>This cannot be undone.</strong></p>
      <button class="btn btn-danger" onclick="if(confirm('Are you absolutely sure? All imported data will be permanently deleted.')) clearAllData()" style="font-size:14px">Clear All Data</button>
    </div>
  </div>

  <!-- Logs Tab -->
  <div id="logs" class="tab-content">
    <h2 style="color:#1e3a8a;font-size:26px;font-weight:800;margin-bottom:24px">📋 Activity Logs</h2>
    <button class="btn btn-danger btn-sm" onclick="clearAllLogs()" style="margin-bottom: 16px;font-size:14px">Clear Logs</button>
    <div class="table-wrapper">
      <table>
        <thead>
          <tr>
            <th>Timestamp</th>
            <th>Action</th>
            <th>Entity Type</th>
            <th>Details</th>
          </tr>
        </thead>
        <tbody id="logsList"></tbody>
      </table>
    </div>
  </div>

</div>

<!-- Modals -->
<div id="drugModal" class="modal">
  <div class="modal-content">
    <div class="modal-header-bar">
      <button onclick="closeAllModals()">← Back</button>
      <h2 id="drugModalTitle">Drug Details</h2>
    </div>
    <div class="modal-inner">
      <div id="drugFormContainer"></div>
    </div>
  </div>
</div>

<div id="trialModal" class="modal">
  <div class="modal-content fullpage">
    <div class="modal-header-bar">
      <button onclick="closeAllModals()">← Back</button>
      <h2 id="trialModalTitle">Trial Details</h2>
    </div>
    <div class="modal-inner">
      <div id="trialFormContainer"></div>
    </div>
  </div>
</div>

<div id="trialDetailsModal" class="modal">
  <div class="modal-content fullpage">
    <div class="modal-header-bar">
      <button onclick="closeAllModals()">← Back</button>
      <h2 id="trialDetailsTitle">Trial Details (Imported Rows)</h2>
    </div>
    <div class="modal-inner">
      <div id="trialDetailsContainer"></div>
    </div>
  </div>
</div>

<div id="companyModal" class="modal">
  <div class="modal-content">
    <div class="modal-header-bar">
      <button onclick="closeAllModals()">← Back</button>
      <h2 id="companyModalTitle">Company Details</h2>
    </div>
    <div class="modal-inner">
      <div id="companyFormContainer"></div>
    </div>
  </div>
</div>

<script>
// ============ IndexedDB Setup ============
const DB_NAME = 'DKLinityDB2';
const DB_VERSION = 1;
let db;

// Pagination variables
let currentDrugPage = 1;
let currentTrialPage = 1;
let currentCompanyPage = 1;
let itemsPerPageDrug = 20;
let itemsPerPageTrial = 20;
const itemsPerPageCompany = 20;
let lastDrugFilterKey = '';
let lastTrialFilterKey = '';

async function initDB() {
  return new Promise((resolve, reject) => {
    const req = indexedDB.open(DB_NAME, DB_VERSION);
    req.onerror = () => reject(req.error);
    req.onsuccess = () => { db = req.result; resolve(db); };
    req.onupgradeneeded = (e) => {
      const database = e.target.result;
      if (!database.objectStoreNames.contains('companies')) {
        database.createObjectStore('companies', { keyPath: 'company_id', autoIncrement: true });
      }
      if (!database.objectStoreNames.contains('drugs')) {
        database.createObjectStore('drugs', { keyPath: 'drug_id', autoIncrement: true });
      }
      if (!database.objectStoreNames.contains('trials')) {
        database.createObjectStore('trials', { keyPath: 'trial_id', autoIncrement: true });
      }
      if (!database.objectStoreNames.contains('logs')) {
        database.createObjectStore('logs', { keyPath: 'log_id', autoIncrement: true });
      }
    };
  });
}

function getStore(storeName, mode = 'readonly') {
  const tx = db.transaction([storeName], mode);
  return tx.objectStore(storeName);
}

async function addRecord(storeName, data) {
  return new Promise((resolve, reject) => {
    const store = getStore(storeName, 'readwrite');
    const req = store.add(data);
    req.onsuccess = () => resolve(req.result);
    req.onerror = () => reject(req.error);
  });
}

async function putRecord(storeName, data) {
  return new Promise((resolve, reject) => {
    const store = getStore(storeName, 'readwrite');
    const req = store.put(data);
    req.onsuccess = () => resolve(req.result);
    req.onerror = () => reject(req.error);
  });
}

async function getAllRecords(storeName) {
  return new Promise((resolve, reject) => {
    const store = getStore(storeName, 'readonly');
    const req = store.getAll();
    req.onsuccess = () => resolve(req.result);
    req.onerror = () => reject(req.error);
  });
}

async function deleteRecord(storeName, key) {
  return new Promise((resolve, reject) => {
    const store = getStore(storeName, 'readwrite');
    const req = store.delete(key);
    req.onsuccess = () => resolve();
    req.onerror = () => reject(req.error);
  });
}

// ============ Logging ============
async function addLog(action, entityType, details) {
  const logEntry = {
    timestamp: new Date().toLocaleString(),
    action: action,
    entityType: entityType,
    details: details || ''
  };
  await addRecord('logs', logEntry);
  await refreshAllViews();
}

// ============ Data Refresh & Render ============
async function refreshAllViews() {
  const companies = await getAllRecords('companies');
  const drugs = await getAllRecords('drugs');
  const trials = await getAllRecords('trials');
  const logs = await getAllRecords('logs');

  // Update stats
  document.getElementById('statDrugs').textContent = drugs.length;
  document.getElementById('statTrials').textContent = trials.length;
  document.getElementById('statCompanies').textContent = companies.length;
  let rowVariantCount = 0;
  trials.forEach(t => { if (t.import_row_details) rowVariantCount += t.import_row_details.length; });
  document.getElementById('statCohorts').textContent = rowVariantCount;
  document.getElementById('statLogs').textContent = logs.length;

  // Render recent logs
  renderRecentLogs(logs.slice(-5).reverse());

  // Preserve filter selections
  const prevDrugSponsor = document.getElementById('drugFilterSponsor')?.value || '';
  const prevTrialSponsor = document.getElementById('trialFilterSponsor')?.value || '';
  const prevDashSponsor = document.getElementById('dashboardFilterSponsor')?.value || '';
  const prevCondition = document.getElementById('trialFilterCondition')?.value || '';
  const prevDashCondition = document.getElementById('dashboardFilterCondition')?.value || '';
  const prevCollaborator = document.getElementById('trialFilterCollaborator')?.value || '';

  // Populate sponsor filters (if present)
  const sponsorOptions = '<option value="">All Sponsors</option>' + companies.map(c => `<option value="${escapeHtml(c.company_name)}">${escapeHtml(c.company_name)}</option>`).join('');
  const drugFilterSponsor = document.getElementById('drugFilterSponsor');
  if(drugFilterSponsor) drugFilterSponsor.innerHTML = sponsorOptions;
  const trialFilterSponsor = document.getElementById('trialFilterSponsor');
  if(trialFilterSponsor) trialFilterSponsor.innerHTML = sponsorOptions;
  const dashboardFilterSponsor = document.getElementById('dashboardFilterSponsor');
  if(dashboardFilterSponsor) dashboardFilterSponsor.innerHTML = sponsorOptions;

  // Populate condition filters (if present) from trials tumor_group
  const conditions = new Set();
  trials.forEach(t => {
    if(t.tumor_group) {
      t.tumor_group.toString().split(/[,;|]/).map(v => v.trim()).filter(Boolean).forEach(v => conditions.add(v));
    }
    if(t.tumors && Array.isArray(t.tumors)) t.tumors.forEach(tu => conditions.add(tu));
  });
  const conditionOptions = '<option value="">All Conditions</option>' + Array.from(conditions).sort().map(c => `<option value="${escapeHtml(c)}">${escapeHtml(c)}</option>`).join('');
  const trialFilterCondition = document.getElementById('trialFilterCondition');
  if(trialFilterCondition) trialFilterCondition.innerHTML = conditionOptions;
  const dashboardFilterCondition = document.getElementById('dashboardFilterCondition');
  if(dashboardFilterCondition) dashboardFilterCondition.innerHTML = conditionOptions;

  // Populate collaborators (if present)
  const collaborators = new Set();
  trials.forEach(t => {
    if(t.collaborator && t.collaborator.trim()) collaborators.add(t.collaborator);
    if(t.sponsor && t.sponsor.trim()) collaborators.add(t.sponsor);
  });
  const collaboratorOptions = '<option value="">All Collaborators</option>' + Array.from(collaborators).sort().map(c => `<option value="${escapeHtml(c)}">${escapeHtml(c)}</option>`).join('');
  const trialFilterCollaborator = document.getElementById('trialFilterCollaborator');
  if(trialFilterCollaborator) trialFilterCollaborator.innerHTML = collaboratorOptions;

  // Restore filter selections when possible
  if (drugFilterSponsor) drugFilterSponsor.value = prevDrugSponsor;
  if (trialFilterSponsor) trialFilterSponsor.value = prevTrialSponsor;
  if (dashboardFilterSponsor) dashboardFilterSponsor.value = prevDashSponsor;
  if (trialFilterCondition) trialFilterCondition.value = prevCondition;
  if (dashboardFilterCondition) dashboardFilterCondition.value = prevDashCondition;
  if (trialFilterCollaborator) trialFilterCollaborator.value = prevCollaborator;

  // Update dashboard stats
  updateDashboardStats();

  // Render tables (will apply active filters)
  renderDrugs(drugs, companies);
  renderTrials(trials, drugs);
  renderCompanies(companies);
  renderLogs(logs);
}

function setItemsPerPage(type, value) {
  const perPage = parseInt(value, 10) === 40 ? 40 : 20;
  if (type === 'drug') {
    itemsPerPageDrug = perPage;
    currentDrugPage = 1;
  } else {
    itemsPerPageTrial = perPage;
    currentTrialPage = 1;
  }
  refreshAllViews();
}

function renderRecentLogs(logs) {
  const html = logs.map(l => `
    <div style="padding: 8px; border-bottom: 1px solid #e6eef8; font-size: 12px;">
      <strong>${l.action}</strong> - ${l.entityType} <br/>
      <small style="color: #64748b;">${l.timestamp} ${l.details ? '- ' + l.details : ''}</small>
    </div>
  `).join('');
  document.getElementById('recentLogs').innerHTML = html || '<div style="padding: 8px; color: #64748b;">No activity</div>';
}

function renderCompanies(companies) {
  const tbody = document.getElementById('companiesList');
  if (!companies.length) {
    tbody.innerHTML = '<tr><td colspan="3" style="text-align: center; color: #64748b;">No companies found</td></tr>';
    document.getElementById('companiesPagination').innerHTML = '';
    return;
  }
  const totalPages = Math.max(1, Math.ceil(companies.length / itemsPerPageCompany));
  currentCompanyPage = Math.min(currentCompanyPage, totalPages);
  const start = (currentCompanyPage - 1) * itemsPerPageCompany;
  const end = start + itemsPerPageCompany;
  const pageItems = companies.slice(start, end);

  tbody.innerHTML = pageItems.map(c => `
    <tr>
      <td>${c.company_id}</td>
      <td>${escapeHtml(c.company_name || '')}</td>
      <td>${c.company_type || ''}</td>
    </tr>
  `).join('');

  let paginationHtml = '';
  if (totalPages > 1) {
    paginationHtml = '<div class="pagination">';
    paginationHtml += `<button onclick="goToCompanyPage(${currentCompanyPage - 1})" ${currentCompanyPage === 1 ? 'disabled' : ''}>← Previous</button>`;
    paginationHtml += `<span>Page ${currentCompanyPage} of ${totalPages}</span>`;
    paginationHtml += `<button onclick="goToCompanyPage(${currentCompanyPage + 1})" ${currentCompanyPage === totalPages ? 'disabled' : ''}>Next →</button>`;
    paginationHtml += '</div>';
  }
  document.getElementById('companiesPagination').innerHTML = paginationHtml;
}

function goToCompanyPage(page) {
  const totalPages = Math.max(1, Math.ceil(document.getElementById('companiesList').parentElement.parentElement.parentElement.querySelector('tbody').childElementCount / itemsPerPageCompany)) || 1;
  currentCompanyPage = Math.max(1, Math.min(page, totalPages));
  refreshAllViews();
}

function renderDrugs(drugs, companies) {
  const tbody = document.getElementById('drugsList');
  const companyMap = {};
  companies.forEach(c => companyMap[c.company_id] = c.company_name);
  // Apply filters
  const search = (document.getElementById('drugSearch')?.value || '').toString().toLowerCase().trim();
  const phaseFilter = (document.getElementById('drugFilterPhase')?.value || '').toString().trim();
  const sponsorFilter = (document.getElementById('drugFilterSponsor')?.value || '').toString().trim();
  const statusFilter = (document.getElementById('drugFilterStatus')?.value || '').toString().trim();

  const filtered = drugs.filter(d => {
    if (search) {
      const name = (d.product_name || '').toString().toLowerCase();
      const syn = (d.asset_synonyms || []).join(' ').toLowerCase();
      const idStr = String(d.drug_id || '').toLowerCase();
      if (!(name.includes(search) || syn.includes(search) || idStr.includes(search))) return false;
    }
    if (phaseFilter) {
      if ((d.highest_phase || '').toString() !== phaseFilter) return false;
    }
    if (sponsorFilter) {
      if ((companyMap[d.sponsor_id] || '') !== sponsorFilter) return false;
    }
    if (statusFilter) {
      const st = (d.active_inactive || d.asset_status || '').toString();
      if (st !== statusFilter && (d.highest_phase || '') !== statusFilter) return false;
    }
    return true;
  });

  if (!filtered.length) {
    tbody.innerHTML = '<tr><td colspan="6" style="text-align: center; color: #64748b;">No drugs found</td></tr>';
    document.getElementById('drugsAlert').innerHTML = '<div class="alert alert-info">No drugs match the current filters.</div>';
    document.getElementById('drugsPagination').innerHTML = '';
    return;
  }
  document.getElementById('drugsAlert').innerHTML = '';
  
  const filterKey = [search, phaseFilter, sponsorFilter, statusFilter].join('|');
  if (filterKey !== lastDrugFilterKey) {
    currentDrugPage = 1;
    lastDrugFilterKey = filterKey;
  }
  
  // Pagination
  const perPage = itemsPerPageDrug === Infinity ? filtered.length : itemsPerPageDrug;
  const totalPages = Math.max(1, Math.ceil(filtered.length / Math.max(1, perPage)));
  currentDrugPage = Math.min(currentDrugPage, totalPages);
  const start = (currentDrugPage - 1) * perPage;
  const end = start + perPage;
  const pageItems = filtered.slice(start, end);
  
  tbody.innerHTML = pageItems.map(d => `
    <tr>
      <td><a href="#" onclick="event.preventDefault();editDrug(${d.drug_id})" style="color:#1e3a8a;text-decoration:none;font-weight:600">${d.drug_id}</a></td>
      <td><a href="#" onclick="event.preventDefault();editDrug(${d.drug_id})" style="color:#1e3a8a;text-decoration:none;font-weight:600">${escapeHtml(d.product_name || '')}</a></td>
      <td>${escapeHtml(d.moa || '')}</td>
      <td><span class="badge">${escapeHtml(d.highest_phase || 'N/A')}</span></td>
      <td>${companyMap[d.sponsor_id] || ''}</td>
      <td>${escapeHtml(d.active_inactive || d.asset_status || 'Active')}</td>
    </tr>
  `).join('');
  
  // Render pagination controls (prev/next only)
  let paginationHtml = '';
  if (itemsPerPageDrug !== Infinity && totalPages > 1) {
    paginationHtml = '<div class="pagination">';
    paginationHtml += `<button onclick="goToDrugPage(${currentDrugPage - 1})" ${currentDrugPage === 1 ? 'disabled' : ''}>← Previous</button>`;
    paginationHtml += `<span>Page ${currentDrugPage} of ${totalPages}</span>`;
    paginationHtml += `<button onclick="goToDrugPage(${currentDrugPage + 1})" ${currentDrugPage === totalPages ? 'disabled' : ''}>Next →</button>`;
    paginationHtml += `<select onchange="setItemsPerPage('drug', this.value)" style="padding:6px 8px;border:1px solid #e6eef8;border-radius:6px;background:white;font-size:12px;">
      <option value="20" ${itemsPerPageDrug === 20 ? 'selected' : ''}>20 rows</option>
      <option value="40" ${itemsPerPageDrug === 40 ? 'selected' : ''}>40 rows</option>
    </select>`;
    paginationHtml += '</div>';
  }
  document.getElementById('drugsPagination').innerHTML = paginationHtml;
}

function goToDrugPage(page) {
  const tbody = document.getElementById('drugsList');
  if (!tbody) return;
  const totalPages = Math.max(1, Math.ceil(document.getElementById('drugsList').parentElement.parentElement.parentElement.querySelector('tbody').childElementCount / Math.max(1, itemsPerPageDrug))) || 1;
  currentDrugPage = Math.max(1, Math.min(page, totalPages));
  refreshAllViews();
}

function renderTrials(trials, drugs) {
  const tbody = document.getElementById('trialsList');
  const drugMap = {};
  drugs.forEach(d => drugMap[d.drug_id] = d.product_name);
  // Apply filters
  const search = (document.getElementById('trialSearch')?.value || '').toString().toLowerCase().trim();
  const conditionFilter = (document.getElementById('trialFilterCondition')?.value || '').toString().trim();
  const phaseFilter = (document.getElementById('trialFilterPhase')?.value || '').toString().trim();
  const sponsorFilter = (document.getElementById('trialFilterSponsor')?.value || '').toString().trim();
  const collaboratorFilter = (document.getElementById('trialFilterCollaborator')?.value || '').toString().trim();
  const statusFilter = (document.getElementById('trialFilterStatus')?.value || '').toString().trim();

  const filtered = trials.filter(t => {
    if (search) {
      const id = (t.trial_identifier || '').toString().toLowerCase();
      const title = (t.trial_title || '').toString().toLowerCase();
      if (!(id.includes(search) || title.includes(search))) return false;
    }
    if (conditionFilter) {
      const tg = (t.tumor_group || '').toString().trim();
      const tgValues = tg ? tg.split(/[,;|]/).map(v => v.trim()).filter(Boolean) : [];
      const tumors = (t.tumors || []).map(tu => tu.toString().trim());
      if (!tumors.includes(conditionFilter) && !tgValues.includes(conditionFilter)) return false;
    }
    if (phaseFilter) {
      const dev = (t.development_status || '').toString();
      if (dev !== phaseFilter) return false;
    }
    if (sponsorFilter) {
      const s = (t.sponsor || '').toString();
      if (s !== sponsorFilter) return false;
    }
    if (collaboratorFilter) {
      const c = (t.collaborator || '').toString();
      if (c !== collaboratorFilter) return false;
    }
    if (statusFilter) {
      const dev = (t.recruitment_status || '').toString();
      if (dev !== statusFilter) return false;
    }
    return true;
  });

  if (!filtered.length) {
    tbody.innerHTML = '<tr><td colspan="6" style="text-align: center; color: #64748b;">No trials found</td></tr>';
    document.getElementById('trialsAlert').innerHTML = '<div class="alert alert-info">No trials match the current filters.</div>';
    document.getElementById('trialsPagination').innerHTML = '';
    return;
  }
  document.getElementById('trialsAlert').innerHTML = '';
  
  const filterKey = [search, conditionFilter, phaseFilter, sponsorFilter, collaboratorFilter, statusFilter].join('|');
  if (filterKey !== lastTrialFilterKey) {
    currentTrialPage = 1;
    lastTrialFilterKey = filterKey;
  }
  
  // Pagination
  const perPage = itemsPerPageTrial === Infinity ? filtered.length : itemsPerPageTrial;
  const totalPages = Math.max(1, Math.ceil(filtered.length / Math.max(1, perPage)));
  currentTrialPage = Math.min(currentTrialPage, totalPages);
  const start = (currentTrialPage - 1) * perPage;
  const end = start + perPage;
  const pageItems = filtered.slice(start, end);
  
  tbody.innerHTML = pageItems.map(t => {
    const indication = (t.tumors && t.tumors.length) ? t.tumors.join('; ') : (t.tumor_group || '');
    return `
    <tr>
      <td><a href="#" onclick="event.preventDefault();editTrial(${t.trial_id})" style="color:#1e3a8a;text-decoration:none;font-weight:600">${t.trial_id}</a></td>
      <td><a href="#" onclick="event.preventDefault();editTrial(${t.trial_id})" style="color:#1e3a8a;text-decoration:none;font-weight:600">${escapeHtml(t.trial_identifier || '')}</a></td>
      <td>${escapeHtml(indication)}</td>
      <td>${drugMap[t.asset_id] || 'N/A'}</td>
      <td><span class="badge-success">${escapeHtml(t.development_status || 'Unknown')}</span></td>
      <td><button class="btn btn-secondary btn-sm" onclick="openTrialDetails(${t.trial_id})">View</button></td>
    </tr>
  `;
  }).join('');
  
  // Render pagination controls (prev/next only)
  let paginationHtml = '';
  if (itemsPerPageTrial !== Infinity && totalPages > 1) {
    paginationHtml = '<div class="pagination">';
    paginationHtml += `<button onclick="goToTrialPage(${currentTrialPage - 1})" ${currentTrialPage === 1 ? 'disabled' : ''}>← Previous</button>`;
    paginationHtml += `<span>Page ${currentTrialPage} of ${totalPages}</span>`;
    paginationHtml += `<button onclick="goToTrialPage(${currentTrialPage + 1})" ${currentTrialPage === totalPages ? 'disabled' : ''}>Next →</button>`;
    paginationHtml += `<select onchange="setItemsPerPage('trial', this.value)" style="padding:6px 8px;border:1px solid #e6eef8;border-radius:6px;background:white;font-size:12px;">
      <option value="20" ${itemsPerPageTrial === 20 ? 'selected' : ''}>20 rows</option>
      <option value="40" ${itemsPerPageTrial === 40 ? 'selected' : ''}>40 rows</option>
    </select>`;
    paginationHtml += '</div>';
  }
  document.getElementById('trialsPagination').innerHTML = paginationHtml;
}

function goToTrialPage(page) {
  const tbody = document.getElementById('trialsList');
  if (!tbody) return;
  const totalPages = Math.max(1, Math.ceil(document.getElementById('trialsList').parentElement.parentElement.parentElement.querySelector('tbody').childElementCount / Math.max(1, itemsPerPageTrial))) || 1;
  currentTrialPage = Math.max(1, Math.min(page, totalPages));
  refreshAllViews();
}

function renderLogs(logs) {
  const tbody = document.getElementById('logsList');
  if (!logs.length) {
    tbody.innerHTML = '<tr><td colspan="4" style="text-align: center; color: #64748b;">No logs</td></tr>';
    return;
  }
  tbody.innerHTML = logs.slice().reverse().map(l => `
    <tr>
      <td style="font-size: 12px;">${l.timestamp}</td>
      <td><strong>${l.action}</strong></td>
      <td>${l.entityType}</td>
      <td style="font-size: 12px;">${escapeHtml(l.details || '')}</td>
    </tr>
  `).join('');
}

function clearDrugFilters(){
  const s = document.getElementById('drugSearch'); if(s) s.value = '';
  const p = document.getElementById('drugFilterPhase'); if(p) p.value = '';
  const sp = document.getElementById('drugFilterSponsor'); if(sp) sp.value = '';
  const st = document.getElementById('drugFilterStatus'); if(st) st.value = '';
  refreshAllViews();
}

function clearTrialFilters(){
  const s = document.getElementById('trialSearch'); if(s) s.value = '';
  const c = document.getElementById('trialFilterCondition'); if(c) c.value = '';
  const p = document.getElementById('trialFilterPhase'); if(p) p.value = '';
  const sp = document.getElementById('trialFilterSponsor'); if(sp) sp.value = '';
  const col = document.getElementById('trialFilterCollaborator'); if(col) col.value = '';
  const st = document.getElementById('trialFilterStatus'); if(st) st.value = '';
  refreshAllViews();
}

function quickFilterDrug(filterValue){
  const p = document.getElementById('drugFilterPhase');
  const st = document.getElementById('drugFilterStatus');
  // Detect if it's a phase or status
  if(['Discovery','Preclinical','IND','Phase 1','Phase 2','Phase 3','Phase 4','Approved'].includes(filterValue)){
    if(p) p.value = filterValue;
    if(st) st.value = '';
  } else {
    if(st) st.value = filterValue;
    if(p) p.value = '';
  }
  document.getElementById('drugSearch').value = '';
  refreshAllViews();
}

function quickFilterTrial(filterValue){
  const st = document.getElementById('trialFilterStatus');
  const ph = document.getElementById('trialFilterPhase');
  if (['Phase 1','Phase 2','Phase 3'].includes(filterValue)) {
    if (ph) ph.value = filterValue;
    if (st) st.value = '';
  } else {
    if (st) st.value = filterValue;
    if (ph) ph.value = '';
  }
  document.getElementById('trialSearch').value = '';
  document.getElementById('trialFilterCondition').value = '';
  document.getElementById('trialFilterSponsor').value = '';
  document.getElementById('trialFilterCollaborator').value = '';
  refreshAllViews();
}

async function updateDashboardStats(){
  const drugs = await getAllRecords('drugs');
  const trials = await getAllRecords('trials');
  const companies = await getAllRecords('companies');
  const sponsorFilter = (document.getElementById('dashboardFilterSponsor')?.value || '').toString().trim();
  const conditionFilter = (document.getElementById('dashboardFilterCondition')?.value || '').toString().trim();

  // Filter drugs by sponsor if selected
  let filteredDrugs = drugs;
  if(sponsorFilter){
    filteredDrugs = drugs.filter(d => {
      const company = companies.find(c => c.company_id === d.sponsor_id);
      return company && company.company_name === sponsorFilter;
    });
  }

  // Calculate drug stats
  const approvedCount = filteredDrugs.filter(d => (d.highest_phase || '') === 'Approved').length;
  const phase1Count = filteredDrugs.filter(d => (d.highest_phase || '') === 'Phase 1').length;
  const phase2Count = filteredDrugs.filter(d => (d.highest_phase || '') === 'Phase 2').length;
  const phase3Count = filteredDrugs.filter(d => (d.highest_phase || '') === 'Phase 3').length;

  // Filter trials by sponsor and condition if selected
  let filteredTrials = trials;
  if(sponsorFilter){
    filteredTrials = trials.filter(t => {
      const sp = (t.sponsor || '').toString().trim();
      return sp === sponsorFilter;
    });
  }
  if(conditionFilter){
    filteredTrials = filteredTrials.filter(t => {
      const tg = (t.tumor_group || '').toString().trim();
      const tgValues = tg ? tg.split(/[,;|]/).map(v => v.trim()).filter(Boolean) : [];
      const tumors = (t.tumors || []).map(tu => tu.toString().trim());
      return tumors.includes(conditionFilter) || tgValues.includes(conditionFilter);
    });
  }

  // Update stats
  document.getElementById('statDrugs').textContent = filteredDrugs.length;
  document.getElementById('statDrugsApproved').textContent = approvedCount;
  document.getElementById('statDrugsPhase1').textContent = phase1Count;
  document.getElementById('statDrugsPhase2').textContent = phase2Count;
  document.getElementById('statDrugsPhase3').textContent = phase3Count;
  document.getElementById('statTrials').textContent = filteredTrials.length;
  document.getElementById('statTrialsForCondition').textContent = conditionFilter ? filteredTrials.length : trials.length;
}

function clearDashboardFilters(){
  const sp = document.getElementById('dashboardFilterSponsor'); if(sp) sp.value = '';
  const c = document.getElementById('dashboardFilterCondition'); if(c) c.value = '';
  updateDashboardStats();
}

// ============ Modal Control ============
function closeAllModals() {
  document.getElementById('drugModal').classList.remove('show');
  document.getElementById('trialModal').classList.remove('show');
  document.getElementById('trialDetailsModal').classList.remove('show');
  document.getElementById('companyModal').classList.remove('show');
}

function switchTab(e, tabName) {
  e.preventDefault();
  document.querySelectorAll('.tab-content').forEach(el => el.classList.remove('active'));
  document.querySelectorAll('.nav-tab').forEach(el => el.classList.remove('active'));
  document.getElementById(tabName).classList.add('active');
  e.target.classList.add('active');
}

// ============ Company Management ============
function openCompanyModal(companyId = null) {
  const isEdit = companyId !== null;
  
  // Update modal title
  document.getElementById('companyModalTitle').textContent = isEdit ? 'Edit Company' : 'Create New Company';
  
  let html = `
    <div class="form-grid">
      <div class="form-group">
        <label>Company Name *</label>
        <input type="text" id="companyName" placeholder="e.g., Pfizer Inc." />
      </div>
      <div class="form-group">
        <label>Company Type</label>
        <select id="companyType">
          <option value="Pharmaceutical">Pharmaceutical</option>
          <option value="Biotech">Biotech</option>
          <option value="CRO">CRO</option>
          <option value="Other">Other</option>
        </select>
      </div>
    </div>
    <div class="form-actions">
      <button class="btn btn-secondary" onclick="closeAllModals()">Cancel</button>
      <button class="btn btn-success" onclick="saveCompany(${companyId || 'null'})">${isEdit ? 'Update' : 'Create'} Company</button>
    </div>
  `;

  if (isEdit) {
    getAllRecords('companies').then(companies => {
      const company = companies.find(c => c.company_id === companyId);
      if (company) {
        document.getElementById('companyName').value = company.company_name || '';
        document.getElementById('companyType').value = company.company_type || 'Pharmaceutical';
      }
    });
  }

  document.getElementById('companyFormContainer').innerHTML = html;
  document.getElementById('companyModal').classList.add('show');
}

async function saveCompany(companyId) {
  const name = document.getElementById('companyName').value.trim();
  const type = document.getElementById('companyType').value;

  if (!name) {
    alert('Company name is required');
    return;
  }

  const data = { company_name: name, company_type: type };

  if (companyId !== null && companyId !== 'null') {
    data.company_id = companyId;
    await putRecord('companies', data);
    await addLog('UPDATE', 'Company', name);
  } else {
    data.created_at = new Date().toISOString();
    await addRecord('companies', data);
    await addLog('CREATE', 'Company', name);
  }

  closeAllModals();
  await refreshAllViews();
}

async function editCompany(companyId) {
  openCompanyModal(companyId);
}

async function deleteCompany(companyId) {
  if (confirm('Are you sure you want to delete this company?')) {
    await deleteRecord('companies', companyId);
    await addLog('DELETE', 'Company', `ID ${companyId}`);
    await refreshAllViews();
  }
}

// ============ Drug Management ============
async function openDrugModal(drugId = null) {
  const isEdit = drugId !== null;
  const companies = await getAllRecords('companies');
  const companyOptions = '<option value="">Select Sponsor</option>' +
    companies.map(c => `<option value="${c.company_id}">${escapeHtml(c.company_name)}</option>`).join('');

  // Update modal title
  document.getElementById('drugModalTitle').textContent = isEdit ? 'Edit Drug' : 'Create New Drug';

  let html = `
    <div style="display:flex;flex-direction:column;gap:10px">
    
    <!-- Sub-tabs for drug sections -->
    <div class="sub-tabs">
      <button type="button" class="sub-tab active" onclick="switchDrugTab('general')">📋 General</button>
      <button type="button" class="sub-tab" onclick="switchDrugTab('preclinical')">🧪 Preclinical</button>
      <button type="button" class="sub-tab" onclick="switchDrugTab('regulatory')">📜 Regulatory</button>
      <button type="button" class="sub-tab" onclick="switchDrugTab('reference')">📚 Reference</button>
      <button type="button" class="sub-tab" id="trialsTabBtn" onclick="switchDrugTab('trials')">🧬 Associated Trials</button>
    </div>

    <!-- General Tab -->
    <div id="tab-general" class="tab-pane active">
      <div style="font-weight:700;margin-bottom:8px">Basic Information</div>
      <div class="form-grid">
        <div class="form-group"><label>Product Name *</label><input type="text" id="drugProduct" placeholder="e.g., Aspirin" /></div>
        <div class="form-group"><label>PD-1/L1 Drug</label><select id="drugPD"><option value="">Select</option><option value="PD-1">PD-1</option><option value="PD-L1">PD-L1</option><option value="Both">Both</option></select></div>
        <div class="form-group"><label>Company (Sponsor)</label><select id="drugSponsor">${companyOptions}</select></div>
        <div class="form-group"><label>Active / Inactive</label><select id="drugActive"><option value="Active">Active</option><option value="Inactive">Inactive</option></select></div>
      </div>

      <div style="font-weight:700;margin-top:10px;margin-bottom:8px">Classification & Development</div>
      <div class="form-grid">
        <div class="form-group"><label>Drug Class</label><input id="drugClass" placeholder="e.g., Immunotherapy, Small Molecule" /></div>
        <div class="form-group"><label>MoA</label><input id="drugMOA" placeholder="e.g., PD-1 inhibitor" /></div>
        <div class="form-group"><label>Target</label><input id="drugTarget" placeholder="e.g., PD-1" /></div>
        <div class="form-group"><label>Modality</label><input id="drugModality" placeholder="e.g., Monoclonal Antibody" /></div>
      </div>

      <div style="font-weight:700;margin-top:10px;margin-bottom:8px">Development Status</div>
      <div class="form-grid">
        <div class="form-group"><label>Highest Phase</label><select id="drugPhase">
          <option value="">Select Phase</option>
          <option value="Discovery">Discovery</option>
          <option value="Preclinical">Preclinical</option>
          <option value="IND">IND</option>
          <option value="Phase 1">Phase 1</option>
          <option value="Phase 2">Phase 2</option>
          <option value="Phase 3">Phase 3</option>
          <option value="Phase 4">Phase 4</option>
          <option value="Approved">Approved</option>
        </select></div>
        <div class="form-group"><label>Development Status</label><input id="drugDevStatus" placeholder="e.g., In Development" /></div>
        <div class="form-group"><label>Asset Status</label><input id="drugAssetStatus" placeholder="e.g., Active" /></div>
      </div>

      <div style="font-weight:700;margin-top:10px;margin-bottom:8px">Additional Information</div>
      <div class="form-grid">
        <div class="form-group" style="grid-column: 1/-1;"><label>Asset Synonyms (comma separated)</label><textarea id="drugSynonyms" placeholder="synonym1, synonym2"></textarea></div>
      </div>
    </div>

    <!-- Preclinical Tab -->
    <div id="tab-preclinical" class="tab-pane">
      <div style="font-weight:700;margin-bottom:8px">Preclinical Evidence</div>
      <div class="form-grid">
        <div class="form-group" style="grid-column: 1/-1;"><label>Evidence In Vitro</label><textarea id="drugEviVitro" placeholder="In vitro efficacy data..."></textarea></div>
        <div class="form-group" style="grid-column: 1/-1;"><label>Evidence In Vivo</label><textarea id="drugEviVivo" placeholder="In vivo efficacy data..."></textarea></div>
      </div>

      <div style="font-weight:700;margin-top:10px;margin-bottom:8px">Source & Publication</div>
      <div class="form-grid">
        <div class="form-group"><label>MoA / Modality Source</label><input id="drugMoASource" placeholder="source text or URL" /></div>
        <div class="form-group"><label>Asset Status Source</label><input id="drugAssetStatusSource" placeholder="source text or URL" /></div>
        <div class="form-group" style="grid-column: 1/-1;"><label>Publication Source (Preclinical)</label><input id="drugPreclinPub" placeholder="URL or source reference" /></div>
      </div>
    </div>

    <!-- Regulatory Tab -->
    <div id="tab-regulatory" class="tab-pane">
      <div style="font-weight:700;margin-bottom:8px">Regulatory Information</div>
      <div class="form-grid">
        <div class="form-group"><label>Regulatory Agency</label><input id="drugRegAgency" placeholder="e.g., FDA, EMA" /></div>
        <div class="form-group"><label>Approved Region</label><input id="drugApprovedRegion" placeholder="e.g., USA, EU, Japan" /></div>
        <div class="form-group"><label>Regulatory Designations</label><input id="drugRegDesignations" placeholder="e.g., Breakthrough Therapy, Fast Track" /></div>
        <div class="form-group"><label>Regulatory Designations Source</label><input id="drugRegDesignationsSource" placeholder="source reference" /></div>
      </div>

      <div style="font-weight:700;margin-top:10px;margin-bottom:8px">Milestones & Partnerships</div>
      <div class="form-grid">
        <div class="form-group"><label>Upcoming Clinical Milestone</label><input id="drugUpcomingClinical" placeholder="e.g., Phase III readout Q2 2024" /></div>
        <div class="form-group"><label>Upcoming Clinical Milestone Source</label><input id="drugUpcomingClinicalSource" placeholder="source reference" /></div>
        <div class="form-group"><label>Upcoming Regulatory Milestone</label><input id="drugUpcomingReg" placeholder="e.g., BLA submission Q3 2024" /></div>
        <div class="form-group"><label>Upcoming Regulatory Milestone Source</label><input id="drugUpcomingRegSource" placeholder="source reference" /></div>
        <div class="form-group" style="grid-column: 1/-1;"><label>Partnerships (comma separated)</label><textarea id="drugPartnerships" placeholder="Partner1, Partner2, Partner3"></textarea></div>
      </div>
    </div>

    <!-- Reference Tab -->
    <div id="tab-reference" class="tab-pane">
      <div style="font-weight:700;margin-bottom:8px">Commercial Information</div>
      <div class="form-grid">
        <div class="form-group"><label>Sales ($ Million)</label><input id="drugSales" type="number" step="0.01" placeholder="e.g., 500" /></div>
        <div class="form-group"><label>Sales Source</label><input id="drugSalesSource" placeholder="source text or URL" /></div>
      </div>

      <div style="font-weight:700;margin-top:10px;margin-bottom:8px">News & Updates</div>
      <div class="form-grid">
        <div class="form-group"><label>News (Regulatory)</label><input id="drugNewsReg" placeholder="Latest regulatory news" /></div>
        <div class="form-group"><label>News (Commercial)</label><input id="drugNewsComm" placeholder="Latest commercial news" /></div>
        <div class="form-group"><label>News Source</label><input id="drugNewsSource" placeholder="source reference" /></div>
      </div>

      <div style="font-weight:700;margin-top:10px;margin-bottom:8px">Remarks</div>
      <div class="form-grid">
        <div class="form-group" style="grid-column: 1/-1;"><label>Additional Notes</label><textarea id="drugRemarks" placeholder="Additional notes or observations..."></textarea></div>
      </div>
    </div>

    <!-- Associated Trials Tab -->
    <div id="tab-trials" class="tab-pane" style="display:none">
      <div style="font-weight:700;margin-bottom:8px">Associated Trials</div>
      <p style="font-size:12px;color:#64748b;margin-bottom:12px">All clinical trials using this drug:</p>
      <div id="associatedTrialsList" style="max-height:300px;overflow-y:auto;border:1px solid #e6eef8;border-radius:6px;padding:8px"></div>
    </div>

    <div class="form-actions">
      <button type="button" class="btn btn-secondary" onclick="closeAllModals()">Cancel</button>
      <button type="button" class="btn btn-success" onclick="saveDrug(${drugId || 'null'})">${isEdit ? 'Update' : 'Create'} Drug</button>
    </div>
    </div>
  `;

  document.getElementById('drugFormContainer').innerHTML = html;

  // Tab switching function
  window.switchDrugTab = function(tabName) {
    // Hide all tabs
    document.querySelectorAll('[id^="tab-"]').forEach(t => {
      if (t.id.startsWith('tab-')) t.classList.remove('active');
    });
    document.querySelectorAll('.sub-tab').forEach(t => t.classList.remove('active'));
    // Show selected tab
    document.getElementById('tab-' + tabName).classList.add('active');
    event.target.classList.add('active');
  };

  if (isEdit) {
    const drugs = await getAllRecords('drugs');
    const drug = drugs.find(d => d.drug_id === drugId);
    if (drug) {
      document.getElementById('drugProduct').value = drug.product_name || '';
      document.getElementById('drugPD').value = drug.pd_l1 || '';
      document.getElementById('drugSynonyms').value = (drug.asset_synonyms || []).join(', ');
      document.getElementById('drugMOA').value = drug.moa || '';
      document.getElementById('drugPhase').value = drug.highest_phase || '';
      document.getElementById('drugSponsor').value = drug.sponsor_id || '';
      document.getElementById('drugClass').value = drug.drug_class || '';
      document.getElementById('drugTarget').value = drug.target || '';
      document.getElementById('drugModality').value = drug.modality || '';
      document.getElementById('drugDevStatus').value = drug.development_status || '';
      document.getElementById('drugAssetStatus').value = drug.asset_status || '';
      document.getElementById('drugActive').value = drug.active_inactive || 'Active';
      document.getElementById('drugMoASource').value = drug.moa_modality_source || '';
      document.getElementById('drugAssetStatusSource').value = drug.asset_status_source || '';
      document.getElementById('drugEviVitro').value = drug.evidence_vitro || '';
      document.getElementById('drugEviVivo').value = drug.evidence_vivo || '';
      document.getElementById('drugPreclinPub').value = drug.preclinical_pub_source || '';
      document.getElementById('drugSales').value = drug.sales_millions || '';
      document.getElementById('drugSalesSource').value = drug.sales_source || '';
      document.getElementById('drugUpcomingClinical').value = drug.upcoming_clinical || '';
      document.getElementById('drugUpcomingClinicalSource').value = drug.upcoming_clinical_source || '';
      document.getElementById('drugUpcomingReg').value = drug.upcoming_regulatory || '';
      document.getElementById('drugUpcomingRegSource').value = drug.upcoming_regulatory_source || '';
      document.getElementById('drugPartnerships').value = (drug.partnerships || []).join(', ');
      document.getElementById('drugNewsReg').value = drug.news_regulatory || '';
      document.getElementById('drugNewsComm').value = drug.news_commercial || '';
      document.getElementById('drugNewsSource').value = drug.news_source || '';
      document.getElementById('drugApprovedRegion').value = drug.approved_region || '';
      document.getElementById('drugRegAgency').value = drug.regulatory_agency || '';
      document.getElementById('drugRegDesignations').value = drug.regulatory_designations || '';
      document.getElementById('drugRegDesignationsSource').value = drug.regulatory_designations_source || '';
      document.getElementById('drugRemarks').value = drug.remarks || '';

      // Load and display associated trials
      const trials = await getAllRecords('trials');
      const associatedTrials = trials.filter(t => t.asset_id === drugId);
      const trialsList = document.getElementById('associatedTrialsList');
      if (associatedTrials.length > 0) {
        trialsList.innerHTML = associatedTrials.map(t => `
          <div style="padding:10px;border-bottom:1px solid #e6eef8;display:flex;justify-content:space-between;align-items:center">
            <div>
              <div style="font-weight:700;color:#0f172a">${escapeHtml(t.trial_identifier || 'N/A')}</div>
              <div style="font-size:12px;color:#64748b">${escapeHtml((t.trial_title || '').substring(0,60))}</div>
              <div style="font-size:11px;color:#9ca3af;margin-top:4px">Row Variants: ${(t.import_row_details||[]).length} | Status: ${escapeHtml(t.development_status||'Unknown')}</div>
            </div>
            <button type="button" class="btn btn-primary btn-sm" onclick="closeAllModals();setTimeout(()=>editTrial(${t.trial_id}),100)">Open Trial</button>
          </div>
        `).join('');
      } else {
        trialsList.innerHTML = '<div style="padding:10px;color:#64748b;text-align:center;font-size:12px">No trials associated with this drug yet.</div>';
      }
    }
  }

  document.getElementById('drugModal').classList.add('show');
  // render parsed references (split by | , ;) and attach URL links for direct URLs
  renderReferencesFromInputs('drugFormContainer');
  attachSourceLinks('drugFormContainer');
}

async function saveDrug(drugId) {
  const product = document.getElementById('drugProduct').value.trim();
  if (!product) {
    alert('Product name is required');
    return;
  }

  const data = {
    product_name: product,
    pd_l1: document.getElementById('drugPD').value || null,
    asset_synonyms: (document.getElementById('drugSynonyms').value || '').split(',').map(s=>s.trim()).filter(Boolean),
    drug_class: document.getElementById('drugClass').value || '',
    moa: document.getElementById('drugMOA').value || '',
    target: document.getElementById('drugTarget').value || '',
    modality: document.getElementById('drugModality').value || '',
    highest_phase: document.getElementById('drugPhase').value || '',
    development_status: document.getElementById('drugDevStatus').value || '',
    asset_status: document.getElementById('drugAssetStatus').value || '',
    active_inactive: document.getElementById('drugActive').value || 'Active',
    sponsor_id: document.getElementById('drugSponsor').value ? parseInt(document.getElementById('drugSponsor').value) : null,
    moa_modality_source: document.getElementById('drugMoASource').value || '',
    asset_status_source: document.getElementById('drugAssetStatusSource').value || '',
    evidence_vitro: document.getElementById('drugEviVitro').value || '',
    evidence_vivo: document.getElementById('drugEviVivo').value || '',
    preclinical_pub_source: document.getElementById('drugPreclinPub').value || '',
    sales_millions: document.getElementById('drugSales').value ? parseFloat(document.getElementById('drugSales').value) : null,
    sales_source: document.getElementById('drugSalesSource').value || '',
    upcoming_clinical: document.getElementById('drugUpcomingClinical').value || '',
    upcoming_clinical_source: document.getElementById('drugUpcomingClinicalSource').value || '',
    upcoming_regulatory: document.getElementById('drugUpcomingReg').value || '',
    upcoming_regulatory_source: document.getElementById('drugUpcomingRegSource').value || '',
    partnerships: (document.getElementById('drugPartnerships').value || '').split(',').map(s=>s.trim()).filter(Boolean),
    news_regulatory: document.getElementById('drugNewsReg').value || '',
    news_commercial: document.getElementById('drugNewsComm').value || '',
    news_source: document.getElementById('drugNewsSource').value || '',
    approved_region: document.getElementById('drugApprovedRegion').value || '',
    regulatory_agency: document.getElementById('drugRegAgency').value || '',
    regulatory_designations: document.getElementById('drugRegDesignations').value || '',
    regulatory_designations_source: document.getElementById('drugRegDesignationsSource').value || '',
    remarks: document.getElementById('drugRemarks').value || ''
  };

  if (drugId !== null && drugId !== 'null') {
    data.drug_id = drugId;
    await putRecord('drugs', data);
    await addLog('UPDATE', 'Drug', product);
  } else {
    data.created_at = new Date().toISOString();
    await addRecord('drugs', data);
    await addLog('CREATE', 'Drug', product);
  }

  closeAllModals();
  await refreshAllViews();
}

async function editDrug(drugId) {
  openDrugModal(drugId);
}

async function deleteDrug(drugId) {
  if (confirm('Are you sure you want to delete this drug?')) {
    await deleteRecord('drugs', drugId);
    await addLog('DELETE', 'Drug', `ID ${drugId}`);
    await refreshAllViews();
  }
}

// ============ Trial Management ============
function openTrialModal(trialId = null, rowIndex = 0) {
  const isEdit = trialId !== null;
  
  // Update modal title
  document.getElementById('trialModalTitle').textContent = isEdit ? 'Edit Trial' : 'Create New Trial';
  
  getAllRecords('drugs').then(drugs => {
    const drugOptions = '<option value="">Select Associated Drug</option>' +
      drugs.map(d => `<option value="${d.drug_id}">${escapeHtml(d.product_name)}</option>`).join('');

    let html = '';

    if (isEdit) {
      getAllRecords('trials').then(trials => {
        const trial = trials.find(t => t.trial_id === trialId);
        if (!trial) return;

        const rows = trial.import_row_details || [];
        const selectedRowIndex = Math.min(rowIndex, rows.length - 1) || 0;

        // Row selector (if multiple rows exist)
        if (rows.length > 0) {
          html += '<div class="details-row-selector" style="margin-bottom:16px" id="editRowSelector">';
          rows.forEach((r, idx) => {
            const label = `Row ${r.row_index || idx + 1}`;
            html += `<button type="button" class="details-row-btn ${idx === selectedRowIndex ? 'active' : ''}" data-row-index="${idx}" onclick="selectTrialEditRow(${idx})">${escapeHtml(label)}</button>`;
          });
          html += '</div>';
        }

        // Section navigation + content
        const slugify = (text) => String(text || '')
          .toLowerCase()
          .replace(/[^a-z0-9]+/g, '-')
          .replace(/^-+|-+$/g, '');
        
        const sectionIds = TRIAL_ROW_SECTIONS.map(section => ({
          section,
          id: slugify(section.title)
        }));

        html += '<div class="details-layout">';
        html += '<div class="details-nav">';
        html += '<div class="details-nav-title">Sections</div>';
        sectionIds.forEach((item, idx) => {
          html += `<button class="details-nav-item ${idx === 0 ? 'active' : ''}" onclick="switchTrialEditSection('${item.id}')">${escapeHtml(item.section.title)}</button>`;
        });
        html += '</div>';
        html += '<div style="flex:1;overflow-y:auto">';

        // Generate section forms
        sectionIds.forEach((item, idx) => {
          html += `<div class="details-page ${idx === 0 ? 'active' : ''}" id="edit-${item.id}">`;
          html += `<div class="trial-section"><h3>${escapeHtml(item.section.title)}</h3>`;
          html += '<div class="form-grid">';

          item.section.fields.forEach(f => {
            const fieldId = `edit_${f.key}`;
            const isBool = ['registrational'].includes(f.key);
            const isNum = ['patient_number', 'efficacy_evaluable', 'safety_evaluable', 'approval_year', 'effORR', 'effDCR', 'effCR', 'effPR', 'effSD', 'effPD', 'effmDoR', 'effPFS', 'effmPFS', 'effOS', 'effmOS', 'effDFS', 'effmDFS', 'effPFSHR', 'effOSHR', 'safEvaluable', 'safGrade3', 'safGrade3TRAEs', 'safIrAes', 'safSAE', 'safAnyGrade', 'safDeaths', 'safTxMortality'].includes(f.key);
            const isDate = ['study_start_date', 'primary_completion_date', 'study_completion_date'].includes(f.key);
            const isTextarea = ['trial_title', 'regimens', 'primary_endpoint', 'secondary_endpoint', 'included_based_on_prior', 'excluded_based_on_prior', 'efficacy_others', 'safety_others', 'reference'].includes(f.key);

            html += '<div class="form-group">';
            html += `<label>${escapeHtml(f.label)}</label>`;
            
            if (isBool) {
              html += `<select id="${fieldId}"><option value="">Select</option><option value="Y">Yes</option><option value="N">No</option></select>`;
            } else if (f.key === 'mono_combo') {
              html += `<select id="${fieldId}"><option value="">Select</option><option value="Mono">Monotherapy</option><option value="Combo">Combination</option></select>`;
            } else if (isDate) {
              html += `<input type="date" id="${fieldId}" />`;
            } else if (isNum) {
              html += `<input type="number" id="${fieldId}" step="0.01" />`;
            } else if (isTextarea) {
              html += `<textarea id="${fieldId}" rows="2"></textarea>`;
            } else {
              html += `<input type="text" id="${fieldId}" />`;
            }
            
            html += '</div>';
          });

          html += '</div></div></div>';
        });

        html += '</div></div>';

        html += '<div class="form-actions" style="margin-top:20px">';
        html += '<button type="button" class="btn btn-secondary" onclick="closeAllModals()">Cancel</button>';
        html += `<button type="button" class="btn btn-success" onclick="saveTrialModal(${trialId}, ${selectedRowIndex})">Update Trial</button>`;
        html += '</div>';

        document.getElementById('trialFormContainer').innerHTML = html;

        // Setup section switching
        window.switchTrialEditSection = function(sectionId) {
          document.querySelectorAll('.details-page').forEach(p => p.classList.remove('active'));
          const target = document.getElementById('edit-' + sectionId);
          if (target) target.classList.add('active');
          document.querySelectorAll('.details-nav-item').forEach(b => b.classList.remove('active'));
          const activeBtn = Array.from(document.querySelectorAll('.details-nav-item'))
            .find(b => (b.getAttribute('onclick') || '').indexOf(`'${sectionId}'`) !== -1);
          if (activeBtn) activeBtn.classList.add('active');
        };

        // Setup row selection
        window.trialEditRows = rows;
        window.trialEditCurrentRowIndex = selectedRowIndex;
        window.selectTrialEditRow = function(idx) {
          document.querySelectorAll('.details-row-btn').forEach(b => b.classList.remove('active'));
          const btn = document.querySelector(`.details-row-btn[data-row-index="${idx}"]`);
          if (btn) btn.classList.add('active');
          window.trialEditCurrentRowIndex = idx;
          window.populateTrialEditForm(rows[idx] || {}, drugOptions);
        };

        window.populateTrialEditForm = function(rowData, drugs_opts) {
          TRIAL_ROW_SECTIONS.forEach(section => {
            section.fields.forEach(f => {
              const fieldId = `edit_${f.key}`;
              const el = document.getElementById(fieldId);
              if (!el) return;
              const val = getTrialRowValue(rowData, f.key) || '';
              if (el.tagName === 'SELECT' || el.tagName === 'TEXTAREA' || el.tagName === 'INPUT') {
                el.value = Array.isArray(val) ? val.join('; ') : String(val || '');
              }
            });
          });
        };

        // Initial population
        window.populateTrialEditForm(rows[selectedRowIndex] || {}, drugOptions);

        attachSourceLinks('trialFormContainer');
      });
    } else {
      // Create mode (no rows)
      html = `
        <div style="display:flex;flex-direction:column;gap:10px">
        <div class="details-layout">
          <div class="details-nav">
            <div class="details-nav-title">Sections</div>
      `;

      const slugify = (text) => String(text || '')
        .toLowerCase()
        .replace(/[^a-z0-9]+/g, '-')
        .replace(/^-+|-+$/g, '');
      
      const sectionIds = TRIAL_ROW_SECTIONS.map(section => ({
        section,
        id: slugify(section.title)
      }));

      sectionIds.forEach((item, idx) => {
        html += `<button class="details-nav-item ${idx === 0 ? 'active' : ''}" onclick="switchTrialEditSection('${item.id}')">${escapeHtml(item.section.title)}</button>`;
      });

      html += '</div><div style="flex:1;overflow-y:auto">';

      sectionIds.forEach((item, idx) => {
        html += `<div class="details-page ${idx === 0 ? 'active' : ''}" id="edit-${item.id}">`;
        html += `<div class="trial-section"><h3>${escapeHtml(item.section.title)}</h3>`;
        html += '<div class="form-grid">';

        item.section.fields.forEach(f => {
          const fieldId = `edit_${f.key}`;
          const isBool = ['registrational'].includes(f.key);
          const isNum = ['patient_number', 'efficacy_evaluable', 'safety_evaluable', 'approval_year', 'effORR', 'effDCR', 'effCR', 'effPR', 'effSD', 'effPD', 'effmDoR', 'effPFS', 'effmPFS', 'effOS', 'effmOS', 'effDFS', 'effmDFS', 'effPFSHR', 'effOSHR', 'safEvaluable', 'safGrade3', 'safGrade3TRAEs', 'safIrAes', 'safSAE', 'safAnyGrade', 'safDeaths', 'safTxMortality'].includes(f.key);
          const isDate = ['study_start_date', 'primary_completion_date', 'study_completion_date'].includes(f.key);
          const isTextarea = ['trial_title', 'regimens', 'primary_endpoint', 'secondary_endpoint', 'included_based_on_prior', 'excluded_based_on_prior', 'efficacy_others', 'safety_others', 'reference'].includes(f.key);

          html += '<div class="form-group">';
          html += `<label>${escapeHtml(f.label)}</label>`;
          
          if (isBool) {
            html += `<select id="${fieldId}"><option value="">Select</option><option value="Y">Yes</option><option value="N">No</option></select>`;
          } else if (f.key === 'mono_combo') {
            html += `<select id="${fieldId}"><option value="">Select</option><option value="Mono">Monotherapy</option><option value="Combo">Combination</option></select>`;
          } else if (isDate) {
            html += `<input type="date" id="${fieldId}" />`;
          } else if (isNum) {
            html += `<input type="number" id="${fieldId}" step="0.01" />`;
          } else if (isTextarea) {
            html += `<textarea id="${fieldId}" rows="2"></textarea>`;
          } else {
            html += `<input type="text" id="${fieldId}" />`;
          }
          
          html += '</div>';
        });

        html += '</div></div></div>';
      });

      html += '</div></div>';

      html += '<div class="form-actions" style="margin-top:20px">';
      html += '<button type="button" class="btn btn-secondary" onclick="closeAllModals()">Cancel</button>';
      html += `<button type="button" class="btn btn-success" onclick="saveTrialModal(null, 0)">Create Trial</button>`;
      html += '</div></div>';

      document.getElementById('trialFormContainer').innerHTML = html;

      window.switchTrialEditSection = function(sectionId) {
        document.querySelectorAll('.details-page').forEach(p => p.classList.remove('active'));
        const target = document.getElementById('edit-' + sectionId);
        if (target) target.classList.add('active');
        document.querySelectorAll('.details-nav-item').forEach(b => b.classList.remove('active'));
        const activeBtn = Array.from(document.querySelectorAll('.details-nav-item'))
          .find(b => (b.getAttribute('onclick') || '').indexOf(`'${sectionId}'`) !== -1);
        if (activeBtn) activeBtn.classList.add('active');
      };

      attachSourceLinks('trialFormContainer');
    }

    document.getElementById('trialModal').classList.add('show');
  });
}


function addMultiEntry(listId, value = '') {
  const list = document.getElementById(listId);
  if (!list) return;
  const placeholder = list.getAttribute('data-placeholder') || '';
  const item = document.createElement('div');
  item.className = 'multi-entry-item';
  const input = document.createElement('input');
  input.type = 'text';
  input.value = value || '';
  input.placeholder = placeholder;
  const add = document.createElement('button');
  add.type = 'button';
  add.className = 'btn btn-secondary btn-sm';
  add.textContent = '+';
  add.onclick = () => addMultiEntry(listId);
  const remove = document.createElement('button');
  remove.type = 'button';
  remove.className = 'btn btn-danger btn-sm';
  remove.textContent = '×';
  remove.onclick = () => item.remove();
  item.appendChild(input);
  item.appendChild(add);
  item.appendChild(remove);
  list.appendChild(item);
}

function renderMultiEntry(listId, values) {
  const list = document.getElementById(listId);
  if (!list) return;
  list.innerHTML = '';
  const vals = (values && values.length) ? values : [''];
  vals.forEach(v => addMultiEntry(listId, v));
}

function collectMultiEntryValues(listId) {
  const list = document.getElementById(listId);
  if (!list) return [];
  const values = Array.from(list.querySelectorAll('input'))
    .map(i => i.value.trim())
    .filter(Boolean);
  return Array.from(new Set(values));
}

async function saveTrialModal(trialId, rowIndex) {
  const identifier = document.getElementById('edit_trial_identifier').value.trim();
  if (!identifier) {
    alert('Trial Identifier is required');
    return;
  }

  const getFormValue = (key) => {
    const el = document.getElementById(`edit_${key}`);
    if (!el) return null;
    const val = el.value;
    if (!val) return null;
    
    // Parse based on field type
    if (['patient_number', 'efficacy_evaluable', 'safety_evaluable', 'approval_year', 'safDeaths'].includes(key)) {
      return parseInt(val) || null;
    }
    if (['orr', 'dcr', 'cr', 'pr', 'sd', 'pd', 'mdor_months', 'pfs_months', 'mpfs_months', 'os_months', 'mos_months', 'dfs', 'mdfs_months', 'pfs_hr', 'os_hr', 'grade_3_ae', 'grade_3_traes', 'immune_related_aes', 'sae', 'any_grade_aes', 'tx_related_mortality'].includes(key)) {
      return parseFloat(val) || null;
    }
    if (['regimens'].includes(key)) {
      return val.split(',').map(s => s.trim()).filter(Boolean);
    }
    if (['tumors', 'mutations', 'prospective_biomarkers'].includes(key)) {
      return Array.isArray(val) ? val : [val].filter(Boolean);
    }
    return val || null;
  };

  let existingTrial = null;
  if (trialId !== null && trialId !== 'null') {
    const trials = await getAllRecords('trials');
    existingTrial = trials.find(t => t.trial_id === trialId) || null;
  }

  // Collect all field values from the form
  const rowData = {};
  TRIAL_ROW_SECTIONS.forEach(section => {
    section.fields.forEach(f => {
      const val = getFormValue(f.key);
      if (val !== null) {
        rowData[f.key] = val;
      }
    });
  });

  if (trialId !== null && trialId !== 'null' && existingTrial) {
    // Update edit mode: update the specific row in import_row_details
    const rows = existingTrial.import_row_details || [];
    if (rowIndex >= 0 && rowIndex < rows.length) {
      rows[rowIndex] = { ...rows[rowIndex], ...rowData };
    }

    // Update the main trial fields with the selected row's data
    const updatedTrial = { ...existingTrial, ...rowData, import_row_details: rows };
    updatedTrial.trial_id = trialId;
    
    await putRecord('trials', updatedTrial);
    await addLog('UPDATE', 'Trial', identifier);
  } else {
    // Create mode: new trial with single row
    const newTrial = {
      trial_identifier: identifier,
      created_at: new Date().toISOString(),
      ...rowData,
      cohorts: [],
      import_row_details: [{
        ...rowData,
        row_index: 1
      }]
    };
    
    await addRecord('trials', newTrial);
    await addLog('CREATE', 'Trial', identifier);
  }

  closeAllModals();
  await refreshAllViews();
}

async function saveTrial(trialId) {
  const identifier = document.getElementById('trialIdentifier').value.trim();
  if (!identifier) {
    alert('Trial Identifier is required');
    return;
  }

  let existingTrial = null;
  if (trialId !== null && trialId !== 'null') {
    const trials = await getAllRecords('trials');
    existingTrial = trials.find(t => t.trial_id === trialId) || null;
  }

  const splitValues = (val) => {
    return Array.from(new Set(String(val || '')
      .split(/[\n,;|]+/)
      .map(s => s.trim())
      .filter(Boolean)));
  };
  const tumorGroupValues = collectMultiEntryValues('trialTumorGroupList');
  const tumorValues = collectMultiEntryValues('trialTumorList');
  const mutationValues = collectMultiEntryValues('trialMutationList');
  const prospectiveValues = collectMultiEntryValues('trialProspectiveList');
  const stageValues = collectMultiEntryValues('trialStageList');
  const lineValues = collectMultiEntryValues('trialLineList');


  const data = {
    trial_identifier: identifier,
    trial_title: document.getElementById('trialTitle').value || '',
    trial_acronym: document.getElementById('trialAcronym').value || '',
    trial_design: document.getElementById('trialDesign').value || '',
    development_status: document.getElementById('trialDevStatus').value || '',
    recruitment_status: document.getElementById('trialRecruitment').value || '',
    registrational: document.getElementById('trialRegistrational').value || '',
    tumor_group: tumorGroupValues.join(' | '),
    tumors: tumorValues,
    patient_age_group: document.getElementById('trialAgeGroup').value || '',
    patient_number: document.getElementById('trialPatientNumber').value ? parseInt(document.getElementById('trialPatientNumber').value) : null,
    study_start_date: document.getElementById('trialStartDate').value || '',
    primary_completion_date: document.getElementById('trialPrimaryCompletion').value || '',
    study_completion_date: document.getElementById('trialStudyCompletion').value || '',
    mono_combo: document.getElementById('trialMonoCombo').value || '',
    experimental_arm: document.getElementById('trialExpArm').value || '',
    regimens: (document.getElementById('trialRegimens').value || '').split(',').map(s=>s.trim()).filter(Boolean),
    regimen_moa: document.getElementById('trialRegimenMoA').value || '',
    comparator_arm: document.getElementById('trialComparator').value || '',
    primary_endpoint: document.getElementById('trialPrimaryEndpoint').value || '',
    secondary_endpoint: document.getElementById('trialSecondaryEndpoint').value || '',
    stage: stageValues.join(' | '),
    line_of_therapy: lineValues.join(' | '),
    mutations: mutationValues,
    prospective_biomarkers: prospectiveValues,
    retrospective_biomarkers: existingTrial?.retrospective_biomarkers || [],
    dx_assay: document.getElementById('trialDxAssay').value || '',
    dx_cutoff_value: document.getElementById('trialDxCutoff').value || '',
    included_based_on_prior: document.getElementById('trialIncludedPrior').value || '',
    excluded_based_on_prior: document.getElementById('trialExcludedPrior').value || '',
    patients_brain_cns_mets: document.getElementById('trialBrainMets').value || '',
    sponsor: document.getElementById('trialSponsor').value || '',
    collaborator: document.getElementById('trialCollaborator').value || '',
    location: document.getElementById('trialLocation').value || '',
    region: document.getElementById('trialRegion').value || '',
    clinical_failure: document.getElementById('trialClinicalFailure').value || '',
    trials_with_results: document.getElementById('trialWithResults').value || '',
    regulatory_agency: document.getElementById('trialRegAgency').value || '',
    approved_region: document.getElementById('trialApprovedRegion').value || '',
    approval_year: document.getElementById('trialApprovalYear').value ? parseInt(document.getElementById('trialApprovalYear').value) : null,
    regulatory_designation: document.getElementById('trialRegDesignation').value || '',
    data_review_committee: document.getElementById('effDataReviewCommittee').value || '',
    study_identifier_source: document.getElementById('trialStudyIdentifierSource').value || '',
    registrational_trial_source: document.getElementById('trialRegistrationalSource').value || '',
    clinical_failure_source: document.getElementById('trialClinicalFailureSource').value || '',
    approval_labels_source: document.getElementById('trialApprovalLabelsSource').value || '',
    regulatory_designations_source: document.getElementById('trialRegDesignationsSource').value || '',
    reference: document.getElementById('trialReferenceNotes').value || '',
    // Efficacy
    efficacy_evaluable: document.getElementById('effEvaluable').value ? parseInt(document.getElementById('effEvaluable').value) : null,
    orr: document.getElementById('effORR').value ? parseFloat(document.getElementById('effORR').value) : null,
    dcr: document.getElementById('effDCR').value ? parseFloat(document.getElementById('effDCR').value) : null,
    cr: document.getElementById('effCR').value ? parseFloat(document.getElementById('effCR').value) : null,
    pr: document.getElementById('effPR').value ? parseFloat(document.getElementById('effPR').value) : null,
    sd: document.getElementById('effSD').value ? parseFloat(document.getElementById('effSD').value) : null,
    pd: document.getElementById('effPD').value ? parseFloat(document.getElementById('effPD').value) : null,
    mdor_months: document.getElementById('effmDoR').value ? parseFloat(document.getElementById('effmDoR').value) : null,
    pfs_months: document.getElementById('effPFS').value ? parseFloat(document.getElementById('effPFS').value) : null,
    mpfs_months: document.getElementById('effmPFS').value ? parseFloat(document.getElementById('effmPFS').value) : null,
    os_months: document.getElementById('effOS').value ? parseFloat(document.getElementById('effOS').value) : null,
    mos_months: document.getElementById('effmOS').value ? parseFloat(document.getElementById('effmOS').value) : null,
    dfs: document.getElementById('effDFS').value ? parseFloat(document.getElementById('effDFS').value) : null,
    mdfs_months: document.getElementById('effmDFS').value ? parseFloat(document.getElementById('effmDFS').value) : null,
    pfs_hr: document.getElementById('effPFSHR').value ? parseFloat(document.getElementById('effPFSHR').value) : null,
    os_hr: document.getElementById('effOSHR').value ? parseFloat(document.getElementById('effOSHR').value) : null,
    efficacy_others: document.getElementById('effOthers').value || '',
    publication_source_clinical: document.getElementById('effPubSource').value || '',
    // Safety
    safety_evaluable: document.getElementById('safEvaluable').value ? parseInt(document.getElementById('safEvaluable').value) : null,
    grade_3_ae: document.getElementById('safGrade3').value ? parseFloat(document.getElementById('safGrade3').value) : null,
    grade_3_traes: document.getElementById('safGrade3TRAEs').value ? parseFloat(document.getElementById('safGrade3TRAEs').value) : null,
    immune_related_aes: document.getElementById('safIrAes').value ? parseFloat(document.getElementById('safIrAes').value) : null,
    sae: document.getElementById('safSAE').value ? parseFloat(document.getElementById('safSAE').value) : null,
    any_grade_aes: document.getElementById('safAnyGrade').value ? parseFloat(document.getElementById('safAnyGrade').value) : null,
    deaths: document.getElementById('safDeaths').value ? parseInt(document.getElementById('safDeaths').value) : null,
    tx_related_mortality: document.getElementById('safTxMortality').value ? parseFloat(document.getElementById('safTxMortality').value) : null,
    safety_others: document.getElementById('safOthers').value || '',
    publication_source_safety: document.getElementById('safPubSource').value || '',
    asset_id: document.getElementById('trialDrugId').value ? parseInt(document.getElementById('trialDrugId').value) : null,
    cohorts: existingTrial?.cohorts || [],
    import_row_details: existingTrial?.import_row_details || [],
    tumor_group_values: tumorGroupValues,
    stage_values: stageValues,
    line_of_therapy_values: lineValues,
    included_based_on_prior_values: existingTrial?.included_based_on_prior_values || splitValues(document.getElementById('trialIncludedPrior').value),
    excluded_based_on_prior_values: existingTrial?.excluded_based_on_prior_values || splitValues(document.getElementById('trialExcludedPrior').value)
  };

  if (trialId !== null && trialId !== 'null') {
    data.trial_id = trialId;
    await putRecord('trials', data);
    await addLog('UPDATE', 'Trial', identifier);
  } else {
    data.created_at = new Date().toISOString();
    await addRecord('trials', data);
    await addLog('CREATE', 'Trial', identifier);
  }

  closeAllModals();
  await refreshAllViews();
}

async function editTrial(trialId) {
  openTrialModal(trialId);
}

async function deleteTrial(trialId) {
  if (confirm('Are you sure you want to delete this trial?')) {
    await deleteRecord('trials', trialId);
    await addLog('DELETE', 'Trial', `ID ${trialId}`);
    await refreshAllViews();
  }
}

const TRIAL_ROW_SECTIONS = [
  {
    title: 'Summary',
    fields: [
      { key: 'trial_identifier', label: 'Trial Identifier' },
      { key: 'trial_title', label: 'Trial Title' },
      { key: 'trial_acronym', label: 'Trial Acronym' },
      { key: 'trial_design', label: 'Trial Design' },
      { key: 'development_status', label: 'Development Status' },
      { key: 'recruitment_status', label: 'Recruitment Status' },
      { key: 'registrational', label: 'Registrational' },
      { key: 'tumor_group', label: 'Tumor Group' },
      { key: 'tumors', label: 'Tumor/Condition' },
      { key: 'patient_age_group', label: 'Patient Age Group' },
      { key: 'patient_number', label: 'Patient Number (N)' },
      { key: 'study_start_date', label: 'Study Start Date' },
      { key: 'primary_completion_date', label: 'Primary Completion Date' },
      { key: 'study_completion_date', label: 'Study Completion Date' },
      { key: 'stage', label: 'Stage' },
      { key: 'line_of_therapy', label: 'Line of Therapy' },
      { key: 'mutations', label: 'Mutations' },
      { key: 'prospective_biomarkers', label: 'Biomarkers' },
      { key: 'dx_assay', label: 'Dx Assay' },
      { key: 'dx_cutoff_value', label: 'Dx Cutoff Value' },
      { key: 'sponsor', label: 'Sponsor' },
      { key: 'collaborator', label: 'Collaborator' },
      { key: 'location', label: 'Location' },
      { key: 'region', label: 'Region' },
      { key: 'clinical_failure', label: 'Clinical Failure' },
      { key: 'trials_with_results', label: 'Trials With Results' }
    ]
  },
  {
    title: 'Arms & Regimen',
    fields: [
      { key: 'mono_combo', label: 'Mono/Combo' },
      { key: 'experimental_arm', label: 'Experimental Arm' },
      { key: 'regimens', label: 'Regimen/Dosing' },
      { key: 'regimen_moa', label: 'Regimen MoA' },
      { key: 'comparator_arm', label: 'Comparator Arm' }
    ]
  },
  {
    title: 'Outcomes',
    fields: [
      { key: 'primary_endpoint', label: 'Primary Endpoint' },
      { key: 'secondary_endpoint', label: 'Secondary Endpoint' }
    ]
  },
  {
    title: 'Inclusion / Exclusion',
    fields: [
      { key: 'included_based_on_prior', label: 'Included Prior Tx' },
      { key: 'excluded_based_on_prior', label: 'Excluded Prior Tx' },
      { key: 'patients_brain_cns_mets', label: 'Brain/CNS Mets' }
    ]
  },
  {
    title: 'Efficacy',
    fields: [
      { key: 'efficacy_evaluable', label: 'Evaluable Patients' },
      { key: 'data_review_committee', label: 'Data Review Committee' },
      { key: 'orr', label: 'ORR (%)' },
      { key: 'dcr', label: 'DCR (%)' },
      { key: 'cr', label: 'CR (%)' },
      { key: 'pr', label: 'PR (%)' },
      { key: 'sd', label: 'SD (%)' },
      { key: 'pd', label: 'PD (%)' },
      { key: 'mdor_months', label: 'mDoR (months)' },
      { key: 'pfs_months', label: 'PFS (months)' },
      { key: 'mpfs_months', label: 'mPFS (months)' },
      { key: 'os_months', label: 'OS (months)' },
      { key: 'mos_months', label: 'mOS (months)' },
      { key: 'dfs', label: 'DFS' },
      { key: 'mdfs_months', label: 'mDFS (months)' },
      { key: 'pfs_hr', label: 'PFS (HR)' },
      { key: 'os_hr', label: 'OS (HR)' },
      { key: 'efficacy_others', label: 'Others (Efficacy)' },
      { key: 'publication_source_clinical', label: 'Publication Source (Clinical)' }
    ]
  },
  {
    title: 'Safety',
    fields: [
      { key: 'safety_evaluable', label: 'Evaluable Patients' },
      { key: 'grade_3_ae', label: 'Grade >=3 AEs (%)' },
      { key: 'grade_3_traes', label: 'Grade >=3 TRAEs (%)' },
      { key: 'immune_related_aes', label: 'Immune Related AEs (%)' },
      { key: 'sae', label: 'SAE (%)' },
      { key: 'any_grade_aes', label: 'Any Grade AEs (%)' },
      { key: 'deaths', label: 'No. of Deaths' },
      { key: 'tx_related_mortality', label: 'Tx Related Mortality (%)' },
      { key: 'safety_others', label: 'Others (Safety)' },
      { key: 'publication_source_safety', label: 'Publication Source (Safety)' }
    ]
  },
  {
    title: 'Regulatory',
    fields: [
      { key: 'approved_region', label: 'Approved Region' },
      { key: 'regulatory_agency', label: 'Regulatory Agency' },
      { key: 'approval_year', label: 'Approval Year' },
      { key: 'regulatory_designation', label: 'Regulatory Designation' }
    ]
  },
  {
    title: 'References',
    fields: [
      { key: 'study_identifier_source', label: 'Study Identifier Source' },
      { key: 'registrational_trial_source', label: 'Registrational Trial Source' },
      { key: 'clinical_failure_source', label: 'Clinical Failure Source' },
      { key: 'approval_labels_source', label: 'Approval Labels Source' },
      { key: 'regulatory_designations_source', label: 'Regulatory Designations Source' },
      { key: 'reference', label: 'Reference Notes' }
    ]
  }
];

const TRIAL_ROW_FIELDS = TRIAL_ROW_SECTIONS.flatMap(section => section.fields.map(f => f.key));

function getTrialRowValue(row, key) {
  if (!row) return '';
  if (row.fields && Object.prototype.hasOwnProperty.call(row.fields, key)) return row.fields[key];
  return row[key];
}

function formatTrialRowValue(val) {
  if (Array.isArray(val)) return val.join('; ');
  if (val === null || val === undefined) return '';
  return String(val);
}

function openTrialDetails(trialId) {
  getAllRecords('trials').then(trials => {
    const trial = trials.find(t => t.trial_id === trialId);
    if (!trial) return;

    const rows = trial.import_row_details || [];
    let html = '';
    if (!rows.length) {
      html = '<div class="alert alert-info">No imported row details found for this trial.</div>';
    } else {
      html += '<div style="margin-bottom:10px;color:#475569;font-size:12px">Select a row version to view all fields. Differences are highlighted vs Row 1.</div>';
      const base = rows[0] || {};
      const slugify = (text) => String(text || '')
        .toLowerCase()
        .replace(/[^a-z0-9]+/g, '-')
        .replace(/^-+|-+$/g, '');
      const sectionIds = TRIAL_ROW_SECTIONS.map(section => ({
        section,
        id: slugify(section.title)
      }));

      html += '<div class="details-row-selector" id="detailsRowSelector">';
      rows.forEach((r, idx) => {
        const label = `Row ${r.row_index || idx + 1}`;
        html += `<button type="button" class="details-row-btn ${idx === 0 ? 'active' : ''}" data-row-index="${idx}" onclick="selectTrialDetailsRow(${idx})">${escapeHtml(label)}</button>`;
      });
      html += '</div>';

      html += '<div class="details-layout">';
      html += '<div class="details-nav">';
      html += '<div class="details-nav-title">Sections</div>';
      sectionIds.forEach((item, idx) => {
        html += `<button class="details-nav-item ${idx === 0 ? 'active' : ''}" onclick="switchTrialDetailsSection('${item.id}')">${escapeHtml(item.section.title)}</button>`;
      });
      html += '</div>';
      html += '<div>';

      sectionIds.forEach((item, idx) => {
        html += `<div class="details-page ${idx === 0 ? 'active' : ''}" id="details-${item.id}">`;
        html += `<div class="details-section">
          <div class="details-body">
            <div class="table-wrapper"><table>
              <thead><tr>
                <th>Field</th>
                <th>Value</th>
              </tr></thead>
              <tbody id="details-body-${item.id}"></tbody>
            </table></div>
          </div>
        </div>
        </div>`;
      });

      html += '</div></div>';
    }

    const titleBase = `Trial Details: ${trial.trial_identifier || trial.trial_id}`;
    document.getElementById('trialDetailsTitle').textContent = titleBase;
    document.getElementById('trialDetailsContainer').innerHTML = html;
    document.getElementById('trialDetailsModal').classList.add('show');
    window.switchTrialDetailsSection = function(sectionId) {
      document.querySelectorAll('.details-page').forEach(p => p.classList.remove('active'));
      const target = document.getElementById('details-' + sectionId);
      if (target) target.classList.add('active');
      document.querySelectorAll('.details-nav-item').forEach(b => b.classList.remove('active'));
      const activeBtn = Array.from(document.querySelectorAll('.details-nav-item'))
        .find(b => (b.getAttribute('onclick') || '').indexOf(`'${sectionId}'`) !== -1);
      if (activeBtn) activeBtn.classList.add('active');
    };
    window.trialDetailsRows = rows;
    window.trialDetailsBaseRow = rows[0] || {};
    window.trialDetailsSectionIds = TRIAL_ROW_SECTIONS.map(section => ({
      section,
      id: String(section.title || '')
        .toLowerCase()
        .replace(/[^a-z0-9]+/g, '-')
        .replace(/^-+|-+$/g, '')
    }));
    window.renderTrialDetailsRow = function(rowIndex) {
      const row = window.trialDetailsRows[rowIndex] || {};
      const baseRow = window.trialDetailsBaseRow || {};
      window.trialDetailsSectionIds.forEach(item => {
        const tbody = document.getElementById('details-body-' + item.id);
        if (!tbody) return;
        let bodyHtml = '';
        item.section.fields.forEach(f => {
          const rowVal = formatTrialRowValue(getTrialRowValue(row, f.key));
          const baseVal = formatTrialRowValue(getTrialRowValue(baseRow, f.key));
          const isDiff = rowVal && rowVal !== baseVal;
          const displayVal = rowVal || '';
          bodyHtml += `<tr><td>${escapeHtml(f.label)}</td><td class="${isDiff ? 'diff-cell' : ''}">${escapeHtml(displayVal)}</td></tr>`;
        });
        tbody.innerHTML = bodyHtml || '<tr><td colspan="2" style="color:#64748b;font-size:12px">No data</td></tr>';
      });
    };
    window.selectTrialDetailsRow = function(rowIndex) {
      document.querySelectorAll('.details-row-btn').forEach(b => b.classList.remove('active'));
      const btn = document.querySelector(`.details-row-btn[data-row-index="${rowIndex}"]`);
      if (btn) btn.classList.add('active');
      const row = window.trialDetailsRows[rowIndex] || {};
      const rowLabel = row.row_index || rowIndex + 1;
      const titleEl = document.getElementById('trialDetailsTitle');
      if (titleEl) titleEl.textContent = `${titleBase} | Row ${rowLabel}`;
      window.renderTrialDetailsRow(rowIndex);
    };
    window.selectTrialDetailsRow(0);
  });
}

// ============ Excel Import ============
let excelHeaders = [];
let excelData = [];

async function handleExcelUpload() {
  const file = document.getElementById('excelInput').files[0];
  if (!file) {
    alert('Please select an Excel file');
    return;
  }

  const reader = new FileReader();
  reader.onload = (e) => {
    try {
      const data = new Uint8Array(e.target.result);
      const workbook = XLSX.read(data, { type: 'array' });
      const worksheet = workbook.Sheets[workbook.SheetNames[0]];
      const rows = XLSX.utils.sheet_to_json(worksheet, { header: 1, defval: '' });

      if (!rows || rows.length < 2) {
        alert('Excel file must have headers and at least one data row');
        return;
      }

      excelHeaders = rows[0].map(h => String(h || '').trim());
      excelData = rows.slice(1).filter(r => r.some(cell => cell !== null && cell !== ''));

      document.getElementById('importStatus').innerHTML = `<div class="alert alert-info">Loaded ${excelData.length} rows. Mapping columns below...</div>`;
      showMappingUI();
    } catch (err) {
      alert('Error reading Excel: ' + err.message);
    }
  };
  reader.readAsArrayBuffer(file);
}

function showMappingUI() {
  const fields = {
    'DRUG - General': [
      'product_name', 'pd_l1', 'asset_synonyms', 'drug_class', 'moa', 'target', 'modality',
      'highest_phase', 'development_status', 'asset_status', 'active_inactive', 'sponsor'
    ],
    'DRUG - Sources & Preclinical': [
      'moa_modality_source', 'asset_status_source', 'evidence_vitro', 'evidence_vivo', 'preclinical_pub_source'
    ],
    'DRUG - Sales & Regulatory': [
      'sales_millions', 'sales_source', 'upcoming_clinical', 'upcoming_clinical_source',
      'upcoming_regulatory', 'upcoming_regulatory_source', 'partnerships', 'news_regulatory',
      'news_commercial', 'news_source', 'approved_region', 'regulatory_agency',
      'regulatory_designations', 'regulatory_designations_source'
    ],
    'TRIAL - Summary': [
      'trial_identifier', 'trial_title', 'trial_acronym', 'trial_design', 'development_status',
      'recruitment_status', 'registrational', 'tumor_group', 'tumors', 'patient_age_group',
      'patient_number', 'study_start_date', 'primary_completion_date', 'study_completion_date',
      'stage', 'line_of_therapy', 'mutations', 'prospective_biomarkers',
      'dx_assay', 'dx_cutoff_value', 'sponsor', 'collaborator', 'location', 'region',
      'clinical_failure', 'trials_with_results'
    ],
    'TRIAL - ARM': [
      'mono_combo', 'experimental_arm', 'regimens', 'regimen_moa', 'comparator_arm'
    ],
    'TRIAL - Outcomes': [
      'primary_endpoint', 'secondary_endpoint'
    ],
    'TRIAL - Inclusion/Exclusion': [
      'included_based_on_prior', 'excluded_based_on_prior', 'patients_brain_cns_mets'
    ],
    'TRIAL - Efficacy': [
      'efficacy_evaluable', 'data_review_committee', 'orr', 'dcr', 'cr', 'pr', 'sd', 'pd',
      'mdor_months', 'pfs_months', 'mpfs_months', 'os_months', 'mos_months', 'dfs',
      'mdfs_months', 'pfs_hr', 'os_hr', 'efficacy_others', 'publication_source_clinical'
    ],
    'TRIAL - Safety': [
      'safety_evaluable', 'grade_3_ae', 'grade_3_traes', 'immune_related_aes', 'sae',
      'any_grade_aes', 'deaths', 'tx_related_mortality', 'safety_others', 'publication_source_safety'
    ],
    'TRIAL - Regulatory': [
      'approved_region', 'regulatory_agency', 'approval_year', 'regulatory_designation'
    ],
    'TRIAL - Reference': [
      'study_identifier_source', 'registrational_trial_source', 'clinical_failure_source',
      'approval_labels_source', 'regulatory_designations_source', 'reference'
    ]
  };

  let html = `
    <h3>Map Excel Columns to Database Fields</h3>
    <div style="display:flex;align-items:center;gap:8px;margin-bottom:8px">
      <input id="mappingSearch" placeholder="Search fields..." style="flex:1;padding:6px;border:1px solid #e6eef8;border-radius:4px" oninput="filterMappingFields(this.value)" />
      <button type="button" class="btn btn-sm" onclick="autoMapColumns()">Auto-map</button>
      <button type="button" class="btn btn-sm btn-secondary" onclick="clearMappings()">Clear</button>
    </div>
    <div style="max-height: 600px; overflow-y: auto; border: 1px solid #e6eef8; border-radius: 6px; padding: 12px;">
  `;

  // Iterate through field categories
  Object.keys(fields).forEach(category => {
    html += `<div style="font-weight: 700; margin-top: 12px; margin-bottom: 8px; background: #f1f5f9; padding: 8px; border-radius: 4px;">${category}</div>`;
    html += '<div class="mapping-grid" style="max-height: none; border: none; padding: 0; margin-bottom: 12px;">';

    fields[category].forEach(field => {
      const suggestion = excelHeaders.find(h => h.toLowerCase().includes(field.replace(/_/g, ' ')) ||
        h.toLowerCase().includes(field)) || '';
      const selectedIdx = suggestion ? excelHeaders.indexOf(suggestion) : -1;

      html += `<div class="mapping-row" style="display:flex;align-items:center;gap:8px;padding:6px;">
        <div style="width:40%;font-size:12px;color:#0f172a">${field}</div>
        <div style="flex:1;">
          <select id="map_${field}" class="mapping-select" style="width:100%;font-size:12px;padding:6px;border:1px solid #e6eef8;border-radius:4px;">
            <option value="-1">-- Skip --</option>
            ${excelHeaders.map((h, i) => `<option value="${i}" ${selectedIdx === i ? 'selected' : ''}>${h}</option>`).join('')}
          </select>
        </div>
      </div>`;
    });

    html += '</div>';
  });

  html += '</div>';
  html += '<div style="margin-top: 16px; display: flex; gap: 8px;">';
  html += '<button class="btn btn-primary" onclick="processExcelImport()">Import Mapped Data</button>';
  html += '<button class="btn btn-secondary" onclick="cancelImport()">Cancel</button>';
  html += '</div>';

  document.getElementById('mappingPreview').innerHTML = html;

  // Helper functions for auto-mapping
  function _norm(s){ return String(s||'').toLowerCase().replace(/[^a-z0-9 ]+/g,' ').replace(/\s+/g,' ').trim(); }

  function bestMatchIndexForFieldName(fieldName){
    if(!excelHeaders || !excelHeaders.length) return -1;
    const target = _norm(fieldName);
    for(let i=0;i<excelHeaders.length;i++){ if(_norm(excelHeaders[i])===target) return i; }
    for(let i=0;i<excelHeaders.length;i++){ const h=_norm(excelHeaders[i]); if(h.includes(target) || target.includes(h)) return i; }
    const toks = new Set(target.split(' ')); let best=-1, bestScore=0;
    for(let i=0;i<excelHeaders.length;i++){ const h=_norm(excelHeaders[i]); const hT=h.split(' '); let score=0; hT.forEach(t=>{ if(toks.has(t)) score++; }); if(score>bestScore){ bestScore=score; best=i; } }
    if(bestScore>0) return best;
    for(let i=0;i<excelHeaders.length;i++){ const h=_norm(excelHeaders[i]); const parts=h.split(' '); for(const p of parts){ if(target.includes(p) || (p && target.indexOf(p)!==-1)) return i; } }
    return -1;
  }

  window.autoMapColumns = function(){
    document.querySelectorAll('select[id^="map_"]').forEach(sel=>{
      const field = sel.id.replace('map_','');
      const idx = bestMatchIndexForFieldName(field);
      if(idx>=0){ sel.value = String(idx); }
    });
  };

  window.clearMappings = function(){
    document.querySelectorAll('select[id^="map_"]').forEach(sel=> sel.value='-1');
  };

  window.filterMappingFields = function(q){
    q = String(q||'').toLowerCase().trim();
    document.querySelectorAll('.mapping-row').forEach((r, idx)=>{
      if(idx===0) return; // header
      const label = (r.querySelector('div')?.textContent||'').toLowerCase();
      r.style.display = (!q || label.indexOf(q)!==-1) ? 'flex' : 'none';
    });
  };
}

async function processExcelImport() {
  // Read mapping selections
  const getColumnIndex = (field) => {
    const select = document.getElementById('map_' + field);
    return select ? parseInt(select.value) : -1;
  };

  // Build mapping object with all fields
  const allFields = [
    // Drug fields
    'product_name', 'pd_l1', 'asset_synonyms', 'drug_class', 'moa', 'target', 'modality',
    'highest_phase', 'development_status', 'asset_status', 'active_inactive', 'sponsor',
    'moa_modality_source', 'asset_status_source', 'evidence_vitro', 'evidence_vivo', 'preclinical_pub_source',
    'sales_millions', 'sales_source', 'upcoming_clinical', 'upcoming_clinical_source',
    'upcoming_regulatory', 'upcoming_regulatory_source', 'partnerships', 'news_regulatory',
    'news_commercial', 'news_source', 'approved_region', 'regulatory_agency',
    'regulatory_designations', 'regulatory_designations_source',
    // Trial fields
    'trial_identifier', 'trial_title', 'trial_acronym', 'trial_design', 'development_status', 'recruitment_status',
    'registrational', 'tumor_group', 'tumors', 'patient_age_group', 'patient_number',
    'study_start_date', 'primary_completion_date', 'study_completion_date', 'stage',
    'line_of_therapy', 'mutations', 'prospective_biomarkers',
    'dx_assay', 'dx_cutoff_value', 'sponsor', 'collaborator', 'location', 'region',
    'clinical_failure', 'trials_with_results', 'mono_combo', 'experimental_arm', 'regimens',
    'regimen_moa', 'comparator_arm', 'primary_endpoint', 'secondary_endpoint',
    'included_based_on_prior', 'excluded_based_on_prior', 'patients_brain_cns_mets',
    'approved_region', 'regulatory_agency', 'approval_year', 'regulatory_designation',
    'reference', 'study_identifier_source', 'registrational_trial_source',
    'clinical_failure_source', 'approval_labels_source', 'regulatory_designations_source',
    'data_review_committee',
    // Efficacy/Safety
    'efficacy_evaluable', 'orr', 'dcr', 'cr', 'pr', 'sd', 'pd', 'mdor_months', 'pfs_months',
    'mpfs_months', 'os_months', 'mos_months', 'dfs', 'mdfs_months', 'pfs_hr', 'os_hr',
    'efficacy_others', 'publication_source_clinical', 'safety_evaluable', 'grade_3_ae',
    'grade_3_traes', 'immune_related_aes', 'sae', 'any_grade_aes', 'deaths',
    'tx_related_mortality', 'safety_others', 'publication_source_safety'
  ];

  const mapping = {};
  allFields.forEach(f => mapping[f] = getColumnIndex(f));

  // Get existing data
  const companies = await getAllRecords('companies');
  const drugs = await getAllRecords('drugs');
  const companyMap = {};
  const drugMap = {};
  companies.forEach(c => companyMap[(c.company_name || '').toLowerCase().trim()] = c.company_id);
  drugs.forEach(d => drugMap[(d.product_name || '').toLowerCase().trim()] = d.drug_id);

  // Helper to get value from row by field mapping
  const getValue = (row, field) => {
    const idx = mapping[field];
    return idx >= 0 ? row[idx] : null;
  };

  const parseNum = (val) => val ? parseFloat(val) : null;
  const parseInt_ = (val) => val ? parseInt(val) : null;
  const parseArray = (val) => val ? String(val).split(',').map(s => s.trim()).filter(Boolean) : [];

  // Process rows: group by trial_identifier only
  const existingTrials = await getAllRecords('trials');
  const existingTrialMap = {};
  existingTrials.forEach(t => {
    const key = (t.trial_identifier || '').toString().trim().toUpperCase();
    if (key) existingTrialMap[key] = t;
  });

  const trialGroups = {};
  for (const row of excelData) {
    const product = (getValue(row, 'product_name') || '').toString().trim();
    const trialId = (getValue(row, 'trial_identifier') || '').toString().trim();

    if (!trialId) continue; // Skip invalid rows

    // Get or create company
    let sponsorId = null;
    const sponsorName = (getValue(row, 'sponsor') || '').toString().trim();
    if (sponsorName) {
      const key = sponsorName.toLowerCase();
      if (companyMap[key]) {
        sponsorId = companyMap[key];
      } else {
        sponsorId = await addRecord('companies', {
          company_name: sponsorName,
          company_type: 'Pharmaceutical',
          created_at: new Date().toISOString()
        });
        companyMap[key] = sponsorId;
      }
    }

    // Get or create drug with all fields
    let drugId = null;
    if (product) {
      const pkey = product.toLowerCase();
      if (drugMap[pkey]) {
        drugId = drugMap[pkey];
      } else {
        const drugData = {
          product_name: product,
          pd_l1: getValue(row, 'pd_l1') || null,
          asset_synonyms: parseArray(getValue(row, 'asset_synonyms')),
          drug_class: (getValue(row, 'drug_class') || '').toString(),
          moa: (getValue(row, 'moa') || '').toString(),
          target: (getValue(row, 'target') || '').toString(),
          modality: (getValue(row, 'modality') || '').toString(),
          highest_phase: (getValue(row, 'highest_phase') || '').toString(),
          development_status: (getValue(row, 'development_status') || '').toString(),
          asset_status: (getValue(row, 'asset_status') || '').toString(),
          active_inactive: (getValue(row, 'active_inactive') || 'Active').toString(),
          sponsor_id: sponsorId,
          moa_modality_source: (getValue(row, 'moa_modality_source') || '').toString(),
          asset_status_source: (getValue(row, 'asset_status_source') || '').toString(),
          evidence_vitro: (getValue(row, 'evidence_vitro') || '').toString(),
          evidence_vivo: (getValue(row, 'evidence_vivo') || '').toString(),
          preclinical_pub_source: (getValue(row, 'preclinical_pub_source') || '').toString(),
          sales_millions: parseNum(getValue(row, 'sales_millions')),
          sales_source: (getValue(row, 'sales_source') || '').toString(),
          upcoming_clinical: (getValue(row, 'upcoming_clinical') || '').toString(),
          upcoming_clinical_source: (getValue(row, 'upcoming_clinical_source') || '').toString(),
          upcoming_regulatory: (getValue(row, 'upcoming_regulatory') || '').toString(),
          upcoming_regulatory_source: (getValue(row, 'upcoming_regulatory_source') || '').toString(),
          partnerships: parseArray(getValue(row, 'partnerships')),
          news_regulatory: (getValue(row, 'news_regulatory') || '').toString(),
          news_commercial: (getValue(row, 'news_commercial') || '').toString(),
          news_source: (getValue(row, 'news_source') || '').toString(),
          approved_region: (getValue(row, 'approved_region') || '').toString(),
          regulatory_agency: (getValue(row, 'regulatory_agency') || '').toString(),
          regulatory_designations: (getValue(row, 'regulatory_designations') || '').toString(),
          regulatory_designations_source: (getValue(row, 'regulatory_designations_source') || '').toString(),
          created_at: new Date().toISOString()
        };
        drugId = await addRecord('drugs', drugData);
        drugMap[pkey] = drugId;
      }
    }

    // Group rows by trial AND cohort signature (intelligent splitting)
    const tkey = trialId.toUpperCase();
    if (!trialGroups[tkey]) {
      trialGroups[tkey] = {
        trialIdentifier: trialId,
        drugId: drugId,
        rows: []
      };
    }

    if (!trialGroups[tkey].drugId && drugId) {
      trialGroups[tkey].drugId = drugId;
    }

    trialGroups[tkey].rows.push(row);
  }

  const listFields = new Set(['tumors', 'mutations', 'prospective_biomarkers', 'regimens']);
  const dateFields = new Set(['study_start_date', 'primary_completion_date', 'study_completion_date']);
  const intFields = new Set(['patient_number', 'efficacy_evaluable', 'safety_evaluable', 'approval_year', 'deaths']);
  const floatFields = new Set([
    'orr', 'dcr', 'cr', 'pr', 'sd', 'pd', 'mdor_months', 'pfs_months', 'mpfs_months',
    'os_months', 'mos_months', 'dfs', 'mdfs_months', 'pfs_hr', 'os_hr',
    'grade_3_ae', 'grade_3_traes', 'immune_related_aes', 'sae', 'any_grade_aes',
    'tx_related_mortality'
  ]);

  const normalizeListFromValue = (val) => {
    if (!val) return [];
    return String(val)
      .split(/[,;|]/)
      .map(s => s.trim())
      .filter(Boolean);
  };

  const normalizeRowValue = (field, val) => {
    if (dateFields.has(field)) return excelDateToISO(val);
    if (intFields.has(field)) return parseInt_(val);
    if (floatFields.has(field)) return parseNum(val);
    if (listFields.has(field)) return normalizeListFromValue(val);
    return (val || '').toString().trim();
  };

  const isEmptyValue = (val) => {
    if (Array.isArray(val)) return val.length === 0;
    if (val === null || val === undefined) return true;
    if (typeof val === 'string') return val.trim() === '';
    return false;
  };

  const buildRowData = (row, trialIdentifier) => {
    const data = {};
    TRIAL_ROW_FIELDS.forEach(field => {
      data[field] = normalizeRowValue(field, getValue(row, field));
    });
    if (!data.trial_identifier) data.trial_identifier = trialIdentifier || '';
    return data;
  };

  let createdTrials = 0;
  let updatedTrials = 0;
  let addedRowVariants = 0;

  for (const key of Object.keys(trialGroups)) {
    const group = trialGroups[key];
    const existingTrial = existingTrialMap[key] || null;
    const baseRowData = buildRowData(group.rows[0], group.trialIdentifier);
    const getBaseValue = (field, fallback) => {
      const existingVal = existingTrial ? existingTrial[field] : null;
      return !isEmptyValue(existingVal) ? existingVal : fallback;
    };

    const tumorGroupValues = !isEmptyValue(existingTrial?.tumor_group_values)
      ? existingTrial.tumor_group_values
      : normalizeListFromValue(baseRowData.tumor_group);
    const stageValues = !isEmptyValue(existingTrial?.stage_values)
      ? existingTrial.stage_values
      : normalizeListFromValue(baseRowData.stage);
    const lineValues = !isEmptyValue(existingTrial?.line_of_therapy_values)
      ? existingTrial.line_of_therapy_values
      : normalizeListFromValue(baseRowData.line_of_therapy);
    const includedValues = !isEmptyValue(existingTrial?.included_based_on_prior_values)
      ? existingTrial.included_based_on_prior_values
      : normalizeListFromValue(baseRowData.included_based_on_prior);
    const excludedValues = !isEmptyValue(existingTrial?.excluded_based_on_prior_values)
      ? existingTrial.excluded_based_on_prior_values
      : normalizeListFromValue(baseRowData.excluded_based_on_prior);

    const existingRowDetails = Array.isArray(existingTrial?.import_row_details) ? existingTrial.import_row_details : [];
    const newRowDetails = [];

    group.rows.forEach((row, idx) => {
      const rowData = buildRowData(row, group.trialIdentifier);
      rowData.row_index = existingRowDetails.length + idx + 1;
      newRowDetails.push(rowData);
    });

    const mergedRowDetails = existingRowDetails.concat(newRowDetails);
    addedRowVariants += newRowDetails.length;

    const trial = {
      trial_identifier: group.trialIdentifier,
      trial_title: getBaseValue('trial_title', baseRowData.trial_title),
      trial_acronym: getBaseValue('trial_acronym', baseRowData.trial_acronym),
      trial_design: getBaseValue('trial_design', baseRowData.trial_design),
      development_status: getBaseValue('development_status', baseRowData.development_status),
      recruitment_status: getBaseValue('recruitment_status', baseRowData.recruitment_status),
      registrational: getBaseValue('registrational', baseRowData.registrational),
      tumor_group: getBaseValue('tumor_group', baseRowData.tumor_group),
      tumor_group_values: tumorGroupValues,
      tumors: getBaseValue('tumors', baseRowData.tumors),
      patient_age_group: getBaseValue('patient_age_group', baseRowData.patient_age_group),
      patient_number: getBaseValue('patient_number', baseRowData.patient_number),
      study_start_date: getBaseValue('study_start_date', baseRowData.study_start_date),
      primary_completion_date: getBaseValue('primary_completion_date', baseRowData.primary_completion_date),
      study_completion_date: getBaseValue('study_completion_date', baseRowData.study_completion_date),
      mono_combo: getBaseValue('mono_combo', baseRowData.mono_combo),
      experimental_arm: getBaseValue('experimental_arm', baseRowData.experimental_arm),
      regimens: getBaseValue('regimens', baseRowData.regimens),
      regimen_moa: getBaseValue('regimen_moa', baseRowData.regimen_moa),
      comparator_arm: getBaseValue('comparator_arm', baseRowData.comparator_arm),
      primary_endpoint: getBaseValue('primary_endpoint', baseRowData.primary_endpoint),
      secondary_endpoint: getBaseValue('secondary_endpoint', baseRowData.secondary_endpoint),
      stage: getBaseValue('stage', baseRowData.stage),
      stage_values: stageValues,
      line_of_therapy: getBaseValue('line_of_therapy', baseRowData.line_of_therapy),
      line_of_therapy_values: lineValues,
      mutations: getBaseValue('mutations', baseRowData.mutations),
      prospective_biomarkers: getBaseValue('prospective_biomarkers', baseRowData.prospective_biomarkers),
      dx_assay: getBaseValue('dx_assay', baseRowData.dx_assay),
      dx_cutoff_value: getBaseValue('dx_cutoff_value', baseRowData.dx_cutoff_value),
      included_based_on_prior: getBaseValue('included_based_on_prior', baseRowData.included_based_on_prior),
      included_based_on_prior_values: includedValues,
      excluded_based_on_prior: getBaseValue('excluded_based_on_prior', baseRowData.excluded_based_on_prior),
      excluded_based_on_prior_values: excludedValues,
      patients_brain_cns_mets: getBaseValue('patients_brain_cns_mets', baseRowData.patients_brain_cns_mets),
      sponsor: getBaseValue('sponsor', baseRowData.sponsor),
      collaborator: getBaseValue('collaborator', baseRowData.collaborator),
      location: getBaseValue('location', baseRowData.location),
      region: getBaseValue('region', baseRowData.region),
      clinical_failure: getBaseValue('clinical_failure', baseRowData.clinical_failure),
      trials_with_results: getBaseValue('trials_with_results', baseRowData.trials_with_results),
      approved_region: getBaseValue('approved_region', baseRowData.approved_region),
      regulatory_agency: getBaseValue('regulatory_agency', baseRowData.regulatory_agency),
      approval_year: getBaseValue('approval_year', baseRowData.approval_year),
      regulatory_designation: getBaseValue('regulatory_designation', baseRowData.regulatory_designation),
      reference: getBaseValue('reference', baseRowData.reference),
      study_identifier_source: getBaseValue('study_identifier_source', baseRowData.study_identifier_source),
      registrational_trial_source: getBaseValue('registrational_trial_source', baseRowData.registrational_trial_source),
      clinical_failure_source: getBaseValue('clinical_failure_source', baseRowData.clinical_failure_source),
      approval_labels_source: getBaseValue('approval_labels_source', baseRowData.approval_labels_source),
      regulatory_designations_source: getBaseValue('regulatory_designations_source', baseRowData.regulatory_designations_source),
      data_review_committee: getBaseValue('data_review_committee', baseRowData.data_review_committee),
      efficacy_evaluable: getBaseValue('efficacy_evaluable', baseRowData.efficacy_evaluable),
      orr: getBaseValue('orr', baseRowData.orr),
      dcr: getBaseValue('dcr', baseRowData.dcr),
      cr: getBaseValue('cr', baseRowData.cr),
      pr: getBaseValue('pr', baseRowData.pr),
      sd: getBaseValue('sd', baseRowData.sd),
      pd: getBaseValue('pd', baseRowData.pd),
      mdor_months: getBaseValue('mdor_months', baseRowData.mdor_months),
      pfs_months: getBaseValue('pfs_months', baseRowData.pfs_months),
      mpfs_months: getBaseValue('mpfs_months', baseRowData.mpfs_months),
      os_months: getBaseValue('os_months', baseRowData.os_months),
      mos_months: getBaseValue('mos_months', baseRowData.mos_months),
      dfs: getBaseValue('dfs', baseRowData.dfs),
      mdfs_months: getBaseValue('mdfs_months', baseRowData.mdfs_months),
      pfs_hr: getBaseValue('pfs_hr', baseRowData.pfs_hr),
      os_hr: getBaseValue('os_hr', baseRowData.os_hr),
      efficacy_others: getBaseValue('efficacy_others', baseRowData.efficacy_others),
      publication_source_clinical: getBaseValue('publication_source_clinical', baseRowData.publication_source_clinical),
      safety_evaluable: getBaseValue('safety_evaluable', baseRowData.safety_evaluable),
      grade_3_ae: getBaseValue('grade_3_ae', baseRowData.grade_3_ae),
      grade_3_traes: getBaseValue('grade_3_traes', baseRowData.grade_3_traes),
      immune_related_aes: getBaseValue('immune_related_aes', baseRowData.immune_related_aes),
      sae: getBaseValue('sae', baseRowData.sae),
      any_grade_aes: getBaseValue('any_grade_aes', baseRowData.any_grade_aes),
      deaths: getBaseValue('deaths', baseRowData.deaths),
      tx_related_mortality: getBaseValue('tx_related_mortality', baseRowData.tx_related_mortality),
      safety_others: getBaseValue('safety_others', baseRowData.safety_others),
      publication_source_safety: getBaseValue('publication_source_safety', baseRowData.publication_source_safety),
      asset_id: existingTrial?.asset_id || group.drugId,
      cohorts: existingTrial?.cohorts || [],
      import_row_details: mergedRowDetails,
      created_at: existingTrial?.created_at || new Date().toISOString()
    };

    if (existingTrial) {
      trial.trial_id = existingTrial.trial_id;
      await putRecord('trials', trial);
      updatedTrials++;
    } else {
      await addRecord('trials', trial);
      createdTrials++;
    }
  }

  // Log import
  await addLog('IMPORT', 'Excel File', `Created ${createdTrials} trials, updated ${updatedTrials} trials, added ${addedRowVariants} row variants`);

  // Clear and reset
  document.getElementById('mappingPreview').innerHTML = `
    <div class="alert alert-success">
      ✓ Import complete! Created ${createdTrials} trials, updated ${updatedTrials} trials.
      <br/>Added ${addedRowVariants} row variants without duplicating trials.
      <br/>Per-row details are stored for review in the Trial Details view across all tabs.
      <br/>New data is now visible in the Drugs and Trials tabs.
    </div>
  `;
  document.getElementById('excelInput').value = '';
  
  // Refresh all views
  await refreshAllViews();
}

function cancelImport() {
  document.getElementById('mappingPreview').innerHTML = '';
  document.getElementById('excelInput').value = '';
  document.getElementById('importStatus').innerHTML = '';
}

// ============ Export Functions ============
async function exportToJSON() {
  const companies = await getAllRecords('companies');
  const drugs = await getAllRecords('drugs');
  const trials = await getAllRecords('trials');
  const logs = await getAllRecords('logs');

  const data = {
    exported_at: new Date().toISOString(),
    companies,
    drugs,
    trials,
    logs
  };

  const blob = new Blob([JSON.stringify(data, null, 2)], { type: 'application/json' });
  const url = URL.createObjectURL(blob);
  const a = document.createElement('a');
  a.href = url;
  a.download = 'dklinity_export_' + new Date().toISOString().split('T')[0] + '.json';
  a.click();
  URL.revokeObjectURL(url);
}

async function exportToExcel() {
  const companies = await getAllRecords('companies');
  const drugs = await getAllRecords('drugs');
  const trials = await getAllRecords('trials');

  const wb = XLSX.utils.book_new();
  XLSX.utils.book_append_sheet(wb, XLSX.utils.json_to_sheet(companies), 'Companies');
  XLSX.utils.book_append_sheet(wb, XLSX.utils.json_to_sheet(drugs), 'Drugs');
  XLSX.utils.book_append_sheet(wb, XLSX.utils.json_to_sheet(trials), 'Trials');

  XLSX.writeFile(wb, 'dklinity_export_' + new Date().toISOString().split('T')[0] + '.xlsx');
}

// ============ Logs Management ============
async function clearAllLogs() {
  if (confirm('Are you sure you want to clear all logs?')) {
    const logs = await getAllRecords('logs');
    for (const log of logs) {
      await deleteRecord('logs', log.log_id);
    }
    await refreshAllViews();
  }
}

// ============ Data Management ============
async function clearAllData() {
  try {
    // Delete all companies
    const companies = await getAllRecords('companies');
    for (const company of companies) {
      await deleteRecord('companies', company.company_id);
    }

    // Delete all drugs
    const drugs = await getAllRecords('drugs');
    for (const drug of drugs) {
      await deleteRecord('drugs', drug.drug_id);
    }

    // Delete all trials
    const trials = await getAllRecords('trials');
    for (const trial of trials) {
      await deleteRecord('trials', trial.trial_id);
    }

    // Log the action
    await addLog('DELETE', 'All Data', 'Cleared all companies, drugs, and trials from database');

    // Show success message and refresh
    alert('✓ All data cleared successfully! Database is now empty.');
    await refreshAllViews();
  } catch (error) {
    alert('Error clearing data: ' + error.message);
    console.error('Clear all data error:', error);
  }
}

// ============ Utility Functions ============
function escapeHtml(text) {
  const div = document.createElement('div');
  div.textContent = text;
  return div.innerHTML;
}

// Create a Google search link for a source text and append next to input fields
function createGoogleSearchLink(text) {
  if(!text) return '';
  // If the text is a direct URL, open it directly. Otherwise no automatic search — user wants exact reference.
  const t = String(text || '').trim();
  if(/^https?:\/\//i.test(t)){
    const href = t;
    return `<a href="${href}" target="_blank" style="margin-left:8px;color:#2563eb;font-weight:600;text-decoration:underline">Open</a>`;
  }
  return '';
}

function attachSourceLinks(containerId){
  const container = document.getElementById(containerId);
  if(!container) return;
  // find inputs whose id contains 'Source' or 'Reference'
  const inputs = container.querySelectorAll('input, textarea');
  inputs.forEach(inp=>{
    const id = inp.id || '';
    if(/source$/i.test(id) || /reference/i.test(id)){
      // remove old link if present
      const next = inp.nextElementSibling;
      if(next && next.classList && next.classList.contains('source-link')) next.remove();
      const span = document.createElement('span'); span.className='source-link'; span.style.marginLeft='8px';
      span.innerHTML = createGoogleSearchLink(inp.value || inp.placeholder || '');
      // Only append if there is a direct link to open
      if(span.innerHTML) inp.parentNode.appendChild(span);
      // update link when value changes and refresh rendered references
      inp.addEventListener('input',()=>{ span.innerHTML = createGoogleSearchLink(inp.value); renderReferencesFromInputs(containerId); });
    }
  });
}

// Split a source string into multiple items using common delimiters
function splitSources(txt){
  if(!txt) return [];
  return String(txt).split(/\||;|,/).map(s=>s.trim()).filter(Boolean);
}

function isUrl(s){ return /^https?:\/\//i.test(String(s||'').trim()); }

// Excel serial date to JS Date -> ISO string (YYYY-MM-DD)
function excelDateToISO(val){
  if(val === null || val === undefined || val === '') return '';
  if(typeof val === 'number'){
    // Excel stores dates as days since 1899-12-31 with a bug; using 25569 offset
    const utc_days = val - 25569;
    const utc_value = utc_days * 86400; // seconds
    const date_info = new Date(utc_value * 1000);
    // correct timezone offset
    const iso = date_info.toISOString().slice(0,10);
    return iso;
  }
  // If it is already a string parseable by Date
  const d = new Date(String(val));
  if(!isNaN(d.getTime())) return d.toISOString().slice(0,10);
  return String(val || '');
}

// Render parsed reference lists inside the drug modal Reference tab
function renderReferencesFromInputs(containerId){
  const container = document.getElementById(containerId);
  if(!container) return;
  const refPane = container.querySelector('#tab-reference');
  if(!refPane) return;
  // Remove existing rendered references area
  const existing = refPane.querySelector('.rendered-references');
  if(existing) existing.remove();

  const fieldsToCollect = [
    {id: 'drugMoASource', label: 'MoA / Modality Sources'},
    {id: 'drugAssetStatusSource', label: 'Asset Status Sources'},
    {id: 'drugPreclinPub', label: 'Preclinical Publications'},
    {id: 'drugSalesSource', label: 'Sales Sources'},
    {id: 'drugNewsSource', label: 'News Sources'},
    {id: 'drugRegDesignationsSource', label: 'Regulatory Designation Sources'},
    {id: 'drugUpcomingClinicalSource', label: 'Upcoming Clinical Milestone Sources'},
    {id: 'drugUpcomingRegSource', label: 'Upcoming Regulatory Milestone Sources'}
  ];

  const wrap = document.createElement('div'); wrap.className = 'rendered-references'; wrap.style.marginTop = '12px';
  wrap.style.padding = '8px'; wrap.style.background = '#f8fafc'; wrap.style.borderRadius = '6px';

  fieldsToCollect.forEach(f=>{
    const el = document.getElementById(f.id);
    if(!el) return;
    const items = splitSources(el.value || '');
    if(items.length){
      const sect = document.createElement('div');
      sect.style.marginBottom = '8px';
      const h = document.createElement('div'); h.style.fontWeight='700'; h.style.marginBottom='6px'; h.textContent = f.label;
      sect.appendChild(h);
      const list = document.createElement('div'); list.style.display='flex'; list.style.flexWrap='wrap'; list.style.gap='6px';
      items.forEach(it => {
        const tag = document.createElement('a');
        tag.style.display='inline-block'; tag.style.padding='6px 8px'; tag.style.background='#eef2ff'; tag.style.borderRadius='8px'; tag.style.color='#1e3a8a'; tag.style.textDecoration='none'; tag.style.fontSize='12px';
        tag.textContent = it;
        if(isUrl(it)) tag.href = it; else tag.href = '#';
        tag.target = '_blank';
        list.appendChild(tag);
      });
      sect.appendChild(list);
      wrap.appendChild(sect);
    }
  });

  if(wrap.childElementCount) refPane.appendChild(wrap);
}

// ============ Initialization ============
window.addEventListener('DOMContentLoaded', async () => {
  await initDB();
  await refreshAllViews();
});

// Close modals on background click
document.addEventListener('click', (e) => {
  if (e.target.classList.contains('modal')) {
    closeAllModals();
  }
});
</script>

</body>
</html>
