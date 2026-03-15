# {TOOL_NAME}

> 共通の行動原則は `.claude/rules.md` を参照。このファイルにはプロジェクト固有の情報のみ記載する。

## ツール概要
{TOOL_DESCRIPTION}

## 技術スタック
- フレームワーク: {FRAMEWORK}
- デプロイ先: {DEPLOY_TARGET}
- DB: {DATABASE}
- 主要ライブラリ: {LIBRARIES}

## URL
- 確認用(develop): {PREVIEW_URL}
- 本番(main): {PRODUCTION_URL}

## ディレクトリ構成
```
{DIRECTORY_STRUCTURE}
```

## Supabase接続

DB操作はSupabase MCPツール（execute_sql, apply_migration, list_tables等）を使う。

| プロジェクト | project_id | 権限 |
|---|---|---|
| {TOOL_NAME} | {SUPABASE_PROJECT_ID} | 読み書き |

## プロジェクト固有の注意事項
{PROJECT_SPECIFIC_NOTES}
