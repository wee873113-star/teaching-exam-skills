# 國小數學教學與出題技能集 (Elementary Math Teaching & Exam Skills)

這是一組專為**國小數學（一至六年級）教學、段考命題、審題、幾何配圖與形成性評量小遊戲**設計的 **Agent Skills**。
每個技能皆為獨立資料夾，內含一份 `SKILL.md`（含 YAML frontmatter 描述觸發時機），可供 AI agent 讀取使用。

> 所有技能皆以**繁體中文**設計，題目與認知分層對齊國小 108 課綱與適配小學生的認知水準。

---

## 📦 包含的技能

| 技能 | 用途 | 額外檔案 |
|------|------|----------|
| [`es-math-exam`](skills/es-math-exam/SKILL.md) | 國小數學段考出題與審題專家。設計一至六年級數學試題、審題、建立雙向細目表、分析 Bloom 認知層次分佈。對齊 108 課綱五大主題（數與計算、空間與形狀、量與實測、關係、數據表示）。 | — |
| [`es-math-context-questions`](skills/es-math-context-questions/SKILL.md) | 國小數學「生活情境非選擇題/應用題」命題。結合貼近小學生生活的時事或日常情境（分糖果、存錢、看時鐘等），產出理解與應用層次的兩小題式應用題（含詳解與驗算）。 | — |
| [`es-math-geometry`](skills/es-math-geometry/SKILL.md) | 國小數學幾何圖形 SVG 產生器。繪製長方形、正方形、三角形（標示底與高）、圓與扇形、長方體/正方體、圓柱體等，可被出題技能呼叫配圖。 | `scripts/`、`references/` |
| [`teaching-minigames`](skills/teaching-minigames/SKILL.md) | 把國小數學教材重點轉成形成性評量互動小遊戲（選擇、是非、配對等），發佈為可分享的 HTML 與 QR Code。 | `references/` |

---

## 🚀 給其他 Agent 使用

這是一個**純技能資料夾**結構。最簡單的用法：

```bash
git clone https://github.com/mathruffian-dot/teaching-exam-skills.git
```

然後讓你的 agent 讀取對應的 `skills/<技能名>/SKILL.md`。

### 安裝到個人技能目錄（PowerShell）

```powershell
Copy-Item teaching-exam-skills/skills/* $HOME/.claude/skills/ -Recurse
```

---

## ⚠️ 使用前注意（外部依賴）

| 技能 | 依賴 | 說明 |
|------|------|------|
| `es-math-context-questions` | `draw` 技能 | 若需生情境插圖，SKILL.md 可呼叫繪圖腳本（如 `generate_image` 或外部生圖工具）。 |
| `es-math-geometry` | Python 與相依套件 | `scripts/` 內為 SVG 轉檔與插入 Word/PPT 的腳本，需 Python 環境並安裝 `cairosvg`, `python-docx`, `python-pptx`。 |
| `teaching-minigames` | GitHub Token | 發佈到 GitHub Pages 時需自備 Personal Access Token（`repo` 權限）。 |

---

## 📄 授權

MIT License，詳見 [LICENSE](LICENSE)。歡迎自由使用與修改。
