# 🍳 open-source-chahan

> **オープンソースチャーハン** — オープンソースレシピ
> License: MIT | Version: 1.0.0

## Overview

家庭用コンロでも再現できる、シンプルで再現性の高いチャーハンレシピです。
材料仕様は `ingredients.json`、製造手順は `chahan.cook` で管理されています。

```
open-source-chahan/
├── README.md          # このファイル
├── SPEC.md            # CookScript 言語仕様
├── ingredients.json   # 材料定義（依存関係）
└── chahan.cook        # 製造パイプライン
```

---

## Recommended Environment

| 項目 | 推奨 | 最小要件 |
|------|------|---------|
| **コンロ** | ガスコンロ（強火可能なもの） | IHコンロ（最大火力）|
| **鍋** | 鉄製中華鍋（直径30cm） | テフロンフライパン（26cm以上）|
| **ご飯** | 前日炊飯・冷蔵保存 | 炊きたて（水分多めにつき難易度上昇）|
| **卵** | 室温保存品 | 冷蔵直後でも可 |
| **CookScript** | v1.0 | v1.0 |

> **Note:**  
> テフロンフライパンの場合、鍋の予熱は `warm` 止まりを推奨。  
> `smoking_hot` まで上げると表面コーティングが損傷します。  
> `Wok` クラスの `heat()` 呼び出し回数を 2 回に抑えてください。

---

## Quick Start

```bash
# リポジトリをクローン
git clone https://github.com/yourname/open-source-chahan.git
cd open-source-chahan

# 材料を確認する
cat ingredients.json

# 製造パイプラインを確認（ドライラン）
cat chahan.cook
```

実際の調理は `chahan.cook` の手順に従い、**キッチンで手動実行**してください。

---

## Key Concepts

### 卵ファーストアーキテクチャ

```
[溶き卵] → (半熟) → [ご飯投入] → コーティング完了 → パラパラ食感
```

先に卵を入れてご飯をコーティングすることで、粒同士のくっつきを防ぎます。

### 醤油の鍋肌投入

```python
wok.season("醤油（鍋肌から）")  # ✅ 香ばしさが生まれる
wok.add("醤油（直接かける）")   # ❌ 蒸気で醤油が飛ぶだけ
```

### ごま油は最後に

ごま油は揮発性の香り成分が主役です。加熱しすぎると香りが失われるため、  
火を止める直前に回しかけてください。

---

## Error Handling

| エラー | 原因 | 対処 |
|--------|------|------|
| `AssertionError: 鍋が十分に熱くありません` | 予熱不足 | 強火でさらに加熱 |
| `RuntimeError: 油が引かれていません` | Step 順序の誤り | Step 1 から再実行 |
| ご飯がべちゃつく | 炊きたてご飯 or 水分過多 | 強火で水分を飛ばしながら炒め続ける |
| 焦げる | 火力過多 or 撹拌不足 | 中火に落とし、ひたすら混ぜる |

---

## Contributing

材料の追加・代替案・バグ（焦げ・くっつき）の修正は  
Issue または Pull Request でお気軽にどうぞ。

レシピ改変後は `version` のパッチ番号をインクリメントしてください（例: `1.0.0` → `1.0.1`）。

---

## License

[MIT](https://opensource.org/licenses/MIT) — 商用利用・改変・再配布自由です。  
おいしく作れたら Star ⭐ をつけてくれると嬉しいです。
