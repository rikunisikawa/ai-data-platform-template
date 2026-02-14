# MVP仕様・要件整理

## MVP仕様（準リアルタイム定義）
- Fitbit Web API を一定間隔でポーリングし差分取得（since/until）。
- Raw 保存を必須とする（APIレスポンスJSONをそのまま保持）。
- Raw → Normalized → Features → Music Params の順で生成。
- MVPの最終成果物は Music Params（JSON）。

## 機能要件
1. **認証**
   - OAuth2 Authorization Code Flow。
   - トークンは `.env` もしくは OS keyring に保存し、Git管理禁止。
   - `.env.example` を用意。

2. **データ取得（Connector）**
   - 初期対象: Heart Rate / Sleep / Activities (Steps)。
   - Fitbit API 主要エンドポイントへの拡張可能構成。
   - 差分取得（since/until）と再実行可能な設計。

3. **データレイヤ**
   - Raw: 生JSONを保存。
   - Normalized: エンドポイント横断で時系列イベント形式へ統一。
   - Features: 音楽生成の特徴量（平均HR、HRV、睡眠指標、活動量）。

4. **音楽パラメータ生成**
   - `music_params.schema.json` に準拠した JSON を生成。
   - Renderer 抽象I/Fを前提に拡張可能（MIDI/WAV/OSC）。

5. **CLI**
   - `fitbit-music auth`
   - `fitbit-music ingest --since <datetime>`
   - `fitbit-music build`
   - `fitbit-music run --interval 15m`

## 非機能要件
- **再処理可能性**: Raw保存を根拠に再生成可能。
- **UTC正規化**: Normalized層でUTC統一。
- **欠損補完方針**: 設計書に明文化。
- **セキュリティ**: トークン・個人データはGitに入れない。
- **拡張性**: コネクタ/Rendererを差し替え可能な構成。

## 未確定事項（優先度つき、最大10件）
1. **中**: 保存先抽象化（将来S3対応の具体設計）。
2. **中**: 初期Rendererの選定（MIDI or OSC）。
3. **中**: 欠損補完の具体アルゴリズム（前方補完/線形補完）。
4. **中**: Featuresの最小セット（HRV指標の具体定義）。
5. **低**: 音楽ジャンル依存パラメータの扱い。
6. **低**: CLI出力ログ形式（JSONログ等）。
7. **低**: サンプルデータのフォーマットと粒度。

## 決定事項（暫定含む）
- ポーリング間隔: 暫定で `15m`（後で変更前提、Fitbit仕様に合わせて再決定）。 
- タイムゾーン方針: UTC固定（すべてUTCで保存・集計）。 
- OAuth Callback方式: 手動入力方式（ブラウザ認証後にコードを手入力）。 
