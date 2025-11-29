<html lang="th">
<head>
  <meta charset="UTF-8" />
  <title>ระบบบันทึกสุขภาพนักเรียน โรงเรียนสุรศักดิ์มนตรี</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <!-- SheetJS ใช้อ่านไฟล์ Excel -->
  <script src="https://cdn.jsdelivr.net/npm/xlsx@0.18.5/dist/xlsx.full.min.js"></script>
  <style>
    * { box-sizing: border-box; font-family: "Sarabun", system-ui, sans-serif; }
    body {
      margin: 0;
      background: #fef3c7; /* เหลืองอ่อน */
      color: #333;
      padding: 20px;
    }
    .container {
      max-width: 1200px;
      margin: 0 auto;
      background: #fff;
      border-radius: 16px;
      padding: 20px;
      box-shadow: 0 4px 10px rgba(0,0,0,0.05);
    }
    h1 { margin-top: 0; }
    h2 { margin-bottom: 8px; }
    h3 { margin-bottom: 6px; }
    .section {
      margin-bottom: 24px;
      padding-bottom: 16px;
      border-bottom: 1px solid #e5e7eb;
    }
    .section:last-child {
      border-bottom: none;
    }
    .small {
      font-size: 0.8rem;
      color: #6b7280;
      margin-top: 4px;
    }
    .grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
      gap: 10px 16px;
      margin-bottom: 10px;
    }
    label {
      font-size: 0.9rem;
      margin-bottom: 4px;
      display: block;
    }
    input, select, textarea {
      width: 100%;
      padding: 6px 8px;
      border-radius: 8px;
      border: 1px solid #ddd;
      font-size: 0.9rem;
    }
    textarea {
      resize: vertical;
      min-height: 60px;
    }
    .btn-row {
      margin: 10px 0 15px;
      display: flex;
      flex-wrap: wrap;
      gap: 10px;
    }
    .btn {
      border: none;
      border-radius: 999px;
      padding: 8px 16px;
      cursor: pointer;
      font-size: 0.9rem;
      color: #fff;
    }
    .btn-blue  { background: #3b82f6; }
    .btn-green { background: #22c55e; }
    .btn-red   { background: #ef4444; }
    .btn-gray  { background: #6b7280; }
    table {
      width: 100%;
      border-collapse: collapse;
      font-size: 0.8rem;
      margin-top: 10px;
    }
    th, td {
      border: 1px solid #e5e7eb;
      padding: 4px 6px;
      text-align: left;
    }
    th {
      background: #e5e7eb;
    }
    tr.clickable:hover {
      background: #fef9c3;
      cursor: pointer;
    }
    .badge {
      display: inline-block;
      padding: 2px 8px;
      border-radius: 999px;
      font-size: 0.75rem;
      background: #fee2e2;
      color: #b91c1c;
    }
    @media (max-width: 768px) {
      body { padding: 10px; }
      .container { padding: 12px; border-radius: 10px; }
    }
  </style>
</head>
<body>
<div class="container">
  <h1>ระบบบันทึกสุขภาพนักเรียน โรงเรียนสุรศักดิ์มนตรี</h1>
  <p class="small">
    ระบบนี้สามารถนำเข้าข้อมูลนักเรียนจากไฟล์ Excel (ฐานข้อมูลนักเรียน) และบันทึกผลสุขภาพเก็บใน LocalStorage บนเบราว์เซอร์
  </p>

  <!-- SECTION 1: นำเข้าข้อมูลนักเรียนจาก Excel -->
  <div class="section">
    <h2>1. นำเข้าข้อมูลนักเรียนจาก Excel</h2>
    <p>
      เลือกไฟล์ Excel ที่มีชีต <strong>Students</strong> และหัวคอลัมน์อย่างน้อย:
      <code>StudentID, FirstName, LastName, Class, Gender, BirthDate, ParentPhone</code>
    </p>
    <input type="file" id="fileExcel" accept=".xlsx,.xls" />
    <div class="btn-row">
      <button class="btn btn-blue" id="btnLoadExcel">📥 โหลดข้อมูลจากไฟล์</button>
      <button class="btn btn-gray" id="btnClearStudents">🧹 ล้างฐานข้อมูลนักเรียน (ในหน้านี้)</button>
    </div>
    <div id="student-summary" class="small"></div>
  </div>

  <!-- SECTION 2: ค้นหาและเลือกนักเรียน -->
  <div class="section">
    <h2>2. ค้นหาและเลือกนักเรียน</h2>
    <div class="grid">
      <div>
        <label for="searchKeyword">ค้นหาจาก รหัส / ชื่อ / ห้อง</label>
        <input id="searchKeyword" type="text" placeholder="เช่น 678001 หรือ ภพนิ หรือ ม.2/1" />
      </div>
    </div>
    <div class="btn-row">
      <button class="btn btn-blue" id="btnSearch">🔍 ค้นหา</button>
      <button class="btn btn-gray" id="btnShowAll">📃 แสดงนักเรียนทั้งหมด</button>
    </div>
    <div id="student-table-container"></div>
    <p class="small">
      เคล็ดลับ: คลิกที่แถวของนักเรียน เพื่อเลือกไปบันทึกข้อมูลสุขภาพด้านล่าง
    </p>
    <div id="selected-student-info" class="small"></div>
  </div>

  <!-- SECTION 3: บันทึกข้อมูลสุขภาพ -->
  <div class="section">
    <h2>3. บันทึกผลสุขภาพนักเรียน</h2>
    <p class="small">
      * ต้องเลือกนักเรียนก่อน จากตารางด้านบน
    </p>
    <div class="grid">
      <div>
        <label for="record-date">วันที่ตรวจสุขภาพ</label>
        <input id="record-date" type="date" />
      </div>
    </div>
    <h3>ข้อมูลรูปร่าง</h3>
    <div class="grid">
      <div>
        <label for="weight">น้ำหนัก (kg)</label>
        <input id="weight" type="number" min="0" step="0.1" />
      </div>
      <div>
        <label for="height">ส่วนสูง (cm)</label>
        <input id="height" type="number" min="0" step="0.1" />
      </div>
    </div>
    <h3>สมรรถภาพ / ตัวชี้วัดอื่น ๆ</h3>
    <div class="grid">
      <div>
        <label for="bp">ความดันโลหิต (เช่น 110/70)</label>
        <input id="bp" type="text" />
      </div>
      <div>
        <label for="vision-left">สายตาซ้าย (เช่น 20/20)</label>
        <input id="vision-left" type="text" />
      </div>
      <div>
        <label for="vision-right">สายตาขวา (เช่น 20/25)</label>
        <input id="vision-right" type="text" />
      </div>
      <div>
        <label for="note">หมายเหตุ / ข้อสังเกต</label>
        <input id="note" type="text" />
      </div>
    </div>

    <div class="btn-row">
      <button class="btn btn-green" id="btnSaveHealth">💾 บันทึกผลสุขภาพ</button>
      <button class="btn btn-gray" id="btnClearHealthForm">🧹 ล้างฟอร์ม</button>
      <button class="btn btn-red" id="btnDeleteAllHealth">🗑 ลบข้อมูลสุขภาพทั้งหมด (LocalStorage)</button>
    </div>
  </div>

  <!-- SECTION 4: ตารางประวัติสุขภาพของนักเรียนที่เลือก -->
  <div class="section">
    <h2>4. ประวัติสุขภาพของนักเรียนที่เลือก</h2>
    <div id="health-table-container"></div>
  </div>

  <!-- SECTION 5: สรุปภาพรวม BMI ทั้งระบบ (ง่าย ๆ) -->
  <div class="section">
    <h2>5. สรุปภาพรวม BMI ง่าย ๆ (ทุกคน)</h2>
    <div id="bmi-summary" class="small"></div>
  </div>
</div>

<script>
  /* -------------------- CONFIG & STORAGE -------------------- */
  const LS_STUDENTS_KEY = 'ssm_health_students';
  const LS_HEALTH_KEY   = 'ssm_health_records';

  let students = [];       // ข้อมูลนักเรียนจาก Excel / LocalStorage
  let healthRecords = [];  // ข้อมูลสุขภาพของทุกคน
  let selectedStudentId = null;

  /* -------------------- Helper LocalStorage -------------------- */
  function loadFromLocalStorage() {
    try {
      const s = localStorage.getItem(LS_STUDENTS_KEY);
      if (s) students = JSON.parse(s);
    } catch (e) { students = []; }

    try {
      const h = localStorage.getItem(LS_HEALTH_KEY);
      if (h) healthRecords = JSON.parse(h);
    } catch (e) { healthRecords = []; }
  }

  function saveStudentsToLocal() {
    localStorage.setItem(LS_STUDENTS_KEY, JSON.stringify(students));
  }

  function saveHealthToLocal() {
    localStorage.setItem(LS_HEALTH_KEY, JSON.stringify(healthRecords));
  }

  /* -------------------- อ่านไฟล์ Excel -------------------- */
  document.getElementById('btnLoadExcel').addEventListener('click', () => {
    const input = document.getElementById('fileExcel');
    if (!input.files || input.files.length === 0) {
      alert('กรุณาเลือกไฟล์ Excel ก่อน');
      return;
    }
    const file = input.files[0];
    const reader = new FileReader();
    reader.onload = function(e) {
      const data = new Uint8Array(e.target.result);
      const workbook = XLSX.read(data, {type: 'array'});
      const sheetName = 'Students';
      if (!workbook.Sheets[sheetName]) {
        alert('ไม่พบชีตชื่อ "Students" ในไฟล์ Excel');
        return;
      }
      const sheet = workbook.Sheets[sheetName];
      const json = XLSX.utils.sheet_to_json(sheet, {defval: ''});
      // ควรมีคอลัมน์ StudentID, FirstName, LastName, Class, Gender, BirthDate, ParentPhone
      students = json.map(row => ({
        StudentID: String(row.StudentID || '').trim(),
        FirstName: String(row.FirstName || '').trim(),
        LastName: String(row.LastName || '').trim(),
        Class: String(row.Class || '').trim(),
        Gender: String(row.Gender || '').trim(),
        BirthDate: String(row.BirthDate || '').trim(),
        ParentPhone: String(row.ParentPhone || '').trim()
      })).filter(s => s.StudentID); // เฉพาะแถวที่มีรหัสนักเรียน

      saveStudentsToLocal();
      renderStudentSummary();
      renderStudentTable(students.slice(0, 50)); // แสดงตัวอย่าง 50 คนแรก
      alert('โหลดข้อมูลนักเรียนจาก Excel สำเร็จ: ' + students.length + ' คน');
    };
    reader.readAsArrayBuffer(file);
  });

  document.getElementById('btnClearStudents').addEventListener('click', () => {
    if (confirm('ยืนยันล้างฐานข้อมูลนักเรียนที่เก็บไว้ในหน้านี้ (LocalStorage)?')) {
      students = [];
      saveStudentsToLocal();
      renderStudentSummary();
      document.getElementById('student-table-container').innerHTML = '';
      selectedStudentId = null;
      document.getElementById('selected-student-info').innerHTML = '';
      alert('ล้างข้อมูลนักเรียนเรียบร้อย');
    }
  });

  /* -------------------- แสดงภาพรวมข้อมูลนักเรียน -------------------- */
  function renderStudentSummary() {
    const div = document.getElementById('student-summary');
    if (!students.length) {
      div.innerHTML = '<span class="badge">ยังไม่มีข้อมูลนักเรียนในระบบ</span>';
      return;
    }
    const classes = Array.from(new Set(students.map(s => s.Class).filter(Boolean)));
    div.innerHTML = `
      มีข้อมูลนักเรียนทั้งหมด <strong>${students.length}</strong> คน<br>
      ห้อง/ชั้นในระบบ: ${classes.join(', ') || '-'}
    `;
  }

  /* -------------------- ค้นหาและแสดงตารางนักเรียน -------------------- */
  document.getElementById('btnSearch').addEventListener('click', () => {
    const keyword = document.getElementById('searchKeyword').value.trim();
    const results = searchStudent(keyword);
    renderStudentTable(results);
  });

  document.getElementById('btnShowAll').addEventListener('click', () => {
    renderStudentTable(students);
  });

  function searchStudent(keyword) {
    if (!keyword) return students;
    const lower = keyword.toLowerCase();
    return students.filter(s =>
      s.StudentID.toLowerCase().includes(lower) ||
      s.FirstName.toLowerCase().includes(lower) ||
      s.LastName.toLowerCase().includes(lower) ||
      s.Class.toLowerCase().includes(lower)
    );
  }

  function renderStudentTable(list) {
    const container = document.getElementById('student-table-container');
    if (!list || !list.length) {
      container.innerHTML = '<p>ไม่พบนักเรียนที่ตรงกับเงื่อนไข</p>';
      return;
    }
    let html = '<table><thead><tr>';
    html += '<th>รหัส</th><th>ชื่อ–นามสกุล</th><th>ห้อง/ชั้น</th><th>เพศ</th><th>วันเกิด</th><th>ผู้ปกครอง</th>';
    html += '</tr></thead><tbody>';
    list.forEach(s => {
      html += `
        <tr class="clickable" data-student-id="${s.StudentID}">
          <td>${s.StudentID}</td>
          <td>${s.FirstName} ${s.LastName}</td>
          <td>${s.Class || '-'}</td>
          <td>${s.Gender || '-'}</td>
          <td>${s.BirthDate || '-'}</td>
          <td>${s.ParentPhone || '-'}</td>
        </tr>
      `;
    });
    html += '</tbody></table>';
    container.innerHTML = html;

    // เพิ่ม event เมื่อคลิกแถวนักเรียน
    container.querySelectorAll('tr.clickable').forEach(tr => {
      tr.addEventListener('click', () => {
        const sid = tr.getAttribute('data-student-id');
        selectStudent(sid);
      });
    });
  }

  function selectStudent(studentId) {
    selectedStudentId = studentId;
    const stu = students.find(s => s.StudentID === studentId);
    if (!stu) return;
    const infoDiv = document.getElementById('selected-student-info');
    infoDiv.innerHTML = `
      <strong>นักเรียนที่เลือก:</strong> 
      [${stu.StudentID}] ${stu.FirstName} ${stu.LastName} (${stu.Class || '-'}) 
      เพศ: ${stu.Gender || '-'} โทรผู้ปกครอง: ${stu.ParentPhone || '-'}
    `;
    renderHealthTableForStudent(studentId);
  }

  /* -------------------- บันทึกสุขภาพ -------------------- */
  function calcBMI(weight, heightCm) {
    const w = Number(weight);
    const h = Number(heightCm) / 100;
    if (!w || !h) return '';
    return (w / (h * h)).toFixed(1);
  }

  document.getElementById('btnSaveHealth').addEventListener('click', () => {
    if (!selectedStudentId) {
      alert('กรุณาเลือกนักเรียนจากรายการก่อน');
      return;
    }
    const recordDate  = document.getElementById('record-date').value;
    const weight      = document.getElementById('weight').value;
    const height      = document.getElementById('height').value;
    const bp          = document.getElementById('bp').value;
    const visionLeft  = document.getElementById('vision-left').value;
    const visionRight = document.getElementById('vision-right').value;
    const note        = document.getElementById('note').value;

    if (!recordDate) {
      alert('กรุณากรอกวันที่ตรวจสุขภาพ');
      return;
    }

    const bmi = calcBMI(weight, height);

    const newRecord = {
      RecordID: Date.now(),
      StudentID: selectedStudentId,
      RecordDate: recordDate,
      WeightKg: weight,
      HeightCm: height,
      BMI: bmi,
      BloodPressure: bp,
      VisionLeft: visionLeft,
      VisionRight: visionRight,
      Note: note
    };

    healthRecords.push(newRecord);
    saveHealthToLocal();
    renderHealthTableForStudent(selectedStudentId);
    renderBMISummary();
    clearHealthForm();
    alert('บันทึกข้อมูลสุขภาพเรียบร้อย');
  });

  document.getElementById('btnClearHealthForm').addEventListener('click', clearHealthForm);

  function clearHealthForm() {
    document.getElementById('record-date').value = '';
    document.getElementById('weight').value = '';
    document.getElementById('height').value = '';
    document.getElementById('bp').value = '';
    document.getElementById('vision-left').value = '';
    document.getElementById('vision-right').value = '';
    document.getElementById('note').value = '';
  }

  document.getElementById('btnDeleteAllHealth').addEventListener('click', () => {
    if (confirm('ยืนยันลบข้อมูลสุขภาพของนักเรียนทุกคน (ลบจาก LocalStorage)?')) {
      healthRecords = [];
      saveHealthToLocal();
      if (selectedStudentId) renderHealthTableForStudent(selectedStudentId);
      renderBMISummary();
      alert('ลบข้อมูลสุขภาพทั้งหมดแล้ว');
    }
  });

  /* -------------------- ตารางประวัติสุขภาพของนักเรียน -------------------- */
  function renderHealthTableForStudent(studentId) {
    const container = document.getElementById('health-table-container');
    const list = healthRecords
      .filter(r => String(r.StudentID) === String(studentId))
      .sort((a, b) => (a.RecordDate > b.RecordDate ? -1 : 1)); // ใหม่อยู่บน

    if (!list.length) {
      container.innerHTML = '<p>ยังไม่มีประวัติสุขภาพของนักเรียนคนนี้</p>';
      return;
    }

    let html = '<table><thead><tr>';
    html += '<th>วันที่ตรวจ</th><th>นน.(kg)</th><th>สส.(cm)</th><th>BMI</th><th>ความดัน</th><th>สายตาซ้าย</th><th>สายตาขวา</th><th>หมายเหตุ</th>';
    html += '</tr></thead><tbody>';
    list.forEach(r => {
      html += `
        <tr>
          <td>${r.RecordDate}</td>
          <td>${r.WeightKg || '-'}</td>
          <td>${r.HeightCm || '-'}</td>
          <td>${r.BMI || '-'}</td>
          <td>${r.BloodPressure || '-'}</td>
          <td>${r.VisionLeft || '-'}</td>
          <td>${r.VisionRight || '-'}</td>
          <td>${r.Note || '-'}</td>
        </tr>
      `;
    });
    html += '</tbody></table>';
    container.innerHTML = html;
  }

  /* -------------------- สรุปภาพรวม BMI ทั้งระบบ -------------------- */
  function renderBMISummary() {
    const div = document.getElementById('bmi-summary');
    if (!healthRecords.length) {
      div.innerHTML = 'ยังไม่มีข้อมูลสุขภาพในระบบ';
      return;
    }
    // เอา "ผลตรวจล่าสุด" ของแต่ละคน
    const latestByStudent = {};
    healthRecords.forEach(r => {
      const sid = r.StudentID;
      if (!latestByStudent[sid] || latestByStudent[sid].RecordDate < r.RecordDate) {
        latestByStudent[sid] = r;
      }
    });
    const latestList = Object.values(latestByStudent);
    // ดึง BMI ที่เป็นตัวเลข
    const bmiValues = latestList
      .map(r => Number(r.BMI))
      .filter(v => !isNaN(v) && v > 0);

    if (!bmiValues.length) {
      div.innerHTML = 'ยังไม่มีค่า BMI ที่คำนวณได้';
      return;
    }

    const avg = (bmiValues.reduce((a,b) => a + b, 0) / bmiValues.length).toFixed(1);
    let under = 0, normal = 0, over = 0, obese = 0;
    bmiValues.forEach(v => {
      if (v < 18.5) under++;
      else if (v < 23) normal++;
      else if (v < 25) over++;
      else obese++;
    });

    div.innerHTML = `
      ใช้ผลตรวจล่าสุดของนักเรียนแต่ละคนที่มีข้อมูล BMI:<br>
      - จำนวนนักเรียนที่มีข้อมูล BMI: <strong>${bmiValues.length}</strong> คน<br>
      - ค่า BMI เฉลี่ย: <strong>${avg}</strong><br>
      - ต่ำกว่าเกณฑ์ (&lt;18.5): <strong>${under}</strong> คน<br>
      - ปกติ (18.5–22.9): <strong>${normal}</strong> คน<br>
      - น้ำหนักเกิน (23.0–24.9): <strong>${over}</strong> คน<br>
      - อ้วน (&ge;25): <strong>${obese}</strong> คน
    `;
  }

  /* -------------------- INIT -------------------- */
  (function init() {
    loadFromLocalStorage();
    renderStudentSummary();
    if (students.length) {
      renderStudentTable(students.slice(0, 50));
    }
    renderBMISummary();
  })();
</script>
</body>
</html>
