# 國小自然科學教學與出題技能集 (Elementary Science Teaching & Exam Skills)

這是一組專為**國小自然科學（三至六年級獨立學科，一至二年級生活課程自然主題）教學、段考命題、審題、幾何與圖表配圖與形成性評量小遊戲**設計的 **Agent Skills**。
每個技能皆為獨立資料夾，內含一份 `SKILL.md`（含 YAML frontmatter 描述觸發時機），可供 AI agent 讀取使用。

> 所有技能皆以**繁體中文**設計，題目與認知分層對齊國小 108 課綱自然科學領域與生活課程，適配小學生的認知與探究水準。

---

## 📦 包含的技能

| 技能 | 用途 | 額外檔案 |
|------|------|----------|
| [`es-science-exam`](skills/es-science-exam/SKILL.md) | 國小自然科學段考出題與審題專家。設計三至六年級自然科（及一至二年級生活課程）試題、審題、建立雙向細目表、分析 Bloom 認知層次分佈。對齊 108 課綱四大主題（物質科學、生命科學、地球與宇宙科學、環境與永續發展）。 | — |
| [`es-science-context-questions`](skills/es-science-context-questions/SKILL.md) | 國小自然科學「生活情境非選擇題/探究與實作題」命題。結合貼近小學生生活的日常情境與科學現象（影子、溫度、溶解、電路、力學等），產出理解與應用/分析層次的兩小題式探究題（含詳解與驗算）。 | — |
| [`es-science-geometry`](skills/es-science-geometry/SKILL.md) | 國小科學幾何與圖表 SVG 產生器。繪製實驗裝置、力學示意圖、坐標圖表、圓與扇形等幾何圖案，可被出題技能呼叫配圖。 | `scripts/`、`references/` |
| [`es-science-minigames`](skills/es-science-minigames/SKILL.md) | 把國小自然科學教材重點轉成形成性評量互動小遊戲（選擇、是非、配對、排序等），發佈為可分享的 HTML 與 QR Code。 | `references/` |

---

## 🚀 給其他 Agent 使用

這是一個**純技能資料夾**結構。最簡單的用法：

```bash
git clone https://github.com/wee873113-star/teaching-exam-skills.git
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
| `es-science-context-questions` | `draw` 技能 | 若需生生活情境插圖，SKILL.md 可呼叫繪圖腳本（如 `generate_image` 或外部生圖工具）。 |
| `es-science-geometry` | Python 與相依套件 | `scripts/` 內為 SVG 轉檔與插入 Word/PPT 的腳本，需 Python 環境並安裝 `cairosvg`, `python-docx`, `python-pptx`。 |
| `es-science-minigames` | GitHub Token | 發佈到 GitHub Pages 時需自備 Personal Access Token（`repo` 權限）。 |

---

## 📄 授權

MIT License，詳見 [LICENSE](LICENSE)。歡迎自由使用與修改。
