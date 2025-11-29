<html lang="th">
<head>
  <meta charset="UTF-8" />
  <title>ระบบยืม–คืนอุปกรณ์กีฬาโรงเรียนสุรศักดิ์มนตรี</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <!-- SweetAlert2 -->
  <script src="https://cdn.jsdelivr.net/npm/sweetalert2@11"></script>
  <!-- Chart.js -->
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <style>
    * {
      box-sizing: border-box;
      font-family: "Sarabun", system-ui, sans-serif;
    }
    body {
      margin: 0;
      background: #ffedd5; /* ส้มอ่อน */
      color: #333;
    }
    .app {
      display: flex;
      min-height: 100vh;
    }
    .sidebar {
      width: 260px;
      background: linear-gradient(180deg, #ff9800, #ffb74d); /* แท็บเมนูส้มไล่เฉด */
      color: #fff;
      padding: 20px 15px;
    }
    .sidebar h1 {
      font-size: 1.2rem;
      margin: 0 0 10px;
      line-height: 1.4;
    }
    .sidebar small {
      display: block;
      opacity: 0.9;
      margin-bottom: 20px;
    }
    .nav-btn {
      width: 100%;
      text-align: left;
      padding: 10px 12px;
      margin-bottom: 8px;
      border: none;
      border-radius: 8px;
      background: rgba(255,255,255,0.15);
      color: #fff;
      cursor: pointer;
      font-size: 0.95rem;
      display: flex;
      align-items: center;
      gap: 8px;
      transition: background 0.2s, transform 0.1s;
    }
    .nav-btn span.icon { font-size: 1.1rem; }
    .nav-btn.active,
    .nav-btn:hover {
      background: rgba(255,255,255,0.3);
      transform: translateY(-1px);
    }
    .main-content {
      flex: 1;
      padding: 20px;
    }
    header h2 { margin: 0 0 5px; }
    header p {
      margin: 0 0 16px;
      color: #555;
      font-size: 0.9rem;
    }
    .page {
      display: none;
      background: #fff8f0;
      border-radius: 16px;
      padding: 20px;
      box-shadow: 0 4px 10px rgba(0,0,0,0.05);
    }
    .page.active { display: block; }
    .card-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
      gap: 15px;
      margin-bottom: 20px;
    }
    .card {
      background: #ffffff;
      border-radius: 12px;
      padding: 15px;
      box-shadow: 0 2px 6px rgba(0,0,0,0.05);
    }
    .card h3 { margin: 0 0 8px; font-size: 1rem; }
    .card p { margin: 0; font-size: 0.9rem; color: #555; }
    .form-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
      gap: 15px;
      margin-bottom: 10px;
    }
    label {
      display: block;
      font-size: 0.9rem;
      margin-bottom: 4px;
      color: #444;
    }
    input[type="text"],
    input[type="number"],
    input[type="date"],
    select,
    textarea {
      width: 100%;
      padding: 8px 10px;
      border-radius: 8px;
      border: 1px solid #ddd;
      font-size: 0.9rem;
    }
    textarea {
      resize: vertical;
      min-height: 60px;
    }
    .btn-row {
      display: flex;
      flex-wrap: wrap;
      gap: 10px;
      margin: 10px 0 15px;
    }
    .btn {
      border: none;
      border-radius: 999px;
      padding: 8px 16px;
      font-size: 0.9rem;
      cursor: pointer;
      display: inline-flex;
      align-items: center;
      gap: 6px;
      transition: transform 0.1s, box-shadow 0.1s;
      color: #fff;
    }
    .btn:active {
      transform: translateY(1px);
      box-shadow: none;
    }
    .btn-green {
      background: #4caf50; /* เขียว */
      box-shadow: 0 2px 4px rgba(76,175,80,0.5);
    }
    .btn-blue {
      background: #2196f3; /* ฟ้า */
      box-shadow: 0 2px 4px rgba(33,150,243,0.5);
    }
    .btn-yellow {
      background: #ffb300; /* เหลือง */
      box-shadow: 0 2px 4px rgba(255,179,0,0.5);
      color: #333;
    }
    table {
      width: 100%;
      border-collapse: collapse;
      margin-top: 10px;
      font-size: 0.85rem;
    }
    th, td {
      border: 1px solid #eee;
      padding: 6px 8px;
      text-align: left;
    }
    th { background: #ffe0b2; }
    ul.announcement {
      list-style: disc;
      padding-left: 22px;
      margin: 4px 0 0;
      font-size: 0.88rem;
    }
    .section-title {
      margin-top: 0;
      margin-bottom: 12px;
      font-size: 1.05rem;
    }
    .chart-container {
      max-width: 480px;
      margin-top: 15px;
    }
    .muted {
      font-size: 0.8rem;
      color: #777;
    }
    @media (max-width: 768px) {
      .app { flex-direction: column; }
      .sidebar { width: 100%; }
    }
  </style>
</head>
<body>
<div class="app">
  <!-- SIDEBAR -->
  <aside class="sidebar">
    <h1>ระบบยืม–คืนอุปกรณ์กีฬา<br>โรงเรียนสุรศักดิ์มนตรี</h1>
    <small>Sports Equipment Borrowing System – Surasakmontree School</small>

    <button class="nav-btn active" data-page="page-dashboard">
      <span class="icon">🏠</span> หน้าแรก (Dashboard)
    </button>
    <button class="nav-btn" data-page="page-equipment">
      <span class="icon">🏀</span> เพิ่มอุปกรณ์กีฬา
    </button>
    <button class="nav-btn" data-page="page-borrow">
      <span class="icon">🤝</span> ยืม–คืนอุปกรณ์
    </button>
    <button class="nav-btn" data-page="page-member">
      <span class="icon">➕</span> เพิ่มผู้ยืม (สมาชิก)
    </button>
    <button class="nav-btn" data-page="page-report">
      <span class="icon">📊</span> รายงานการยืมใช้
    </button>
  </aside>

  <!-- MAIN -->
  <main class="main-content">
    <header>
      <h2 id="page-title">หน้าแรก (Dashboard)</h2>
      <p id="page-subtitle">ภาพรวมการยืม–คืนอุปกรณ์กีฬาโรงเรียนสุรศักดิ์มนตรี และเมนูนำทางหลัก</p>
    </header>

    <!-- 1. DASHBOARD -->
    <section id="page-dashboard" class="page active">
      <div class="card-grid">
        <div class="card">
          <h3>ภาพรวมอุปกรณ์</h3>
          <p>จำนวนอุปกรณ์ทั้งหมด: <strong id="dash-total-equipment">0</strong></p>
          <p>จำนวนอุปกรณ์ที่ถูกยืมอยู่: <strong id="dash-total-borrowed">0</strong></p>
        </div>
        <div class="card">
          <h3>ภาพรวมการยืม</h3>
          <p>จำนวนการยืมวันนี้: <strong id="dash-today-borrow">0</strong></p>
          <p>จำนวนผู้ยืม/สมาชิกทั้งหมด: <strong id="dash-total-members">0</strong></p>
        </div>
      </div>

      <div class="card">
        <h3>ข่าวประกาศ / ข้อเตือนการใช้อุปกรณ์กีฬา</h3>
        <ul class="announcement">
          <li>โปรดตรวจเช็กสภาพอุปกรณ์ก่อนและหลังการใช้งาน</li>
          <li>อุปกรณ์ที่ยืมต้องคืนภายในวันเดียวกัน เว้นแต่ได้รับอนุญาตเป็นพิเศษ</li>
        </ul>
      </div>
    </section>

    <!-- 2. ADD EQUIPMENT -->
    <section id="page-equipment" class="page">
      <h3 class="section-title">เพิ่มอุปกรณ์กีฬา (Add Equipment)</h3>
      <div class="form-grid">
        <div>
          <label for="eq-name">ชื่ออุปกรณ์กีฬา</label>
          <input id="eq-name" type="text" placeholder="เช่น ฟุตบอล, ลูกบาส, ไม้แบด" />
        </div>
        <div>
          <label for="eq-category">ประเภท/หมวดหมู่</label>
          <input id="eq-category" type="text" placeholder="ฟุตบอล, วอลเลย์บอล, ฟิตเนส ฯลฯ" />
        </div>
        <div>
          <label for="eq-qty">จำนวนคงเหลือ/จำนวนทั้งหมด</label>
          <input id="eq-qty" type="number" min="0" />
        </div>
        <div>
          <label for="eq-location">สถานที่เก็บ</label>
          <input id="eq-location" type="text" placeholder="เช่น ห้องพละ ชั้น 1, ห้องเก็บอุปกรณ์กีฬา" />
        </div>
        <div style="grid-column: 1 / -1;">
          <label for="eq-desc">รายละเอียด/คำอธิบายเพิ่มเติม</label>
          <textarea id="eq-desc" placeholder="ขนาด, เบอร์อุปกรณ์, รุ่น/ยี่ห้อ เป็นต้น"></textarea>
        </div>
      </div>
      <div class="btn-row">
        <button class="btn btn-green" id="btnEqSave">💾 บันทึกข้อมูลใน Google Sheet</button>
        <button class="btn btn-blue" id="btnEqLoad">📂 เรียกดูข้อมูล Google Sheet</button>
      </div>
      <div id="equipment-table-container"></div>
      <p class="muted">เมื่อดึงข้อมูลจาก Google Sheet แล้ว ระบบจะเก็บซ้ำใน Local Storage เพื่อเรียกดูได้เร็วขึ้น</p>
    </section>

    <!-- 3. BORROW / RETURN -->
    <section id="page-borrow" class="page">
      <h3 class="section-title">ยืม–คืนอุปกรณ์กีฬา (Borrow / Return)</h3>
      <div class="form-grid">
        <div>
          <label for="borrow-member">ผู้ยืม (สมาชิก)</label>
          <select id="borrow-member">
            <option value="">-- เลือกชื่อสมาชิก --</option>
          </select>
        </div>
        <div>
          <label for="borrow-equipment">อุปกรณ์กีฬา</label>
          <select id="borrow-equipment">
            <option value="">-- เลือกอุปกรณ์กีฬา --</option>
          </select>
        </div>
        <div>
          <label for="borrow-qty">จำนวนที่ยืม</label>
          <input id="borrow-qty" type="number" min="1" value="1" />
        </div>
        <div>
          <label for="borrow-date">วันที่ยืม/บันทึก</label>
          <input id="borrow-date" type="date" />
        </div>
        <div>
          <label for="borrow-due">วันที่กำหนดคืน</label>
          <input id="borrow-due" type="date" />
        </div>
        <div>
          <label for="borrow-type">ประเภทการบันทึก</label>
          <select id="borrow-type">
            <option value="borrow">ยืมอุปกรณ์</option>
            <option value="return">คืนอุปกรณ์</option>
          </select>
        </div>
      </div>
      <div class="btn-row">
        <button class="btn btn-green" id="btnBorrowSave">✅ ยืนยันบันทึกการยืม/คืน</button>
        <button class="btn btn-blue" id="btnBorrowLoad">📂 เรียกดูข้อมูล Google Sheet</button>
      </div>
      <div id="borrow-table-container"></div>
      <p class="muted">ประวัติการยืม–คืนล่าสุดจะถูกเก็บใน Local Storage เพื่อเรียกดูได้แม้ยังไม่ดึงข้อมูลใหม่</p>
    </section>

    <!-- 4. ADD MEMBER -->
    <section id="page-member" class="page">
      <h3 class="section-title">เพิ่มผู้ยืม (สมาชิก)</h3>
      <div class="form-grid">
        <div>
          <label for="mem-id">รหัสนักเรียน / รหัสสมาชิก</label>
          <input id="mem-id" type="text" />
        </div>
        <div>
          <label for="mem-name">ชื่อ–นามสกุล</label>
          <input id="mem-name" type="text" />
        </div>
        <div>
          <label for="mem-class">ห้องเรียน / ชั้นปี</label>
          <input id="mem-class" type="text" placeholder="เช่น ม.2/1, ม.5/3" />
        </div>
        <div>
          <label for="mem-phone">เบอร์โทรศัพท์ (ถ้ามี)</label>
          <input id="mem-phone" type="text" />
        </div>
      </div>
      <div class="btn-row">
        <button class="btn btn-green" id="btnMemSave">💾 เพิ่มสมาชิก / บันทึกใน Google Sheet</button>
        <button class="btn btn-blue" id="btnMemLoad">📂 เรียกดูข้อมูล Google Sheet</button>
      </div>
      <div id="member-table-container"></div>
      <p class="muted">รายชื่อสมาชิกจะถูกใช้เป็น Dropdown ในหน้าการยืม–คืนอุปกรณ์</p>
    </section>

    <!-- 5. REPORT -->
    <section id="page-report" class="page">
      <h3 class="section-title">รายงานการยืม–คืนอุปกรณ์ (Borrowing Report)</h3>
      <div class="btn-row">
        <button class="btn btn-blue" id="btnReportLoad">🔄 เรียกดูข้อมูล Google Sheet / อัปเดตรายงาน</button>
      </div>
      <div class="card-grid">
        <div class="card">
          <h3>สรุปภาพรวม</h3>
          <p>จำนวนการยืมทั้งหมด: <strong id="rep-total-borrow">0</strong></p>
          <p>จำนวนการคืนทั้งหมด: <strong id="rep-total-return">0</strong></p>
          <p>อุปกรณ์ที่ถูกยืมบ่อยที่สุด: <strong id="rep-top-equipment">-</strong></p>
        </div>
        <div class="card">
          <h3>จำนวนสมาชิกผู้ยืม</h3>
          <p><strong id="rep-member-count">0</strong> คน</p>
        </div>
      </div>
      <div class="chart-container">
        <canvas id="borrowChart"></canvas>
      </div>
      <div id="report-table-container"></div>
      <p class="muted">ข้อมูลล่าสุดจะถูกเก็บไว้เป็นสำรองใน Local Storage ไม่ต้องดึงจาก Google Sheet ทุกครั้ง</p>
    </section>
  </main>
</div>

<script>
  /* ================== CONFIG ================== */
  // ❗ ต้องเปลี่ยนค่านี้เป็น URL Web App ของ Apps Script
  // ที่ผูกกับ Google Sheet ID: 1o9B3fb1E6I8iB6naDcXbwR9CONZT2QbSemUQ9JguWpc
  const APP_SCRIPT_URL = 'https://script.google.com/macros/s/XXXX/exec';

  const LS_KEYS = {
    EQUIP: 'ssm_sports_equipment',
    MEMBER: 'ssm_sports_members',
    BORROW: 'ssm_sports_borrow',
  };

  /* ================== JSONP HELPER ================== */
  function callAppsScript(params, onSuccess, onError) {
    const callbackName = 'gsCallback_' + Date.now() + '_' + Math.floor(Math.random()*1000);
    params.callback = callbackName;

    const query = Object.keys(params)
      .map(k => encodeURIComponent(k) + '=' + encodeURIComponent(params[k]))
      .join('&');

    const script = document.createElement('script');
    script.src = APP_SCRIPT_URL + '?' + query;

    window[callbackName] = function(res) {
      delete window[callbackName];
      document.body.removeChild(script);
      if (res && res.success) {
        onSuccess && onSuccess(res);
      } else {
        onError && onError(res || {success:false, message:'Unknown error'});
      }
    };

    script.onerror = function() {
      delete window[callbackName];
      document.body.removeChild(script);
      onError && onError({success:false, message:'ไม่สามารถติดต่อ Google Apps Script ได้'});
    };

    document.body.appendChild(script);
  }

  /* ================== LOCAL STORAGE ================== */
  function getLS(key) {
    try { return JSON.parse(localStorage.getItem(key) || '[]'); }
    catch(e) { return []; }
  }
  function setLS(key, value) {
    localStorage.setItem(key, JSON.stringify(value));
  }

  /* ================== NAVIGATION ================== */
  const pageTitle = document.getElementById('page-title');
  const pageSubtitle = document.getElementById('page-subtitle');
  const pageMeta = {
    'page-dashboard': {
      title: 'หน้าแรก (Dashboard)',
      subtitle: 'ภาพรวมการยืม–คืนอุปกรณ์กีฬาโรงเรียนสุรศักดิ์มนตรี และเมนูนำทางหลัก'
    },
    'page-equipment': {
      title: 'หน้าเพิ่มอุปกรณ์กีฬา',
      subtitle: 'บันทึกและจัดการข้อมูลอุปกรณ์กีฬาในห้องพละ'
    },
    'page-borrow': {
      title: 'หน้ายืม–คืนอุปกรณ์กีฬา',
      subtitle: 'บันทึกการยืมและการคืนอุปกรณ์ของนักเรียนและครู'
    },
    'page-member': {
      title: 'หน้าเพิ่มผู้ยืม (สมาชิก)',
      subtitle: 'จัดการข้อมูลสมาชิกที่สามารถยืมอุปกรณ์กีฬาได้'
    },
    'page-report': {
      title: 'หน้ารายงานการยืม–คืนอุปกรณ์',
      subtitle: 'ตรวจสอบสถิติการยืม–คืนอุปกรณ์กีฬา'
    }
  };

  document.querySelectorAll('.nav-btn').forEach(btn => {
    btn.addEventListener('click', () => {
      document.querySelectorAll('.nav-btn').forEach(b => b.classList.remove('active'));
      btn.classList.add('active');

      const pageId = btn.getAttribute('data-page');
      document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
      document.getElementById(pageId).classList.add('active');

      if (pageMeta[pageI]()
