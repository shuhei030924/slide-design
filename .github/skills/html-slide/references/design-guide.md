# デザインガイド(スライドの見た目の品質基準)

スライドの視覚品質を安定させるための**定型レシピ集**。
**配色・文字サイズ・余白は、このガイドの値をそのままコピーして使うこと。自分で色コードを発明しない。**

## スライドの解剖図(全パターン共通の構造)

品質の高いスライドは、パターンによらず次の4層構造を持つ。

```
┌─────────────────────────────────────┐
│ ① トップバー: キッカー(左) ロゴ(右)          │ ← 高さ約40px
│ ② タイトル: メッセージ文(1行、一部を強調色)      │ ← 36〜44px
│                                     │
│ ③ ボディ: カード/図解/グラフ(flex: 1 で伸ばす)   │
│                                     │
│ ④ フッター: 社名(左) 日付(右) + 上に1pxの罫線    │ ← 12〜13px
└─────────────────────────────────────┘
```

### ① キッカー(セクションラベル)— 必ず入れる

タイトルの上に置く小さな英語ラベル。これがあるだけで「デザインされた資料」に見える。

```css
.kicker {
  font-family: 'Inter', sans-serif;
  font-size: 13px; font-weight: 600;
  letter-spacing: 0.35em; text-transform: uppercase;
  color: var(--accent);
}
```
```html
<span class="kicker">Sales Growth</span>  <!-- 内容を表す英語1〜3語 -->
```

### ② タイトルは「メッセージ文」で書く — 最重要ルール

- **NG**: 「売上推移」「サービス概要」(ただの名詞ラベル)
- **OK**: 「売上高は6年連続成長、<em>128億円</em>へ。」(結論を言い切る文)
- 数値・キーワードなど**1箇所だけ** `<em>` でアクセント色にする。2箇所以上は散漫になる

```css
.slide-title { font-size: 40px; font-weight: 700; color: var(--ink); line-height: 1.4; }
.slide-title em { font-style: normal; color: var(--accent); }
/* ダーク背景ではグラデーション文字にするとより映える */
.slide-title em.grad {
  background: var(--grad);
  -webkit-background-clip: text; background-clip: text; color: transparent;
}
```

### ④ ヘアラインフッター — 必ず入れる

```css
.footer {
  font-size: 12px; color: var(--sub); letter-spacing: 0.15em;
  padding-top: 20px; border-top: 1px solid var(--border);
  display: flex; justify-content: space-between;
}
```
```html
<div class="footer"><span>YOUR COMPANY INC.</span><span>2026.07</span></div>
```

## タイポグラフィスケール(この値以外を使わない)

| 用途 | サイズ | weight | 備考 |
|---|---|---|---|
| キッカー | 13px | 600 | Inter、letter-spacing 0.35em、大文字 |
| タイトル | 36〜44px | 700〜900 | line-height 1.3〜1.4 |
| カード見出し | 18〜20px | 700 | |
| 本文 | 15〜18px | 400〜500 | line-height 1.7〜1.8 |
| 注釈・キャプション | 13〜14px | 400 | color: var(--sub) |
| KPI数値 | 56〜72px | 800 | Inter、letter-spacing -0.02em |
| KPI単位 | 数値の40%程度 | 600 | color: var(--sub)、数値の右に小さく |

- 数字・英語は `'Inter', sans-serif`、日本語は `'Noto Sans JP', sans-serif`
- **日本語本文に letter-spacing を付けない**(キッカー等の英語のみ可)
- Google Fonts の読み込みは Noto Sans JP + Inter の2つで足りる(和モダンのみ Noto Serif JP)

## 余白・レイアウト(8pxグリッド)

- スライド内側パディング: `60px 80px`(上下60・左右80)。**48px未満にしない**
- タイトルとボディの間: 36〜48px
- カード間のgap: 24〜36px
- カード内パディング: 28〜40px
- 情報が収まらない場合は**余白を削らず内容を削る**(箇条書き5→3個、文を短く)

## カラーテーマ(6種 — この :root をそのままコピーする)

トーンに合うテーマを1つ選び、`:root` ブロックを丸ごとコピーして使う。
**1スライド内で使う色はこのトークンだけ。それ以外の色コードを書かない。**

### A. コーポレートブルー(ライト) — 提案書・報告書のデフォルト

```css
:root {
  --bg: #ffffff;        /* スライド背景 */
  --surface: #f8fafc;   /* カード背景 */
  --surface-2: #eff6ff; /* 強調カード背景(青みがかった面) */
  --border: #e2e8f0;    /* 罫線・カード枠 */
  --ink: #0f172a;       /* 見出し文字 */
  --text: #334155;      /* 本文文字 */
  --sub: #64748b;       /* 補足文字 */
  --accent: #2563eb;    /* アクセント */
  --accent-2: #0ea5e9;  /* 第2アクセント(グラデ用) */
  --grad: linear-gradient(120deg, #2563eb, #0ea5e9);
}
```
装飾: 薄い円(`background: var(--accent); opacity: 0.05;` を絶対配置)

### B. テックダーク — 技術説明・モダンな発表・表紙

```css
:root {
  --bg: #0c1222;
  --surface: rgba(255,255,255,0.04);
  --surface-2: rgba(56,189,248,0.08);
  --border: rgba(255,255,255,0.09);
  --ink: #f1f5f9;
  --text: #cbd5e1;
  --sub: #94a3b8;
  --accent: #38bdf8;
  --accent-2: #818cf8;
  --grad: linear-gradient(120deg, #38bdf8, #818cf8);
}
```
装飾: グロー(下のレシピ)。カードは `background: var(--surface); border: 1px solid var(--border); border-radius: 20px;`

### C. ミッドナイトグリーン(ダーク) — 実績報告・決算ハイライト

```css
:root {
  --bg: #0c1222;
  --surface: rgba(255,255,255,0.04);
  --surface-2: rgba(52,211,153,0.10);
  --border: rgba(255,255,255,0.09);
  --ink: #f1f5f9;
  --text: #cbd5e1;
  --sub: #94a3b8;
  --accent: #34d399;
  --accent-2: #38bdf8;
  --grad: linear-gradient(120deg, #34d399, #38bdf8);
}
```

### D. ウォームカジュアル(ライト) — toCサービス・社内告知

```css
:root {
  --bg: #fffaf3;
  --surface: #ffffff;
  --surface-2: #ffedd5;
  --border: #f3e3cf;
  --ink: #431f0e;
  --text: #6b4a35;
  --sub: #a1836f;
  --accent: #f97316;
  --accent-2: #fbbf24;
  --grad: linear-gradient(120deg, #f97316, #fbbf24);
}
```
装飾: 大きな淡色の円(`background: #fde8c8` 等をopacityなしで直接置いてよい)

### E. フォレストグリーン(ライト) — 環境・医療・安心感

```css
:root {
  --bg: #f8fbf8;
  --surface: #ffffff;
  --surface-2: #ecf7ee;
  --border: #dceae0;
  --ink: #14352a;
  --text: #3f5c50;
  --sub: #719183;
  --accent: #16a34a;
  --accent-2: #34d399;
  --grad: linear-gradient(120deg, #16a34a, #34d399);
}
```

### F. バイオレットSaaS(ライト) — プロダクト紹介・スタートアップ

```css
:root {
  --bg: #f8f7fd;
  --surface: #ffffff;
  --surface-2: #f1edfd;
  --border: #e5e0f5;
  --ink: #231a4f;
  --text: #46406b;
  --sub: #7d76a3;
  --accent: #7c3aed;
  --accent-2: #a78bfa;
  --grad: linear-gradient(120deg, #7c3aed, #a78bfa);
}
```

## 定型パーツレシピ

### カード(ライトテーマ)

```css
.card {
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: 16px;
  padding: 32px;
  /* 影は付けるならこの1種類だけ。濃い影は禁止 */
  box-shadow: 0 1px 3px rgba(15, 23, 42, 0.05);
}
```

### カード(ダークテーマ)

```css
.card {
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: 20px;
  padding: 36px;
  /* ダークでは box-shadow を付けない(borderだけで十分) */
}
```

### バッジ・ピル(前年比+32% など)

```css
.pill {
  display: inline-flex; align-items: center; gap: 6px;
  font-family: 'Inter', sans-serif;
  font-size: 14px; font-weight: 600;
  color: var(--accent);
  background: var(--surface-2);
  padding: 6px 14px; border-radius: 999px;
}
```

### アイコンチップ(カード見出しの左に置く小さな角丸アイコン)

```css
.icon-chip {
  width: 44px; height: 44px; border-radius: 12px;
  background: var(--grad);
  display: flex; align-items: center; justify-content: center;
  color: #fff;  /* 中に24×24の線画SVG(svg-guide参照) */
}
```

### 背景グロー(ダークテーマ専用)

```css
.glow { position: absolute; border-radius: 50%; filter: blur(100px); pointer-events: none; }
```
```html
<div class="glow" style="width:500px; height:500px; top:-220px; left:-120px; background:rgba(56,189,248,0.14);"></div>
<div class="glow" style="width:500px; height:500px; bottom:-240px; right:-140px; background:rgba(129,140,248,0.13);"></div>
```

### 背景装飾(ライトテーマ)

```css
.deco { position: absolute; border-radius: 50%; background: var(--accent); opacity: 0.05; pointer-events: none; }
```

### 会社ロゴプレースホルダー(右上)

```html
<div class="logo">
  <!-- ★ロゴ差し替え: <img src="../images/logo.png" alt="会社ロゴ" style="height:100%; object-fit:contain;"> -->
  <svg viewBox="0 0 40 40" fill="none" style="width:32px; margin-right:9px;">
    <path d="M20 4 L36 32 H4 Z" stroke="var(--accent)" stroke-width="2.5" stroke-linejoin="round"/>
    <circle cx="20" cy="24" r="5" fill="var(--accent)"/>
  </svg>
  <span class="logo-text">YOUR LOGO</span>
</div>
```
```css
.logo { height: 38px; max-width: 220px; display: flex; align-items: center; }
.logo-text { font-family: 'Inter', sans-serif; font-weight: 800; font-size: 15px; letter-spacing: 0.08em; color: var(--ink); }
```

### プログレスバー(進捗・達成率)

```css
.track { flex: 1; height: 10px; border-radius: 999px; background: var(--surface-2); overflow: hidden; }
.fill  { height: 100%; border-radius: 999px; background: var(--grad); }
/* ダークテーマのトラックは background: rgba(255,255,255,0.08) にする */
```
```html
<div class="track"><div class="fill" style="width: 72%;"></div></div>
```
- 横棒グラフ(アンケート結果など)も同じ構造で height を 26〜30px にすれば作れる(SVG不要)

### 構成比バンド(内訳・シェアの積み上げ横帯)

円グラフより崩れにくく読みやすい。**内訳を見せるときの第一選択。**

```html
<div style="display: flex; height: 80px; border-radius: 16px; overflow: hidden;">
  <div style="width: 62%; background: var(--accent);"><!-- %とラベルを中に --></div>
  <div style="width: 22%; background: var(--accent-2);"></div>
  <div style="width: 11%; background: var(--border);"></div>
  <div style="width: 5%;  background: var(--surface-2);"></div>
</div>
```
- width の合計を必ず100%にする。ラベルが入らない細いセグメントは凡例のみで示す
- 色はアクセント2色+ニュートラル2段階まで。3色以上のアクセントを使わない

### 特大アウトライン数字(章番号・ランキングの装飾)

```css
.big-num {
  position: absolute; right: 48px; top: 50%; transform: translateY(-50%);
  font-family: 'Inter', sans-serif;
  font-size: 380px; font-weight: 800; line-height: 1;
  color: transparent;
  -webkit-text-stroke: 2px var(--border);  /* 線だけの数字 */
  letter-spacing: -0.04em; pointer-events: none;
}
```
- 本文と重ならない位置に置く(本文側は max-width で制限)。ライトテーマでは線色を --border 程度の薄さに

### 縦タイムライン(時刻付きプログラム・手順)

```html
<div class="tl-row">
  <span class="tl-time">13:00</span>          <!-- 時刻チップ -->
  <div class="tl-node"><span class="dot"></span><span class="line"></span></div>
  <div class="tl-body">タイトルと説明</div>
</div>
```
```css
.tl-time { font-family: 'Inter', sans-serif; font-weight: 700; color: var(--accent);
           background: var(--surface-2); padding: 6px 12px; border-radius: 9px; width: 74px; text-align: center; }
.tl-node { width: 14px; position: relative; align-self: stretch; }
.tl-node .dot  { position: absolute; top: 10px; width: 12px; height: 12px; border-radius: 50%; background: var(--grad); }
.tl-node .line { position: absolute; top: 28px; bottom: -12px; left: 5px; width: 2px; background: var(--border); }
/* 最終行の .line は消す: .tl-row:last-child .tl-node .line { display: none; } */
```

### 評価記号(比較・意思決定スライド)

- `◎ ○ △` の3段階を使う(`×`は否定が強すぎるため課題列挙以外では避ける)
- 色: ◎ = var(--accent)、○ = var(--sub)、△ = #d97706(アンバー)。記号+短い言葉(「◎ 低い」)を併記する

### ブレークイーブンバー(投資回収・期限までの残り)

```html
<div style="position: relative; height: 18px; border-radius: 999px; background: var(--surface-2);">
  <div style="position: absolute; inset: 0; width: 38.9%; border-radius: 999px; background: var(--grad);"></div>
  <!-- 旗マーカー: left をfillと同じ%にして translateX(-50%) -->
  <div style="position: absolute; left: 38.9%; top: -34px; transform: translateX(-50%);">
    <span>14ヶ月で回収完了</span>
  </div>
</div>
```
- width% = 到達点 ÷ 全期間(例: 14ヶ月/36ヶ月 = 38.9%)。計算式をコメントに残す
- 軸ラベルは絶対配置+`translateX(-50%)`。**両端だけは `transform: none` / `translateX(-100%)`** にしないと端で折り返す(実例: slide-roi)

### 階段カード(30-60-90日プランなどの段階的成長)

```css
.stairs { display: flex; align-items: flex-end; }  /* 下端を揃える */
.phase.p1 { height: 76%; } .phase.p2 { height: 88%; } .phase.p3 { height: 100%; }
```
- 高さの差で「右肩上がり」を表現する。最終カードだけ `border: 2px solid var(--accent)` で強調(実例: slide-plan90)
- カード間は `›` の山括弧テキストで軽くつなぐ(太い矢印は重い)

### 担当・期限チップ(ToDo・アクションアイテム)

```html
<span class="who"><i>田</i>田中</span>  <span class="due">7/11</span>
```
```css
.who { display: inline-flex; align-items: center; gap: 6px; font-size: 12px; font-weight: 600;
       background: var(--surface-2); padding: 4px 10px 4px 4px; border-radius: 999px; }
.who i { font-style: normal; width: 20px; height: 20px; border-radius: 50%;
         background: var(--grad); color: #fff; font-size: 11px; font-weight: 700;
         display: flex; align-items: center; justify-content: center; }
.due { font-family: 'Inter', sans-serif; font-size: 11.5px; font-weight: 700;
       color: var(--accent); border: 1px solid var(--border); padding: 3px 10px; border-radius: 999px; }
```
- ToDoには**必ず担当と期限を付ける**(ないToDoは書かない)。実例: slide-decisions

### セマンティック4色の例外(SWOT・リスク評価など診断系のみ)

通常は「アクセント2色まで」だが、**意味が色に固定されている診断系フレームワークだけ**は4色を許可する:

| 意味 | 面(淡色) | 枠 | チップ/文字 |
|---|---|---|---|
| ポジティブ(強み) | #ecfdf5 | #bbe7d1 | #059669 |
| ネガティブ(弱み) | #fff1f2 | #fbd5da | #e11d48 |
| 中立・機会 | #eff6ff | #c9dff8 | #2563eb |
| 注意・脅威 | #fffbeb | #f5e5b8 | #d97706 |

**面は必ず淡色、本文文字は --ink / --text のまま。** 濃色の面に白文字を並べない(チップのみ濃色可)。

## やってはいけない集(頻出の失敗と修正)

| NG | なぜダメか | 修正 |
|---|---|---|
| タイトルが「〜について」「〜の概要」 | 結論がなく素人資料に見える | 結論を言い切るメッセージ文にする |
| 原色(#ff0000, #00ff00, #ffff00)の使用 | 安っぽい | テーマの --accent を使う |
| 3色以上のアクセント | 散漫 | --accent + --accent-2 の2色まで(SWOT等の診断系のみ上記の例外) |
| 真っ黒 #000 の文字 | コントラストが強すぎて硬い | --ink(濃紺・濃茶系)を使う |
| 濃い影 `box-shadow: 0 10px 30px rgba(0,0,0,0.5)` | 2010年代のデザイン | 影なし or 上記カードレシピの薄い影 |
| 日本語本文への letter-spacing | 読みにくい | 英語キッカーのみに付ける |
| 全要素をセンター揃え | 視線が定まらない | 左揃え基本。センターは表紙・引用のみ |
| グラデーションを面(背景全体・カード全体)に多用 | ギラつく | グラデは文字・アイコンチップ・細い線に限定 |
| 余白を削って情報を詰め込む | 読めないスライドは無価値 | 内容を削る(1枚1メッセージ) |
| 絵文字をアイコン代わりに多用 | 環境で見た目が変わる・幼い | 線画SVGアイコン(svg-guide参照)。カジュアル型のみ絵文字可 |

## 完成品質の目安

- 3メートル離れて見てもタイトルの結論が読める
- 色数を数えると「背景系 + 文字系 + アクセント2色以内」に収まっている
- どの要素も他の要素と端が揃っている(グリッドに乗っている)
- 空白領域が全体の30%以上ある(詰め込んでいない)
