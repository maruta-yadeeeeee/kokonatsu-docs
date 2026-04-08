---
sidebar_position: 7
title: Cookie・セッション
---

# プライバシーポリシー — Cookie・セッション

**最終更新日: 2026年4月8日**

---

## Cookieとは

Cookie（クッキー）とは、Webサイトがユーザーのブラウザに保存する小さなテキストファイルです。次回同じサイトを訪問した際に読み取られ、ログイン状態の維持などに使用されます。

---

## 本サービスで使用するCookieの種類

### 1. セッションCookie（必須）

ログイン状態を維持するために使用します。

| Cookie名 | 用途 | 保存期間 |
|---------|------|---------|
| セッショントークン | ログイン認証状態の維持 | ブラウザを閉じるまで、または明示的にログアウトするまで |
| CSRF防止トークン | クロスサイトリクエストフォージェリ（CSRF）攻撃の防止 | セッション中 |

これらのCookieは本サービスの基本機能に必要なものであり、無効化するとログインが正常に機能しなくなります。

### 2. 設定Cookie（機能性）

ユーザーの設定・環境を記憶するために使用します。

| 用途 | 保存場所 | 保存期間 |
|------|---------|---------|
| 言語・テーマ等の表示設定 | LocalStorage / Cookie | 設定変更まで |

### 3. セキュリティCookie（必須）

セキュリティ目的で使用します。

| 用途 | 保存場所 | 保存期間 |
|------|---------|---------|
| デバイスフィンガープリント関連情報 | LocalStorage | 最大180日 |
| 新規デバイス検知のための識別子 | Cookie / LocalStorage | 最大90日 |

---

## 第三者のCookie・スクリプト

### Cloudflare Turnstile

本サービスでは、Botによる自動登録・スパムを防ぐために **Cloudflare Turnstile** を使用しています。Turnstileはユーザーがボットでないことを検証するために、CloudflareによりCookieやローカルストレージが使用される場合があります。

- [Cloudflare プライバシーポリシー](https://www.cloudflare.com/privacypolicy/)

---

## セッション管理

### セッションとは

セッションとは、ログインから次のログアウトまでの一連のアクセスを管理する仕組みです。本サービスでは、以下のセッション情報を記録します。

- ログイン日時・ログアウト日時
- 使用したIPアドレス
- ブラウザ・OS情報（ユーザーエージェント）
- おおよその地理情報（GeoIP）

### セッションの管理方法

ユーザーはアカウント設定の「セッション管理」から以下の操作が行えます。

- 現在アクティブなセッションの一覧確認
- 特定のセッションを個別にログアウト
- すべてのセッションから一括ログアウト

### セッションの自動無効化

以下の場合、セッションは自動的に無効化されます。

- パスワードを変更した場合（全セッション無効化）
- MFAの設定を変更した場合（全セッション無効化）
- アカウントが停止された場合

---

## Cookieの管理

ブラウザの設定からCookieを無効化・削除することができます。ただし、必須Cookieを無効化した場合、ログイン機能等が正常に動作しなくなる場合があります。

主要なブラウザでのCookie管理方法：

- [Google Chrome](https://support.google.com/chrome/answer/95647)
- [Mozilla Firefox](https://support.mozilla.org/ja/kb/cookies-information-websites-store-on-your-computer)
- [Safari](https://support.apple.com/ja-jp/guide/safari/sfri11471/mac)
- [Microsoft Edge](https://support.microsoft.com/ja-jp/microsoft-edge/microsoft-edge-%E3%81%AEcookie-%E3%82%92%E5%89%8A%E9%99%A4%E3%81%99%E3%82%8B)

---

## 関連ページ

- [収集する情報](./data-collection)
- [セキュリティ対策](./security)
- [保存期間・削除](./data-retention)
