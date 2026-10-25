# Changelog

## [1.1.2] - 2026-10-25

### Security
- 必要なNode.jsを24.22.1 LTS以上へ更新（2026-10-20 Node.jsセキュリティリリース対応、CVE-2026-111497ほか）。Dockerイメージも`node:24.22.1-alpine`へ更新。
- 依存関係の下限を引き上げ：`@modelcontextprotocol/sdk` 1.32.0以上、`zod` 4.6.6以上（GHSA-mrhx-c7xc-m3w5）、`tsx` 4.19.3以上。`package-lock.json`をコミット対象に変更。`overrides`でgaxios 6配下の`uuid`を11.1.1以上に固定（このリポジトリからのインストール（Dockerイメージなど）に適用され、npm経由の利用者には適用されない）。
- 既知の問題：google-ads-api 21経由の`stream-json` 1.9.1に中程度の脆弱性（GHSA-528h-pc64-c93x、GHSA-hqr4-qq8f-hg3x、GHSA-mjw6-4jj6-33hc）が残っている。互換性のある修正版はまだ無い。用途はGoogle Ads APIのエラーレスポンスの解析のみ。後続の対応で追跡する。

## [1.0.0] - 2025-10-11

### Platforms
- ✅ **macOS**: Full support
- ✅ **Windows**: Full support
- ✅ **Linux**: Full support (Node.js required)

### Added
- Initial release of Google Ads MCP Server
- **クロスプラットフォーム対応**: Windows, macOS, Linux
- キーワードプランナーAPI統合
- 3つのMCPツール:
  - `analyze_global_keyword_interest`: グローバルキーワード分析
  - `analyze_keywords_by_location`: 地域別キーワード分析
  - `get_detailed_keyword_plan`: 詳細なキーワードプラン取得
- Refresh Token取得用ヘルパースクリプト (`npm run get-refresh-token`)
- レイヤードアーキテクチャ実装
- MCC（マネージャーアカウント）サポート

### Changed
- エラーログを簡潔に改善（不要な詳細ログを削除）
- MetadataLookupWarningを抑制
- エラーメッセージをユーザーフレンドリーに改善

### Fixed
- LOGIN_CUSTOMER_ID設定によるMCCアカウント配下のクライアントアカウントアクセス問題を解決
- Google Ads APIエラーの適切な処理とメッセージ表示

### Documentation
- `SETUP_GUIDE.md`: クイックスタートガイドを追加
- `WINDOWS_SETUP.md`: **Windows専用セットアップガイド**を追加
- `setup-auth.md`: 認証情報取得の詳細ガイド
- `USAGE_EXAMPLES.md`: 使用例集
- `README.md`: MCCアカウント設定とWindows対応を追加
- `SECURITY.md`: セキュリティガイドを追加
- `.env.example`: LOGIN_CUSTOMER_ID設定を追加
- `claude-config-example-windows.json`: Windows用設定例を追加

## アーキテクチャ

```
src/
├── types/              # 型定義
├── config/             # 設定管理
├── infrastructure/     # Google Ads API クライアント
├── domain/             # ビジネスロジック
├── application/        # ユースケース
└── presentation/       # MCP サーバー実装
```

### 設計原則

- **依存性の逆転**: 上位層は下位層に依存せず、抽象に依存
- **単一責任の原則**: 各クラスは単一の責任を持つ
- **開放閉鎖の原則**: 拡張に対して開き、修正に対して閉じている
- **型安全性**: TypeScriptの型システムを最大限活用

