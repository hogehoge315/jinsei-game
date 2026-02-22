あなたはリードフロントエンドエンジニア兼ゲームアーキテクト兼UXデザイナーです。
GitHub Pagesで公開する「スマホ向け・すごろく型人生シミュレーション」を、
プロダクション品質で完成させます。

これはMVPではない。
初回リリースで“十分遊べるボリューム”と“最高水準のUI/UX”を両立する。
手抜き・TODO放置・ダミーデータ・件数不足・仮実装は禁止。

========================================================
■ 絶対ルール（厳守）
========================================================
1) 指定件数のデータを満たすまで次工程に進まない。
2) 「あとで追加」「省略」「例として数件」などは禁止。
3) 市販人生ゲームの複製は禁止（固有名詞/同一文章/同一マス配置/カード内容流用NG）。
   完全オリジナル。
4) UIとロジックを完全分離（coreは純粋関数中心）。
5) 各STEP終了時に自己検証チェックを必ず出力。
6) 実装順を変更しない。
7) 禁止ワードを出力したら即修正して再出力。
8) ファイルは「ファイルパス → 内容」の形式で提示。
9) npm run build が通る前提でコードを書く。

禁止ワード：
TODO / placeholder / dummy / 後で追加 / 省略

========================================================
■ 技術スタック（固定）
========================================================
- Vite + React + TypeScript
- React Router（HashRouter採用。理由をdocsに記載）
- Zustand
- Tailwind CSS
- zod
- idb (IndexedDB)
- Vitest
- vite-plugin-pwa
- GitHub Actions → GitHub Pages

変更不可。

========================================================
■ UI/UX 最高品質要件（必須）
========================================================

A) モバイルベストプラクティス
- 片手操作設計（主要操作は画面下部）
- タップ領域 44px以上
- 1画面1タスク原則
- 押下フィードバック必須
- 危険操作は確認付き
- 高速モード切替あり

B) アクセシビリティ
- aria属性適切付与
- キーボード操作対応
- WCAG AA相当コントラスト目標
- prefers-reduced-motion対応
- remベース設計

C) デザインシステム
docs/ui-guidelines.md を作成し、
- タイポグラフィ
- スペーシング
- 色
- 状態（hover/active/disabled）
- コンポーネント規約
を定義する。

src/ui/ に再利用コンポーネントを作成：
Button / Modal / Drawer / Toast / Card / StatBadge / Progress

D) UX監査ドキュメント
docs/ux-audit.md を作成し、
ヒューリスティック10原則で各画面を評価・自己監査する。

E) パフォーマンス
- 依存ライブラリ最小
- ルート単位コード分割
- 盤面描画軽量化
- perf.md を作成

========================================================
■ 必須コンテンツ量（未達なら進むな）
========================================================
- events.json 240件以上
- cards.json 140件以上
- items.json 120件以上
- jobs.json 30件
- board.json 160〜200マス
- achievements 50件以上
- endings 16件以上

生成後に必ず件数を明示して確認。

========================================================
■ ゲーム仕様（固定）
========================================================
- 1ゲーム 96ターン（月）
- 3章構成 + 6分岐ルート
- ステータス：
  money / happiness / health / stress / reputation
  skills: tech / creative / social / finance / body
- 職業システム（昇進条件/解雇リスク/イベント補正あり）
- 投資/保険/ローン/家族システム
- 状態異常（burnout等）
- seed付き乱数
- セーブ5スロット + autosave + export/import

========================================================
■ 実装順（厳守）
========================================================

STEP1:
- スタック確認
- tree提示
- docs目次
- 画面一覧 + 導線
- ワイヤーフレーム（簡易ASCII）

STEP2:
docs生成
- game-design.md
- architecture.md
- data-schema.md
- ui-guidelines.md
- ux-audit.md
- perf.md

STEP3:
型定義 + zodスキーマ

STEP4:
src/core 実装（UI禁止）
- rng
- dice
- move
- weightedEventDraw
- applyEffects
- monthlyEconomy
- endingsCheck

STEP5:
store + persistence

STEP6:
UI実装

STEP7:
データ生成（件数達成まで続ける）

STEP8:
Vitestテスト

STEP9:
CI/CD + PWA実装

========================================================
■ CI/CD要件（必須）
========================================================

.github/workflows/pages.yml を作成。

- permissions:
  contents: read
  pages: write
  id-token: write
- concurrency設定
- push main のみ deploy
- PRはbuild/testのみ
- checkout → setup-node → npm ci → npm test → npm run build
  → upload-pages-artifact → deploy-pages

ViteのbaseはGitHub Pages用に対応。
READMEにPages有効化手順を書く：
Settings → Pages → Source: GitHub Actions

========================================================
■ 自己検証チェック（各STEP終了時）
========================================================

CHECK:
- 指定件数達成: OK/NG
- 禁止ワード: なし
- 仮実装: なし
- A11y対応: OK
- UX要件: OK
- npm test: OK想定
- npm run build: OK想定
- CI/CD（STEP9）: workflow作成OK

NGなら修正してから進む。

========================================================
