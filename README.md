<html lang="th">
<head>
  <meta charset="UTF-8" />
  <title>ระบบยืมคืนอุปกรณ์กีฬาโรงเรียน</title>
  <!-- SweetAlert2 -->
  <script src="https://cdn.jsdelivr.net/npm/sweetalert2@11"></script>
  <!-- Chart.js สำหรับกราฟรายงาน -->
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
      background: linear-gradient(180deg, #ff9800, #ffb74d);
      color: #fff;
      padding: 20px 15px;
    }

    .sidebar h1 {
      font-size: 1.2rem;
      margin: 0 0 15px;
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
      background: rgba(255, 255, 255, 0.15);
      color: #fff;
      cursor: pointer;
      font-size: 0.95rem;
      display: flex;
      align-items: center;
      gap: 8px;
      transition: background 0.2s, transform 0.1s;
    }

    .nav-btn span.icon {
      font-size: 1.1rem;
    }

    .nav-btn.active,
    .nav-btn:hover {
      background: rgba(255, 255, 255, 0.3);
      transform: translateY(-1px);
    }

    .main-content {
      flex: 1;
      padding: 20px;
    }

    header {
      margin-bottom: 20px;
    }

    header h2 {
      margin: 0 0 5px;
    }

    header p {
      margin: 0;
      color: #555;
      font-size: 0.95rem;
    }

    .page {
      display: none;
      background: #fff8f0;
      border-radius: 16px;
      padding: 20px;
      box-shadow: 0 4px 10px rgba(0,0,0,0.05);
    }

    .page.active {
      display: block;
    }

    .card-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
      gap: 15px;
      margin-top: 15px;
      margin-bottom: 20px;
    }

    .card {
      background: #ffffff;
      border-radius: 12px;
      padding: 15px;
      box-shadow: 0 2px 6px rgba(0,0,0,0.05);
    }

    .card h3 {
      margin: 0 0 8px;
      font-size: 1rem;
    }

    .card p {
      margin: 0;
      font-size: 0.9rem;
      color: #555;
    }

    .form-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
      gap: 15px;
      margin-bottom: 15px;
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
      margin-top: 10px;
      margin-bottom: 15px;
    }

    .btn {
      border: none;
      border-radius: 999px;
      padding: 8px 16px;
      font-size: 0.9rem;
      cursor: pointer;
      color: #fff;
      display: inline-flex;
      align-items: center;
      gap: 6px;
      transition: transform 0.1s, box-shadow 0.1s;
    }

    .btn:active {
      transform: translateY(1px);
      box-shadow: none;
    }

    .btn-green {
      background: #4caf50;
      box-shadow: 0 2px 4px rgba(76,175,80,0.4);
    }

    .btn-blue {
      background: #2196f3;
      box-shadow: 0 2px 4px rgba(33,150,243,0.4);
    }

    .btn-yellow {
      background: #ffb300;
      box-shadow: 0 2px 4px rgba(255,179,0,0.4);
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

    th {
      background: #ffe0b2;
    }

    .badge {
      display: inline-block;
      padding: 2px 8px;
      border-radius: 999px;
      font-size: 0.75rem;
      background: #ffe0b2;
      color: #bf360c;
    }

    .announcement {
      list-style: none;
      padding-left: 18px;
      margin: 0;
      font-size: 0.88rem;
    }

    .announcement li::marker {
      color: #ff9800;
    }

    .section-title {
      margin-top: 0;
      margin-bottom: 10px;
      font-size: 1.05rem;
    }

    .chart-container {
      max-width: 500px;
      margin-top: 15px;
    }

    .muted {
      font-size: 0.8rem;
      color: #777;
    }

    @media (max-width: 768px) {
      .app {
        flex-direction: column;
      }

      .sidebar {
        width: 100%;
      }
    }
  </style>
</head>
<body>
<div class="app">
  <aside class="sidebar">
    <h1>ระบบยืมคืน<br>อุปกรณ์กีฬาโรงเรียน</h1>
    <small>Sports Equipment Borrowing System</small>
    <button class="nav-btn active" data-page="page-dashboard">
      <span class="icon">🏠</span> หน้าแรก (Dashboard)
    </button>
    <button class="nav-btn" data-page="page-equipment">
      <span class="icon">🏀</span> เพิ่ม/จัดการอุปกรณ์
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

  <main class="main-content">
    <header>
      <h2 id="page-title">หน้าแรก (Dashboard)</h2>
      <p id="page-subtitle">ภาพรวมการยืม–คืนอุปกรณ์กีฬา</p>
    </header>

    <!-- หน้า Dashboard -->
    <section id="page-dashboard" class="page active">
      <div class="card-grid">
        <div class="card">
          <h3>ภาพรวมอุปกรณ์ในระบบ</h3>
          <p>จำนวนอุปกรณ์ทั้งหมด: <strong id="total-equipment">-</strong></p>
          <p>จำนวนอุปกรณ์ที่มีการยืม: <strong id="total-borrowed">-</strong></p>
        </div>
        <div class="card">
          <h3>การยืมวันนี้</h3>
          <p>จำนวนครั้งที่ยืมวันนี้: <strong id="today-borrow-count">-</strong></p>
          <p>จำนวนผู้ยืมทั้งหมด: <strong id="total-members">-</strong></p>
        </div>
      </div>

      <div class="card">
        <h3>ข่าว / ประกาศ</h3>
        <ul class="announcement" id="announcement-list">
          <li>ประกาศ: โปรดตรวจสอบอุปกรณ์ก่อน–หลังยืมทุกครั้ง</li>
          <li>นักเรียนทุกคนควรคืนอุปกรณ์ภายในวันเดียวกัน หากไม่ได้รับอนุญาตเป็นพิเศษ</li>
        </ul>
      </div>
    </section>

    <!-- หน้าเพิ่มอุปกรณ์ -->
    <section id="page-equipment" class="page">
      <h3 class="section-title">เพิ่ม/จัดการอุปกรณ์กีฬา</h3>
      <div class="form-grid">
        <div>
          <label for="eq-name">ชื่ออุปกรณ์</label>
          <input type="text" id="eq-name" placeholder="เช่น ฟุตบอล, ลูกบาสเกตบอล" />
        </div>
        <div>
          <label for="eq-category">ประเภทอุปกรณ์</label>
          <input type="text" id="eq-category" placeholder="เช่น ฟุตบอล, วอลเลย์บอล, อื่น ๆ" />
        </div>
        <div>
          <label for="eq-qty">จำนวนทั้งหมด</label>
          <input type="number" id="eq-qty" min="0" />
        </div>
        <div>
          <label for="eq-location">สถานที่เก็บ</label>
          <input type="text" id="eq-location" placeholder="เช่น ห้องพละ ชั้น 1" />
        </div>
        <div style="grid-column: 1 / -1;">
          <label for="eq-desc">คำอธิบาย/รายละเอียดเพิ่มเติม</label>
          <textarea id="eq-desc" placeholder="ขนาด เบอร์ รุ่น ฯลฯ"></textarea>
        </div>
      </div>
      <div class="btn-row">
        <button class="btn btn-green" id="btnSaveEquipment">💾 บันทึกข้อมูลอุปกรณ์ลง Google Sheet</button>
        <button class="btn btn-blue" id="btnLoadEquipment">📂 เรียกดูข้อมูลอุปกรณ์จาก Google Sheet</button>
        <button class="btn btn-yellow" id="btnLoadEquipmentLocal">🗂 โหลดข้อมูลจาก Local Storage</button>
      </div>
      <div id="equipment-table-container"></div>
      <p class="muted">ข้อมูลอุปกรณ์ที่โหลดล่าสุดจะถูกเก็บไว้ใน Local Storage เพื่อเปิดดูซ้ำได้แม้ยังไม่โหลดจาก Google Sheet อีกครั้ง</p>
    </section>

    <!-- หน้ายืม–คืนอุปกรณ์ -->
    <section id="page-borrow" class="page">
      <h3 class="section-title">บันทึกการยืม–คืนอุปกรณ์กีฬา</h3>
      <div class="form-grid">
        <div>
          <label for="borrow-member">ผู้ยืม (สมาชิก)</label>
          <select id="borrow-member">
            <option value="">-- เลือกผู้ยืม --</option>
          </select>
        </div>
        <div>
          <label for="borrow-equipment">รายการอุปกรณ์</label>
          <select id="borrow-equipment">
            <option value="">-- เลือกอุปกรณ์ --</option>
          </select>
        </div>
        <div>
          <label for="borrow-qty">จำนวนที่ยืม</label>
          <input type="number" id="borrow-qty" min="1" value="1" />
        </div>
        <div>
          <label for="borrow-date">วันที่ยืม</label>
          <input type="date" id="borrow-date" />
        </div>
        <div>
          <label for="borrow-due">กำหนดคืน</label>
          <input type="date" id="borrow-due" />
        </div>
        <div>
          <label for="borrow-type">ประเภทบันทึก</label>
          <select id="borrow-type">
            <option value="borrow">ยืมอุปกรณ์</option>
            <option value="return">คืนอุปกรณ์</option>
          </select>
        </div>
      </div>
      <div class="btn-row">
        <button class="btn btn-green" id="btnSaveBorrow">✅ ยืนยันบันทึกการยืม/คืน (บันทึกลง Google Sheet)</button>
        <button class="btn btn-blue" id="btnLoadBorrow">📂 เรียกดูประวัติการยืม–คืนจาก Google Sheet</button>
        <button class="btn btn-yellow" id="btnLoadBorrowLocal">🗂 โหลดประวัติจาก Local Storage</button>
      </div>
      <div id="borrow-table-container"></div>
      <p class="muted">การบันทึกทุกครั้งจะถูกส่งไปยัง Google Sheet และสำรองไว้ใน Local Storage ด้วย</p>
    </section>

    <!-- หน้าเพิ่มสมาชิก -->
    <section id="page-member" class="page">
      <h3 class="section-title">เพิ่มผู้ยืม (สมาชิก)</h3>
      <div class="form-grid">
        <div>
          <label for="mem-id">รหัสนักเรียน/รหัสสมาชิก</label>
          <input type="text" id="mem-id" />
        </div>
        <div>
          <label for="mem-name">ชื่อ - นามสกุล</label>
          <input type="text" id="mem-name" />
        </div>
        <div>
          <label for="mem-class">ห้องเรียน / ชั้นปี</label>
          <input type="text" id="mem-class" placeholder="เช่น ม.2/1, ม.5/3" />
        </div>
        <div>
          <label for="mem-phone">เบอร์โทร (ถ้ามี)</label>
          <input type="text" id="mem-phone" />
        </div>
      </div>
      <div class="btn-row">
        <button class="btn btn-green" id="btnSaveMember">💾 บันทึกสมาชิกลง Google Sheet</button>
        <button class="btn btn-blue" id="btnLoadMember">📂 เรียกดูสมาชิกจาก Google Sheet</button>
        <button class="btn btn-yellow" id="btnLoadMemberLocal">🗂 โหลดสมาชิกจาก Local Storage</button>
      </div>
      <div id="member-table-container"></div>
      <p class="muted">เมื่อเพิ่มสมาชิกแล้ว จะสามารถเลือกชื่อจากหน้าบันทึกการยืม–คืนอุปกรณ์ได้</p>
    </section>

    <!-- หน้ารายงาน -->
    <section id="page-report" class="page">
      <h3 class="section-title">รายงานการยืมใช้และสถิติ</h3>
      <div class="btn-row">
        <button class="btn btn-blue" id="btnLoadReport">📂 ดึงข้อมูลรายงานจาก Google Sheet</button>
        <button class="btn btn-yellow" id="btnLoadReportLocal">🗂 โหลดข้อมูลรายงานจาก Local Storage</button>
      </div>
      <div class="card-grid">
        <div class="card">
          <h3>สรุปภาพรวม</h3>
          <p>จำนวนการยืมทั้งหมด: <strong id="report-total-borrow">-</strong></p>
          <p>จำนวนการคืนทั้งหมด: <strong id="report-total-return">-</strong></p>
          <p>จำนวนอุปกรณ์ที่มีการยืมมากที่สุด: <strong id="report-top-equipment">-</strong></p>
        </div>
        <div class="card">
          <h3>จำนวนสมาชิกปัจจุบัน</h3>
          <p><strong id="report-member-count">-</strong> คน</p>
        </div>
      </div>
      <div class="chart-container">
        <canvas id="borrowChart"></canvas>
      </div>
      <div id="report-table-container"></div>
      <p class="muted">ระบบจะคำนวณสถิติจากประวัติการยืม–คืนที่ดึงมาจาก Google Sheet และเก็บสำรองไว้ใน Local Storage ด้วย</p>
    </section>
  </main>
</div>

<script>
  // ---------- CONFIG ----------
  const APP_SCRIPT_URL = 'XXX'; // ใส่ URL Web App ของ Google Apps Script ตรงนี้

  // ---------- ตัวช่วย JSONP ----------
  function callAppsScript(params, onSuccess, onError) {
    const callbackName = 'gsCallback_' + Date.now() + '_' + Math.floor(Math.random() * 1000);
    params.callback = callbackName;

    const query = Object.keys(params)
      .map(k => encodeURIComponent(k) + '=' + encodeURIComponent(params[k]))
      .join('&');

    const script = document.createElement('script');
    script.src = APP_SCRIPT_URL + '?' + query;

    window[callbackName] = function(response) {
      delete window[callbackName];
      document.body.removeChild(script);
      if (response && response.success) {
        onSuccess && onSuccess(response);
      } else {
        onError && onError(response);
      }
    };

    script.onerror = function() {
      delete window[callbackName];
      document.body.removeChild(script);
      onError && onError({ success: false, message: 'ไม่สามารถติดต่อ Apps Script ได้' });
    };

    document.body.appendChild(script);
  }

  // ---------- เปลี่ยนหน้า ----------
  const pageTitle = document.getElementById('page-title');
  const pageSubtitle = document.getElementById('page-subtitle');
  const pageConfigs = {
    'page-dashboard': {
      title: 'หน้าแรก (Dashboard)',
      subtitle: 'ภาพรวมการยืม–คืนอุปกรณ์กีฬา'
    },
    'page-equipment': {
      title: 'เพิ่ม/จัดการอุปกรณ์กีฬา',
      subtitle: 'บันทึกและจัดการข้อมูลอุปกรณ์ในห้องพละ'
    },
    'page-borrow': {
      title: 'บันทึกการยืม–คืนอุปกรณ์',
      subtitle: 'บันทึกประวัติการยืมและคืนอุปกรณ์ของนักเรียน/ครู'
    },
    'page-member': {
      title: 'เพิ่มผู้ยืม (สมาชิก)',
      subtitle: 'เพิ่มข้อมูลผู้ยืมเพื่อใช้ในระบบยืม–คืน'
    },
    'page-report': {
      title: 'รายงานการยืมใช้',
      subtitle: 'ดูสถิติการยืม–คืนอุปกรณ์กีฬา'
    }
  };

  document.querySelectorAll('.nav-btn').forEach(btn => {
    btn.addEventListener('click', () => {
      document.querySelectorAll('.nav-btn').forEach(b => b.classList.remove('active'));
      btn.classList.add('active');
      const pageId = btn.getAttribute('data-page');
      document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
      document.getElementById(pageId).classList.add('active');

      if (pageConfigs[pageId]) {
        pageTitle.textContent = pageConfigs[pageId].title;
        pageSubtitle.textContent = pageConfigs[pageId].subtitle;
      }

      // โหลด dropdown เมื่อเข้าไปหน้าที่เกี่ยวข้อง
      if (pageId === 'page-borrow') {
        populateMemberDropdown();
        populateEquipmentDropdown();
      }
    });
  });

  // ---------- Local Storage Keys ----------
  const LS_KEYS = {
    EQUIPMENT: 'sports_equipment_data',
    MEMBERS: 'sports_member_data',
    BORROW: 'sports_borrow_data',
    REPORT: 'sports_report_data'
  };

  // ---------- แสดงตาราง ----------
  function renderEquipmentTable(data) {
    const container = document.getElementById('equipment-table-container');
    if (!data || data.length === 0) {
      container.innerHTML = '<p>ยังไม่มีข้อมูลอุปกรณ์</p>';
      return;
    }
    let html = '<table><thead><tr>';
    html += '<th>ชื่ออุปกรณ์</th><th>ประเภท</th><th>จำนวน</th><th>สถานที่เก็บ</th><th>รายละเอียด</th>';
    html += '</tr></thead><tbody>';
    data.forEach(row => {
      html += `<tr>
        <td>${row.name || ''}</td>
        <td>${row.category || ''}</td>
        <td>${row.quantity || ''}</td>
        <td>${row.location || ''}</td>
        <td>${row.description || ''}</td>
      </tr>`;
    });
    html += '</tbody></table>';
    container.innerHTML = html;
  }

  function renderMemberTable(data) {
    const container = document.getElementById('member-table-container');
    if (!data || data.length === 0) {
      container.innerHTML = '<p>ยังไม่มีข้อมูลสมาชิก</p>';
      return;
    }
    let html = '<table><thead><tr>';
    html += '<th>รหัสสมาชิก</th><th>ชื่อ - นามสกุล</th><th>ห้อง/ชั้นปี</th><th>เบอร์โทร</th>';
    html += '</tr></thead><tbody>';
    data.forEach(row => {
      html += `<tr>
        <td>${row.memberId || ''}</td>
        <td>${row.name || ''}</td>
        <td>${row.className || ''}</td>
        <td>${row.phone || ''}</td>
      </tr>`;
    });
    html += '</tbody></table>';
    container.innerHTML = html;
  }

  function renderBorrowTable(data) {
    const container = document.getElementById('borrow-table-container');
    if (!data || data.length === 0) {
      container.innerHTML = '<p>ยังไม่มีประวัติการยืม–คืน</p>';
      return;
    }
    let html = '<table><thead><tr>';
    html += '<th>วันที่</th><th>ประเภท</th><th>ผู้ยืม</th><th>อุปกรณ์</th><th>จำนวน</th><th>กำหนดคืน</th>';
    html += '</tr></thead><tbody>';
    data.forEach(row => {
      html += `<tr>
        <td>${row.date || ''}</td>
        <td>${row.type === 'borrow' ? 'ยืม' : 'คืน'}</td>
        <td>${row.memberName || ''}</td>
        <td>${row.equipmentName || ''}</td>
        <td>${row.quantity || ''}</td>
        <td>${row.dueDate || ''}</td>
      </tr>`;
    });
    html += '</tbody></table>';
    container.innerHTML = html;
  }

  // ---------- Dashboard ----------
  function updateDashboardFromLocal() {
    const eqData = JSON.parse(localStorage.getItem(LS_KEYS.EQUIPMENT) || '[]');
    const borrowData = JSON.parse(localStorage.getItem(LS_KEYS.BORROW) || '[]');
    const memberData = JSON.parse(localStorage.getItem(LS_KEYS.MEMBERS) || '[]');

    document.getElementById('total-equipment').textContent = eqData.length;
    document.getElementById('total-members').textContent = memberData.length;

    const todayStr = new Date().toISOString().slice(0, 10);
    const todayBorrow = borrowData.filter(b => b.type === 'borrow' && b.date === todayStr);
    document.getElementById('today-borrow-count').textContent = todayBorrow.length;
    document.getElementById('total-borrowed').textContent =
      borrowData.filter(b => b.type === 'borrow').length;
  }

  // ---------- Dropdown ----------
  function populateMemberDropdown() {
    const sel = document.getElementById('borrow-member');
    const memberData = JSON.parse(localStorage.getItem(LS_KEYS.MEMBERS) || '[]');
    sel.innerHTML = '<option value="">-- เลือกผู้ยืม --</option>';
    memberData.forEach(m => {
      const opt = document.createElement('option');
      opt.value = m.memberId || m.name;
      opt.textContent = `${m.memberId || ''} - ${m.name || ''}`;
      opt.dataset.name = m.name;
      sel.appendChild(opt);
    });
  }

  function populateEquipmentDropdown() {
    const sel = document.getElementById('borrow-equipment');
    const eqData = JSON.parse(localStorage.getItem(LS_KEYS.EQUIPMENT) || '[]');
    sel.innerHTML = '<option value="">-- เลือกอุปกรณ์ --</option>';
    eqData.forEach(e => {
      const opt = document.createElement('option');
      opt.value = e.name;
      opt.textContent = `${e.name} (คงเหลือ: ${e.quantity || '-'} ชิ้น)`;
      opt.dataset.name = e.name;
      sel.appendChild(opt);
    });
  }

  // ---------- ปุ่ม: บันทึก/โหลดอุปกรณ์ ----------
  document.getElementById('btnSaveEquipment').addEventListener('click', () => {
    const name = document.getElementById('eq-name').value.trim();
    const category = document.getElementById('eq-category').value.trim();
    const qty = document.getElementById('eq-qty').value.trim();
    const location = document.getElementById('eq-location').value.trim();
    const desc = document.getElementById('eq-desc').value.trim();

    if (!name || !qty) {
      Swal.fire('กรุณากรอกชื่ออุปกรณ์และจำนวน', '', 'warning');
      return;
    }

    Swal.fire({
      title: 'กำลังบันทึก...',
      didOpen: () => Swal.showLoading(),
      allowOutsideClick: false
    });

    callAppsScript(
      {
        action: 'addEquipment',
        name,
        category,
        quantity: qty,
        location,
        description: desc
      },
      (res) => {
        Swal.fire('บันทึกสำเร็จ', res.message || '', 'success');
        // อัปเดต Local Storage โดยการดึงใหม่
        loadEquipmentFromServer(false);
      },
      (err) => {
        Swal.fire('เกิดข้อผิดพลาด', err.message || 'บันทึกไม่สำเร็จ', 'error');
      }
    );
  });

  function loadEquipmentFromServer(showAlert = true) {
    if (showAlert) {
      Swal.fire({
        title: 'กำลังโหลดข้อมูลอุปกรณ์...',
        didOpen: () => Swal.showLoading(),
        allowOutsideClick: false
      });
    }

    callAppsScript(
      { action: 'getEquipment' },
      (res) => {
        const data = res.data || [];
        localStorage.setItem(LS_KEYS.EQUIPMENT, JSON.stringify(data));
        renderEquipmentTable(data);
        updateDashboardFromLocal();
        if (showAlert) {
          Swal.fire('สำเร็จ', 'โหลดข้อมูลอุปกรณ์จาก Google Sheet แล้ว', 'success');
        }
      },
      (err) => {
        if (showAlert) {
          Swal.fire('เกิดข้อผิดพลาด', err.message || 'ไม่สามารถโหลดข้อมูลได้', 'error');
        }
      }
    );
  }

  document.getElementById('btnLoadEquipment').addEventListener('click', () => {
    loadEquipmentFromServer(true);
  });

  document.getElementById('btnLoadEquipmentLocal').addEventListener('click', () => {
    const data = JSON.parse(localStorage.getItem(LS_KEYS.EQUIPMENT) || '[]');
    renderEquipmentTable(data);
    updateDashboardFromLocal();
  });

  // ---------- ปุ่ม: บันทึก/โหลดสมาชิก ----------
  document.getElementById('btnSaveMember').addEventListener('click', () => {
    const memberId = document.getElementById('mem-id').value.trim();
    const name = document.getElementById('mem-name').value.trim();
    const className = document.getElementById('mem-class').value.trim();
    const phone = document.getElementById('mem-phone').value.trim();

    if (!memberId || !name) {
      Swal.fire('กรุณากรอกรหัสและชื่อสมาชิก', '', 'warning');
      return;
    }

    Swal.fire({
      title: 'กำลังบันทึกสมาชิก...',
      didOpen: () => Swal.showLoading(),
      allowOutsideClick: false
    });

    callAppsScript(
      {
        action: 'addMember',
        memberId,
        name,
        className,
        phone
      },
      (res) => {
        Swal.fire('บันทึกสำเร็จ', res.message || '', 'success');
        loadMemberFromServer(false);
      },
      (err) => {
        Swal.fire('เกิดข้อผิดพลาด', err.message || 'บันทึกไม่สำเร็จ', 'error');
      }
    );
  });

  function loadMemberFromServer(showAlert = true) {
    if (showAlert) {
      Swal.fire({
        title: 'กำลังโหลดข้อมูลสมาชิก...',
        didOpen: () => Swal.showLoading(),
        allowOutsideClick: false
      });
    }

    callAppsScript(
      { action: 'getMembers' },
      (res) => {
        const data = res.data || [];
        localStorage.setItem(LS_KEYS.MEMBERS, JSON.stringify(data));
        renderMemberTable(data);
        updateDashboardFromLocal();
        if (showAlert) {
          Swal.fire('สำเร็จ', 'โหลดข้อมูลสมาชิกจาก Google Sheet แล้ว', 'success');
        }
      },
      (err) => {
        if (showAlert) {
          Swal.fire('เกิดข้อผิดพลาด', err.message || 'ไม่สามารถโหลดข้อมูลสมาชิกได้', 'error');
        }
      }
    );
  }

  document.getElementById('btnLoadMember').addEventListener('click', () => {
    loadMemberFromServer(true);
  });

  document.getElementById('btnLoadMemberLocal').addEventListener('click', () => {
    const data = JSON.parse(localStorage.getItem(LS_KEYS.MEMBERS) || '[]');
    renderMemberTable(data);
    updateDashboardFromLocal();
  });

  // ---------- ปุ่ม: บันทึก/โหลดการยืม–คืน ----------
  document.getElementById('btnSaveBorrow').addEventListener('click', () => {
    const memberSelect = document.getElementById('borrow-member');
    const equipmentSelect = document.getElementById('borrow-equipment');
    const qty = document.getElementById('borrow-qty').value.trim();
    const date = document.getElementById('borrow-date').value;
    const dueDate = document.getElementById('borrow-due').value;
    const type = document.getElementById('borrow-type').value;

    if (!memberSelect.value || !equipmentSelect.value || !qty || !date) {
      Swal.fire('กรุณากรอกข้อมูลให้ครบ (ผู้ยืม, อุปกรณ์, จำนวน, วันที่ยืม)', '', 'warning');
      return;
    }

    const memberName = memberSelect.options[memberSelect.selectedIndex].textContent;
    const equipmentName = equipmentSelect.options[equipmentSelect.selectedIndex].dataset.name || equipmentSelect.value;

    Swal.fire({
      title: 'กำลังบันทึกการยืม/คืน...',
      didOpen: () => Swal.showLoading(),
      allowOutsideClick: false
    });

    callAppsScript(
      {
        action: 'addBorrow',
        memberId: memberSelect.value,
        memberName,
        equipmentName,
        quantity: qty,
        date,
        dueDate,
        type
      },
      (res) => {
        Swal.fire('บันทึกสำเร็จ', res.message || '', 'success');
        loadBorrowFromServer(false);
      },
      (err) => {
        Swal.fire('เกิดข้อผิดพลาด', err.message || 'บันทึกไม่สำเร็จ', 'error');
      }
    );
  });

  function loadBorrowFromServer(showAlert = true) {
    if (showAlert) {
      Swal.fire({
        title: 'กำลังโหลดประวัติการยืม–คืน...',
        didOpen: () => Swal.showLoading(),
        allowOutsideClick: false
      });
    }

    callAppsScript(
      { action: 'getBorrowRecords' },
      (res) => {
        const data = res.data || [];
        localStorage.setItem(LS_KEYS.BORROW, JSON.stringify(data));
        renderBorrowTable(data);
        updateDashboardFromLocal();
        if (showAlert) {
          Swal.fire('สำเร็จ', 'โหลดประวัติการยืม–คืนจาก Google Sheet แล้ว', 'success');
        }
      },
      (err) => {
        if (showAlert) {
          Swal.fire('เกิดข้อผิดพลาด', err.message || 'ไม่สามารถโหลดประวัติได้', 'error');
        }
      }
    );
  }

  document.getElementById('btnLoadBorrow').addEventListener('click', () => {
    loadBorrowFromServer(true);
  });

  document.getElementById('btnLoadBorrowLocal').addEventListener('click', () => {
    const data = JSON.parse(localStorage.getItem(LS_KEYS.BORROW) || '[]');
    renderBorrowTable(data);
    updateDashboardFromLocal();
  });

  // ---------- รายงาน ----------
  let borrowChart = null;

  function buildReportFromData(borrowData, memberData) {
    const totalBorrow = borrowData.filter(b => b.type === 'borrow').length;
    const totalReturn = borrowData.filter(b => b.type === 'return').length;

    document.getElementById('report-total-borrow').textContent = totalBorrow;
    document.getElementById('report-total-return').textContent = totalReturn;
    document.getElementById('report-member-count').textContent = memberData.length;

    // นับอุปกรณ์ที่ถูกยืมบ่อย
    const countByEquipment = {};
    borrowData.forEach(b => {
      if (!b.equipmentName) return;
      if (!countByEquipment[b.equipmentName]) {
        countByEquipment[b.equipmentName] = 0;
      }
      if (b.type === 'borrow') {
        countByEquipment[b.equipmentName] += Number(b.quantity || 0);
      }
    });

    let topEq = '-';
    if (Object.keys(countByEquipment).length > 0) {
      topEq = Object.entries(countByEquipment).sort((a, b) => b[1] - a[1])[0][0];
    }
    document.getElementById('report-top-equipment').textContent = topEq;

    // สร้างกราฟจำนวนการยืมตามอุปกรณ์
    const labels = Object.keys(countByEquipment);
    const values = Object.values(countByEquipment);

    const ctx = document.getElementById('borrowChart').getContext('2d');
    if (borrowChart) {
      borrowChart.destroy();
    }
    borrowChart = new Chart(ctx, {
      type: 'bar',
      data: {
        labels,
        datasets: [{
          label: 'จำนวนที่ถูกยืม',
          data: values
        }]
      },
      options: {
        responsive: true,
        plugins: {
          legend: { display: false }
        },
        scales: {
          y: {
            beginAtZero: true
          }
        }
      }
    });

    // แสดงตารางสรุป
    const container = document.getElementById('report-table-container');
    if (borrowData.length === 0) {
      container.innerHTML = '<p>ยังไม่มีข้อมูลประวัติการยืม–คืน</p>';
      return;
    }
    let html = '<table><thead><tr>';
    html += '<th>วันที่</th><th>ประเภท</th><th>ผู้ยืม</th><th>อุปกรณ์</th><th>จำนวน</th><th>กำหนดคืน</th>';
    html += '</tr></thead><tbody>';
    borrowData.forEach(row => {
      html += `<tr>
        <td>${row.date || ''}</td>
        <td>${row.type === 'borrow' ? 'ยืม' : 'คืน'}</td>
        <td>${row.memberName || ''}</td>
        <td>${row.equipmentName || ''}</td>
        <td>${row.quantity || ''}</td>
        <td>${row.dueDate || ''}</td>
      </tr>`;
    });
    html += '</tbody></table>';
    container.innerHTML = html;
  }

  document.getElementById('btnLoadReport').addEventListener('click', () => {
    Swal.fire({
      title: 'กำลังโหลดข้อมูลรายงาน...',
      didOpen: () => Swal.showLoading(),
      allowOutsideClick: false
    });

    callAppsScript(
      { action: 'getBorrowRecords' },
      (res) => {
        const borrowData = res.data || [];
        localStorage.setItem(LS_KEYS.BORROW, JSON.stringify(borrowData));
        // โหลดสมาชิกเพื่อใช้ในรายงาน
        callAppsScript(
          { action: 'getMembers' },
          (res2) => {
            const memberData = res2.data || [];
            localStorage.setItem(LS_KEYS.MEMBERS, JSON.stringify(memberData));
            buildReportFromData(borrowData, memberData);
            updateDashboardFromLocal();
            localStorage.setItem(LS_KEYS.REPORT, JSON.stringify({ borrowData, memberData }));
            Swal.fire('สำเร็จ', 'โหลดข้อมูลรายงานจาก Google Sheet แล้ว', 'success');
          },
          (err2) => {
            Swal.fire('เกิดข้อผิดพลาด', err2.message || 'ไม่สามารถโหลดข้อมูลสมาชิกได้', 'error');
          }
        );
      },
      (err) => {
        Swal.fire('เกิดข้อผิดพลาด', err.message || 'ไม่สามารถโหลดประวัติการยืม–คืนได้', 'error');
      }
    );
  });

  document.getElementById('btnLoadReportLocal').addEventListener('click', () => {
    const report = JSON.parse(localStorage.getItem(LS_KEYS.REPORT) || '{}');
    const borrowData = report.borrowData || JSON.parse(localStorage.getItem(LS_KEYS.BORROW) || '[]');
    const memberData = report.memberData || JSON.parse(localStorage.getItem(LS_KEYS.MEMBERS) || '[]');
    buildReportFromData(borrowData, memberData);
    updateDashboardFromLocal();
  });

  // ---------- Initial ----------
  // โหลดจาก LocalStorage เท่าที่มี เพื่อให้ Dashboard ไม่ว่างเปล่า
  updateDashboardFromLocal();
</script>
</body>
</html>
