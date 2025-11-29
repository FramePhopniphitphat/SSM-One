<html lang="th">
<head>
  <meta charset="UTF-8" />
  <title>ระบบยืม–คืนอุปกรณ์กีฬาโรงเรียนสุรศักดิ์มนตรี</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <!-- SweetAlert2 -->
  <script src="https://cdn.jsdelivr.net/npm/sweetalert2@11"></script>
  <!-- Chart.js ใช้แสดงกราฟในหน้ารายงาน -->
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <style>
    * {
      box-sizing: border-box;
      font-family: "Sarabun", system-ui, sans-serif;
    }
    body {
      margin: 0;
      background: #ffedd5; /* พื้นหลังโทนสีส้มอ่อน */
      color: #333;
    }
    .app {
      display: flex;
      min-height: 100vh;
    }
    /* SIDEBAR */
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
      background: rgba(255,255,255,0.18);
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
      background: rgba(255,255,255,0.3);
      transform: translateY(-1px);
    }

    /* MAIN */
    .main-content {
      flex: 1;
      padding: 20px;
    }
    header h2 {
      margin: 0 0 4px;
    }
    header p {
      margin: 0 0 16px;
      font-size: 0.9rem;
      color: #555;
    }
    .page {
      display: none;
      background: #fffaf2;
      border-radius: 16px;
      padding: 20px;
      box-shadow: 0 4px 10px rgba(0,0,0,0.05);
    }
    .page.active {
      display: block;
    }

    .card-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
      gap: 15px;
      margin-bottom: 16px;
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

    .section-title {
      margin-top: 0;
      margin-bottom: 10px;
      font-size: 1.05rem;
    }
    .form-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
      gap: 14px;
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
      color: #fff;
      transition: transform 0.1s, box-shadow 0.1s;
    }
    .btn:active {
      transform: translateY(1px);
      box-shadow: none;
    }
    .btn-green {
      background: #4caf50; /* ปุ่มสีเขียว */
      box-shadow: 0 2px 4px rgba(76,175,80,0.4);
    }
    .btn-blue {
      background: #2196f3; /* ปุ่มสีฟ้า */
      box-shadow: 0 2px 4px rgba(33,150,243,0.4);
    }
    .btn-yellow {
      background: #f9a825; /* ปุ่มสีเหลือง */
      color: #333;
      box-shadow: 0 2px 4px rgba(249,168,37,0.4);
    }
    .btn-gray {
      background: #6b7280;
      box-shadow: 0 2px 4px rgba(107,114,128,0.4);
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
    ul.announcement {
      list-style: disc;
      padding-left: 22px;
      margin: 4px 0 0;
      font-size: 0.88rem;
    }
    .muted {
      font-size: 0.8rem;
      color: #777;
    }
    .chart-container {
      max-width: 480px;
      margin-top: 15px;
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
  <!-- SIDEBAR -->
  <aside class="sidebar">
    <h1>ระบบยืม–คืนอุปกรณ์กีฬา<br>โรงเรียนสุรศักดิ์มนตรี</h1>
    <small>Sports Equipment Borrow & Return – SSM</small>

    <button class="nav-btn active" data-page="page-dashboard">
      <span class="icon">🏠</span> หน้าแรก (Dashboard)
    </button>
    <button class="nav-btn" data-page="page-equipment">
      <span class="icon">🏀</span> หน้าเพิ่มอุปกรณ์
    </button>
    <button class="nav-btn" data-page="page-borrow">
      <span class="icon">🤝</span> หน้ายืม–คืนอุปกรณ์
    </button>
    <button class="nav-btn" data-page="page-member">
      <span class="icon">➕</span> หน้าเพิ่มผู้ยืม (สมาชิก)
    </button>
    <button class="nav-btn" data-page="page-report">
      <span class="icon">📊</span> หน้ารายงานสถิติ
    </button>
  </aside>

  <!-- MAIN CONTENT -->
  <main class="main-content">
    <header>
      <h2 id="page-title">หน้าแรก (Dashboard)</h2>
      <p id="page-subtitle">
        แสดงภาพรวมของการยืม–คืนอุปกรณ์กีฬา และทางลัดไปยังเมนูหลักของระบบ
      </p>
    </header>

    <!-- 1. DASHBOARD -->
    <section id="page-dashboard" class="page active">
      <div class="card-grid">
        <div class="card">
          <h3>ภาพรวมอุปกรณ์ในระบบ</h3>
          <p>จำนวนอุปกรณ์ทั้งหมด: <strong id="dash-total-equipment">0</strong></p>
          <p>จำนวนอุปกรณ์ที่ถูกยืมอยู่ตอนนี้: <strong id="dash-total-borrowed">0</strong></p>
        </div>
        <div class="card">
          <h3>ภาพรวมการยืมวันนี้</h3>
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

    <!-- 2. ADD EQUIPMENT PAGE -->
    <section id="page-equipment" class="page">
      <h3 class="section-title">หน้าเพิ่มอุปกรณ์กีฬา (Add Equipment)</h3>
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
          <input id="eq-location" type="text" placeholder="เช่น ห้องพละ ชั้น 1" />
        </div>
        <div style="grid-column: 1/-1;">
          <label for="eq-desc">รายละเอียด/คำอธิบายเพิ่มเติม</label>
          <textarea id="eq-desc" placeholder="ขนาด เบอร์อุปกรณ์ รุ่น/ยี่ห้อ ฯลฯ"></textarea>
        </div>
      </div>
      <div class="btn-row">
        <button class="btn btn-green" id="btnEqSave">💾 บันทึกข้อมูลใน Google Sheet</button>
        <button class="btn btn-blue" id="btnEqLoad">📂 เรียกดูข้อมูล Google Sheet</button>
      </div>
      <div id="equipment-table-container"></div>
      <p class="muted">
        เมื่อดึงข้อมูลจาก Google Sheet แล้ว ระบบจะเก็บซ้ำใน Local Storage
        เพื่อเรียกดูได้โดยไม่ต้องโหลดใหม่ทุกครั้ง
      </p>
    </section>

    <!-- 3. BORROW / RETURN PAGE -->
    <section id="page-borrow" class="page">
      <h3 class="section-title">หน้ายืม–คืนอุปกรณ์กีฬา (Borrow / Return)</h3>
      <div class="form-grid">
        <div>
          <label for="borrow-member">ชื่อผู้ยืม (สมาชิก)</label>
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
          <label for="borrow-date">วันที่ยืม</label>
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
      <p class="muted">
        ประวัติการยืม–คืนที่โหลดล่าสุดจะถูกเก็บใน Local Storage เพื่อดูย้อนหลังได้แบบออฟไลน์บางส่วน
      </p>
    </section>

    <!-- 4. ADD MEMBER PAGE -->
    <section id="page-member" class="page">
      <h3 class="section-title">หน้าเพิ่มผู้ยืม (Add Member)</h3>
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
        <button class="btn btn-green" id="btnMemSave">💾 เพิ่มสมาชิก / บันทึกข้อมูลใน Google Sheet</button>
        <button class="btn btn-blue" id="btnMemLoad">📂 เรียกดูข้อมูล Google Sheet</button>
      </div>
      <div id="member-table-container"></div>
      <p class="muted">
        ข้อมูลสมาชิกที่ดึงจาก Google Sheet จะเก็บซ้ำใน Local Storage และใช้เติมชื่อใน Dropdown หน้ายืม–คืนอุปกรณ์
      </p>
    </section>

    <!-- 5. REPORT PAGE -->
    <section id="page-report" class="page">
      <h3 class="section-title">หน้ารายงานการยืม–คืนอุปกรณ์ (Borrowing Report)</h3>
      <div class="btn-row">
        <button class="btn btn-blue" id="btnReportLoad">
          🔄 เรียกดูข้อมูลจาก Google Sheet / อัปเดตรายงาน
        </button>
        <!-- ปุ่มนี้เผื่อในอนาคต ถ้าต้องการบันทึกสรุปกลับชีต
        <button class="btn btn-yellow" id="btnReportSave">
          💾 บันทึกข้อมูลสรุปรายงานใน Google Sheet
        </button>
        -->
      </div>
      <div class="card-grid">
        <div class="card">
          <h3>สรุปจำนวนการยืม–คืน</h3>
          <p>จำนวนการยืมทั้งหมด: <strong id="rep-total-borrow">0</strong></p>
          <p>จำนวนการคืนทั้งหมด: <strong id="rep-total-return">0</strong></p>
        </div>
        <div class="card">
          <h3>สมาชิกและอุปกรณ์ยอดนิยม</h3>
          <p>จำนวนสมาชิกผู้ยืมปัจจุบัน: <strong id="rep-member-count">0</strong> คน</p>
          <p>อุปกรณ์ที่ถูกยืมบ่อยที่สุด: <strong id="rep-top-equipment">-</strong></p>
        </div>
      </div>
      <div class="chart-container">
        <canvas id="borrowChart"></canvas>
      </div>
      <div id="report-table-container"></div>
      <p class="muted">
        ข้อมูลรายงานที่ดึงล่าสุดจะถูกเก็บไว้ใน Local Storage เพื่อลดจำนวนครั้งที่ต้องดึงจาก Google Sheet
      </p>
    </section>
  </main>
</div>

<script>
  /* ============ CONFIG ============ */
  const APP_SCRIPT_URL =
    "https://script.google.com/macros/s/AKfycbyV9c1boywpxvuW05xUsBKnXpCBGvUNat7xy1Y5nYgEzXNwHZV2K4RrqHobLAH-wnUo/exec";

  const LS_KEYS = {
    EQUIP: "ssm_sports_equipment",
    MEMBER: "ssm_sports_members",
    BORROW: "ssm_sports_borrow",
  };

  /* ============ JSONP HELPER ============ */
  function callAppsScript(params, onSuccess, onError) {
    const callbackName =
      "gsCallback_" + Date.now() + "_" + Math.floor(Math.random() * 1000);
    params.callback = callbackName;

    const query = Object.keys(params)
      .map((k) => encodeURIComponent(k) + "=" + encodeURIComponent(params[k]))
      .join("&");

    const script = document.createElement("script");
    script.src = APP_SCRIPT_URL + "?" + query;

    window[callbackName] = function (res) {
      delete window[callbackName];
      document.body.removeChild(script);
      if (res && res.success) {
        onSuccess && onSuccess(res);
      } else {
        onError && onError(res || { success: false, message: "Unknown error" });
      }
    };

    script.onerror = function () {
      delete window[callbackName];
      document.body.removeChild(script);
      onError &&
        onError({
          success: false,
          message: "ไม่สามารถติดต่อ Google Apps Script ได้",
        });
    };

    document.body.appendChild(script);
  }

  /* ============ LOCAL STORAGE HELPER ============ */
  function getLS(key) {
    try {
      return JSON.parse(localStorage.getItem(key) || "[]");
    } catch (e) {
      return [];
    }
  }
  function setLS(key, value) {
    localStorage.setItem(key, JSON.stringify(value));
  }

  /* ============ NAVIGATION ============ */
  const pageTitle = document.getElementById("page-title");
  const pageSubtitle = document.getElementById("page-subtitle");
  const pageMeta = {
    "page-dashboard": {
      title: "หน้าแรก (Dashboard)",
      subtitle:
        "แสดงภาพรวมของการยืม–คืนอุปกรณ์กีฬา และทางลัดไปยังเมนูหลักของระบบ",
    },
    "page-equipment": {
      title: "หน้าเพิ่มอุปกรณ์กีฬา",
      subtitle: "ใช้บันทึกข้อมูลอุปกรณ์กีฬาในห้องพละ",
    },
    "page-borrow": {
      title: "หน้ายืม–คืนอุปกรณ์กีฬา",
      subtitle: "ใช้บันทึกการยืมและการคืนอุปกรณ์ของนักเรียนหรือครู",
    },
    "page-member": {
      title: "หน้าเพิ่มผู้ยืม (สมาชิก)",
      subtitle: "ใช้เพิ่มสมาชิกที่สามารถยืมอุปกรณ์กีฬาได้",
    },
    "page-report": {
      title: "หน้ารายงานการยืม–คืนอุปกรณ์",
      subtitle: "ใช้สำหรับครูพละหรือผู้ดูแลตรวจสอบสถิติการยืม–คืนอุปกรณ์",
    },
  };

  document.querySelectorAll(".nav-btn").forEach((btn) => {
    btn.addEventListener("click", () => {
      document
        .querySelectorAll(".nav-btn")
        .forEach((b) => b.classList.remove("active"));
      btn.classList.add("active");

      const pageId = btn.getAttribute("data-page");
      document
        .querySelectorAll(".page")
        .forEach((p) => p.classList.remove("active"));
      document.getElementById(pageId).classList.add("active");

      if (pageMeta[pageId]) {
        pageTitle.textContent = pageMeta[pageId].title;
        pageSubtitle.textContent = pageMeta[pageId].subtitle;
      }

      if (pageId === "page-borrow") {
        populateMemberDropdown();
        populateEquipmentDropdown();
      }
      if (pageId === "page-dashboard") {
        updateDashboard();
      }
    });
  });

  /* ============ EQUIPMENT (เพิ่มอุปกรณ์) ============ */
  const eqNameEl = document.getElementById("eq-name");
  const eqCatEl = document.getElementById("eq-category");
  const eqQtyEl = document.getElementById("eq-qty");
  const eqLocEl = document.getElementById("eq-location");
  const eqDescEl = document.getElementById("eq-desc");
  const eqTableContainer = document.getElementById(
    "equipment-table-container"
  );

  document.getElementById("btnEqSave").addEventListener("click", () => {
    const name = eqNameEl.value.trim();
    const category = eqCatEl.value.trim();
    const quantity = eqQtyEl.value.trim();
    const location = eqLocEl.value.trim();
    const description = eqDescEl.value.trim();

    if (!name || !quantity) {
      Swal.fire(
        "ข้อมูลไม่ครบ",
        "กรุณากรอกชื่ออุปกรณ์และจำนวนคงเหลือ",
        "warning"
      );
      return;
    }

    Swal.fire({
      title: "กำลังบันทึก...",
      allowOutsideClick: false,
      didOpen: () => Swal.showLoading(),
    });

    callAppsScript(
      {
        action: "addEquipment",
        name,
        category,
        quantity,
        location,
        description,
      },
      (res) => {
        Swal.fire("สำเร็จ", res.message || "บันทึกข้อมูลสำเร็จ", "success");
        loadEquipmentFromServer(false);
      },
      (err) => {
        Swal.fire("ผิดพลาด", err.message || "ไม่สามารถบันทึกได้", "error");
      }
    );
  });

  document.getElementById("btnEqLoad").addEventListener("click", () => {
    loadEquipmentFromServer(true);
  });

  function loadEquipmentFromServer(showAlert) {
    if (showAlert) {
      Swal.fire({
        title: "กำลังดึงข้อมูลอุปกรณ์...",
        allowOutsideClick: false,
        didOpen: () => Swal.showLoading(),
      });
    }
    callAppsScript(
      { action: "getEquipment" },
      (res) => {
        const data = res.data || [];
        setLS(LS_KEYS.EQUIP, data);
        renderEquipmentTable(data);
        updateDashboard();
        if (showAlert) {
          Swal.fire("สำเร็จ", "โหลดข้อมูลอุปกรณ์เรียบร้อย", "success");
        }
      },
      (err) => {
        if (showAlert) {
          Swal.fire("ผิดพลาด", err.message || "ไม่สามารถโหลดข้อมูลได้", "error");
        }
      }
    );
  }

  function renderEquipmentTable(data) {
    const list = data || getLS(LS_KEYS.EQUIP);
    if (!list.length) {
      eqTableContainer.innerHTML = "<p>ยังไม่มีข้อมูลอุปกรณ์กีฬา</p>";
      return;
    }
    let html = "<table><thead><tr>";
    html +=
      "<th>ชื่ออุปกรณ์</th><th>ประเภท/หมวดหมู่</th><th>จำนวน</th><th>สถานที่เก็บ</th><th>รายละเอียด</th>";
    html += "</tr></thead><tbody>";
    list.forEach((e) => {
      html += `<tr>
        <td>${e.name || ""}</td>
        <td>${e.category || "-"}</td>
        <td>${e.quantity || "-"}</td>
        <td>${e.location || "-"}</td>
        <td>${e.description || "-"}</td>
      </tr>`;
    });
    html += "</tbody></table>";
    eqTableContainer.innerHTML = html;
  }

  /* ============ MEMBER (เพิ่มผู้ยืม) ============ */
  const memIdEl = document.getElementById("mem-id");
  const memNameEl = document.getElementById("mem-name");
  const memClassEl = document.getElementById("mem-class");
  const memPhoneEl = document.getElementById("mem-phone");
  const memTableContainer = document.getElementById("member-table-container");

  document.getElementById("btnMemSave").addEventListener("click", () => {
    const memberId = memIdEl.value.trim();
    const name = memNameEl.value.trim();
    const className = memClassEl.value.trim();
    const phone = memPhoneEl.value.trim();

    if (!memberId || !name) {
      Swal.fire(
        "ข้อมูลไม่ครบ",
        "กรุณากรอกรหัสสมาชิก และชื่อ–นามสกุล",
        "warning"
      );
      return;
    }

    Swal.fire({
      title: "กำลังบันทึกสมาชิก...",
      allowOutsideClick: false,
      didOpen: () => Swal.showLoading(),
    });

    callAppsScript(
      {
        action: "addMember",
        memberId,
        name,
        className,
        phone,
      },
      (res) => {
        Swal.fire("สำเร็จ", res.message || "บันทึกข้อมูลสำเร็จ", "success");
        loadMembersFromServer(false);
      },
      (err) => {
        Swal.fire("ผิดพลาด", err.message || "ไม่สามารถบันทึกได้", "error");
      }
    );
  });

  document.getElementById("btnMemLoad").addEventListener("click", () => {
    loadMembersFromServer(true);
  });

  function loadMembersFromServer(showAlert) {
    if (showAlert) {
      Swal.fire({
        title: "กำลังดึงข้อมูลสมาชิก...",
        allowOutsideClick: false,
        didOpen: () => Swal.showLoading(),
      });
    }
    callAppsScript(
      { action: "getMembers" },
      (res) => {
        const data = res.data || [];
        setLS(LS_KEYS.MEMBER, data);
        renderMemberTable(data);
        updateDashboard();
        if (showAlert) {
          Swal.fire("สำเร็จ", "โหลดข้อมูลสมาชิกเรียบร้อย", "success");
        }
      },
      (err) => {
        if (showAlert) {
          Swal.fire(
            "ผิดพลาด",
            err.message || "ไม่สามารถโหลดข้อมูลสมาชิกได้",
            "error"
          );
        }
      }
    );
  }

  function renderMemberTable(data) {
    const list = data || getLS(LS_KEYS.MEMBER);
    if (!list.length) {
      memTableContainer.innerHTML = "<p>ยังไม่มีข้อมูลสมาชิกผู้ยืม</p>";
      return;
    }
    let html = "<table><thead><tr>";
    html +=
      "<th>รหัสสมาชิก</th><th>ชื่อ–นามสกุล</th><th>ห้องเรียน/ชั้นปี</th><th>เบอร์โทรศัพท์</th>";
    html += "</tr></thead><tbody>";
    list.forEach((m) => {
      html += `<tr>
        <td>${m.memberId || ""}</td>
        <td>${m.name || ""}</td>
        <td>${m.className || "-"}</td>
        <td>${m.phone || "-"}</td>
      </tr>`;
    });
    html += "</tbody></table>";
    memTableContainer.innerHTML = html;
  }

  function populateMemberDropdown() {
    const members = getLS(LS_KEYS.MEMBER);
    const select = document.getElementById("borrow-member");
    select.innerHTML = '<option value="">-- เลือกชื่อสมาชิก --</option>';
    members.forEach((m) => {
      if (!m.memberId) return;
      const opt = document.createElement("option");
      opt.value = m.memberId;
      opt.textContent = `${m.memberId} - ${m.name || ""}`;
      select.appendChild(opt);
    });
  }

  /* ============ BORROW / RETURN ============ */
  const borrowMemberEl = document.getElementById("borrow-member");
  const borrowEquipEl = document.getElementById("borrow-equipment");
  const borrowQtyEl = document.getElementById("borrow-qty");
  const borrowDateEl = document.getElementById("borrow-date");
  const borrowDueEl = document.getElementById("borrow-due");
  const borrowTypeEl = document.getElementById("borrow-type");
  const borrowTableContainer = document.getElementById(
    "borrow-table-container"
  );

  document.getElementById("btnBorrowSave").addEventListener("click", () => {
    const memberId = borrowMemberEl.value;
    const equipmentName = borrowEquipEl.value;
    const quantity = borrowQtyEl.value.trim();
    const date = borrowDateEl.value;
    const dueDate = borrowDueEl.value;
    const type = borrowTypeEl.value;

    if (!memberId || !equipmentName || !quantity || !date) {
      Swal.fire(
        "ข้อมูลไม่ครบ",
        "กรุณาเลือกสมาชิก เลือกอุปกรณ์ ใส่จำนวน และวันที่",
        "warning"
      );
      return;
    }

    const members = getLS(LS_KEYS.MEMBER);
    const m = members.find((x) => x.memberId === memberId);
    const memberName = m ? m.name : memberId;

    Swal.fire({
      title: "กำลังบันทึกการยืม/คืน...",
      allowOutsideClick: false,
      didOpen: () => Swal.showLoading(),
    });

    callAppsScript(
      {
        action: "addBorrow",
        memberId,
        memberName,
        equipmentName,
        quantity,
        date,
        dueDate,
        type,
      },
      (res) => {
        Swal.fire("สำเร็จ", res.message || "บันทึกข้อมูลสำเร็จ", "success");
        loadBorrowFromServer(false);
      },
      (err) => {
        Swal.fire("ผิดพลาด", err.message || "ไม่สามารถบันทึกได้", "error");
      }
    );
  });

  document.getElementById("btnBorrowLoad").addEventListener("click", () => {
    loadBorrowFromServer(true);
  });

  function loadBorrowFromServer(showAlert) {
    if (showAlert) {
      Swal.fire({
        title: "กำลังดึงประวัติการยืม–คืน...",
        allowOutsideClick: false,
        didOpen: () => Swal.showLoading(),
      });
    }
    callAppsScript(
      { action: "getBorrowRecords" },
      (res) => {
        const data = res.data || [];
        setLS(LS_KEYS.BORROW, data);
        renderBorrowTable(data);
        updateDashboard();
        if (showAlert) {
          Swal.fire("สำเร็จ", "โหลดประวัติการยืม–คืนเรียบร้อย", "success");
        }
      },
      (err) => {
        if (showAlert) {
          Swal.fire(
            "ผิดพลาด",
            err.message || "ไม่สามารถโหลดข้อมูลได้",
            "error"
          );
        }
      }
    );
  }

  function renderBorrowTable(data) {
    const list = data || getLS(LS_KEYS.BORROW);
    if (!list.length) {
      borrowTableContainer.innerHTML =
        "<p>ยังไม่มีข้อมูลประวัติการยืม–คืน</p>";
      return;
    }
    let html = "<table><thead><tr>";
    html +=
      "<th>วันที่</th><th>ประเภท</th><th>ชื่อผู้ยืม</th><th>ชื่ออุปกรณ์</th><th>จำนวน</th><th>กำหนดคืน</th>";
    html += "</tr></thead><tbody>";
    list
      .slice()
      .sort((a, b) => (a.date > b.date ? -1 : 1))
      .forEach((r) => {
        html += `<tr>
        <td>${r.date || ""}</td>
        <td>${r.type === "borrow" ? "ยืมอุปกรณ์" : "คืนอุปกรณ์"}</td>
        <td>${r.memberName || ""}</td>
        <td>${r.equipmentName || ""}</td>
        <td>${r.quantity || ""}</td>
        <td>${r.dueDate || "-"}</td>
      </tr>`;
      });
    html += "</tbody></table>";
    borrowTableContainer.innerHTML = html;
  }

  function populateEquipmentDropdown() {
    const equipment = getLS(LS_KEYS.EQUIP);
    const select = document.getElementById("borrow-equipment");
    select.innerHTML = '<option value="">-- เลือกอุปกรณ์กีฬา --</option>';
    equipment.forEach((e) => {
      if (!e.name) return;
      const opt = document.createElement("option");
      opt.value = e.name;
      opt.textContent = `${e.name} (จำนวน: ${e.quantity || "-"})`;
      select.appendChild(opt);
    });
  }

  /* ============ REPORT (สถิติ) ============ */
  let borrowChart = null;

  document.getElementById("btnReportLoad").addEventListener("click", () => {
    Swal.fire({
      title: "กำลังดึงข้อมูลรายงาน...",
      allowOutsideClick: false,
      didOpen: () => Swal.showLoading(),
    });

    // โหลด Borrow
    callAppsScript(
      { action: "getBorrowRecords" },
      (res) => {
        const borrowData = res.data || [];
        setLS(LS_KEYS.BORROW, borrowData);
        // โหลด Member
        callAppsScript(
          { action: "getMembers" },
          (res2) => {
            const memberData = res2.data || [];
            setLS(LS_KEYS.MEMBER, memberData);
            buildReport(borrowData, memberData);
            updateDashboard();
            Swal.fire("สำเร็จ", "อัปเดตรายงานเรียบร้อย", "success");
          },
          (err2) => {
            Swal.fire(
              "ผิดพลาด",
              err2.message || "โหลดข้อมูลสมาชิกไม่สำเร็จ",
              "error"
            );
          }
        );
      },
      (err) => {
        Swal.fire(
          "ผิดพลาด",
          err.message || "โหลดประวัติการยืม–คืนไม่สำเร็จ",
          "error"
        );
      }
    );
  });

  function buildReport(borrowData, memberData) {
    const records = borrowData || getLS(LS_KEYS.BORROW);
    const members = memberData || getLS(LS_KEYS.MEMBER);

    const totalBorrow = records.filter((r) => r.type === "borrow").length;
    const totalReturn = records.filter((r) => r.type === "return").length;
    document.getElementById("rep-total-borrow").textContent = totalBorrow;
    document.getElementById("rep-total-return").textContent = totalReturn;
    document.getElementById("rep-member-count").textContent = members.length;

    // นับอุปกรณ์ที่ถูกยืมบ่อยที่สุด
    const countByEq = {};
    records.forEach((r) => {
      if (r.type === "borrow") {
        if (!countByEq[r.equipmentName]) countByEq[r.equipmentName] = 0;
        countByEq[r.equipmentName] += Number(r.quantity || 0);
      }
    });

    let topEq = "-";
    if (Object.keys(countByEq).length > 0) {
      topEq = Object.entries(countByEq).sort((a, b) => b[1] - a[1])[0][0];
    }
    document.getElementById("rep-top-equipment").textContent = topEq;

    // กราฟ bar แสดงจำนวนการยืมต่ออุปกรณ์
    const labels = Object.keys(countByEq);
    const values = Object.values(countByEq);
    const ctx = document.getElementById("borrowChart").getContext("2d");
    if (borrowChart) borrowChart.destroy();
    borrowChart = new Chart(ctx, {
      type: "bar",
      data: {
        labels,
        datasets: [
          {
            label: "จำนวนครั้ง/จำนวนที่ถูกยืม",
            data: values,
          },
        ],
      },
      options: {
        responsive: true,
        plugins: {
          legend: { display: false },
        },
        scales: {
          y: { beginAtZero: true },
        },
      },
    });

    const container = document.getElementById("report-table-container");
    if (!records.length) {
      container.innerHTML = "<p>ยังไม่มีข้อมูลประวัติการยืม–คืน</p>";
      return;
    }
    let html = "<table><thead><tr>";
    html +=
      "<th>วันที่</th><th>ประเภท</th><th>ชื่อผู้ยืม</th><th>อุปกรณ์</th><th>จำนวน</th><th>กำหนดคืน</th>";
    html += "</tr></thead><tbody>";
    records
      .slice()
      .sort((a, b) => (a.date > b.date ? -1 : 1))
      .forEach((r) => {
        html += `<tr>
        <td>${r.date || ""}</td>
        <td>${r.type === "borrow" ? "ยืมอุปกรณ์" : "คืนอุปกรณ์"}</td>
        <td>${r.memberName || ""}</td>
        <td>${r.equipmentName || ""}</td>
        <td>${r.quantity || ""}</td>
        <td>${r.dueDate || "-"}</td>
      </tr>`;
      });
    html += "</tbody></table>";
    container.innerHTML = html;
  }

  /* ============ DASHBOARD SUMMARY ============ */
  function updateDashboard() {
    const eq = getLS(LS_KEYS.EQUIP);
    const mem = getLS(LS_KEYS.MEMBER);
    const bor = getLS(LS_KEYS.BORROW);

    document.getElementById("dash-total-equipment").textContent = eq.length;
    document.getElementById("dash-total-members").textContent = mem.length;

    const today = new Date().toISOString().slice(0, 10);
    const todayBorrow = bor.filter(
      (r) => r.type === "borrow" && r.date === today
    ).length;
    const totalBorrowed = bor.filter((r) => r.type === "borrow").length;

    document.getElementById("dash-today-borrow").textContent = todayBorrow;
    document.getElementById("dash-total-borrowed").textContent =
      totalBorrowed;
  }

  /* ============ INIT ============ */
  (function init() {
    // โหลดจาก LocalStorage รอบแรก
    renderEquipmentTable();
    renderMemberTable();
    renderBorrowTable();
    updateDashboard();
  })();
</script>
</body>
</html>
