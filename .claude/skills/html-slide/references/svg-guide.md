# SVG図解作成ガイド(スライド用)

スライドに埋め込むインラインSVGの定型レシピ集。**推測で座標を書かず、必ず本ガイドの計算式に従うこと。**

## 大原則

1. **論理座標で描く**: `<svg viewBox="0 0 W H" style="width: 100%;">` とし、中の座標はviewBox基準で計算する。CSSサイズ変更で自動スケールされる
2. **座標は先に計算してからコードを書く**: 各要素のx/y座標をコメントに計算式ごと書き残す
3. **id はSVGごとに一意にする**: 同一HTML内に複数SVGがある場合、`<defs>` 内の gradient/marker の id が重複すると後のSVGが壊れる。`id="bar-g"`, `id="flow-arrow"` のように用途名を付ける
4. **端の見切れを防ぐ**: stroke は座標線の両側に太さの半分ずつ広がる。図形はviewBoxの端から `stroke-width` 以上内側に置く
5. **色はスライドのアクセントカラーと統一する**

## テキスト配置(最重要・失敗しやすい)

- **水平中央揃え**: `text-anchor="middle"` を使い、x に中心座標を指定
- **垂直中央揃え**: `dominant-baseline` は環境差があるため使わない。**y = 中心y + fontSize × 0.35** で手動調整する
  - 例: 高さ60のボックス(y=100〜160)内に16pxの文字 → `y = 130 + 16*0.35 ≈ 136`
- **日本語テキストの幅見積り**: `全角文字数 × fontSize` (半角は × 0.55)。この幅がボックス幅 − パディング(左右各12px以上)に収まるか必ず確認する
  - 例: 「自動化フロー」(6文字) × 13px = 78px → ボックス幅は 78 + 24 = 102px 以上必要
- 収まらない場合は「文字を短くする」か「ボックスを広げる」。フォントを小さくしすぎない(最小12px)

## 定型レシピ

### 矢印(marker)

```html
<defs>
  <marker id="arrow1" viewBox="0 0 10 10" refX="9" refY="5"
          markerWidth="7" markerHeight="7" orient="auto">
    <path d="M 0 0 L 10 5 L 0 10 z" fill="#2563eb"/>
  </marker>
</defs>
<line x1="100" y1="50" x2="200" y2="50" stroke="#2563eb" stroke-width="2" marker-end="url(#arrow1)"/>
```
- 矢印の先端は線の終点に付く。ボックスに刺さらないよう終点はボックスの手前 4px に置く

### フローボックス+ラベル

```html
<rect x="40" y="40" width="140" height="60" rx="10" fill="none" stroke="#2563eb" stroke-width="2"/>
<!-- 中央: x = 40+70 = 110, y = 40+30 + 16*0.35 ≈ 76 -->
<text x="110" y="76" text-anchor="middle" font-size="16" fill="#1a2b4a">要素A</text>
```

### 棒グラフ

計算式を先に決める:
```
baseY  = 下端のy座標(例: 310)
plotH  = グラフ描画高さ(例: 250)
maxV   = データ最大値を少し超えるキリの良い値(例: max=128 → 130)
barH   = value / maxV * plotH   (四捨五入)
barY   = baseY - barH
barX_i = 左余白 + i * (barW + gap)
```
- 値ラベルは棒の上 `y = barY - 12`、`text-anchor="middle"`、x は棒の中心
- 軸ラベルは `y = baseY + 26`
- 基準線: `<line x1.. y1=baseY x2.. y2=baseY stroke="#cbd5e1"/>`
- 強調したい棒だけ濃色/グラデーション、他は淡色にすると視線誘導できる

### 円グラフ / ドーナツ(達成率など単一値向き)

```html
<!-- 例: 達成率72%。r=70 → 円周 = 2πr ≈ 439.8、表示長 = 439.8 × 0.72 ≈ 316.7 -->
<svg viewBox="0 0 200 200">
  <circle cx="100" cy="100" r="70" fill="none" stroke="#e2e8f0" stroke-width="22"/>
  <circle cx="100" cy="100" r="70" fill="none" stroke="#2563eb" stroke-width="22"
          stroke-dasharray="316.7 439.8"
          stroke-linecap="round"
          transform="rotate(-90 100 100)"/>
  <text x="100" y="112" text-anchor="middle" font-size="36" font-weight="800" fill="#1a2b4a">72%</text>
</svg>
```
- `stroke-dasharray="表示長 円周"`、`rotate(-90 cx cy)` で12時位置スタート
- 3値以上の円グラフは stroke-dashoffset の計算が複雑になるため、**横帯グラフか棒グラフに置き換えることを推奨**

### 折れ線グラフ

```html
<!-- x_i = 40 + i*80, y_i = baseY - v_i/maxV*plotH を先に全点計算 -->
<polyline points="40,200 120,170 200,180 280,120 360,90"
          fill="none" stroke="#2563eb" stroke-width="3"
          stroke-linecap="round" stroke-linejoin="round"/>
<!-- データ点: 各座標に circle r=5 -->
<circle cx="360" cy="90" r="6" fill="#2563eb"/>
```

### 交換矢印(相関図: お金と価値の流れ)

ビジネスモデル図など「AとBが何かを交換する」図の定型。**箱の間に上下2本の平行矢印**を引く。
斜めの矢印は座標計算が難しいため、箱は必ず横一列に並べて水平矢印だけで描く(実例: slide-bizmodel)。

```
箱: y=70, h=80(上下の帯 y=95 / y=130 は箱の高さ内に収める)
箱A: x=30..230、箱B: x=350..550 → ギャップ 230..350(中心x=290)
上段(価値・破線グレー): B→A  (346,95)→(238,95)   ラベルは箱の上 y=60
下段(お金・実線アクセント): A→B (234,130)→(342,130) ラベルは箱の下 y=170
```
```html
<line x1="346" y1="95" x2="238" y2="95" stroke="#94a3b8" stroke-width="2"
      stroke-dasharray="6 5" marker-end="url(#value-arrow)"/>
<line x1="234" y1="130" x2="342" y2="130" stroke="#2563eb" stroke-width="2.5"
      marker-end="url(#money-arrow)"/>
```
- 実線(アクセント色)=お金、破線(グレー)=価値・サービス、と決めて凡例を必ず添える
- marker はお金用・価値用で色が違うため **2つ別 id** で定義する

### 放射状ハブ&スポーク(拠点・エコシステム図)

中心1点(自社・本社・製品)から周辺ノード(拠点・連携先)へ均等角度で線を伸ばす図の定型(実例: slide-map, slide-ecosystem)。斜め線でも角度を先に計算しておけば破綻しない。

```
中心: (cx, cy)、ハブ半径 rHub、ノード半径 rNode(中心からの距離)
ノード数 n、開始角 θ0(度)、間隔 = 360/n
i番目のノード角度: θi = θ0 + i * (360/n)
座標: x = cx + rNode * cos(θi × π/180)、y = cy + rNode * sin(θi × π/180)
接続線: ハブ円周上の点(cx + rHub*cos, cy + rHub*sin) → ノード手前(ノード半径 - 15px程度)
```
- 4方向(上下左右)なら θ=-90,0,90,180 で cos/sin が 0 か ±1 になり暗算できる。6方向以上は上記の式で必ず計算してからコードを書く
- ラベルは中心から見て外側になる向きに配置する(上→上寄せ、右→左揃えで右側、下→下寄せ、左→右揃えで左側)
- viewBoxはノード+ラベル分の余白を確保する(ノード座標の上下左右に最低60px)

### シンプルアイコン(線画)

24×24 viewBox、`fill="none" stroke="currentColor(または色)" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"` が基本形。
```html
<!-- チェック -->  <path d="M4 12 l5 5 L20 7"/>
<!-- 人 -->        <circle cx="12" cy="8" r="4"/><path d="M4 21 v-1 a6 5 0 0 1 16 0 v1"/>
<!-- 歯車(簡易) --> <circle cx="12" cy="12" r="3"/><path d="M12 2 v3 M12 19 v3 M2 12 h3 M19 12 h3 M4.9 4.9 l2.1 2.1 M17 17 l2.1 2.1 M19.1 4.9 L17 7 M7 17 l-2.1 2.1"/>
<!-- 吹き出し -->   <path d="M21 15a2 2 0 0 1-2 2H7l-4 4V5a2 2 0 0 1 2-2h14a2 2 0 0 1 2 2z"/>
```
- 複雑なアイコンを自作しない。上記程度の単純図形の組み合わせに留める

### 背景装飾

- 薄い円: `<div>` + `border-radius: 50%` + `opacity: 0.05〜0.1` を絶対配置(SVG不要)
- グリッド線: CSS `background-image: linear-gradient(...)` × 2方向
- 有機的ブロブ: 複雑なpathが必要なため、既存のblob pathを流用し `fill` だけ変える

### 縦書きラベル(軸ラベルなど、CSS側の注意)

- 縦書きは `writing-mode: vertical-rl` のみで実現する。**`rotate(180deg)` を併用すると文字が上下反転するので禁止**
- 矢印記号(↑→)は縦書き内で回転してしまうため、矢印だけ別の横書き要素に分けて配置する

## 作成後のセルフチェック(必須)

コードを書いた後、以下を座標計算で機械的に確認する:

- [ ] すべての図形が viewBox の範囲内(x+width ≤ W、y+height ≤ H)
- [ ] テキスト幅の見積り(文字数×fontSize)がボックス幅に収まる
- [ ] text-anchor="middle" のxがボックス中心と一致(x = boxX + boxW/2)
- [ ] テキストのyが「中心y + fontSize×0.35」になっている
- [ ] defs内のidがHTML内で一意
- [ ] 最後にブラウザでスクリーンショットを撮り、はみ出し・重なりを目視確認
