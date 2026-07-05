# ワークスペースルール (slide_design)

## Gitワークフロー(必須)

- **タスクの最後は必ずマージまで完了させる。** PRを作成したら、検証(スクリーンショット確認)が済み次第 `gh pr merge <番号> --merge --delete-branch` でmasterへマージし、マージ後に `git status` と `origin/master` の一致を確認して報告する。PRを未マージのまま作業を終えない
- 作業はmaster直コミットではなくブランチ+PR経由で行う(軽微なドキュメント修正のみ直コミット可)

## このリポジトリの構成ルール

- スキルのマスターは `.github/skills/html-slide/`。編集したら `.claude/skills/html-slide/` へディレクトリごとコピーして同期する
- 実例スライドを追加したら: ①冒頭コメント(パターン/使うべきタイミング/向かないケース) ②`references/pattern-guide.md` の判定フローと表 ③`slides/gallery.html`(件数も更新) ④`slides/previews/*.png` の4点をセットで更新する
- プレビューPNGの生成コマンドはREADME参照(ヘッドレスChrome、1280×720)
- スライドの品質基準は `references/design-guide.md` に従う(色トークンをコピーし、色を発明しない)
