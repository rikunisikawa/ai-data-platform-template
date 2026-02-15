## 開発方針
本プロジェクトの開発方針は「AIエージェントが主体となって自律的に開発・運用できるAIネイティブデータ基盤」です。人間は指示とレビューに集中し、実装・運用はAIエージェントが自律的に行える構造を前提とします。

## AIツール共通運用
本リポジトリは Codex / Claude / Gemini CLI のいずれでも同一の設計・運用ができる「ツール非依存」を前提とする。MCP サーバー設定は `docs/mcp_setup.md` を参照する。

## Skills の共通化方針
Skills はツール非依存の Markdown として扱い、全AIエージェントで同一の知見を参照できるようにする。プロジェクト内の `ai/` と、ローカルの共通 Skills（`~/.codex/skills/`）の両方を参照対象とする。

## AIネイティブ化の工夫
- エージェントが理解しやすいモノレポ構造を採用し、`packages/` 配下で機能を分離。
- `AGENT.md` に運用・実装手順を集約し、AIがこのファイルだけで作業可能にする。
- `packages/ai/prompts/` にAI向けプロンプトテンプレートを配置。
- `scripts/` に実行スクリプトを集約し、`dbt`・`ingestion`・`terraform` を同一導線で操作可能にする。
- `ai/` ディレクトリに設計原則・パターン・テンプレートを配置し、設計意図を明文化。

## 主要ディレクトリ
```
apps/                # 実行アプリ
packages/            # 機能別パッケージ（dbt/ingestion/infra/ai など）
scripts/             # 実行スクリプト
docs/                # 設計・構造・運用ドキュメント
configs/             # config例・schema・mappingルール
data/                # Raw/Normalized/Features（gitignore対象）
tests/               # 単体/統合テスト
```

## 初期ファイル一覧（雛形）
- `README.md`
- `docs/requirements.md`
- `docs/architecture.md`
- `docs/fitbit_api_endpoints.md`
- `configs/config.example.toml`
- `configs/music_params.schema.json`
- `.env.example`
- `.gitignore`
- `pyproject.toml`

  
## 実行前提
- dbt: `configs/dbt/profiles.yml.example` を基に `configs/dbt/profiles.yml` を作成する。
- Terraform: `configs/terraform_backend.hcl.example` を基に backend 設定を用意する。
- Docker と Docker Compose が利用可能であること。
- ホストの `~/.aws` に AWS 認証情報が設定されていること。
- `.env` が `.env.example` を基に作成済みであること（`APP_ENV` で環境切替）。

## 実行準備チェック
- `docs/setup_checklist.md` を参照する。

## Docker コンテナ運用
- `docs/container_operations.md` を参照する。

## 環境切り替え
- `APP_ENV` の使い方は `docs/env_usage.md` を参照する。
