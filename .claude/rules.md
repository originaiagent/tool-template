# 共通行動原則（全リポ共通・同期対象）

> tool-template から自動同期。改善提案は docs/evolution-log.md に記録 → トム承認後に反映。

---

## 作業フロー（全作業でこの順序に従う）

**YOU MUST follow this flow for every task. Do not skip steps.**

```
1. planner  → 影響範囲を調査し、実装計画を作成
2. ユーザー承認（デザイン・仕様の判断が必要な場合のみ。不要なら即3へ）
3. builder   → 計画に沿って実装
4. reviewer  → バグ・影響漏れ・回帰をチェック
5. verifier  → ビルド・動作・回帰の最終検証
6. push + 完了報告
```

### 差し戻しルール（厳守）

**reviewerが❌差し戻しを返した場合:**
→ reviewerの指摘内容を読み、builderで修正
→ 再度reviewerを呼ぶ。✅パスするまで繰り返す。

**verifierが❌修正必要を返した場合:**
→ verifierの指摘内容を読み、builderで修正
→ 再度verifierを呼ぶ。✅push可が出るまで繰り返す。

**3回差し戻しても解決しない場合:**
→ エスカレーション。問題の内容をユーザーに報告する。

**❌が残った状態でのpushは絶対禁止。全エージェントが✅を返してからpushする。**

- 軽微な修正（typo、1行変更等）でもplanner→verifierの流れは守る（plannerは簡略可）
- DB変更がある場合はdb-designerを最初に呼ぶ（planner前）

---

## ブランチ運用（厳守）

作業開始時に **必ず** 実行: `git checkout develop && git pull origin develop`

- main/developには絶対に直接pushしない
- 作業は `claude/[作業内容]` ブランチのみ
- 古いclaude/ブランチは再利用しない
- main = 本番（トムの明示指示のみ） / develop = 確認用 / claude/xxx = 作業用

---

## 影響範囲の調査（変更前に必須）

**YOU MUST investigate impact before making any change.**

何かを変更する前に、必ず grep/検索で影響範囲を調べてから手を動かすこと。

- カラム名・テーブル構造 → 参照する全ファイル、RLSポリシー、ビュー、関連クエリ
- コンポーネント → importしている全ページ・全コンポーネント
- API → 呼んでいるフロントエンドのコード
- 型・interface → 使っている全ファイル
- 関数名・引数 → 呼び出し元すべて

影響箇所は全て一緒に修正。「1箇所だけ直して他は後で」は禁止。部分最適より全体最適。

---

## 確認基準

### トムに聞くこと
- デザイン・UXの好み
- ビジネスロジックの判断（仕様の解釈が複数ある時）
- 本番反映（mainマージ）
- 破壊的変更（データ削除、テーブル構造の破壊的変更）
- 権限上できないこと（GitHub設定、GCPコンソール、デプロイ先ダッシュボード）

### 聞かずに自分で処理すること
- ビルドエラー、型エラー、import漏れ
- 画面が白い、ボタンが反応しない、レイアウト崩壊
- コンソールエラー、API 500エラー
- typo、不要なconsole.log
- docs/やlessons-learnedに記載済みの既知問題

---

## 自己進化

自由に更新（承認不要）: docs/、CLAUDE.mdの事実情報
提案→承認後: rules.md, commands/*, agents/*, settings.json
改善したら docs/evolution-log.md に記録。

---

## エラー対応

3回同じエラーで失敗 → エスカレーション（技術用語禁止、小学生でもわかる表現で）。

---

## 完了報告

```
【完了報告】
■ 実施した作業
■ 自己検証結果（ビルド/画面表示/変更機能/回帰/エラー: 各✅or❌）
■ トムの確認が必要な項目（なければ「なし」）
■ 確認URL
```
