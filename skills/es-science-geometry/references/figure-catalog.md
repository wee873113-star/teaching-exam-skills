# 科學圖表幾何參數目錄（figure-catalog.md）

本文件列出所有支援的圖形類型及其完整參數說明，供自然科學命題繪圖或需要精確控制科學實驗示意圖時查閱。

---

## 通用結構

每個 figure spec 的頂層結構：

```json
{
  "id": "fig1",
  "type": "<圖形類型>",
  "config": { ... },
  "canvas": { "width": 280, "height": 220, "bg": "white" }
}
```

批次規格（提供給 `geometry_renderer.py`）：

```json
{
  "figures": [ {...}, {...} ],
  "options": { "format": "png", "dpi": 150 }
}
```

---

## 1. coordinate_plane（坐標平面 - 用於繪製科學數據折線圖與曲線）

```json
{
  "type": "coordinate_plane",
  "config": {
    "x_range": [0, 10],
    "y_range": [10, 50],
    "show_grid": true,
    "tick_interval": 2,
    "x_label": "時間 (分)",
    "y_label": "溫度 (°C)",
    "points": [
      {"x": 0, "y": 20, "label": "20°C", "color": "#cc0000"},
      {"x": 2, "y": 26},
      {"x": 4, "y": 32},
      {"x": 6, "y": 38},
      {"x": 8, "y": 44},
      {"x": 10, "y": 50, "label": "50°C"}
    ],
    "segments": [
      {"x1": 0, "y1": 20, "x2": 10, "y2": 50, "color": "#ff3333"}
    ]
  }
}
```

| 參數 | 說明 |
|------|------|
| x_range | $[x_{\min}, x_{\max}]$ 範圍，如 `[0, 10]` |
| y_range | $[y_{\min}, y_{\max}]$ 範圍，如 `[0, 100]` |
| show_grid | 是否顯示格線（建議科學圖表設為 `true`，以利學生讀取數據值） |
| x_label / y_label | 橫軸/縱軸物理量與單位名稱（如「溶解度 (g/100g水)」、「拉力 (N)」） |
| points | 各個測量點數據標籤與點坐標 |
| segments | 連接各數據點的線段陣列，呈現折線圖趨勢 |

**常用範例**（加熱時間與溫度變化）：
```json
{"type":"coordinate_plane","config":{"x_range":[0,8],"y_range":[20,80],"x_label":"加熱時間(分)","y_label":"水溫(°C)","show_grid":true,"points":[{"x":0,"y":25,"label":"25"},{"x":2,"y":38},{"x":4,"y":51},{"x":6,"y":64},{"x":8,"y":77,"label":"77"}],"segments":[{"x1":0,"y1":25,"x2":8,"y2":77,"color":"#ff3333"}]}}
```

---

## 2. triangle（三角形 - 用於斜面重物實驗、光反射/折射角）

```json
{
  "type": "triangle",
  "config": {
    "subtype": "right",
    "vertex_labels": ["A", "B", "C"],
    "right_angle_at": "C",
    "angle_arcs": {"A": 1},
    "side_labels": {"AB": "斜面長 100 cm", "BC": "底面", "CA": "高度 50 cm"},
    "show_dots": false
  }
}
```

| 參數 | 說明 | 可選值 |
|------|------|--------|
| subtype | 三角形種類 | `general`（預設）、`right`（適合斜面）、`isosceles`、`equilateral` |
| right_angle_at | 畫直角符號的頂點 | 在斜面中通常為 C (直角頂點) |
| angle_arcs | 各頂點畫幾條角弧 | 用於示意斜面傾角或入射角 |
| side_labels | 各邊的文字標籤 | 示意斜面長度或高度 |

**常用範例**（斜面力學）：
```json
{"type":"triangle","config":{"subtype":"right","vertex_labels":["A","B","C"],"right_angle_at":"C","side_labels":{"AB":"斜面","CA":"高度 30cm","BC":"40cm"}}}
```

---

## 3. quadrilateral（四邊形 - 用於實驗木塊、電池、燒杯截面）

```json
{
  "type": "quadrilateral",
  "config": {
    "subtype": "rectangle",
    "side_labels": {"AD": "長 10 cm", "AB": "寬 5 cm"}
  }
}
```

**常用範例**（待測木塊）：
```json
{"type":"quadrilateral","config":{"subtype":"rectangle","side_labels":{"AD":"木塊 A","AB":"5 cm"}}}
```

---

## 4. circle（圓 - 用於滑輪組、天平秤盤、日地月軌道、月相）

```json
{
  "type": "circle",
  "config": {
    "center_label": "軸心",
    "show_center": true,
    "radius_lines": ["A"],
    "radius_label": "半徑 10 cm",
    "points": {
      "A": 0
    }
  }
}
```

**常用範例**（定滑輪示意）：
```json
{"type":"circle","config":{"center_label":"O","show_center":true,"points":{"A":270},"radius_lines":["A"],"radius_label":"懸掛重物"}}
```

---

## 5. solid_3d（立體圖形 - 用於量筒、試管、排水法待測塊體積）

```json
{
  "type": "solid_3d",
  "config": {
    "subtype": "cylinder",
    "show_hidden": true,
    "labels": {
      "radius": "半徑 3cm",
      "height": "水面高 15cm"
    }
  }
}
```

**常用範例**（量筒液面高度）：
```json
{"type":"solid_3d","config":{"subtype":"cylinder","labels":{"height":"液面：100 mL"}}}
```

---

## 快速複製區（常用科學題目圖示規格）

### 斜面實驗（重物沿斜面拉動）
```json
{"id":"inclined_plane","type":"triangle","config":{"subtype":"right","vertex_labels":["A","B","C"],"right_angle_at":"C","side_labels":{"AB":"斜面 (100cm)","CA":"高度 (30cm)"}}}
```

### 氣溫隨時間變化圖表 (坐標圖)
```json
{"id":"temp_graph","type":"coordinate_plane","config":{"x_range":[8,16],"y_range":[20,35],"x_label":"時間(時)","y_label":"氣溫(°C)","show_grid":true,"points":[{"x":8,"y":22,"label":"8點"},{"x":10,"y":26},{"x":12,"y":31,"label":"12點"},{"x":14,"y":33,"label":"最高氣溫"},{"x":16,"y":29}],"segments":[{"x1":8,"y1":22,"x2":10,"y2":26},{"x1":10,"y1":26,"x2":12,"y2":31},{"x1":12,"y1":31,"x2":14,"y2":33},{"x1":14,"y1":33,"x2":16,"y2":29}]}}
```

### 量筒液面與排水法 (立體柱體)
```json
{"id":"displacement_cylinder","type":"solid_3d","config":{"subtype":"cylinder","labels":{"radius":"r = 2 cm","height":"水深 h = 8 cm"}}}
```

### 天平與重物 (圓形與線段組合)
```json
{"id":"balance_scale","type":"circle","config":{"center_label":"支點","show_center":true,"radius_lines":["A","B"],"radius_label":"秤盤","points":{"A":180,"B":0}}}
```
