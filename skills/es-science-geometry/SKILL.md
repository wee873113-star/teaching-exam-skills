---
name: es-science-geometry
description: >
  國小科學幾何與圖表 SVG 產生器。當任何教學或考試情境需要生成或繪製國小科學實驗示意圖、力學圖、坐標圖表或幾何圖案時，請一定要使用此技能。
  【獨立使用觸發情境】「幫我畫斜面物體受力圖」、「畫一個標出刻度的燒杯/量筒」、「畫圓與天平示意圖」、「畫氣溫與時間坐標折線圖」、「畫凸透鏡成像光路」、「幫我產生國小自然科圖表」等。
  【被其他技能呼叫觸發情境】出題技能（es-science-exam、es-science-context-questions）需要科學圖表或實驗步驟配圖時。
  支援圖形類型（含刻度標示、受力箭頭、坐標標記）：
  三角形（斜面、光線反射/折射）、四邊形（實驗木塊、電池、天平底座）、
  圓與弧（滑輪、月相變化、天平秤盤）、立體圖形（量筒、試管、待測塊）、
  坐標平面（氣溫折線圖、溶解度曲線圖、v-t 圖）。
  圖形可匯出至 Word（.docx）或 PowerPoint（.pptx）。
---

# 國小科學圖表幾何繪製技能 (es-science-geometry)

## 技能概覽

本技能生成適合國小試卷、學習單、教材的科學示意 SVG 圖形與實驗圖表，並輸出為 PNG 圖片檔，可直接插入 Word 文件或 PowerPoint 投影片。

**核心腳本位置**：
```bash
GEOM_DIR=""
for d in /mnt/skills/user/es-science-geometry/scripts \
          /tmp/es-science-geometry/scripts \
          C:/antigravity/0601math/skills/es-science-geometry/scripts; do
  [ -d "$d" ] && GEOM_DIR="$d" && break
done
echo "科學繪圖腳本目錄：$GEOM_DIR"
```

---

## 處理流程

### Step 1：理解需求

根據學生的年級與題目描述，判定圖形的標籤與標記複雜度。對於小學生：
- **實驗裝置/受力圖**：清晰標出力的大小的箭頭、角度，或是容器刻度（如「50毫升」），字體需清晰且偏大。
- **坐標圖表**：X 軸與 Y 軸需有明確的物理量與單位（如：時間 (分)、溫度 (°C)），折線上的數據點需有明顯的圓點標記。
- **天平/滑輪**：標示支點、力臂與重物。

### Step 2：建立圖形規格 JSON

依需求建立 spec，儲存至 `/home/claude/geometry_spec.json`：

```json
{
  "figures": [
    {
      "id": "fig1",
      "type": "coordinate_plane",
      "config": {
        "x_range": [0, 10],
        "y_range": [10, 40],
        "x_label": "時間 (分)",
        "y_label": "水溫 (°C)",
        "show_grid": true,
        "points": [
          {"x": 0, "y": 15, "label": "15°C"},
          {"x": 2, "y": 20},
          {"x": 4, "y": 25},
          {"x": 6, "y": 30},
          {"x": 8, "y": 35},
          {"x": 10, "y": 40, "label": "40°C"}
        ],
        "segments": [
          {"x1": 0, "y1": 15, "x2": 10, "y2": 40, "color": "#ff3333"}
        ]
      },
      "canvas": {"width": 300, "height": 220}
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
python C:/antigravity/0601math/skills/es-science-geometry/scripts/geometry_renderer.py `/
    /home/claude/geometry_spec.json `/
    /home/claude/geometry_output/
```

---

## 國小科學常用圖形規格目錄

### 1. coordinate_plane (坐標平面 - 用於氣溫、溶解度與速度圖表)
*   **加熱時間與水溫折線圖**：
    ```json
    {"type":"coordinate_plane","config":{"x_range":[0,8],"y_range":[20,80],"x_label":"時間(分)","y_label":"溫度(°C)","show_grid":true}}
    ```

### 2. triangle (三角形 - 用於斜面與力學、光反射)
*   **斜面受力分析**（直角在 C，標示斜角與長度）：
    ```json
    {"type":"triangle","config":{"subtype":"right","vertex_labels":["A","B","C"],"right_angle_at":"C","side_labels":{"AB":"斜面長 10m"}}}
    ```

### 3. circle (圓 - 用於滑輪、月相與天平)
*   **滑輪示意**：
    ```json
    {"type":"circle","config":{"center_label":"軸心","points":{"A":270},"radius_lines":["A"],"radius_label":"半徑"}}
    ```
*   **日地月公轉軌道 (簡化版)**：
    ```json
    {"type":"circle","config":{"center_label":"太陽","show_center":true}}
    ```

### 4. solid_3d (立體圖形 - 用於量筒、待測積木、試管)
*   **測量排開水的體積 (圓柱體/長方體)**：
    ```json
    {"type":"solid_3d","config":{"subtype":"cylinder","labels":{"radius":"r","height":"h (水面高度)"}}}
    ```
