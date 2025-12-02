<html lang="th">
<head>
<meta charset="UTF-8">
<title>เกมทายสถานการณ์เสี่ยง – Healthy Hero Edition</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<style>
/* ฟอนต์และการตั้งค่าเริ่มต้น */
* { font-family: "Sarabun", sans-serif; box-sizing: border-box; }
body {
  margin: 0;
  padding: 20px;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  background: linear-gradient(135deg, #ffafbd, #ffc3a0, #fff6b7);
}

/* กล่องหลัก */
.container {
  width: 95%;
  max-width: 820px;
  background: #ffffffd9;
  backdrop-filter: blur(6px);
  padding: 25px;
  border-radius: 20px;
  box-shadow: 0px 8px 20px rgba(0,0,0,0.2);
  animation: fadeIn .5s ease;
}

@keyframes fadeIn {
  from { opacity:0; transform: scale(0.96); }
  to { opacity:1; transform: scale(1); }
}

/* ส่วนหัว */
h1 {
  text-align: center;
  font-size: 1.8rem;
  color: #e11d48;
  margin-top: 0;
}

/* ปุ่ม */
.btn {
  padding: 12px 20px;
  font-size: 1rem;
  border: none;
  border-radius: 30px;
  cursor: pointer;
  color: white;
  transition: .2s;
}

.btn:hover { transform: scale(1.05); }

.btn-start { background: #ff6b81; }
.btn-next { background: #1e90ff; }
.btn-restart { background: #2ed573; }

/* กล่องคำถาม */
.question-box {
  background: #fff9db;
  border: 2px solid #ffd36b;
  padding: 15px;
  border-radius: 15px;
  margin-bottom: 15px;
}

/* ช้อยส์ */
.choice-btn {
  width: 100%;
  text-align: left;
  padding: 12px 12px 12px 45px;
  margin: 6px 0;
  border-radius: 15px;
  border: none;
  font-size: 1rem;
  background: #e8f9fd;
  color: #222;
  cursor: pointer;
  position: relative;
  transition: .2s;
}

.choice-btn:hover { background:#d0f1ff; transform: scale(1.03); }

.choice-btn span.label {
  position: absolute;
  left: 12px;
  top: 50%;
  transform: translateY(-50%);
  width: 26px;
  height: 26px;
  background: #ff6b81;
  border-radius: 50%;
  color: white;
  display: flex;
  justify-content: center;
  align-items: center;
}

/* เมื่อเลือกคำตอบ */
.correct { background:#d4fcd4 !important; border:2px solid #2ed573; }
.wrong { background:#ffd7d7 !important; border:2px solid #ff6b6b; }

/* กล่องคำอธิบาย */
.feedback {
  display: none;
  margin-top: 10px;
  padding: 12px;
  border-radius: 12px;
  font-size: 1rem;
  background: #ecf0f1;
}

/* หน้าเริ่มเกม */
#startScreen { text-align:center; }

#playerInput {
  width: 75%;
  padding: 10px;
  border-radius: 12px;
  border: 2px solid #ff6b81;
  font-size: 1.1rem;
}

/* สรุปคะแนน */
#summaryScreen { display:none; text-align:center; }

.score-box {
  background:#fff1c1;
  padding:20px;
  border-radius:15px;
  margin-top:15px;
}

</style>
</head>

<body>
<div class="container">

<!-- ⭐ หน้าเริ่มเกม ⭐ -->
<section id="startScreen">
  <h1>🎮 เกมทายสถานการณ์เสี่ยง 🎮</h1>
  <p>พิมพ์ชื่อผู้เล่นก่อนเริ่มเกม</p>
  <input type="text" id="playerInput" placeholder="เช่น ใบตอง, โฟกัส, พี" />
  <br><br>
  <button class="btn btn-start" id="startBtn">เริ่มเกม ▶</button>
</section>

<!-- ⭐ หน้าคำถาม ⭐ -->
<section id="gameScreen" style="display:none;">
  <h1>Healthy Hero 💡</h1>
  <p style="text-align:center; font-size:1.1rem; color:#d6336c;">
    ผู้เล่น: <span id="playerName"></span>
  </p>

  <p>ข้อที่ <span id="qNumber">1</span> / <span id="qTotal">10</span></p>
  <p>คะแนน: <span id="score">0</span></p>

  <div class="question-box">
    <div id="questionText">กำลังโหลด...</div>
  </div>

  <div id="choicesContainer"></div>

  <div id="feedback" class="feedback"></div>

  <br>
  <button class="btn btn-next" id="nextBtn" disabled>ข้อถัดไป ▶</button>
</section>

<!-- ⭐ หน้าสรุปคะแนน ⭐ -->
<section id="summaryScreen">
  <h1>🎉 ผลสรุปของเกม 🎉</h1>
  <p style="font-size:1.3rem;">ผู้เล่น: <b><span id="finalName"></span></b></p>

  <div class="score-box">
    <h2>คะแนนรวม: <span id="finalScore"></span> / <span id="finalTotal"></span></h2>
    <p id="finalComment" style="font-size:1.1rem;"></p>
  </div>

  <br>
  <button class="btn btn-restart" id="restartBtn">เล่นอีกครั้ง 🔁</button>
</section>

</div>

<script>
// =======================
// ชุดคำถาม
// =======================
const QUESTIONS = [
  {
    text: "มีคนแปลกหน้าขอรูปส่วนตัวในโซเชียล",
    choices: [
      "ส่งรูปให้โดยไม่เห็นหน้า",
      "ส่งรูปเฉพาะเขาคนเดียว",
      "ปฏิเสธ บล็อก และรายงานบัญชี",
      "โพสต์ประจานลงโซเชียล"
    ],
    correct: 2,
    explain: "การบล็อกและรายงานเป็นวิธีที่ปลอดภัยที่สุด"
  },
  {
    text: "เพื่อนชวนไปบ้านตอนพ่อแม่ไม่อยู่",
    choices: [
      "ไปโดยไม่บอกใคร",
      "ไปแต่แชร์โลเคชันให้ที่บ้านรู้",
      "ไม่ไปเด็ดขาด",
      "ไปนอนค้างโดยไม่ขออนุญาต"
    ],
    correct: 1,
    explain: "ควรบอกผู้ปกครองและกำหนดเวลาชัดเจน"
  },
  {
    text: "แฟนพูดว่า “ถ้ารักกันต้องยอมทุกอย่าง”",
    choices: [
      "ยอมทำตาม",
      "คุยและตั้งขอบเขตอย่างชัดเจน",
      "เงียบไม่ตอบ",
      "ปรึกษาเพื่อนวัยเดียวกันเท่านั้น"
    ],
    correct: 1,
    explain: "ต้องเคารพขอบเขตซึ่งกันและกันเสมอ"
  },
  {
    text: "เพื่อนส่งคลิปโป๊ในไลน์และให้ช่วยส่งต่อ",
    choices: [
      "ส่งต่อให้หลายกลุ่ม",
      "ดูคนเดียวแล้วเก็บไว้",
      "เตือนเพื่อนและออกจากกลุ่มหากยังส่งต่อ",
      "แคปลงสตอรี่"
    ],
    correct: 2,
    explain: "หยุดส่งต่อคือการลดความเสี่ยงและผิดกฎหมาย"
  }
];

let index = 0;
let score = 0;
const total = QUESTIONS.length;
document.getElementById("qTotal").textContent = total;

// =======================
// เริ่มเกม
// =======================
document.getElementById("startBtn").onclick = () => {
  const name = document.getElementById("playerInput").value.trim();
  if (name === "") { alert("กรุณากรอกชื่อ"); return; }

  document.getElementById("playerName").textContent = name;

  document.getElementById("startScreen").style.display = "none";
  document.getElementById("gameScreen").style.display = "block";

  loadQuestion();
};

// =======================
// โหลดคำถาม
// =======================
function loadQuestion() {
  const q = QUESTIONS[index];
  document.getElementById("qNumber").textContent = index + 1;
  document.getElementById("questionText").textContent = q.text;

  const choicesContainer = document.getElementById("choicesContainer");
  choicesContainer.innerHTML = "";

  document.getElementById("feedback").style.display = "none";
  document.getElementById("nextBtn").disabled = true;

  q.choices.forEach((c, i) => {
    const btn = document.createElement("button");
    btn.className = "choice-btn";
    btn.innerHTML = `<span class="label">${String.fromCharCode(65 + i)}</span> ${c}`;
    btn.onclick = () => checkAnswer(btn, i);
    choicesContainer.appendChild(btn);
  });
}

// =======================
// ตรวจคำตอบ
// =======================
function checkAnswer(btn, chosenIndex) {
  const q = QUESTIONS[index];
  const allButtons = document.querySelectorAll(".choice-btn");

  allButtons.forEach(b => b.disabled = true);

  const feedback = document.getElementById("feedback");

  if (chosenIndex === q.correct) {
    score++;
    feedback.className = "feedback correct";
    feedback.innerHTML = "✔ ตอบถูก!<br>" + q.explain;
    btn.classList.add("correct");
  } else {
    feedback.className = "feedback wrong";
    feedback.innerHTML = "✘ ยังไม่ใช่คำตอบที่ดีที่สุด<br>" + q.explain;
    btn.classList.add("wrong");

    // ไฮไลต์คำตอบที่ถูกต้อง
    allButtons[q.correct].classList.add("correct");
  }

  feedback.style.display = "block";
  document.getElementById("score").textContent = score;
  document.getElementById("nextBtn").disabled = false;
}

// =======================
// ปุ่มถัดไป
// =======================
document.getElementById("nextBtn").onclick = () => {
  index++;
  if (index < QUESTIONS.length) {
    loadQuestion();
  } else {
    showSummary();
  }
};

// =======================
// สรุปผลคะแนน
// =======================
function showSummary() {
  document.getElementById("gameScreen").style.display = "none";
  document.getElementById("summaryScreen").style.display = "block";

  document.getElementById("finalName").textContent =
    document.getElementById("playerName").textContent;

  document.getElementById("finalScore").textContent = score;
  document.getElementById("finalTotal").textContent = total;

  let comment = "";
  const ratio = score / total;

  if (ratio >= 0.8) comment = "เยี่ยมมาก! คุณวิเคราะห์ความเสี่ยงได้ดีมาก 👍";
  else if (ratio >= 0.5)
    comment = "ทำได้ดี แต่ยังพัฒนาได้อีก ลองอ่านคำอธิบายเพิ่มนะ 💡";
  else comment = "ควรทบทวนสถานการณ์เพื่อเพิ่มความปลอดภัยในชีวิตจริง ⚠";

  document.getElementById("finalComment").textContent = comment;
}

// =======================
// เริ่มใหม่
// =======================
document.getElementById("restartBtn").onclick = () => {
  index = 0;
  score = 0;
  document.getElementById("score").textContent = score;
  document.getElementById("summaryScreen").style.display = "none";
  document.getElementById("gameScreen").style.display = "block";
  loadQuestion();
};

</script>

</body>
</html>
