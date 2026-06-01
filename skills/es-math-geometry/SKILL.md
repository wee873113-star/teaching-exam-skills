---
name: es-math-geometry
description: >
  國小數學幾何圖形 SVG 產生器。當任何教學或考試情境需要生成或繪製國小數學幾何圖形時，請一定要使用此技能。
  【獨立使用觸發情境】「幫我畫直角三角形」、「畫一個標出長寬的長方形」、「畫圓與扇形角度示意圖」、
  「畫長方體/正方體立體圖」、「畫等腰梯形」、「幫我產生國小幾何圖形」等。
  【被其他技能呼叫觸發情境】出題技能（es-math-exam、es-math-context-questions）需要幾何題目配圖時。
  支援圖形類型（含底高標示、邊長與頂點標記）：
  三角形（直角/等腰/等邊）、四邊形（矩形/正方形/平行四邊形/梯形）、
  圓與扇形（標示半徑、圓心角）、立體圖形（長方體、正方體、圓柱體、圓錐體）。
  圖形可匯出至 Word（.docx）或 PowerPoint（.pptx）。
---

# 國小幾何圖形繪製技能 (es-math-geometry)

## 技能概覽

本技能生成適合國小試卷、學習單、教材的幾何 SVG 圖形，並輸出為 PNG 圖片檔，可直接插入 Word 文件或 PowerPoint 投影片。

**核心腳本位置**：
```bash
GEOM_DIR=""
for d in /mnt/skills/user/es-math-geometry/scripts \
          /tmp/es-math-geometry/scripts \
          C:/antigravity/0601math/skills/es-math-geometry/scripts; do
  [ -d "$d" ] && GEOM_DIR="$d" && break
done
echo "幾何腳本目錄：$GEOM_DIR"
```

---

## 處理流程

### Step 1：理解需求

根據學生的年級與題目描述，判定圖形的標籤與標記複雜度。對於小學生：
- **平面圖形**：清晰標出邊長數值（如「5公分」）或代號（如「?」），字體需清晰且偏大。
- **立體圖形**：使用虛線表示看不見的邊（長方體/正方體），並標示「長、寬、高」。
- **扇形**：標示圓心角角度（如 `90°`）與半徑。

### Step 2：建立圖形規格 JSON

依需求建立 spec，儲存至 `/home/claude/geometry_spec.json`：

```json
{
  "figures": [
    {
      "id": "fig1",
      "type": "quadrilateral",
      "config": {
        "subtype": "rectangle",
        "vertex_labels": ["A", "B", "C", "D"],
        "side_labels": {"AD": "8 公分", "AB": "5 公分"}
      },
      "canvas": {"width": 280, "height": 220}
    }
  ],
  "options": {"format": "png", "dpi": 150}
}
```

### Step 3：執行渲染

在您的 Windows 環境中執行以下腳本：

```powershell
pip install cairosvg python-docx python-pptx --break-system-packages -q

# 執行渲染
python C:/antigravity/0601math/skills/es-math-geometry/scripts/geometry_renderer.py `/
    /home/claude/geometry_spec.json `/
    /home/claude/geometry_output/
```

---

## 國小常用圖形規格目錄

### 1. triangle (三角形)
*   **直角三角形**（標直角在 C，標示底與高）：
    ```json
    {"type":"triangle","config":{"subtype":"right","vertex_labels":["A","B","C"],"right_angle_at":"C","side_labels":{"BC":"4 cm","CA":"3 cm"}}}
    ```
*   **等腰三角形**（標記等邊刻度）：
    ```json
    {"type":"triangle","config":{"subtype":"isosceles","equal_marks":{"AB":1,"AC":1}}}
    ```

### 2. quadrilateral (四邊形)
*   **長方形**：
    ```json
    {"type":"quadrilateral","config":{"subtype":"rectangle","side_labels":{"AD":"8","AB":"5"}}}
    ```
*   **平行四邊形**（畫出高與直角標記）：
    ```json
    {"type":"quadrilateral","config":{"subtype":"parallelogram","side_labels":{"AB":"6","BC":"4"}}}
    ```
*   **梯形**（標示上底、下底、高）：
    ```json
    {"type":"quadrilateral","config":{"subtype":"trapezoid","side_labels":{"AD":"上底","BC":"下底"}}}
    ```

### 3. circle (圓與扇形)
*   **圓心與半徑標示**：
    ```json
    {"type":"circle","config":{"center_label":"O","points":{"A":45},"radius_lines":["A"],"radius_label":"5公分"}}
    ```
*   **扇形與角度標示 (例：120度)**：
    ```json
    {"type":"circle","config":{"center_label":"O","points":{"A":0,"B":120},"radius_lines":["A","B"],"central_angle":["A","B"]}}
    ```

### 4. solid_3d (立體圖形)
*   **長方體/正方體**（含看不見的虛線稜）：
    ```json
    {"type":"solid_3d","config":{"subtype":"rectangular_prism","vertex_labels":["A","B","C","D","E","F","G","H"],"show_hidden":true}}
    ```
*   **圓柱體**（高年級常用於計算表面積或體積）：
    ```json
    {"type":"solid_3d","config":{"subtype":"cylinder","labels":{"radius":"r","height":"h"}}}
    ```
