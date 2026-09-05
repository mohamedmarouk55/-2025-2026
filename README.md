<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>لوحة تحكم تكاليف الكهرباء - شركة الراشد للصناعة</title>
<link href="https://fonts.googleapis.com/css2?family=Tajawal:wght@400;500;700;900&display=swap" rel="stylesheet">
<script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.0/dist/chart.umd.min.js"></script>
<style>
  * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Tajawal', sans-serif; }
  body {
    background: #FAF8F5;
    color: #2D1B4E;
    min-height: 100vh;
    padding: 20px;
  }
  .container { max-width: 1600px; margin: 0 auto; }

  /* Header */
  .header {
    background: linear-gradient(135deg, #6B46C1 0%, #9333EA 50%, #A855F7 100%);
    border-radius: 20px;
    padding: 30px 40px;
    color: white;
    box-shadow: 0 10px 40px rgba(107, 70, 193, 0.25);
    margin-bottom: 25px;
    position: relative;
    overflow: hidden;
  }
  .header::before {
    content: '';
    position: absolute;
    top: -50%;
    left: -50%;
    width: 200%;
    height: 200%;
    background: radial-gradient(circle, rgba(255,255,255,0.1) 0%, transparent 70%);
    animation: pulse 8s ease-in-out infinite;
  }
  @keyframes pulse {
    0%, 100% { transform: scale(1); opacity: 0.5; }
    50% { transform: scale(1.1); opacity: 0.8; }
  }
  .header-content { position: relative; z-index: 2; }
  .header h1 {
    font-size: 2.2rem;
    font-weight: 900;
    margin-bottom: 8px;
    text-shadow: 0 2px 10px rgba(0,0,0,0.1);
  }
  .header p { font-size: 1rem; opacity: 0.95; font-weight: 500; }
  .header-meta {
    display: flex;
    gap: 20px;
    margin-top: 15px;
    flex-wrap: wrap;
  }
  .header-meta span {
    background: rgba(255,255,255,0.15);
    padding: 6px 14px;
    border-radius: 30px;
    font-size: 0.85rem;
    backdrop-filter: blur(10px);
    border: 1px solid rgba(255,255,255,0.2);
  }

  /* Tabs */
  .tabs {
    display: flex;
    gap: 10px;
    margin-bottom: 20px;
    background: white;
    padding: 8px;
    border-radius: 16px;
    box-shadow: 0 4px 20px rgba(107, 70, 193, 0.08);
  }
  .tab-btn {
    flex: 1;
    padding: 14px 20px;
    background: transparent;
    border: none;
    border-radius: 12px;
    font-size: 1rem;
    font-weight: 700;
    color: #6B46C1;
    cursor: pointer;
    transition: all 0.3s ease;
    font-family: 'Tajawal', sans-serif;
  }
  .tab-btn.active {
    background: linear-gradient(135deg, #6B46C1, #9333EA);
    color: white;
    box-shadow: 0 6px 20px rgba(107, 70, 193, 0.35);
  }
  .tab-btn:hover:not(.active) { background: #F3E8FF; }

  /* Filters */
  .filters {
    background: white;
    border-radius: 16px;
    padding: 20px 25px;
    margin-bottom: 25px;
    box-shadow: 0 4px 20px rgba(107, 70, 193, 0.08);
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
    gap: 15px;
    align-items: end;
  }
  .filter-group label {
    display: block;
    font-size: 0.85rem;
    font-weight: 700;
    color: #6B46C1;
    margin-bottom: 8px;
  }
  .filter-group select {
    width: 100%;
    padding: 10px 14px;
    border: 2px solid #E9D5FF;
    border-radius: 10px;
    background: #FAF8F5;
    color: #2D1B4E;
    font-size: 0.95rem;
    font-weight: 500;
    cursor: pointer;
    transition: all 0.3s ease;
    font-family: 'Tajawal', sans-serif;
  }
  .filter-group select:focus {
    outline: none;
    border-color: #9333EA;
    background: white;
    box-shadow: 0 0 0 3px rgba(147, 51, 234, 0.1);
  }

  /* YoY Toggle */
  .yoy-toggle {
    display: flex;
    align-items: center;
    gap: 12px;
    padding: 10px 14px;
    background: #FAF8F5;
    border: 2px solid #E9D5FF;
    border-radius: 10px;
    height: 44px;
  }
  .yoy-toggle span {
    font-size: 0.9rem;
    font-weight: 600;
    color: #6B46C1;
  }
  .switch {
    position: relative;
    display: inline-block;
    width: 50px;
    height: 26px;
  }
  .switch input { opacity: 0; width: 0; height: 0; }
  .slider {
    position: absolute;
    cursor: pointer;
    top: 0; left: 0; right: 0; bottom: 0;
    background-color: #E9D5FF;
    transition: .4s;
    border-radius: 26px;
  }
  .slider:before {
    position: absolute;
    content: "";
    height: 20px;
    width: 20px;
    right: 3px;
    bottom: 3px;
    background-color: white;
    transition: .4s;
    border-radius: 50%;
    box-shadow: 0 2px 4px rgba(0,0,0,0.2);
  }
  input:checked + .slider { background: linear-gradient(135deg, #6B46C1, #9333EA); }
  input:checked + .slider:before { transform: translateX(-24px); }

  /* KPI Cards */
  .kpi-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(240px, 1fr));
    gap: 18px;
    margin-bottom: 25px;
  }
  .kpi-card {
    background: white;
    border-radius: 16px;
    padding: 22px;
    box-shadow: 0 4px 20px rgba(107, 70, 193, 0.08);
    position: relative;
    overflow: hidden;
    transition: transform 0.3s ease, box-shadow 0.3s ease;
    border-right: 5px solid;
  }
  .kpi-card:hover {
    transform: translateY(-4px);
    box-shadow: 0 10px 30px rgba(107, 70, 193, 0.15);
  }
  .kpi-card::before {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: linear-gradient(135deg, transparent 70%, rgba(147, 51, 234, 0.05));
    pointer-events: none;
  }
  .kpi-card.c1 { border-color: #6B46C1; }
  .kpi-card.c2 { border-color: #9333EA; }
  .kpi-card.c3 { border-color: #A855F7; }
  .kpi-card.c4 { border-color: #C084FC; }
  .kpi-icon {
    width: 48px;
    height: 48px;
    border-radius: 12px;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 1.5rem;
    margin-bottom: 12px;
    color: white;
  }
  .c1 .kpi-icon { background: linear-gradient(135deg, #6B46C1, #9333EA); }
  .c2 .kpi-icon { background: linear-gradient(135deg, #9333EA, #A855F7); }
  .c3 .kpi-icon { background: linear-gradient(135deg, #A855F7, #C084FC); }
  .c4 .kpi-icon { background: linear-gradient(135deg, #C084FC, #D8B4FE); }
  .kpi-label {
    font-size: 0.85rem;
    color: #7C6B9E;
    font-weight: 500;
    margin-bottom: 6px;
  }
  .kpi-value {
    font-size: 1.6rem;
    font-weight: 900;
    color: #2D1B4E;
    line-height: 1.2;
    margin-bottom: 4px;
  }
  .kpi-sub {
    font-size: 0.8rem;
    color: #9333EA;
    font-weight: 600;
  }

  /* Charts Grid */
  .charts-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 20px;
    margin-bottom: 25px;
  }
  .chart-card {
    background: white;
    border-radius: 16px;
    padding: 22px;
    box-shadow: 0 4px 20px rgba(107, 70, 193, 0.08);
  }
  .chart-card h3 {
    font-size: 1.05rem;
    color: #2D1B4E;
    margin-bottom: 15px;
    font-weight: 700;
    padding-right: 12px;
    border-right: 4px solid #9333EA;
  }
  .chart-container {
    position: relative;
    height: 320px;
  }

  /* Tables */
  .table-card {
    background: white;
    border-radius: 16px;
    padding: 22px;
    box-shadow: 0 4px 20px rgba(107, 70, 193, 0.08);
    margin-bottom: 25px;
    overflow: hidden;
  }
  .table-card h3 {
    font-size: 1.1rem;
    color: #2D1B4E;
    margin-bottom: 18px;
    font-weight: 700;
    padding-right: 12px;
    border-right: 4px solid #9333EA;
  }
  .table-wrapper { overflow-x: auto; }
  table {
    width: 100%;
    border-collapse: collapse;
    font-size: 0.9rem;
  }
  thead th {
    background: linear-gradient(135deg, #6B46C1, #9333EA);
    color: white;
    padding: 14px 12px;
    text-align: right;
    font-weight: 700;
    white-space: nowrap;
  }
  thead th:first-child { border-radius: 0 10px 10px 0; }
  thead th:last-child { border-radius: 10px 0 0 10px; }
  tbody td {
    padding: 12px;
    border-bottom: 1px solid #F3E8FF;
    text-align: right;
    font-weight: 500;
  }
  tbody tr:hover { background: #FAF5FF; }
  tbody tr.region-row {
    background: #F3E8FF;
    font-weight: 700;
    color: #6B46C1;
  }
  tbody tr.total-row {
    background: linear-gradient(135deg, #6B46C1, #9333EA);
    color: white;
    font-weight: 900;
    font-size: 1rem;
  }
  tbody tr.total-row td { border-bottom: none; }
  .num { font-variant-numeric: tabular-nums; }
  .pct {
    display: inline-block;
    padding: 3px 8px;
    background: #E9D5FF;
    color: #6B46C1;
    border-radius: 6px;
    font-size: 0.8rem;
    font-weight: 700;
  }
  .total-row .pct {
    background: rgba(255,255,255,0.25);
    color: white;
  }

  .page { display: none; }
  .page.active { display: block; animation: fadeIn 0.4s ease; }
  @keyframes fadeIn {
    from { opacity: 0; transform: translateY(10px); }
    to { opacity: 1; transform: translateY(0); }
  }

  .footer-note {
    text-align: center;
    color: #7C6B9E;
    font-size: 0.85rem;
    padding: 20px;
    font-weight: 500;
  }

  @media (max-width: 900px) {
    .charts-grid { grid-template-columns: 1fr; }
    .header h1 { font-size: 1.5rem; }
    .kpi-value { font-size: 1.3rem; }
  }
</style>
</head>
<body>
<div class="container">

  <!-- Header -->
  <div class="header">
    <div class="header-content">
      <h1>⚡ لوحة تحكم تكاليف الكهرباء</h1>
      <p>شركة الراشد للصناعة - تحليل استهلاك وتكاليف الكهرباء للمصانع</p>
      <div class="header-meta">
        <span>📅 الفترة: يناير 2025 - يوليو 2026</span>
        <span>🏭 المصانع: 5 مصانع</span>
        <span>📊 إجمالي الشهور: 19 شهر</span>
      </div>
    </div>
  </div>

  <!-- Tabs -->
  <div class="tabs">
    <button class="tab-btn active" data-page="overview">📊 اللوحة التنفيذية والملخص</button>
    <button class="tab-btn" data-page="detailed">📋 التقرير التفصيلي والتحليلات</button>
  </div>

  <!-- Filters -->
  <div class="filters">
    <div class="filter-group">
      <label>السنة</label>
      <select id="yearFilter">
        <option value="all">الكل</option>
        <option value="2025">2025</option>
        <option value="2026">2026</option>
      </select>
    </div>
    <div class="filter-group">
      <label>الشهر</label>
      <select id="monthFilter">
        <option value="all">الكل</option>
      </select>
    </div>
    <div class="filter-group">
      <label>مقارنة سنوية (YoY)</label>
      <div class="yoy-toggle">
        <span id="yoyLabel">إيقاف</span>
        <label class="switch">
          <input type="checkbox" id="yoyToggle">
          <span class="slider"></span>
        </label>
      </div>
    </div>
    <div class="filter-group">
      <label>المنطقة</label>
      <select id="regionFilter">
        <option value="all">كل المناطق</option>
        <option value="riyadh">مصانع الرياض</option>
        <option value="qassim">مصانع القصيم</option>
      </select>
    </div>
    <div class="filter-group">
      <label>المصنع</label>
      <select id="factoryFilter">
        <option value="all">الكل</option>
      </select>
    </div>
  </div>

  <!-- Page 1: Executive Overview -->
  <div class="page active" id="page-overview">
    <!-- KPI Cards -->
    <div class="kpi-grid">
      <div class="kpi-card c1">
        <div class="kpi-icon">💰</div>
        <div class="kpi-label">إجمالي التكلفة (ريال سعودي)</div>
        <div class="kpi-value" id="kpiTotalCost">0.00 SAR</div>
        <div class="kpi-sub" id="kpiTotalSub">—</div>
      </div>
      <div class="kpi-card c2">
        <div class="kpi-icon">📈</div>
        <div class="kpi-label">متوسط الاستهلاك الشهري</div>
        <div class="kpi-value" id="kpiAvgMonthly">0.00 SAR</div>
        <div class="kpi-sub" id="kpiAvgSub">—</div>
      </div>
      <div class="kpi-card c3">
        <div class="kpi-icon">🏆</div>
        <div class="kpi-label">المصنع الأعلى استهلاكاً</div>
        <div class="kpi-value" id="kpiTopFactory">—</div>
        <div class="kpi-sub" id="kpiTopSub">—</div>
      </div>
      <div class="kpi-card c4">
        <div class="kpi-icon">🗺️</div>
        <div class="kpi-label">التوزيع الإقليمي</div>
        <div class="kpi-value" id="kpiRegionDist">—</div>
        <div class="kpi-sub" id="kpiRegionSub">—</div>
      </div>
    </div>

    <!-- Charts -->
    <div class="charts-grid">
      <div class="chart-card">
        <h3>🍩 مقارنة استهلاك السنتين (2025 vs 2026)</h3>
        <div class="chart-container"><canvas id="chartYears"></canvas></div>
      </div>
      <div class="chart-card">
        <h3>📊 اتجاه الاستهلاك الشهري</h3>
        <div class="chart-container"><canvas id="chartMonthly"></canvas></div>
      </div>
      <div class="chart-card">
        <h3>🍩 مقارنة المناطق (الرياض vs القصيم)</h3>
        <div class="chart-container"><canvas id="chartRegions"></canvas></div>
      </div>
      <div class="chart-card">
        <h3>📊 مقارنة استهلاك المصانع (مرتبة تنازلياً)</h3>
        <div class="chart-container"><canvas id="chartFactories"></canvas></div>
      </div>
    </div>

    <!-- Summary Table -->
    <div class="table-card">
      <h3>📋 جدول الملخص المجمع</h3>
      <div class="table-wrapper">
        <table id="summaryTable">
          <thead>
            <tr>
              <th>المصنع</th>
              <th>إجمالي 2025 (SAR)</th>
              <th>إجمالي 2026 (SAR)</th>
              <th>الإجمالي الكلي (SAR)</th>
              <th>نسبة المساهمة %</th>
            </tr>
          </thead>
          <tbody id="summaryBody"></tbody>
        </table>
      </div>
    </div>
  </div>

  <!-- Page 2: Detailed Report -->
  <div class="page" id="page-detailed">
    <div class="table-card">
      <h3>📋 التقرير التفصيلي الكامل (19 شهر)</h3>
      <div class="table-wrapper">
        <table id="detailedTable">
          <thead>
            <tr>
              <th>الشهر</th>
              <th>السنة</th>
              <th>مصنع الرياض 1</th>
              <th>مصنع الرياض 2</th>
              <th>مصنع القصيم 1</th>
              <th>مصنع القصيم 2</th>
              <th>مصنع القصيم 3</th>
              <th>إجمالي الشهر</th>
            </tr>
          </thead>
          <tbody id="detailedBody"></tbody>
        </table>
      </div>
    </div>
  </div>

  <div class="footer-note">
    © 2026 شركة الراشد للصناعة - نظام تحليل تكاليف الكهرباء التفاعلي
  </div>
</div>

<script>
/* ============ DATA ============ */
const MONTHS_AR = ['يناير','فبراير','مارس','أبريل','مايو','يونيو','يوليو','أغسطس','سبتمبر','أكتوبر','نوفمبر','ديسمبر'];
const FACTORIES = {
  riyadh1: 'مصنع الرياض 1',
  riyadh2: 'مصنع الرياض 2',
  qassim1: 'مصنع القصيم 1',
  qassim2: 'مصنع القصيم 2',
  qassim3: 'مصنع القصيم 3'
};
const FACTORY_COLORS = {
  riyadh1: '#6B46C1',
  riyadh2: '#9333EA',
  qassim1: '#A855F7',
  qassim2: '#C084FC',
  qassim3: '#D8B4FE'
};

const DATA = [
  {month:'يناير',year:2025,riyadh1:105681.76,riyadh2:76668.38,qassim1:229029.86,qassim2:9490.64,qassim3:0},
  {month:'فبراير',year:2025,riyadh1:91142.74,riyadh2:75698.38,qassim1:205138.31,qassim2:10500.70,qassim3:0},
  {month:'مارس',year:2025,riyadh1:84573.55,riyadh2:69615.07,qassim1:175969.84,qassim2:9518.18,qassim3:0},
  {month:'أبريل',year:2025,riyadh1:80671.53,riyadh2:79146.17,qassim1:114630.12,qassim2:14960.44,qassim3:0},
  {month:'مايو',year:2025,riyadh1:97872.02,riyadh2:84438.64,qassim1:82687.38,qassim2:10218.27,qassim3:0},
  {month:'يونيو',year:2025,riyadh1:93198.49,riyadh2:80333.94,qassim1:105899.13,qassim2:19457.56,qassim3:0},
  {month:'يوليو',year:2025,riyadh1:97715.50,riyadh2:97127.92,qassim1:90936.48,qassim2:19705.04,qassim3:0},
  {month:'أغسطس',year:2025,riyadh1:94199.25,riyadh2:100573.02,qassim1:94265.04,qassim2:24819.93,qassim3:0},
  {month:'سبتمبر',year:2025,riyadh1:44060.00,riyadh2:99659.46,qassim1:97628.10,qassim2:24120.01,qassim3:0},
  {month:'أكتوبر',year:2025,riyadh1:21571.33,riyadh2:98207.70,qassim1:95722.32,qassim2:15718.79,qassim3:0},
  {month:'نوفمبر',year:2025,riyadh1:344086.58,riyadh2:100862.82,qassim1:110072.25,qassim2:22107.31,qassim3:0},
  {month:'ديسمبر',year:2025,riyadh1:145061.14,riyadh2:105501.00,qassim1:106260.69,qassim2:19579.28,qassim3:0},
  {month:'يناير',year:2026,riyadh1:128630.77,riyadh2:99883.02,qassim1:98318.10,qassim2:33800.32,qassim3:0},
  {month:'فبراير',year:2026,riyadh1:146916.68,riyadh2:116144.33,qassim1:113264.88,qassim2:31839.45,qassim3:0},
  {month:'مارس',year:2026,riyadh1:129194.81,riyadh2:101840.67,qassim1:102409.11,qassim2:22167.06,qassim3:0},
  {month:'أبريل',year:2026,riyadh1:170910.10,riyadh2:134447.17,qassim1:123821.88,qassim2:23267.20,qassim3:0},
  {month:'مايو',year:2026,riyadh1:151531.87,riyadh2:101533.48,qassim1:183760.09,qassim2:20100.87,qassim3:0},
  {month:'يونيو',year:2026,riyadh1:184116.33,riyadh2:127376.13,qassim1:131923.17,qassim2:27258.36,qassim3:906.09},
  {month:'يوليو',year:2026,riyadh1:185980.17,riyadh2:115827.44,qassim1:259198.50,qassim2:30488.94,qassim3:355.42}
];

/* ============ UTILS ============ */
const fmt = n => (n || 0).toLocaleString('en-US', {minimumFractionDigits:2, maximumFractionDigits:2}) + ' SAR';
const fmtShort = n => {
  if (n >= 1000000) return (n/1000000).toFixed(2) + 'M SAR';
  if (n >= 1000) return (n/1000).toFixed(1) + 'K SAR';
  return n.toFixed(2) + ' SAR';
};
const pct = (v, t) => t > 0 ? ((v/t)*100).toFixed(2) + '%' : '0.00%';

/* ============ STATE ============ */
let state = {
  year: 'all',
  month: 'all',
  yoy: false,
  region: 'all',
  factory: 'all',
  crossFilter: null
};

/* ============ FILTER LOGIC ============ */
function updateMonthFilter() {
  const sel = document.getElementById('monthFilter');
  const current = sel.value;
  sel.innerHTML = '<option value="all">الكل</option>';
  let months;
  if (state.year === '2025') months = MONTHS_AR;
  else if (state.year === '2026') months = MONTHS_AR.slice(0, 7);
  else months = MONTHS_AR;
  months.forEach(m => {
    const opt = document.createElement('option');
    opt.value = m; opt.textContent = m;
    sel.appendChild(opt);
  });
  if ([...sel.options].some(o => o.value === current)) sel.value = current;
  else sel.value = 'all';
  state.month = sel.value;
}

function updateFactoryFilter() {
  const sel = document.getElementById('factoryFilter');
  sel.innerHTML = '<option value="all">الكل</option>';
  let factories = [];
  if (state.region === 'riyadh') factories = ['riyadh1','riyadh2'];
  else if (state.region === 'qassim') factories = ['qassim1','qassim2','qassim3'];
  else factories = ['riyadh1','riyadh2','qassim1','qassim2','qassim3'];
  factories.forEach(f => {
    const opt = document.createElement('option');
    opt.value = f; opt.textContent = FACTORIES[f];
    sel.appendChild(opt);
  });
  if ([...sel.options].some(o => o.value === state.factory)) sel.value = state.factory;
  else { sel.value = 'all'; state.factory = 'all'; }
}

function getFilteredData() {
  let data = [...DATA];
  if (state.year !== 'all') data = data.filter(d => d.year == state.year);
  if (state.month !== 'all') data = data.filter(d => d.month === state.month);
  return data;
}

function getFactoryValue(row, key) {
  if (state.factory !== 'all' && state.factory !== key) return 0;
  if (state.region === 'riyadh' && !key.startsWith('riyadh')) return 0;
  if (state.region === 'qassim' && !key.startsWith('qassim')) return 0;
  return row[key] || 0;
}

function getRowTotal(row) {
  return ['riyadh1','riyadh2','qassim1','qassim2','qassim3'].reduce((s,k) => s + getFactoryValue(row,k), 0);
}

/* ============ KPIs ============ */
function updateKPIs() {
  const data = getFilteredData();
  const factoryKeys = ['riyadh1','riyadh2','qassim1','qassim2','qassim3'];

  let totalCost = 0;
  data.forEach(d => {
    factoryKeys.forEach(k => { totalCost += getFactoryValue(d, k); });
  });
  document.getElementById('kpiTotalCost').textContent = fmt(totalCost);
  document.getElementById('kpiTotalSub').textContent = `${data.length} شهر مُفلتر`;

  const avg = data.length > 0 ? totalCost / data.length : 0;
  document.getElementById('kpiAvgMonthly').textContent = fmt(avg);
  document.getElementById('kpiAvgSub').textContent = 'متوسط شهري';

  const factoryTotals = {};
  factoryKeys.forEach(k => {
    let sum = 0;
    data.forEach(d => { sum += getFactoryValue(d, k); });
    factoryTotals[k] = sum;
  });
  const topKey = Object.keys(factoryTotals).reduce((a,b) => factoryTotals[a] > factoryTotals[b] ? a : b);
  const topVal = factoryTotals[topKey];
  document.getElementById('kpiTopFactory').textContent = FACTORIES[topKey];
  document.getElementById('kpiTopSub').textContent = `${fmt(topVal)} (${pct(topVal, totalCost)})`;

  let riyadhSum = 0, qassimSum = 0;
  data.forEach(d => {
    riyadhSum += getFactoryValue(d, 'riyadh1') + getFactoryValue(d, 'riyadh2');
    qassimSum += getFactoryValue(d, 'qassim1') + getFactoryValue(d, 'qassim2') + getFactoryValue(d, 'qassim3');
  });
  const grand = riyadhSum + qassimSum;
  document.getElementById('kpiRegionDist').textContent = `${pct(riyadhSum, grand)} / ${pct(qassimSum, grand)}`;
  document.getElementById('kpiRegionSub').textContent = `الرياض ${fmtShort(riyadhSum)} | القصيم ${fmtShort(qassimSum)}`;
}

/* ============ CHARTS ============ */
let charts = {};

function destroyChart(name) {
  if (charts[name]) { charts[name].destroy(); charts[name] = null; }
}

function buildYearsChart() {
  destroyChart('years');
  const data = getFilteredData();
  let y2025 = 0, y2026 = 0;
  data.forEach(d => {
    const rowTotal = getRowTotal(d);
    if (d.year === 2025) y2025 += rowTotal;
    else if (d.year === 2026) y2026 += rowTotal;
  });
  const ctx = document.getElementById('chartYears').getContext('2d');
  charts.years = new Chart(ctx, {
    type: 'doughnut',
    data: {
      labels: ['2025', '2026'],
      datasets: [{
        data: [y2025, y2026],
        backgroundColor: ['#9333EA', '#C084FC'],
        borderColor: '#fff',
        borderWidth: 3,
        hoverOffset: 15
      }]
    },
    options: {
      responsive: true, maintainAspectRatio: false,
      cutout: '60%',
      plugins: {
        legend: { position: 'bottom', labels: { font: { family: 'Tajawal', size: 13, weight: '700' }, color: '#2D1B4E', padding: 15 } },
        tooltip: {
          callbacks: {
            label: ctx => `${ctx.label}: ${fmt(ctx.raw)} (${pct(ctx.raw, y2025+y2026)})`
          }
        }
      },
      onClick: (e, els) => {
        if (els.length > 0) {
          const idx = els[0].index;
          const year = idx === 0 ? 2025 : 2026;
          state.crossFilter = state.crossFilter && state.crossFilter.type === 'year' && state.crossFilter.value === year ? null : {type:'year', value:year};
          renderAll();
        }
      }
    }
  });
}

function buildRegionsChart() {
  destroyChart('regions');
  const data = getFilteredData();
  let riyadh = 0, qassim = 0;
  data.forEach(d => {
    riyadh += getFactoryValue(d,'riyadh1') + getFactoryValue(d,'riyadh2');
    qassim += getFactoryValue(d,'qassim1') + getFactoryValue(d,'qassim2') + getFactoryValue(d,'qassim3');
  });
  const ctx = document.getElementById('chartRegions').getContext('2d');
  charts.regions = new Chart(ctx, {
    type: 'doughnut',
    data: {
      labels: ['مصانع الرياض', 'مصانع القصيم'],
      datasets: [{
        data: [riyadh, qassim],
        backgroundColor: ['#6B46C1', '#A855F7'],
        borderColor: '#fff',
        borderWidth: 3,
        hoverOffset: 15
      }]
    },
    options: {
      responsive: true, maintainAspectRatio: false,
      cutout: '60%',
      plugins: {
        legend: { position: 'bottom', labels: { font: { family: 'Tajawal', size: 13, weight: '700' }, color: '#2D1B4E', padding: 15 } },
        tooltip: {
          callbacks: {
            label: ctx => `${ctx.label}: ${fmt(ctx.raw)} (${pct(ctx.raw, riyadh+qassim)})`
          }
        }
      },
      onClick: (e, els) => {
        if (els.length > 0) {
          const idx = els[0].index;
          const region = idx === 0 ? 'riyadh' : 'qassim';
          state.crossFilter = state.crossFilter && state.crossFilter.type === 'region' && state.crossFilter.value === region ? null : {type:'region', value:region};
          renderAll();
        }
      }
    }
  });
}

function buildMonthlyChart() {
  destroyChart('monthly');
  const ctx = document.getElementById('chartMonthly').getContext('2d');

  if (state.yoy) {
    const month = state.month !== 'all' ? state.month : null;
    let labels = [], d2025 = [], d2026 = [];
    if (month) {
      labels = [month];
      const r25 = DATA.find(d => d.month === month && d.year === 2025);
      const r26 = DATA.find(d => d.month === month && d.year === 2026);
      const sum25 = r25 ? getRowTotal(r25) : 0;
      const sum26 = r26 ? getRowTotal(r26) : 0;
      d2025 = [sum25]; d2026 = [sum26];
    } else {
      labels = MONTHS_AR;
      MONTHS_AR.forEach(m => {
        const r25 = DATA.find(d => d.month === m && d.year === 2025);
        const r26 = DATA.find(d => d.month === m && d.year === 2026);
        d2025.push(r25 ? getRowTotal(r25) : 0);
        d2026.push(r26 ? getRowTotal(r26) : 0);
      });
    }
    charts.monthly = new Chart(ctx, {
      type: 'bar',
      data: {
        labels: labels,
        datasets: [
          { label: '2025', data: d2025, backgroundColor: '#9333EA', borderRadius: 8 },
          { label: '2026', data: d2026, backgroundColor: '#C084FC', borderRadius: 8 }
        ]
      },
      options: {
        responsive: true, maintainAspectRatio: false,
        plugins: {
          legend: { labels: { font: { family: 'Tajawal', size: 12, weight: '700' }, color: '#2D1B4E' } },
          tooltip: { callbacks: { label: ctx => `${ctx.dataset.label}: ${fmt(ctx.raw)}` } }
        },
        scales: {
          x: { ticks: { font: { family: 'Tajawal', weight: '600' }, color: '#6B46C1' }, grid: { display: false } },
          y: { ticks: { font: { family: 'Tajawal' }, color: '#7C6B9E', callback: v => fmtShort(v) }, grid: { color: '#F3E8FF' } }
        }
      }
    });
  } else {
    const data = getFilteredData();
    const labels = data.map(d => `${d.month} ${d.year}`);
    const values = data.map(d => getRowTotal(d));
    const gradient = ctx.createLinearGradient(0, 0, 0, 320);
    gradient.addColorStop(0, '#9333EA');
    gradient.addColorStop(1, '#C084FC');
    charts.monthly = new Chart(ctx, {
      type: 'bar',
      data: {
        labels: labels,
        datasets: [{
          label: 'إجمالي الشهر',
          data: values,
          backgroundColor: gradient,
          borderRadius: 8,
          borderSkipped: false
        }]
      },
      options: {
        responsive: true, maintainAspectRatio: false,
        plugins: {
          legend: { display: false },
          tooltip: { callbacks: { label: ctx => fmt(ctx.raw) } }
        },
        scales: {
          x: { ticks: { font: { family: 'Tajawal', size: 10, weight: '600' }, color: '#6B46C1', maxRotation: 45, minRotation: 45 }, grid: { display: false } },
          y: { ticks: { font: { family: 'Tajawal' }, color: '#7C6B9E', callback: v => fmtShort(v) }, grid: { color: '#F3E8FF' } }
        },
        onClick: (e, els) => {
          if (els.length > 0) {
            const idx = els[0].index;
            const row = data[idx];
            state.crossFilter = state.crossFilter && state.crossFilter.type === 'month' && state.crossFilter.month === row.month && state.crossFilter.year === row.year ? null : {type:'month', month:row.month, year:row.year};
            renderAll();
          }
        }
      }
    });
  }
}

function buildFactoriesChart() {
  destroyChart('factories');
  const data = getFilteredData();
  const totals = {};
  ['riyadh1','riyadh2','qassim1','qassim2','qassim3'].forEach(k => {
    let sum = 0;
    data.forEach(d => { sum += getFactoryValue(d, k); });
    totals[k] = sum;
  });
  const sorted = Object.entries(totals).sort((a,b) => b[1] - a[1]);
  const ctx = document.getElementById('chartFactories').getContext('2d');
  charts.factories = new Chart(ctx, {
    type: 'bar',
    data: {
      labels: sorted.map(s => FACTORIES[s[0]]),
      datasets: [{
        label: 'الإجمالي',
        data: sorted.map(s => s[1]),
        backgroundColor: sorted.map(s => FACTORY_COLORS[s[0]]),
        borderRadius: 8,
        borderSkipped: false
      }]
    },
    options: {
      indexAxis: 'y',
      responsive: true, maintainAspectRatio: false,
      plugins: {
        legend: { display: false },
        tooltip: { callbacks: { label: ctx => fmt(ctx.raw) } }
      },
      scales: {
        x: { ticks: { font: { family: 'Tajawal' }, color: '#7C6B9E', callback: v => fmtShort(v) }, grid: { color: '#F3E8FF' } },
        y: { ticks: { font: { family: 'Tajawal', size: 12, weight: '700' }, color: '#2D1B4E' }, grid: { display: false } }
      },
      onClick: (e, els) => {
        if (els.length > 0) {
          const idx = els[0].index;
          const factoryKey = sorted[idx][0];
          state.crossFilter = state.crossFilter && state.crossFilter.type === 'factory' && state.crossFilter.value === factoryKey ? null : {type:'factory', value:factoryKey};
          renderAll();
        }
      }
    }
  });
}

/* ============ SUMMARY TABLE ============ */
function buildSummaryTable() {
  const tbody = document.getElementById('summaryBody');
  tbody.innerHTML = '';

  let data = getFilteredData();
  if (state.crossFilter) {
    if (state.crossFilter.type === 'year') data = data.filter(d => d.year === state.crossFilter.value);
    else if (state.crossFilter.type === 'month') data = data.filter(d => d.month === state.crossFilter.month && d.year === state.crossFilter.year);
  }

  const factoryKeys = ['riyadh1','riyadh2','qassim1','qassim2','qassim3'];
  const byFactory = {};
  factoryKeys.forEach(k => {
    let s25 = 0, s26 = 0;
    data.forEach(d => {
      const v = getFactoryValue(d, k);
      if (d.year === 2025) s25 += v;
      else if (d.year === 2026) s26 += v;
    });
    byFactory[k] = {y2025: s25, y2026: s26, total: s25+s26};
  });

  const grandTotal = factoryKeys.reduce((s,k) => s + byFactory[k].total, 0);

  const riyadhTotal = byFactory.riyadh1.total + byFactory.riyadh2.total;
  const riyadh25 = byFactory.riyadh1.y2025 + byFactory.riyadh2.y2025;
  const riyadh26 = byFactory.riyadh1.y2026 + byFactory.riyadh2.y2026;

  const qassimTotal = byFactory.qassim1.total + byFactory.qassim2.total + byFactory.qassim3.total;
  const qassim25 = byFactory.qassim1.y2025 + byFactory.qassim2.y2025 + byFactory.qassim3.y2025;
  const qassim26 = byFactory.qassim1.y2026 + byFactory.qassim2.y2026 + byFactory.qassim3.y2026;

  const rows = [];
  rows.push({name: FACTORIES.riyadh1, ...byFactory.riyadh1, grand: grandTotal});
  rows.push({name: FACTORIES.riyadh2, ...byFactory.riyadh2, grand: grandTotal});
  rows.push({region: true, name: 'إجمالي مصانع الرياض', y2025: riyadh25, y2026: riyadh26, total: riyadhTotal, grand: grandTotal});
  rows.push({name: FACTORIES.qassim1, ...byFactory.qassim1, grand: grandTotal});
  rows.push({name: FACTORIES.qassim2, ...byFactory.qassim2, grand: grandTotal});
  rows.push({name: FACTORIES.qassim3, ...byFactory.qassim3, grand: grandTotal});
  rows.push({region: true, name: 'إجمالي مصانع القصيم', y2025: qassim25, y2026: qassim26, total: qassimTotal, grand: grandTotal});

  rows.forEach(r => {
    const tr = document.createElement('tr');
    if (r.region) tr.className = 'region-row';
    tr.innerHTML = `
      <td>${r.name}</td>
      <td class="num">${fmt(r.y2025)}</td>
      <td class="num">${fmt(r.y2026)}</td>
      <td class="num">${fmt(r.total)}</td>
      <td><span class="pct">${pct(r.total, r.grand)}</span></td>
    `;
    tbody.appendChild(tr);
  });

  const tr = document.createElement('tr');
  tr.className = 'total-row';
  tr.innerHTML = `
    <td>الإجمالي العام للشركة</td>
    <td class="num">${fmt(riyadh25 + qassim25)}</td>
    <td class="num">${fmt(riyadh26 + qassim26)}</td>
    <td class="num">${fmt(grandTotal)}</td>
    <td><span class="pct">100.00%</span></td>
  `;
  tbody.appendChild(tr);
}

/* ============ DETAILED TABLE ============ */
function buildDetailedTable() {
  const tbody = document.getElementById('detailedBody');
  tbody.innerHTML = '';
  let data = [...DATA];

  if (state.year !== 'all') data = data.filter(d => d.year == state.year);
  if (state.month !== 'all') data = data.filter(d => d.month === state.month);

  if (state.crossFilter) {
    if (state.crossFilter.type === 'year') data = data.filter(d => d.year === state.crossFilter.value);
    else if (state.crossFilter.type === 'month') data = data.filter(d => d.month === state.crossFilter.month && d.year === state.crossFilter.year);
  }

  let grand = 0;
  data.forEach(d => {
    const r1 = getFactoryValue(d,'riyadh1');
    const r2 = getFactoryValue(d,'riyadh2');
    const q1 = getFactoryValue(d,'qassim1');
    const q2 = getFactoryValue(d,'qassim2');
    const q3 = getFactoryValue(d,'qassim3');
    const total = r1+r2+q1+q2+q3;
    grand += total;
    const tr = document.createElement('tr');
    tr.innerHTML = `
      <td>${d.month}</td>
      <td>${d.year}</td>
      <td class="num">${fmt(r1)}</td>
      <td class="num">${fmt(r2)}</td>
      <td class="num">${fmt(q1)}</td>
      <td class="num">${fmt(q2)}</td>
      <td class="num">${fmt(q3)}</td>
      <td class="num" style="font-weight:700;color:#6B46C1">${fmt(total)}</td>
    `;
    tr.style.cursor = 'pointer';
    tr.addEventListener('click', () => {
      state.crossFilter = state.crossFilter && state.crossFilter.type === 'month' && state.crossFilter.month === d.month && state.crossFilter.year === d.year ? null : {type:'month', month:d.month, year:d.year};
      renderAll();
    });
    tbody.appendChild(tr);
  });

  const tr = document.createElement('tr');
  tr.className = 'total-row';
  const totals = data.reduce((acc, d) => {
    acc.r1 += getFactoryValue(d,'riyadh1');
    acc.r2 += getFactoryValue(d,'riyadh2');
    acc.q1 += getFactoryValue(d,'qassim1');
    acc.q2 += getFactoryValue(d,'qassim2');
    acc.q3 += getFactoryValue(d,'qassim3');
    return acc;
  }, {r1:0,r2:0,q1:0,q2:0,q3:0});
  tr.innerHTML = `
    <td>الإجمالي</td>
    <td>—</td>
    <td class="num">${fmt(totals.r1)}</td>
    <td class="num">${fmt(totals.r2)}</td>
    <td class="num">${fmt(totals.q1)}</td>
    <td class="num">${fmt(totals.q2)}</td>
    <td class="num">${fmt(totals.q3)}</td>
    <td class="num">${fmt(grand)}</td>
  `;
  tbody.appendChild(tr);
}

/* ============ RENDER ============ */
function renderAll() {
  updateKPIs();
  buildYearsChart();
  buildRegionsChart();
  buildMonthlyChart();
  buildFactoriesChart();
  buildSummaryTable();
  buildDetailedTable();
}

/* ============ EVENTS ============ */
document.querySelectorAll('.tab-btn').forEach(btn => {
  btn.addEventListener('click', () => {
    document.querySelectorAll('.tab-btn').forEach(b => b.classList.remove('active'));
    btn.classList.add('active');
    document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
    document.getElementById('page-' + btn.dataset.page).classList.add('active');
  });
});

document.getElementById('yearFilter').addEventListener('change', e => {
  state.year = e.target.value;
  updateMonthFilter();
  renderAll();
});

document.getElementById('monthFilter').addEventListener('change', e => {
  state.month = e.target.value;
  renderAll();
});

document.getElementById('yoyToggle').addEventListener('change', e => {
  state.yoy = e.target.checked;
  document.getElementById('yoyLabel').textContent = state.yoy ? 'تفعيل' : 'إيقاف';
  renderAll();
});

document.getElementById('regionFilter').addEventListener('change', e => {
  state.region = e.target.value;
  updateFactoryFilter();
  renderAll();
});

document.getElementById('factoryFilter').addEventListener('change', e => {
  state.factory = e.target.value;
  renderAll();
});

/* ============ INIT ============ */
updateMonthFilter();
updateFactoryFilter();
renderAll();
</script>
</body>
</html>
