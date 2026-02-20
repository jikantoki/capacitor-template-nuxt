# ENOKI Appcenter（エノキ電気ホームページ）

Nuxt 4 + Capacitor を使用したポートフォリオ兼アプリセンターのテンプレート

- NOLICENSED ご自由にお使いください

## 概要

ときえのきが制作したアプリ・ツール・楽曲を一箇所で公開するためのポートフォリオ兼簡易アプリストアです。  
Nuxt 4 と Capacitor をベースにしており、Web・Android・iOS に対応しています。

- 公式サイト: <https://app.enoki.xyz>

## 使用技術

| カテゴリ | 技術 |
| --- | --- |
| フレームワーク | [Nuxt 4](https://nuxt.com/) |
| UI コンポーネント | [Vuetify](https://vuetifyjs.com/) |
| モバイルアプリ | [Capacitor 8](https://capacitorjs.com/) |
| 状態管理 | [Pinia](https://pinia.vuejs.org/) |
| 多言語対応 | [@nuxtjs/i18n](https://i18n.nuxtjs.org/) |
| コンテンツ管理 | [@nuxt/content](https://content.nuxt.com/) |
| テンプレートエンジン | [Pug](https://pugjs.org/) |
| スタイル | SASS（SCSS） |
| リンター | ESLint + Prettier |
| バックエンド | PHP + MySQL |
| パッケージマネージャー | yarn |

## ファイル構成

```
.
├── app/                        # Nuxt アプリケーション本体
│   ├── app.vue                 # アプリケーションのルートコンポーネント
│   ├── components/             # Vue コンポーネント
│   │   └── common/             # 共通コンポーネント（スプラッシュ等）
│   ├── js/                     # ユーティリティ関数
│   │   ├── Functions.ts        # 汎用関数群
│   │   ├── ajaxFunctions.ts    # Ajax 通信処理
│   │   ├── metaFunctions.ts    # メタ情報操作
│   │   ├── muni.ts             # その他ユーティリティ
│   │   ├── setup.ts            # 初期セットアップ処理
│   │   └── webpush.ts          # Web Push 通知処理
│   ├── mixins/
│   │   └── mixins.ts           # Vue ミックスイン
│   ├── pages/                  # ページコンポーネント
│   │   ├── index.vue           # トップページ
│   │   ├── login.vue           # ログイン
│   │   ├── registar.vue        # アカウント登録
│   │   ├── password_reset.vue  # パスワードリセット
│   │   ├── tutorial.vue        # チュートリアル
│   │   ├── terms.vue           # 利用規約
│   │   ├── friendlist.vue      # フレンドリスト
│   │   ├── qrcode.vue          # QR コード
│   │   ├── about.vue           # このアプリについて
│   │   ├── [ready].vue         # 読み込み完了ページ
│   │   ├── settings/           # 設定ページ
│   │   │   ├── index.vue       # 設定トップ
│   │   │   ├── profile.vue     # プロフィール設定
│   │   │   ├── display.vue     # 表示設定
│   │   │   └── developer-options.vue  # 開発者オプション
│   │   └── user/
│   │       └── [userId].vue    # ユーザープロフィールページ
│   └── stores/                 # Pinia ストア
│       ├── meta.ts             # メタ情報
│       ├── myProfile.ts        # 自分のプロフィール
│       └── settings.ts         # アプリ設定
├── php/                        # PHP バックエンド
│   ├── functions/              # PHP 共通関数
│   ├── createAccount.php       # アカウント作成
│   ├── loginAccount.php        # ログイン処理
│   ├── updateProfile.php       # プロフィール更新
│   ├── sendPushForAccount.php  # プッシュ通知送信
│   └── ...                     # その他 API エンドポイント
├── public/                     # 静的ファイル
│   ├── manifest.json           # PWA マニフェスト
│   ├── sw.js                   # Service Worker
│   └── icons/                  # アプリアイコン各サイズ
├── assets/                     # ビルド時に処理されるアセット
├── capacitor.config.ts         # Capacitor 設定
├── nuxt.config.ts              # Nuxt 設定
├── content.config.ts           # Nuxt Content 設定
├── eslint.config.mjs           # ESLint 設定
├── tsconfig.json               # TypeScript 設定
├── database.sql                # MySQL データベース定義
├── composer.json               # PHP 依存パッケージ
└── package.json                # Node.js 依存パッケージ
```

## セットアップ

このプロジェクトは **表示用サーバー（Nuxt）** と **処理用サーバー（PHP + MySQL）** の 2 つが必要です。

### 前提条件

- Node.js（最新 LTS 推奨）
- yarn
- PHP + Composer（バックエンド機能を使う場合）
- MySQL（バックエンド機能を使う場合）

### 1. 依存パッケージのインストール

```shell
yarn install
composer install  # PHP バックエンドを使う場合
```

### 2. 環境変数の設定

プロジェクトルートに `.env` ファイルを作成し、以下を記述します（クォーテーション不要）。

```env
NUXT_WEBPUSH_PUBLICKEY=WebPushパブリックキー
NUXT_WEBPUSH_PRIVATEKEY=WebPushプライベートキー

NUXT_API_ID=default
NUXT_API_TOKEN=PHPで発行するアクセストークン
NUXT_API_ACCESSKEY=PHPで発行するアクセスキー

NUXT_API_HOST=APIサーバーのホスト
```

> WebPush 用の鍵は <https://web-push-codelab.glitch.me/> で生成できます。

**Vercel 等にデプロイする場合**: Project Settings → Environment Variables で同様に設定してください。

### 3. 開発サーバーの起動

```shell
yarn run dev
```

`http://localhost:10000` でアクセスできます。

### 4. ビルド

```shell
yarn run build     # 本番用ビルド
yarn run generate  # 静的ファイル生成
yarn run preview   # ビルド結果のプレビュー
```

## PHP バックエンドのセットアップ

バックエンド機能（アカウント認証・プッシュ通知等）を使用する場合に必要です。

### 1. PHP サーバーの用意

レンタルサーバー等を利用し、`php/` フォルダをドメインのルートに配置します（`.htaccess` 等で設定）。

### 2. `/env.php` の作成

リポジトリルート直下に `env.php` を作成し、以下を記述します。

```php
<?php
define('DIRECTORY_NAME', '/プロジェクトルートのディレクトリ名');

define('NUXT_WebPush_PublicKey', 'パブリックキー');
define('NUXT_WebPush_PrivateKey', 'プライベートキー');
define('WebPush_URL', 'プッシュ通知を使うドメイン');
define('WebPush_icon', 'プッシュ通知アイコンURL');
define('Default_user_icon', 'デフォルトユーザーアイコンURL');

define('MySQL_Host', 'MySQLサーバー');
define('MySQL_DBName', 'DB名');
define('MySQL_User', 'DB操作ユーザー名');
define('MySQL_Password', 'DBパスワード');

define('SMTP_Name', '自動メール送信時の差出名');
define('SMTP_Username', 'SMTPユーザー名');
define('SMTP_Mailaddress', '送信に使うメールアドレス');
define('SMTP_Password', 'SMTPパスワード');
define('SMTP_Server', 'SMTPサーバー');
define('SMTP_Port', 587);
```

### 3. MySQL データベースの初期化

`database.sql` を MySQL にインポートします（PHPMyAdmin から DB 直下にインポート可）。

### 4. デフォルト API トークンの発行

1. セットアップした API サーバーの `/makeApiForAdmin.php` にアクセス
2. 表示された内容を `.env` に記述
3. 以後その値を使って API を操作できます

> **注意**: トークンを忘れた場合は MySQL の `api_list` テーブルから `secretId='default'` の行を削除し、再度 `/makeApiForAdmin.php` にアクセスして再発行してください。

## 主な設定箇所

| 項目 | 設定ファイル |
| --- | --- |
| アプリ名・バージョン | `/package.json` |
| Nuxt 全般設定・OGP | `/nuxt.config.ts` |
| Capacitor（モバイルアプリ）設定 | `/capacitor.config.ts` |
| テーマ・Vuetify 設定 | `/nuxt.config.ts` の `vuetify` セクション |
| PWA マニフェスト | `/public/manifest.json` |
| 404 ページ | `/error.vue` |

## 注意事項

- VSCode での開発を推奨します
- `.vscode/` に推奨設定・拡張機能の設定が含まれています

## 参考資料

- WebPush: <https://tech.excite.co.jp/entry/2021/06/30/104213>
- Nuxt 公式: <https://nuxt.com/docs>
- Capacitor 公式: <https://capacitorjs.com/docs>
- Vuetify 公式: <https://vuetifyjs.com/>
