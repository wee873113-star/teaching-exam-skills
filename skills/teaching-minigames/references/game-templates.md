# 國小數學互動小遊戲 HTML 模板參考

本文件包含各類型小遊戲的完整 HTML 模板。製作小遊戲時應複製並填入對應的題目與答案。

---

## 🎨 通用 CSS 樣式（國小版，字體加大、色彩溫馨）

```css
* { box-sizing: border-box; margin: 0; padding: 0; }
body {
  font-family: 'Segoe UI', '微軟正黑體', 'Apple LiGothic', sans-serif;
  background: #e7f5ff; /* 溫馨的軟萌粉藍色背景 */
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
  box-shadow: 0 8px 32px rgba(76, 110, 245, 0.12); /* 柔和的藍色陰影 */
}
h1 { font-size: 1.4rem; color: #3b5bdb; margin-bottom: 6px; text-align: center; }
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
.btn:hover { background: #edf2ff; border-color: #4c6ef5; }
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
.score-display { font-size: 1.3rem; font-weight: 800; color: #3b5bdb; margin: 20px 0; text-align: center; }
.replay-btn {
  background: #3b5bdb; color: white; border: none;
  border-radius: 12px; padding: 16px 24px;
  font-size: 1.1rem; cursor: pointer; width: 100%; margin-top: 20px; font-weight: bold;
}
.replay-btn:hover { background: #2f4ac0; }
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
  <title>重點1：分數的認識 - 選擇題</title>
  <style>
    /* 貼上通用 CSS */
  </style>
</head>
<body>
<div class="game-card">
  <h1>🎯 重點1：分數的認識</h1>
  <p class="subtitle">選擇題 · 請選出正確答案</p>
  <div class="progress" id="progress">⭐ 第 1 / 3 題</div>
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
    q: "把一個披薩平分切成 8 塊，小明拿了其中的 3 塊，請問是多少個披薩？",
    options: ["A. 8分之3 個", "B. 3分之8 個", "C. 3 個", "D. 8 個"],
    answer: 0,
    explanation: "平分切成 8 塊，每一塊是 8分之1 個，3 塊就是 8分之3 個喔！"
  },
  {
    q: "下列哪一個分數和「2分之1」一樣大？",
    options: ["A. 3分之1", "B. 4分之2", "C. 6分之2", "D. 5分之1"],
    answer: 1,
    explanation: "4分之2 約分之後（分子分母同除以 2）就是 2分之1 喔！"
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
  }, 2200);
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
  { text: "正方形也是長方形的一種。", answer: true, explanation: "對的！正方形有四個直角，符合長方形的特徵，而且它的四條邊都一樣長喔！" },
  { text: "分數的分子越大，分數的值就一定越大。", answer: false, explanation: "不對喔！還要看分母的大小。例如 2分之1 比 10分之1 大，但 1 的分子比 2 小。" }
];
// 其餘邏輯比照選擇題，按鈕改為「⭕ 正確」與「❌ 錯誤」
</script>
```
