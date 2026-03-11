# CookScript 言語仕様 v1.0

> **"あなたがランタイムです。"**

CookScript は料理手順を記述するための擬似プログラミング言語です。  
インタプリタはコンピュータではなく、**あなた自身**です。

---

## 基本概念

| 概念 | 意味 |
|------|------|
| **変数** | 材料・道具 |
| **関数** | 手作業（切る・炒めるなど） |
| **while ループ** | 「〜になるまで繰り返す」 |
| **for ループ** | 「〜を順番に処理する」 |
| **if 文** | 感覚による分岐 |
| **assert** | 「この条件を満たしていなければ先に進むな」 |
| **panic!** | 致命的なエラー。即座に対処せよ |

---

## 型システム

### 感覚型（Sensory Types）
CookScript の最大の特徴。目・耳・鼻・触覚で評価する型です。

```
// Color 型
RAW | PALE | GOLDEN | AMBER | BROWN | CHARRED

// Temp 型
COLD | WARM | HOT | SMOKING_HOT

// Sound 型
SILENT | SIZZLING | POPPING | CRACKLING

// Texture 型
STICKY | LOOSE | SEPARATED | CRISPY

// Smell 型
RAW_EGG | FRAGRANT | NUTTY | BURNT
```

### 単位付き数値型
```
400g     // 重量
大さじ2  // 体積（大さじ）
小さじ1  // 体積（小さじ）
2個      // 個数
30sec    // 時間
```

---

## 構文

### import（外部材料ファイルの読み込み）
材料定義を JSON ファイルから読み込みます。JSON 内の `id` がそのまま変数名になります。
```
import { <id>, <id> as <alias>, ... } from "<file.json>"

// devDependencies（道具）を読み込む場合
import { <id>, ... } from "<file.json>#devDependencies"
```

例：
```
import {
    rice,
    egg         as eggs,    // JSON の id "egg" を "eggs" として使う
    long_onion  as onion,
    sesame_oil  as sesame
} from "./ingredients.json"

import { wok, spatula } from "./ingredients.json#devDependencies"
```

### 変数宣言（インライン）
外部ファイルを使わず直接定義する場合：
```
let <name>: <量> = <材料>   // 食材
let <name>: Tool = <道具>  // 道具
```

### 関数呼び出し
```
<動詞>(<対象>, <引数...>)
```

### while ループ（感覚条件）
```
while <感覚式> {
    <手順>
}

// 例：色が変わるまで炒める
while color(egg) != HALF_COOKED {
    stir(wok)
}
```

### until ループ（達成条件、while の糖衣構文）
```
until <達成条件> {
    <手順>
}

// 例：パラパラになるまで
until texture(rice) == SEPARATED {
    stir(wok)
    press_lumps(wok)
}
```

### for ループ
```
for <item> in <list> {
    <手順>
}

for _ in 0..60sec {   // 時間ループ
    <手順>
}
```

### if 文
```
if <感覚式> {
    <手順>
} else {
    <手順>
}
```

### assert（安全チェック）
```
assert <条件>, "<失敗メッセージ>"
```

### panic!（致命的エラー）
```
panic!("<対処法>")
```

### コメント
```
// 一行コメント
/* 複数行
   コメント */
```

---

## 組み込み関数

| 関数 | 意味 | 戻り値型 |
|------|------|---------|
| `color(target)` | 色を目で確認 | Color |
| `temp(target)` | 温度を確認 | Temp |
| `sound(target)` | 音を耳で確認 | Sound |
| `smell(target)` | 香りを鼻で確認 | Smell |
| `texture(target)` | 触感を確認 | Texture |
| `taste(target)` | 味見 | "OK" or "要調整" |
| `smoke(target)` | 煙が出ているか | bool |
| `heat(target, until)` | 加熱する | void |
| `add(ingredient)` | 投入する | void |
| `stir(target)` | 混ぜる・炒める | void |
| `press_lumps(target)` | 塊を潰す | void |
| `season(s, from: "edge")` | 鍋肌から調味 | void |
| `drizzle(s)` | 回しかける | void |
| `plate()` | 皿に盛り付ける | void |
| `wait(duration)` | 待機 | void |

---

## エラーコード

```
E001: TEMP_TOO_LOW     — 鍋が冷えている
E002: RICE_TOO_WET     — 水分過多（炊きたてご飯）
E003: SEQUENCE_ERROR   — 手順の順番が間違っている
E004: OVERCOOKED       — 加熱しすぎ
E005: UNDERSEASONED    — 味が薄い（taste() で検出）
```
