---
name: es-science-exam-layout
description: >
  國小自然科評量卷排版與凸排格式專家技能。當使用者需要設計、生成或排版國小自然科試卷（是非題、選擇題、閱讀測驗等），
  並要求符合特定規格（主標題 14 號字標楷體、題目與內文 12 號字書法中楷且行距固定為 23 點、是非題與選擇題題首括號起點在左邊界且第二行起首字對齊題目第一個字之凸排縮排等）時，
  請一定要使用此技能。
  觸發情境包含：「自然評量」、「自然評量排版」、「自然評量格式」、「小學自然考卷排版」等。
---

# 國小自然評量卷排版與格式技能 (es-science-exam-layout)

## 技能概覽

本技能定義了國小自然評量卷（段考卷或平時練習卷）在 Microsoft Word (DOCX) 格式下的排版設計標準與對齊規範。透過本技能，您可以產出格式工整、文字垂直對齊完全一致，且能緊湊控制題目在 4 頁以內（解答在第 5 頁）的高品質評量卷。

---

## 排版規格標準

### 1. 字型與字體大小 (Font & Size)
* **主標題 (Section 0)**: 統一為 **`14` 號字 標楷體 (Bold)**。
  * *註：為防止 14 號字過大導致標題折行，必須稍微精簡校名或考試名稱長度，確保主標題完美在「單行」內呈現。*
* **個人資訊欄**: 統一為 **`10` 號字 標楷體 (Bold)**。
* **大題標題**: 統一為 **`12.5` 號字 書法中楷（注音一） (Bold)**。
* **試題內文 & 選項**: 統一為 **`12` 號字 書法中楷（注音一） (Regular)**。
* **閱讀測驗短文**: 統一為 **`12` 號字 書法中楷（注音一） (Regular)**。

### 2. 行距規範 (Line Spacing)
* **固定值行距**: 所有題目段落、選項段落、閱讀測驗與標題的行距，一律設定為**固定值 23 點** (`Pt(23)` / `WD_LINE_SPACING.EXACTLY`)。這在 12 號字及書法中楷注音字型下能提供最整齊的行與行間距。

### 3. 是非題與選擇題對齊（首行凸排 Hanging Indent）
* **格式**: 題首前置填答括號與題號，如 **`（  ）1. `**（全形括號，內置兩個半形空白，後接題號、句點與半形空白）。
* **凸排對齊規則**:
  * 第一行的「`（  ）1. `」起點對齊最左側邊界（`first_line_indent` 設為負值）。
  * 第二行起及後續換行之字首，必須**垂直對齊題目文字的第一個字**（例如「毛」），而非對齊括號。
  * 左縮排 (`left_indent`) 與負首行縮排 (`first_line_indent`) 統一設定為 **`45` 點 (`Pt(45)`)**。
* **選項對齊規則**:
  * 選擇題的 4 個選項採雙欄定位（一行 2 個選項），**左縮排亦設定為 45 點 (`Pt(45)`)**。
  * 這樣能使選項的 `(1)` 與 `(3)` 與上方題目的第一個文字完美垂直切齊。

### 4. 4 頁題目紙嚴格壓縮 (不含解答)
在 12 號字與 23 點固定行距下，為確保 15 題是非、15 題選擇與 2 篇閱讀測驗（共 6 題）能塞在 4 頁內，需實施：
* **欄位合併**: 素養問答或實驗題中，問題與答案底線應合併於同一行（例如：`(1) 問題？ 答：___________`），避免單獨換行產生空行。
* **插圖尺寸**: 所有插圖寬度最大限制為 **`1.5` 英吋** (高約 `1.125` 英吋)，並在雙欄中置中。
* **段落間距緊縮**: 是非題段後 `0pt`，選擇題段後 `0pt`，選項段後 `1.0pt` (作為題間距)。大題標題段前 `4pt`，避免使用空白段落。

---

## Python-docx 實現程式碼範本

以下為 python-docx 的精確排版實現 helper 函數，出題時應套用此段碼以確保 Word 排版無誤：

```python
import docx
from docx.shared import Pt, Inches
from docx.enum.text import WD_LINE_SPACING, WD_ALIGN_PARAGRAPH
from docx.oxml import OxmlElement
from docx.oxml.ns import qn

def add_styled_run(paragraph, text, font_name='書法中楷（注音一）', font_size=12, bold=False, italic=False, color=None):
    run = paragraph.add_run(text)
    run.bold = bold
    run.italic = italic
    if font_size:
        run.font.size = Pt(font_size)
    run.font.name = font_name
    
    # 設置亞洲文字字型，確保注音字型正確套用
    rPr = run._r.get_or_add_rPr()
    rFonts = OxmlElement('w:rFonts')
    rFonts.set(qn('w:ascii'), font_name)
    rFonts.set(qn('w:hAnsi'), font_name)
    rFonts.set(qn('w:eastAsia'), font_name)
    rPr.append(rFonts)
    
    if color:
        run.font.color.rgb = color
    return run

def add_aligned_paragraph(doc, text="", font_size=12, bold=False, align=None, space_before=0, space_after=0):
    p = doc.add_paragraph()
    if align is not None:
        p.alignment = align
    p.paragraph_format.space_before = Pt(space_before)
    p.paragraph_format.space_after = Pt(space_after)
    p.paragraph_format.line_spacing_rule = WD_LINE_SPACING.EXACTLY
    p.paragraph_format.line_spacing = Pt(23)
    p.paragraph_format.left_indent = Inches(0)
    p.paragraph_format.first_line_indent = Inches(0)
    
    if text:
        add_styled_run(p, text, font_size=font_size, bold=bold)
    return p

def add_indented_paragraph(doc, prefix, text, font_size=12, space_before=0, space_after=0.5):
    """
    建立首行凸排（Hanging Indent）的題目段落。
    設定左縮排為 45pt，首行負縮排 -45pt。
    這使「（  ）1. 」的起點在 0，而後續行數的字首均對齊 45pt 處（即題目文字的第一個字）。
    """
    p = doc.add_paragraph()
    p.paragraph_format.space_before = Pt(space_before)
    p.paragraph_format.space_after = Pt(space_after)
    p.paragraph_format.line_spacing_rule = WD_LINE_SPACING.EXACTLY
    p.paragraph_format.line_spacing = Pt(23)
    
    p.paragraph_format.left_indent = Pt(45)
    p.paragraph_format.first_line_indent = Pt(-45)
    
    full_text = f"{prefix}{text}"
    add_styled_run(p, full_text, font_size=font_size)
    return p

def add_options_paragraph(doc, opt1, opt2, opt3, opt4, font_size=12, space_before=0, space_after=1.0):
    """
    建立選擇題選項段落，左縮排設定為 45pt（無首行負縮排），
    使其與題目第一個字垂直對齊，並配置雙欄選項定位點。
    """
    p = doc.add_paragraph()
    p.paragraph_format.space_before = Pt(space_before)
    p.paragraph_format.space_after = Pt(space_after)
    p.paragraph_format.line_spacing_rule = WD_LINE_SPACING.EXACTLY
    p.paragraph_format.line_spacing = Pt(23)
    
    p.paragraph_format.left_indent = Pt(45)
    p.paragraph_format.first_line_indent = Inches(0)
    
    # 由於段落已有 45pt 的左縮排，定位點需加上 45pt（約 0.625 英吋）
    # 故將原本的 1.85 英吋定位點調整為 1.225 英吋
    p.paragraph_format.tab_stops.add_tab_stop(Inches(1.225))
    
    add_styled_run(p, f"(1) {opt1}\t(2) {opt2}\n", font_size=font_size)
    add_styled_run(p, f"(3) {opt3}\t(4) {opt4}", font_size=font_size)
    return p
```

---

## 雙向細目表輸出規範

產出考卷時，必須一併產出雙向細目表，並依循以下標準：
1. **獨立文件**: 生成為獨立 Word 文件 `[單元名]_雙向細目表.docx`。
2. **網格表格**: 使用 `Table Grid` 樣式建立 $5 \times 6$ 或與單元概念數相符的表格。
3. **字型設定**: 表頭及首列為 `標楷體` (10pt Bold)，表格內容為 `書法中楷（注音一）` (9.5pt Regular)，行距設定為單倍或較緊湊的 `1.15`，以利閱讀。
4. **內容標註**: 表格必須精準標出每個概念在記憶/理解、應用、分析/探究等認知層級的題號分佈、分數與百分比，且總分須與練習卷完全相符。
