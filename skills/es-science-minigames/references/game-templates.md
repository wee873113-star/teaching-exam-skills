# 國小自然科學互動小遊戲 HTML 模板參考

本文件包含各類型自然科小遊戲的完整 HTML 模板。製作小遊戲時應複製並填入對應的科學題目與答案。

---

## 🎨 通用 CSS 樣式（國小版，字體加大、色彩溫馨）

```css
* { box-sizing: border-box; margin: 0; padding: 0; }
body {
  font-family: 'Segoe UI', '微軟正黑體', 'Apple LiGothic', sans-serif;
  background: #ebfbee; /* 溫馨的軟萌粉綠色背景，符合自然科主題 */
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 16px;
}
.game-card {
  background: white;
  border-radius: 20px; /* 更圓潤的卡片 */
  padding: 32px 24px;
  max-width: 500px;
  width: 100%;
  box-shadow: 0 8px 32px rgba(40, 160, 87, 0.12); /* 柔和的綠色陰影 */
}
h1 { font-size: 1.4rem; color: #2b8a3e; margin-bottom: 6px; text-align: center; }
.subtitle { font-size: 0.95rem; color: #868e96; margin-bottom: 24px; text-align: center; font-weight: bold; }
.progress { font-size: 0.95rem; color: #495057; margin-bottom: 16px; font-weight: bold; }
.question { font-size: 1.25rem; color: #212529; margin-bottom: 20px; font-weight: 800; line-height: 1.6; }
.options { display: flex; flex-direction: column; gap: 12px; }
.btn {
  background: #f8f9fa; border: 3px solid #dee2e6; /* 加粗按鈕邊框 */
  border-radius: 12px; padding: 16px 20px;
  font-size: 1.1rem; cursor: pointer; text-align: left;
  transition: all 0.2s; color: #343a40; font-weight: bold;
}
.btn:hover { background: #ebfbee; border-color: #40c057; }
.btn.correct { background: #ebfbee; border-color: #40c057; color: #2b8a3e; }
.btn.wrong { background: #fff5f5; border-color: #fa5252; color: #c92a2a; }
.feedback {
  margin-top: 18px; padding: 14px 18px;
  border-radius: 12px; font-size: 1.05rem; font-weight: bold;
  display: none; line-height: 1.5;
}
.feedback.show { display: block; }
.feedback.ok { background: #ebfbee; color: #2b8a3e; border: 2px solid #40c057; }
.feedback.fail { background: #fff5f5; color: #c92a2a; border: 2px solid #fa5252; }
.score-display { font-size: 1.3rem; font-weight: 800; color: #2b8a3e; margin: 20px 0; text-align: center; }
.replay-btn {
  background: #40c057; color: white; border: none;
  border-radius: 12px; padding: 16px 24px;
  font-size: 1.1rem; cursor: pointer; width: 100%; margin-top: 20px; font-weight: bold;
}
.replay-btn:hover { background: #2b8a3e; }
.qr-section { text-align: center; margin-top: 28px; border-top: 2px dashed #e9ecef; padding-top: 20px; }
.qr-section p { font-size: 0.85rem; color: #868e96; margin-top: 8px; font-weight: bold; }
```

---

## 模板 A：選擇題 (MCQ)

```html
<!DOCTYPE html>
<html lang="zh-TW">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>重點1：熱的傳導 - 選擇題</title>
  <style>
    /* 貼上通用 CSS */
  </style>
</head>
<body>
<div class="game-card">
  <h1>🎯 重點1：熱的傳導</h1>
  <p class="subtitle">選擇題 · 請選出正確答案</p>
  <div class="progress" id="progress">⭐ 第 1 / 2 題</div>
  <div class="question" id="question"></div>
  <div class="options" id="options"></div>
  <div class="feedback" id="feedback"></div>
  <div id="result" style="display:none">
    <div class="score-display" id="score"></div>
    <button class="replay-btn" onclick="startGame()">🔄 再玩一次</button>
  </div>
  <div class="qr-section">
    <img id="qr" width="120" height="120" alt="QR Code">
    <p>掃描 QR Code 挑戰這關遊戲</p>
  </div>
</div>
<script>
const questions = [
  {
    q: "把鐵匙、木匙和塑膠匙同時放入熱水中，摸哪一個湯匙會感覺最燙？",
    options: ["A. 鐵匙", "B. 木匙", "C. 塑膠匙", "D. 三個湯匙摸起來一樣燙"],
    answer: 0,
    explanation: "鐵是金屬，屬於熱的良導體，所以熱傳導得最快，摸起來會最燙喔！"
  },
  {
    q: "下列關於空氣的熱對流現象，哪一個描述是正確的？",
    options: ["A. 冷空氣上升，熱空氣下降", "B. 熱空氣上升，冷空氣下降", "C. 冷熱空氣都只會往旁邊流動", "D. 空氣不會發生熱對流"],
    answer: 1,
    explanation: "熱空氣受熱膨脹密度變小會上升，冷空氣密度大會下降填補，這就是空氣的對流現象。因此冷氣機通常裝在房間的高處喔！"
  }
];

let current = 0, score = 0;

function startGame() {
  current = 0; score = 0;
  document.getElementById('result').style.display = 'none';
  showQuestion();
}

function showQuestion() {
  const q = questions[current];
  document.getElementById('progress').textContent = `⭐ 第 ${current+1} / ${questions.length} 題`;
  document.getElementById('question').textContent = q.q;
  const opts = document.getElementById('options');
  opts.innerHTML = '';
  const fb = document.getElementById('feedback');
  fb.className = 'feedback'; fb.textContent = '';
  q.options.forEach((opt, i) => {
    const btn = document.createElement('button');
    btn.className = 'btn';
    btn.textContent = opt;
    btn.onclick = () => answer(i, btn);
    opts.appendChild(btn);
  });
}

function answer(i, btn) {
  const q = questions[current];
  document.querySelectorAll('.btn').forEach(b => b.onclick = null);
  const fb = document.getElementById('feedback');
  if (i === q.answer) {
    btn.classList.add('correct');
    fb.textContent = `🎉 太棒了！答對了！✨ ${q.explanation}`;
    fb.className = 'feedback show ok';
    score++;
  } else {
    btn.classList.add('wrong');
    document.querySelectorAll('.btn')[q.answer].classList.add('correct');
    fb.textContent = `💪 差一點點，加油！正確答案是：${q.options[q.answer]}。${q.explanation}`;
    fb.className = 'feedback show fail';
  }
  current++;
  setTimeout(() => {
    if (current < questions.length) showQuestion();
    else {
      document.getElementById('result').style.display = 'block';
      document.getElementById('score').textContent = `🏆 挑戰成功！你的得分是：${score} / ${questions.length}`;
      document.getElementById('options').innerHTML = '';
      document.getElementById('question').textContent = '遊戲結束囉！';
    }
  }, 3000); // 自然科解釋較長，延時至 3 秒
}

document.getElementById('qr').src =
  `https://api.qrserver.com/v1/create-qr-code/?size=120x120&data=${encodeURIComponent(window.location.href)}`;

startGame();
</script>
</body>
</html>
```

---

## 模板 B：是非題 (True/False)

```html
<!-- 核心是非題 JS 結構 -->
<script>
const statements = [
  { text: "植物的根主要負責行光合作用，幫植物製造養分。", answer: false, explanation: "不對喔！行光合作用製造養分主要是靠含有葉綠體的『葉子』。而植物的『根』主要是負責吸收水分和礦物質，並把植物身體固定在泥土裡。" },
  { text: "月亮在白天也是有機會出現在天空中的。", answer: true, explanation: "對的！月亮繞地球公轉，當月球與太陽和地球的角度適合時，我們在清晨或傍晚的白天也有機會看到月亮喔！" }
];
// 其餘邏輯比照選擇題，按鈕改為「⭕ 正確」與「❌ 錯誤」
</script>
```
