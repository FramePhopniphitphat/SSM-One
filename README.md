<html lang="th">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Think Before You Risk — เกมทดสอบทักษะการคิดและการตัดสินใจ</title>

  <meta name="description" content="Think Before You Risk — เกมทดสอบทักษะการคิดและการตัดสินใจ เพื่อเอาตัวรอดจากความเสี่ยงในโลกจริงและโลกออนไลน์">
  <meta property="og:title" content="Think Before You Risk — คิดก่อนเสี่ยง">
  <meta property="og:description" content="เกมทดสอบทักษะการคิดและการตัดสินใจ เพื่อเอาตัวรอดจากความเสี่ยงในโลกจริงและโลกออนไลน์">
  <meta property="og:type" content="website">

  <!-- Tailwind CDN -->
  <script src="https://cdn.tailwindcss.com"></script>
  <link href="https://fonts.googleapis.com/css2?family=Sarabun:wght@300;400;600;700&display=swap" rel="stylesheet">

  <style>
    :root{
      --primary: #0f766e; /* teal-700 */
      --accent: #f97316;  /* orange-500 */
      --bg: #f8fafc;      /* slate-50 */
    }
    body { font-family: "Sarabun", system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial; background: linear-gradient(180deg,#eef2ff 0%, #f8fafc 60%); }
    .card-glass { background: linear-gradient(180deg, rgba(255,255,255,0.85), rgba(255,255,255,0.72)); backdrop-filter: blur(6px); }
    .btn-primary { background: linear-gradient(90deg,var(--primary), #0369a1); color: white; box-shadow: 0 6px 18px rgba(15,118,110,0.12); }
    .choice-btn:hover { transform: translateY(-4px); box-shadow: 0 8px 20px rgba(2,6,23,0.08); }
    .progress-track { background: linear-gradient(90deg,#d1fae5 0%, #ecfeff 100%); border-radius: 999px; height: 12px; overflow: hidden; }
    .progress-fill { height: 100%; border-radius: 999px; transition: width 420ms ease; background: linear-gradient(90deg,var(--accent), #f43f5e); box-shadow: 0 6px 18px rgba(244,115,54,0.18); }
    /* confetti */
    .confetti-piece { position: absolute; width: 10px; height: 16px; opacity: 0.95; transform-origin: center; will-change: transform; animation: confetti-fall 2000ms linear forwards; }
    @keyframes confetti-fall {
      0% { transform: translateY(-20vh) rotate(0deg); opacity: 1; }
      100% { transform: translateY(100vh) rotate(720deg); opacity: 0.95; }
    }
    /* subtle appear */
    .fade { animation: fadeIn .28s ease-out both; }
    @keyframes fadeIn { from { opacity: 0; transform: translateY(6px) } to { opacity: 1; transform: translateY(0) } }

    /* cartoon character */
    .character { width: 110px; height: 110px; border-radius: 24px; background: linear-gradient(180deg,#fff 0%, #fef3c7 50%); box-shadow: 0 10px 30px rgba(2,6,23,0.06); display:flex; align-items:center; justify-content:center; }
  </style>
</head>
<body class="min-h-screen flex items-center justify-center p-6">
  <div id="app" class="w-full max-w-4xl mx-auto card-glass rounded-3xl shadow-2xl overflow-hidden border border-white/30">

    <!-- Header -->
    <header class="flex items-center gap-4 p-6 bg-gradient-to-r from-indigo-600 to-cyan-600 text-white">
      <div class="character shrink-0">
        <!-- Friendly SVG character -->
        <svg viewBox="0 0 120 120" xmlns="http://www.w3.org/2000/svg" class="w-full h-full">
          <defs>
            <linearGradient id="g1" x1="0" x2="1"><stop offset="0" stop-color="#fff" /><stop offset="1" stop-color="#fef08a"/></linearGradient>
            <linearGradient id="g2" x1="0" x2="1"><stop offset="0" stop-color="#60a5fa"/><stop offset="1" stop-color="#06b6d4"/></linearGradient>
          </defs>
          <rect x="6" y="6" rx="18" ry="18" width="108" height="108" fill="url(#g1)" opacity="0.95"/>
          <!-- face -->
          <circle cx="42" cy="52" r="6" fill="#0f172a"/>
          <circle cx="78" cy="52" r="6" fill="#0f172a"/>
          <path d="M46 74 C56 86, 74 86, 84 74" stroke="#0f172a" stroke-width="3" fill="none" stroke-linecap="round"/>
          <!-- headband -->
          <rect x="22" y="30" rx="6" ry="6" width="76" height="14" fill="url(#g2)" />
          <circle cx="92" cy="37" r="6" fill="#f97316" />
        </svg>
      </div>

      <div>
        <h1 class="text-2xl font-bold">Think Before You Risk</h1>
        <p class="text-sm opacity-90">เกมทดสอบทักษะการคิดและการตัดสินใจ เพื่อเอาตัวรอดจากความเสี่ยงในโลกจริงและโลกออนไลน์</p>
      </div>

      <div class="ml-auto text-right text-sm opacity-90">
        <div>เวลาเล่น: ~3 นาที</div>
        <div>คำถาม: 10 ข้อ</div>
      </div>
    </header>

    <!-- Main -->
    <main class="p-6 bg-transparent">
      <div id="page-container" class="fade"></div>
    </main>

    <footer class="p-4 text-center text-xs text-slate-600 bg-white/40">
      สร้างขึ้นเพื่อฝึกการตัดสินใจ — เก็บข้อมูลในเครื่องเท่านั้น
    </footer>
  </div>

  <!-- confetti container -->
  <div id="confetti-root" style="position:fixed; inset:0; pointer-events:none;"></div>

<script>
/* ====== QUESTIONS (unchanged content) ====== */
const QUESTIONS = [
  {id:1,text:"คุณได้รับข้อความจากคนไม่รู้จักในโซเชียลว่า 'ฉันมีของดีจะให้ดู' พร้อมลิงก์ คุณจะทำอย่างไร?",choices:[
    {text:"ไม่คลิกลิงก์ และบล็อก/รายงานผู้ส่ง",score:2,explain:"ลิงก์จากคนไม่รู้จักอาจเป็นฟิชชิ่งหรือมัลแวร์ — ปลอดภัยที่สุดคือตัดการติดต่อ"},
    {text:"คลิกแต่ระวัง ถ้ามีปัญหาจะปิดทันที",score:1,explain:"การคลิกยังเสี่ยง — ถ้าจำเป็นต้องตรวจสอบใช้เครื่องที่ปลอดภัยหรือ sandbox"},
    {text:"คลิกและกรอกข้อมูลเพื่อรับของฟรี",score:0,explain:"ให้ข้อมูลส่วนตัวบนลิงก์ที่ไม่แน่ชัดเสี่ยงต่อการถูกหลอกหรือถูกขโมยข้อมูล"}
  ]},
  {id:2,text:"คุณเจอ Wi-Fi สาธารณะชื่อ 'Free Airport WiFi' ในร้านกาแฟและต้องการเช็กข่าวด่วน จะเชื่อมต่ออย่างไร?",choices:[
    {text:"หลีกเลี่ยงการทำธุรกรรมสำคัญและใช้ VPN หากจำเป็น",score:2,explain:"Wi-Fi สาธารณะเสี่ยง ควรไม่ทำธุรกรรมหรือใช้ VPN เพื่อเข้ารหัสข้อมูล"},
    {text:"เชื่อมต่อแล้วเข้าเว็บปกติ แต่ไม่ใส่รหัสผ่าน",score:1,explain:"พอรับได้สำหรับการท่องเว็บทั่วไป แต่ยังเสี่ยงหากเว็บไซต์ไม่เข้ารหัส"},
    {text:"เชื่อมต่อแล้วล็อกอินเข้าแอปธนาคารทันที",score:0,explain:"ห้ามทำบัญชี/ธุรกรรมสำคัญบนเครือข่ายสาธารณะที่ไม่เชื่อถือได้"}
  ]},
  {id:3,text:"มีคนขอเข้ากลุ่มแชทโรงเรียนและส่งรูปเด็กนักเรียนหลายคนมาให้ตรวจ คุณจะทำอย่างไร?",choices:[
    {text:"ไม่แชร์/ไม่ดาวน์โหลดรูปโดยไม่ได้รับอนุญาตจากผู้ปกครอง",score:2,explain:"ภาพเด็กเป็นข้อมูลอ่อนไหว ต้องได้รับความยินยอมก่อนเผยแพร่"},
    {text:"ดาวน์โหลดแล้วแชร์ต่อให้คนในกลุ่มที่อาจช่วยได้",score:1,explain:"แชร์โดยไม่ขออนุญาตเพิ่มความเสี่ยงต่อความเป็นส่วนตัว"},
    {text:"โพสต์ภาพลงโซเชียลเพื่อให้คนเห็นเยอะๆ",score:0,explain:"การโพสต์เปิดเผยรูปเด็กโดยไม่มีอนุญาตเป็นความเสี่ยงสูงและผิดจริยธรรม"}
  ]},
  {id:4,text:"คุณเห็นโฆษณา 'รับเงินคืนง่ายๆ' ที่ขอรหัส OTP ของคุณเพื่อยืนยัน จะให้รหัสหรือไม่?",choices:[
    {text:"ไม่ให้ OTP แก่ใครรวมทั้งคนที่อ้างว่าเป็นบริษัท",score:2,explain:"OTP คือข้อมูลสำคัญ ไม่ควรส่งให้ผู้อื่นแม้เขาจะอ้างว่าเป็นฝ่ายบริการ"},
    {text:"ให้ถ้าคนส่งบอกว่าเป็นฝ่ายบริการลูกค้าที่เชื่อถือได้",score:1,explain:"แม้จะดูเชื่อถือได้ แต่ควรตรวจสอบช่องทางอย่างเป็นทางการก่อนส่ง OTP"},
    {text:"ให้ทันทีเพื่อรับสิทธิ์",score:0,explain:"ให้ OTP เสี่ยงต่อการถูกแฮ็กบัญชีหรือถูกหลอกให้โอนเงิน"}
  ]},
  {id:5,text:"คุณได้ไฟล์ .exe จากเพื่อนที่ส่งมาทางแชท บอกว่าเป็นเกมใหม่ คุณจะทำอย่างไร?",choices:[
    {text:"ไม่เปิดไฟล์ และขอแหล่งที่มาชัดเจนหรือสแกนก่อน",score:2,explain:"ไฟล์ปฏิบัติการอาจมีมัลแวร์ ควรยืนยันแหล่งที่มาหรือใช้ sandbox/สแกนก่อน"},
    {text:"เปิดบนเครื่องบ้านถ้าอยากลอง",score:1,explain:"ยังเสี่ยง — ควรใช้เครื่องที่แยกจากข้อมูลสำคัญหรือ VM"},
    {text:"เปิดทันทีเพราะไว้ใจเพื่อน",score:0,explain:"การไว้ใจโดยไม่ตรวจสอบเป็นสาเหตุหลักของการติดมัลแวร์"}
  ]},
  {id:6,text:"คุณต้องการนัดเจอคนรู้จักจากแอปหาคู่ จะทำอย่างไรเพื่อความปลอดภัย?",choices:[
    {text:"นัดในที่สาธารณะ บอกคนใกล้ชิดและไม่ไปบ้านคนแปลกหน้า",score:2,explain:"นัดในที่สาธารณะและบอกคนรอบตัวช่วยลดความเสี่ยงได้มาก"},
    {text:"นัดที่บ้านหรือที่เปลี่ยวแต่ไปพร้อมเพื่อน",score:1,explain:"ยังเสี่ยงอยู่ ควรหลีกเลี่ยงสถานที่ปิดหรือเปลี่ยว"},
    {text:"ไปตามลำพังที่บ้านเขาเพราะอยากรู้จักเร็วๆ",score:0,explain:"เสี่ยงต่อการถูกข่มขืน/ทำร้าย/หลอกลวง — ห้ามไปคนเดียว"}
  ]},
  {id:7,text:"คุณสร้างรหัสผ่านใหม่ จะเลือกแบบไหนดีที่สุด?",choices:[
    {text:"รหัสยาวผสมตัวอักษร ตัวเลข สัญลักษณ์ และไม่ใช้ซ้ำ",score:2,explain:"รหัสผ่านแข็งแรงและไม่ซ้ำคือการป้องกันบัญชีขั้นพื้นฐาน"},
    {text:"ใช้รหัสเดิมที่แก้ไขนิดหน่อยเพื่อจำง่าย",score:1,explain:"สะดวกแต่เสี่ยงเมื่อรหัสเก่าโดนแฮ็กแล้ว"},
    {text:"ใช้รหัสสั้น/คำง่าย เช่น '12345' เพราะจำง่าย",score:0,explain:"รหัสง่ายถูกเดาได้เร็วและเสี่ยงสูงต่อการถูกเข้าถึง"}
  ]},
  {id:8,text:"คุณเห็นโพสต์ระดมเงินช่วยเหลือและลิงก์ให้บริจาค ให้วิธีไหนปลอดภัยสุด?",choices:[
    {text:"ตรวจสอบแหล่งที่มาผ่านเว็บไซต์ทางการหรือช่องทางที่เชื่อถือได้ก่อนบริจาค",score:2,explain:"ตรวจสอบองค์กรก่อนบริจาคเพื่อลดการโดนหลอก"},
    {text:"บริจาคผ่านลิงก์ที่โพสต์ถ้ามีการแชร์เยอะๆ",score:1,explain:"การแชร์จำนวนมากไม่รับประกันความถูกต้อง — อาจเป็นการล้างข้อมูล"},
    {text:"คลิกลิงก์แล้วโอนทันที เพราะสงสาร",score:0,explain:"โอนโดยไม่ตรวจสอบเสี่ยงโดนมิจฉาชีพ"}
  ]},
  {id:9,text:"เพื่อนถามรหัสผ่าน Wi-Fi บ้านของคุณผ่านข้อความ จริงหรือไม่ที่ควรให้?",choices:[
    {text:"ไม่ให้ทางข้อความ — แนะนำให้ตั้ง Guest Wi-Fi หรือให้แบบส่วนตัวเท่านั้น",score:2,explain:"การให้รหัสผ่านผ่านช่องทางง่ายๆ เสี่ยงต่อการแพร่ค่านิยมการแชร์ข้อมูลสำคัญ"},
    {text:"ให้แต่เปลี่ยนรหัสทีหลัง",score:1,explain:"การให้แล้วเปลี่ยนเป็นมาตรการลดความเสี่ยง แต่ไม่ใช่วิธีที่ดีที่สุด"},
    {text:"ให้ทันทีเพื่อความสะดวก",score:0,explain:"การส่งรหัสผ่านเสี่ยงต่อการถูกส่งต่อและการเข้าถึงโดยผู้ไม่หวังดี"}
  ]},
  {id:10,text:"คุณถูกชวนเข้าร่วมแคมเปญออนไลน์ให้แสดงความคิดเห็นแล้วจะได้รางวัล ต้องลงทะเบียนด้วยข้อมูลส่วนตัว คุณจะทำอย่างไร?",choices:[
    {text:"ให้ข้อมูลพื้นฐานเท่าที่จำเป็นและตรวจสอบนโยบายความเป็นส่วนตัว",score:2,explain:"ให้ข้อมูลขั้นต่ำและอ่านนโยบายช่วยให้คุณรู้ว่าข้อมูลจะถูกใช้ยังไง"},
    {text:"ให้ข้อมูลทั้งหมดเพื่อเพิ่มโอกาสชนะ",score:1,explain:"การให้ข้อมูลมากเกินจำเป็นเพิ่มความเสี่ยงข้อมูลรั่วไหล"},
    {text:"ไม่สนใจเพราะกลัวข้อมูลจะถูกเอาไปใช้",score:0,explain:"หลีกเลี่ยงเสี่ยงได้แต่ถ้าต้องการเข้าร่วมควรให้ข้อมูลขั้นต่ำและรู้เงื่อนไข"}
  ]}
];

/* ====== App State ====== */
const container = document.getElementById('page-container');
const confettiRoot = document.getElementById('confetti-root');

let state = {
  name: localStorage.getItem('tbry_name') || '',
  current: 0,
  answers: []
};

function saveName(n){ state.name=n; try{localStorage.setItem('tbry_name', n);}catch(e){} }

function renderStart(){
  container.innerHTML = `
    <div class="grid md:grid-cols-2 gap-6 items-center">
      <div>
        <h2 class="text-2xl font-bold text-slate-800 mb-2">พร้อมจะทดสอบสกิลการตัดสินใจของคุณหรือยัง?</h2>
        <p class="text-slate-600 mb-4">เกมสั้น 10 คำถาม — เลือกตัวเลือกที่คุณคิดว่าเหมาะสมที่สุด แต่ละข้อคือสถานการณ์เสี่ยงที่เจอบ่อยทั้งโลกจริงและออนไลน์</p>
        <div class="mb-4">
          <label class="block text-sm font-medium text-slate-700 mb-2">ชื่อผู้เล่น</label>
          <input id="player-name" value="${escapeHtml(state.name)}" placeholder="พิมพ์ชื่อหรือชื่อเล่น" class="w-full rounded-xl border border-slate-200 px-4 py-3 shadow-sm focus:outline-none focus:ring-2 focus:ring-cyan-200" />
        </div>

        <div class="flex gap-3">
          <button id="btn-start" class="btn-primary px-5 py-3 rounded-xl font-semibold hover:scale-[1.02] transition">เริ่มเล่น</button>
          <button id="btn-sample" class="px-4 py-3 rounded-xl border border-slate-200">ดูตัวอย่าง</button>
          <button id="btn-reset" class="px-4 py-3 rounded-xl border border-red-200 text-red-600">ล้างชื่อ</button>
        </div>

        <div class="mt-6 text-sm text-slate-500">
          <strong>คำแนะนำ:</strong> เลือกตัวเลือกที่คุณคิดว่าเหมาะสมที่สุด — เมื่อจบเกมจะมีคำแนะนำเชิงปฏิบัติให้
        </div>
      </div>

      <div class="text-center">
        <!-- decorative card -->
        <div class="bg-gradient-to-br from-white to-amber-50 rounded-2xl p-6 shadow-xl">
          <div class="text-left mb-3">
            <div class="text-xs text-slate-500">เครื่องมือเช็กสกิล</div>
            <div class="text-lg font-semibold text-slate-800">Think Before You Risk</div>
          </div>
          <div class="mb-4">
            <svg viewBox="0 0 320 180" class="w-full h-32 rounded-lg overflow-visible">
              <defs>
                <linearGradient id="gA" x1="0" x2="1"><stop offset="0" stop-color="#60a5fa"/><stop offset="1" stop-color="#06b6d4"/></linearGradient>
              </defs>
              <rect x="0" y="0" width="320" height="120" rx="12" fill="url(#gA)" opacity="0.12"></rect>
              <g transform="translate(12,12)">
                <circle cx="48" cy="48" r="24" fill="#fff"></circle>
                <path d="M90 36 L120 60 L90 84" stroke="#0f766e" stroke-width="3" fill="none" stroke-linecap="round"></path>
              </g>
            </svg>
          </div>

          <div class="text-sm text-slate-600">สีสันสดใสและการ์ตูนแอนิเมชันช่วยให้การฝึกตัดสินใจสนุกขึ้น ลองเล่นแล้วแชร์ผลกับเพื่อน!</div>
        </div>
      </div>
    </div>
  `;

  document.getElementById('btn-start').onclick = () => {
    const nm = (document.getElementById('player-name').value || 'ผู้เล่นไม่ระบุ').trim();
    saveName(nm);
    state.current = 1; state.answers=[];
    renderQuestion(1);
  };
  document.getElementById('btn-sample').onclick = renderSample;
  document.getElementById('btn-reset').onclick = ()=>{ saveName(''); state.name=''; renderStart(); }
}

function renderSample(){
  const q = QUESTIONS[0];
  container.innerHTML = `
    <div>
      <div class="flex items-center justify-between mb-4">
        <div>
          <h3 class="text-lg font-semibold">ตัวอย่างคำถาม</h3>
          <p class="text-sm text-slate-500">ลองดูตัวอย่างนี้ก่อนเริ่มเล่น</p>
        </div>
        <div class="text-sm text-slate-500">คำถาม 1 / ${QUESTIONS.length}</div>
      </div>

      <div class="bg-white rounded-xl p-5 shadow-sm border border-slate-100 mb-4">
        <p class="font-medium text-slate-800">${escapeHtml(q.text)}</p>
        <div class="mt-3 space-y-2">
          ${q.choices.map((c,i)=>`<div class="p-3 rounded-lg ${i===0?'bg-green-50 border border-green-100':''}">${escapeHtml(c.text)}</div>`).join('')}
        </div>
      </div>

      <div class="flex gap-3">
        <button id="back" class="px-4 py-2 rounded-xl border">กลับ</button>
        <button id="begin" class="btn-primary px-4 py-2 rounded-xl">เริ่มเล่นจริง</button>
      </div>
    </div>
  `;
  document.getElementById('back').onclick = renderStart;
  document.getElementById('begin').onclick = ()=>{ state.current=1; state.answers=[]; renderQuestion(1); };
}

function renderQuestion(index){
  const q = QUESTIONS[index-1];
  if(!q) return renderResults();

  const progressPct = Math.round(((index-1)/QUESTIONS.length)*100);

  container.innerHTML = `
    <div class="space-y-4">
      <div class="flex items-center justify-between">
        <div>
          <div class="text-sm text-slate-500">ผู้เล่น: <span class="font-medium text-slate-700">${escapeHtml(state.name||'ไม่ระบุ')}</span></div>
          <h3 class="text-xl font-semibold text-slate-800 mt-1">คำถามที่ ${index} จาก ${QUESTIONS.length}</h3>
        </div>
        <div class="w-56">
          <div class="text-xs text-slate-500 mb-1">ความคืบหน้า</div>
          <div class="progress-track"><div class="progress-fill" style="width:${progressPct}%;"></div></div>
          <div class="text-xs text-slate-400 mt-1 text-right">${progressPct}%</div>
        </div>
      </div>

      <div class="bg-white p-6 rounded-2xl shadow-sm border border-slate-100">
        <p class="text-base font-medium text-slate-800">${escapeHtml(q.text)}</p>
      </div>

      <div class="grid gap-3">
        ${q.choices.map((c,i)=>`
          <button data-choice="${i}" class="choice-btn text-left p-4 rounded-2xl border border-slate-200 bg-white hover:bg-gradient-to-r hover:from-white hover:to-amber-50 transition">
            <div class="flex items-center justify-between">
              <div class="min-w-0">
                <div class="text-sm font-medium text-slate-800">${escapeHtml(c.text)}</div>
                <div class="text-xs text-slate-400 mt-1">ตัวเลือก ${i+1}</div>
              </div>
              <div class="text-2xl opacity-80">${i===0? '🟢' : i===1? '🟡' : '🔴'}</div>
            </div>
          </button>
        `).join('')}
      </div>

      <div class="flex items-center justify-between">
        <button id="btn-back" class="px-4 py-2 rounded-xl border">ย้อนกลับ</button>
        <div class="text-sm text-slate-500">คำตอบที่เลือกแล้ว: <strong class="text-slate-700">${state.answers.length}</strong></div>
      </div>
    </div>
  `;

  // attach handlers
  Array.from(container.querySelectorAll('.choice-btn')).forEach(btn=>{
    btn.onclick = ()=>{
      const cidx = Number(btn.getAttribute('data-choice'));
      const choice = q.choices[cidx];
      state.answers.push({ qid:q.id, choiceIndex:cidx, score: choice.score });
      if(state.current < QUESTIONS.length){ state.current+=1; renderQuestion(state.current); }
      else { state.current = QUESTIONS.length+1; renderResults(); }
    };
  });

  document.getElementById('btn-back').onclick = ()=>{
    if(state.current<=1){ state.current=0; renderStart(); return; }
    if(state.answers.length>0) state.answers.pop();
    state.current -= 1;
    renderQuestion(state.current);
  };
}

function renderResults(){
  // compute total internally but do NOT display numeric score
  const total = state.answers.reduce((s,a)=>s+a.score,0);
  const max = QUESTIONS.length * 2;
  const pct = Math.round((total/max)*100);

  // feedback label (no numbers shown)
  let title, adv;
  if(total <= 6){
    title = "เสี่ยงสูง — ควรปรับปรุง";
    adv = ["เรียนรู้พื้นฐานความปลอดภัยออนไลน์ (OTP, ฟิชชิ่ง)", "ตั้งรหัสผ่านที่แข็งแรง & เปิด 2FA", "ไม่แชร์ข้อมูล/รูปภาพโดยไม่ได้รับอนุญาต", "ใช้ VPN/หลีกเลี่ยงเครือข่ายสาธารณะสำหรับธุรกรรม"];
  } else if(total <= 13){
    title = "พอใช้ — ควรเสริมทักษะ";
    adv = ["ฝึกสังเกตกลลวงออนไลน์", "สอบทวนก่อนคลิกลิงก์/ดาวน์โหลด", "อ่านนโยบายความเป็นส่วนตัวก่อนให้ข้อมูล", "สอนคนใกล้ชิดเรื่องความปลอดภัยพื้นฐาน"];
  } else {
    title = "ดีมาก! — สกิลแข็งแรง";
    adv = ["ลงลึกเรื่องการจัดการความเป็นส่วนตัวแพลตฟอร์ม", "ช่วยสอนผู้อื่น แชร์แนวปฏิบัติที่ดี", "ตรวจสอบความปลอดภัยขั้นสูงของอุปกรณ์และบัญชี"];
  }

  container.innerHTML = `
    <div class="space-y-4">
      <div class="flex items-center justify-between">
        <div>
          <h2 class="text-2xl font-bold text-slate-800">ผลการทดสอบ</h2>
          <p class="text-sm text-slate-500">ผู้เล่น: <strong>${escapeHtml(state.name||'ไม่ระบุ')}</strong></p>
        </div>
        <div class="text-right">
          <div class="text-lg font-extrabold" style="background:linear-gradient(90deg,#f97316,#ef4444); -webkit-background-clip:text; color:transparent;">${title}</div>
          <div class="text-xs text-slate-500 mt-1">ประเมินเชิงคุณภาพ (ไม่แสดงคะแนนตัวเลข)</div>
        </div>
      </div>

      <div class="p-4 rounded-2xl bg-white border border-slate-100 shadow-sm">
        <div class="flex items-center gap-4">
          <div class="w-16 h-16 rounded-lg flex items-center justify-center bg-gradient-to-br from-amber-100 to-pink-50">
            <div class="text-3xl">🎉</div>
          </div>
          <div>
            <div class="font-semibold text-slate-800">ข้อเสนอแนะ</div>
            <div class="text-sm text-slate-600 mt-1">คำแนะนำที่ควรศึกษาเพิ่มเติม:</div>
            <ul class="mt-2 text-sm text-slate-600 list-disc list-inside">
              ${adv.map(it=>`<li>${escapeHtml(it)}</li>`).join('')}
            </ul>
          </div>
        </div>
      </div>

      <details class="bg-white border border-slate-100 rounded-xl p-4">
        <summary class="font-medium cursor-pointer">ดูคำอธิบายสำหรับแต่ละคำตอบ</summary>
        <div class="mt-3 space-y-3">
          ${QUESTIONS.map((q,idx)=>{
            const ans = state.answers.find(a=>a.qid===q.id);
            const chosen = ans ? q.choices[ans.choiceIndex] : null;
            return `
              <div class="border-b border-slate-100 pb-3">
                <div class="text-sm font-medium">${idx+1}. ${escapeHtml(q.text)}</div>
                <div class="text-xs mt-1 ${chosen ? '' : 'text-red-600'}">คำตอบของคุณ: <strong>${chosen?escapeHtml(chosen.text):'ไม่ได้ตอบ'}</strong></div>
                <div class="text-xs text-slate-500 mt-1">${chosen?escapeHtml(chosen.explain):''}</div>
              </div>
            `;
          }).join('')}
        </div>
      </details>

      <div class="flex gap-3">
        <button id="btn-retry" class="btn-primary px-4 py-3 rounded-xl">เล่นอีกครั้ง</button>
        <button id="btn-share" class="px-4 py-3 rounded-xl border">คัดลอกสรุป</button>
        <button id="btn-home" class="px-4 py-3 rounded-xl border">กลับหน้าหลัก</button>
      </div>
    </div>
  `;

  // confetti for decent performance (still internal logic uses numeric but we don't show)
  if(pct >= 50){
    launchConfetti(28);
  }

  document.getElementById('btn-retry').onclick = ()=>{ state.answers=[]; state.current=1; renderQuestion(1); }
  document.getElementById('btn-home').onclick = ()=>{ state.current=0; renderStart(); }
  document.getElementById('btn-share').onclick = ()=>{
    // share qualitative summary only (no numeric score shown)
    const summary = `ThinkBeforeYouRisk | ผู้เล่น:${state.name||'ไม่ระบุ'} | ผล: ${title}`;
    copyTextToClipboard(summary);
    alert('คัดลอกข้อความสรุปแล้ว: ' + summary);
  }
}

/* ===== helpers ===== */
function escapeHtml(s){ return String(s||'').replaceAll('&','&amp;').replaceAll('<','&lt;').replaceAll('>','&gt;'); }
function copyTextToClipboard(text){
  if(navigator.clipboard && navigator.clipboard.writeText) navigator.clipboard.writeText(text).catch(()=>fallback(text));
  else fallback(text);
  function fallback(t){ const ta=document.createElement('textarea'); ta.value=t; document.body.appendChild(ta); ta.select(); try{document.execCommand('copy')}catch(e){} ta.remove(); }
}

/* ===== confetti ===== */
function randomInt(min,max){ return Math.floor(Math.random()*(max-min+1))+min; }
function launchConfetti(count=24){
  const colors = ['#f97316','#06b6d4','#60a5fa','#f43f5e','#34d399','#f59e0b'];
  const root = confettiRoot;
  for(let i=0;i<count;i++){
    const el = document.createElement('div');
    el.className = 'confetti-piece';
    el.style.left = (Math.random()*100)+'vw';
    el.style.top = '-10vh';
    el.style.background = colors[i % colors.length];
    el.style.width = (8 + Math.random()*8)+'px';
    el.style.height = (10 + Math.random()*14)+'px';
    el.style.transform = `rotate(${randomInt(0,360)}deg)`;
    el.style.opacity = (0.7 + Math.random()*0.3);
    el.style.animationDuration = (1400 + Math.random()*1200) + 'ms';
    root.appendChild(el);
    setTimeout(()=> el.remove(), 3500);
  }
}

/* ===== start ===== */
renderStart();
</script>
</body>
</html>
