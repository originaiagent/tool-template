# セットアップガイド（技術リファレンス）

> Claude Codeが新規ツールの初期セットアップ時に参照するドキュメント。
> コマンド /project:setup から呼び出される。

---

## 1. 必須ファイル（全デプロイ先共通）

### .github/workflows/auto-merge.yml

claude/ブランチへのpushをdevelopに自動マージするGitHub Actions。
以下の内容をそのまま使用すること（改変禁止）。

#### 重要な注意点
- permissions: contents: write は jobsの外側（トップレベル）に書く
- git fetch origin develop が必須（ないとdevelopブランチの参照に失敗する）
- --no-edit を使用（--no-ffではない）

### CLAUDE.md

テンプレートは /CLAUDE.md を参照。セットアップ時にプレースホルダーを実際の値に置換する:

| プレースホルダー | 説明 |
|---|---|
| {TOOL_NAME} | ツール名（リポジトリ名と同じ） |
| {TOOL_DESCRIPTION} | ツールの概要説明 |
| {FRAMEWORK} | 使用フレームワーク |
| {LANGUAGE} | 使用言語 |
| {DEPLOY_TARGET} | Vercel / Streamlit Cloud / Cloud Run / Cloudflare Pages |
| {PREVIEW_URL} | develop用の確認URL |
| {PRODUCTION_URL} | main用の本番URL |

---

## 2. デプロイ先別の初期ファイル構成

### 2-A. Vercel（Next.js）

作成するファイル:
- package.json（next, react, react-dom, typescript, @types/react, @types/node）
- next.config.js
- tsconfig.json
- app/layout.tsx（App Router）
- app/page.tsx（「{ツール名} - セットアップ完了」と表示）
- app/globals.css
- .gitignore（Node用 + .next/, out/）

URL体系:
- 確認: https://{ツール名}-git-develop-origin-trees-projects.vercel.app
- 本番: https://{ツール名}.vercel.app

### 2-B. Streamlit（Python）

作成するファイル:
- app.py（st.title + 「セットアップ完了」表示）
- requirements.txt（streamlit）
- .gitignore（Python用）

URL体系: Streamlit Cloudでデプロイ後に確定。

### 2-C. Cloud Run（Node.js/TypeScript）

作成するファイル:
- package.json（express, typescript, ts-node, @types/express, @types/node）
- tsconfig.json
- src/index.ts（Express。ポート process.env.PORT || 8080）
- Dockerfile（Node.js 20、multi-stage build）
- .dockerignore
- .gitignore（Node用 + dist/）

URL体系:
- 確認: https://{ツール名}-dev-465031496778.asia-northeast1.run.app
- 本番: https://{ツール名}-465031496778.asia-northeast1.run.app

GCP: プロジェクト logistics-app-481912 / リージョン asia-northeast1

### 2-D. Cloudflare Pages（SPA）

作成するファイル:
- index.html
- style.css
- .gitignore

URL体系: Cloudflare Pagesでデプロイ後に確定。

---

## 3. セットアップ後の動作確認

1. claude/initial-setup ブランチを作成してpush
2. GitHub Actionsが自動実行されdevelopにマージされることを確認
3. mainが更新されていないことを確認
4. 報告文を出力して止まる

---

## 4. 完了報告フォーマット

【完了報告】{ツール名} 初期セットアップ

■ 実施した作業
- [x] .github/workflows/auto-merge.yml 作成
- [x] CLAUDE.md 作成（行動原則・ブランチルール・エラー対応記載済み）
- [x] .claude/settings.json 作成
- [x] .claude/commands/ 作成（plan, implement, fix, save, status, setup, fix-conflict, check-deploy）
- [x] .claude/agents/ 作成（architect, reviewer, error-fixer, investigator, db-designer）
- [x] docs/ 作成（setup-guide, architecture, vision, preferences, lessons-learned, progress, roadmap）
- [x] 初期ファイル構成作成（{デプロイ先}用）
- [x] claude/initial-setup → develop 自動マージ確認

■ ユーザーが行う残作業
- [ ] デプロイ先の接続（{デプロイ先}側の設定）
- [ ] 確認URLでの動作確認

■ URL
- 確認用(develop): {PREVIEW_URL}
- 本番(main): {PRODUCTION_URL}

---

## 5. トラブルシューティング

### auto-mergeが動かない
1. Settings → Actions → Workflow permissions が「Read and write」か確認
2. auto-merge.yml の permissions: contents: write がトップレベルにあるか確認
3. git fetch origin develop の行があるか確認

### マージコンフリクト
1. developの最新をpull
2. developベースで新しい claude/fix-xxx ブランチを作成
3. 変更内容をdevelop上のコードに正しく適用し直す
4. 既存のclaude/ブランチは使わない

### DBスキーマ変更がある場合
- Supabase MCPの apply_migration でSQL実行（コードデプロイとは別系統）
- コード + DB両方変更: 先にDB → 後でコード
