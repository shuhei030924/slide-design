# slide_design

HTMLでPowerPoint風スライド(16:9 / 1280×720px)を1枚単位で作るためのスキルと、48種のレイアウトパターン実例集。

## 構成

```
.github/skills/html-slide/   # スキル本体(GitHub Copilot用・マスター)
  SKILL.md                   #   ワークフローと絶対ルール
  assets/template.html       #   ライトテーマ基本テンプレート
  assets/template-dark.html  #   ダークテーマ基本テンプレート
  references/pattern-guide.md#   48パターンの選択ガイド(判定フロー)
  references/design-guide.md #   デザインシステム(カラートークン6テーマ・定型CSSレシピ)
  references/svg-guide.md    #   SVG図解の座標計算・定型コード
.claude/skills/html-slide/   # 同内容のミラー(Claude Code用)
slides/                      # 実例48枚(slide-*.html)+ gallery.html + previews/*.png
```

## 運用ルール

- **スキルを編集したら `.github/skills/html-slide/` と `.claude/skills/html-slide/` の両方に反映する**(マスターは `.github` 側。編集後にディレクトリごとコピーすればよい)
- 実例スライドを追加したら: 冒頭コメント(パターン/使うべきタイミング/向かないケース)を書き、`pattern-guide.md` の表と `gallery.html` に行を追加し、`previews/` にスクリーンショットを保存する

## プレビュー画像の生成(Windows / ヘッドレスChrome)

```powershell
& "C:\Program Files\Google\Chrome\Application\chrome.exe" --headless=new --disable-gpu `
  --screenshot="D:\task\slide_design\slides\previews\slide-XXX.png" `
  --window-size=1280,720 --hide-scrollbars --virtual-time-budget=10000 `
  "file:///D:/task/slide_design/slides/slide-XXX.html"
```

`slides/gallery.html` をブラウザで開くと全パターンを一覧できる。
