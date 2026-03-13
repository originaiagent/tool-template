# /project:setup コマンド

新規ツールの初期セットアップを実行する。

## 実行条件

- developブランチで作業していること
- リポジトリが originaiagent 配下であること

## 実行手順

### 1. ユーザーに確認する（必ず最初に聞く）

以下を確認してから作業開始:
- ツール名（リポジトリ名と一致しているか）
- デプロイ先（Vercel / Streamlit / Cloud Run / Cloudflare Pages）
- ツールの概要（何をするツールか）

### 2. docs/setup-guide.md を読む

`docs/setup-guide.md` を参照して、デプロイ先に応じた正しいファイル構成を確認する。

### 3. ファイルを作成する（以下の順番で）

1. `.github/workflows/auto-merge.yml` — setup-guide.md の内容をそのまま使用（改変禁止）
2. `.claude/settings.json` — setup-guide.md の内容をそのまま使用
3. `CLAUDE.md` — テンプレートのプレースホルダーを実際の値に置換
4. `docs/architecture.md` — ツールの技術仕様
5. デプロイ先別の初期ファイル構成 — setup-guide.md のセクション2を参照

### 4. 動作確認

1. `claude/initial-setup` ブランチを作成
2. 全ファイルをcommit & push
3. GitHub Actionsの実行を確認（auto-mergeがdevelopにマージすること）
4. mainが更新されていないことを確認

### 5. 報告して止まる

setup-guide.md セクション4の完了報告フォーマットに従って報告し、必ず止まる。
ユーザーの承認なしに次の作業へ進まない。
