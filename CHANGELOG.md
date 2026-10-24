# Changelog

## [Unreleased] - 2026-09-30

### Security

- 必要なNode.jsを24.22.1 LTS以上へ更新（2026-10-20 Node.jsセキュリティリリース対応、CVE-2026-111497ほか）。Dockerイメージも`node:24.22.1-alpine`へ更新。
- 依存関係の下限を引き上げ：`@modelcontextprotocol/sdk` 1.32.0以上、`zod` 4.6.6以上（GHSA-mrhx-c7xc-m3w5）、`tsx` 4.19.3以上。lockfileで`follow-redirects`を1.16.1、`body-parser`を2.4.0へ更新。
- 既知の問題：google-ads-api 25経由の`stream-json` 1.9.1に中程度の脆弱性（GHSA-528h-pc64-c93x、GHSA-hqr4-qq8f-hg3x、GHSA-mjw6-4jj6-33hc）が残っている。修正版（3.6.0以上）はgoogle-ads-apiが読み込む`streamers/StreamArray.js`を削除しており、互換性のある修正版はまだ無い。google-ads-apiではストリーミング検索とそのエラー応答の解析にのみ使われ、本サーバーが使う`query`・キーワードプランAPIの経路では使われない。後続の対応で追跡する。

### Updated

- OSSのMCP識別名・設定キーを`google-keyword-planner`へ統一（既存npm名は維持）。

- Google Cloudプロジェクト単位の認可、利用者自身によるブランド・所有ドメイン確認、Basic申請を説明。
- Google Ads API v25 / Node.js 24.22.1以上へ更新（2026-10-20 Node.jsセキュリティリリース対応）。Developer Tokenのユーザー設定を任意化し、ライブラリ向けの非機密互換値を使用。
- 旧OOBヘルパーをloopbackコールバック・state・PKCEへ変更。トークンは非公開ファイルに保存し、標準出力へ出さない。
- Desktop OAuthと自分のWeb OAuthクライアントを使うPlaygroundの両方の取得方法を追加。
- Codex・AGY・Claude Desktopのユーザー共通MCP設定、失効時の再認証と公開前確認を整理。
- 再現可能な依存インストール用lockfileと、実資格情報を使用しない自動テストを追加。

GitHubソースの更新です。npm/Docker Hubのリリースは別作業です。

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

