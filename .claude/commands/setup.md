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
- メインSupabase project ID（DBを使う場合）

### 2. docs/setup-guide.md を読む

`docs/setup-guide.md` を参照して、デプロイ先に応じた正しいファイル構成を確認する。

### 3. ファイルを作成する（以下の順番で）

1. `.github/workflows/auto-merge.yml` — setup-guide.md の内容をそのまま使用（改変禁止）
2. `.claude/settings.json` — setup-guide.md の内容をそのまま使用
3. `CLAUDE.md` — テンプレートのプレースホルダーを実際の値に置換
   - Supabase project IDが入力された場合 → `{SUPABASE_PROJECT_ID}` を入力値に置換し、Supabase接続セクションを有効化
   - Supabase project IDが未入力・不明の場合 → Supabase接続セクションをコメントで残し、完了報告に「後でCLAUDE.mdにSupabase project IDを追記してください」を含める
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

### 同期設定（新規リポ作成時に必ず実施）

新しいリポジトリを作成した場合、tool-templateの同期対象に追加する必要がある。
完了報告の「ユーザーが行う残作業」に以下を必ず含めること:

```
- [ ] tool-templateの `.github/workflows/dispatch-sync.yml` にリポ名を追加
      → Claude Codeでtool-templateを開いて「dispatch-sync.ymlのmatrix.repoに {リポ名} を追加して」と指示
```
