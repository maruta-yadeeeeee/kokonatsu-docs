# ここなつ OAuth2 / OpenID Connect 仕様詳細

ここなつアカウントは、外部アプリケーションがユーザー認証を行えるように、標準的な **OAuth 2.0** および **OpenID Connect (OIDC)** プロトコルをサポートしています。
これにより、開発者は自身のサービスに「ここなつアカウントでログイン (Login with Kokonatsu)」機能を実装でき、セキュアな認証基盤として利用できます。

---

## 1. エンドポイント概要 (Discovery)

設定用の主要な情報を提供するディスカバリーエンドポイントが存在します。
各種ライブラリ（NextAuth.js, Passport.js, 汎用OAuthクライアント等）に OIDC 発行者 (Issuer) としてこのドメインを設定するだけで、大半の連携が自動化されます。

*   **OpenID Configuration**: `/.well-known/openid-configuration`
*   **JWKS (JSON Web Key Set)**: `/.well-known/jwks.json`

## 2. 開発者向け機能 (アプリ登録)

マイページの「開発者向け（OAuth2）」メニューから、新しいアプリケーションの作成と管理が可能です。

### アプリの登録と管理
*   **基本情報の設定**: アプリ名（3〜100文字）、アプリの概要、および許可する リダイレクトURI（複数可、HTTPS または `localhost` / `127.0.0.1` のみ）を設定します。
*   **認証情報の取得**: `client_id` と `client_secret` がすぐに発行されます。`client_secret` は画面に一度しか表示されませんが、後からいつでも再生成が可能です。
*   **アイコンの設定**: アプリの顔となるアイコン画像（2MB以下のPNG等）をアップロードでき、ユーザーの認可画面に表示されます。
*   **統計情報の確認**: 認可済みの総ユーザー数、アクティブなアクセストークン数、リフレッシュトークン数、最終認可日時など、アプリの利用状況をリアルタイムで確認できます。

---

## 3. 主要APIの詳細とフロー

### ① Authorize Endpoint (認可画面への遷移)
**パス**: `GET /oauth/authorize`
ユーザーを認証し、アプリケーションへの権限付与に同意させるためのエンドポイントです。
*   **必須パラメータ**: `response_type=code`, `client_id`, `redirect_uri`, `scope`
*   **推奨パラメータ**: `state` (CSRF対策), `nonce` (OIDC リプレイ攻撃対策)
*   スコープを確認する認可画面では、アプリのアイコン、名前、および詳細な権限リストが表示されます。

### ② Token Endpoint (トークン発行)
**パス**: `POST /oauth/token`
認可コード (`authorization_code`) をトークンと交換します。

*   **サポートされる Grant Types**:
    *   `authorization_code`: 認可コードをトークンに引き換えます。（コード有効期限：10分）
    *   `refresh_token`: トークンを再生成します。
*   **サポートされる Auth Methods**:
    *   `client_secret_post`: リクエストボディに `client_id` と `client_secret` を含める。
    *   `client_secret_basic`: HTTP Headerの `Authorization: Basic ...` で渡す。
*   **トークン有効期限**:
    *   **Access Token**: 60分 (1時間)
    *   **Refresh Token**: 30日

### ③ UserInfo Endpoint (ユーザー情報取得)
**パス**: `GET または POST /oauth/userinfo`
取得したアクセストークン (`Bearer` トークン) を `Authorization` ヘッダーに付与し、ユーザーの情報（JSON）を取得します。

### ④ Revocation Endpoint (トークン無効化)
**パス**: `POST /oauth/revoke`
トークンを確実に破棄するためのエンドポイントです。ログアウト時などに利用します。

---

## 4. サポートされている Scopes (スコープ)

要求するスコープに応じて、ユーザーが許可しなければならない情報と、UserInfo で返却される情報の量が変わります。

1.  **`openid`**:
    *   OpenID Connectプロトコルを使用する際に必須のスコープです。
    *   このスコープが含まれる場合、`/oauth/token` エンドポイントで RSA(RS256) によって署名された **ID Token**（`id_token`）が発行されます。
2.  **`profile`**:
    *   基本プロフィール情報を取得します。
    *   返却項目：`name` (表示名), `preferred_username` (登録ユーザーID), `picture` (アイコンの絶対URL), `bio` (自己紹介)
3.  **`email`**:
    *   連携ユーザーの `email` (メールアドレス) を取得します。
4.  **`email:verified`**:
    *   `email_verified` (メールアドレスが有効化済みかどうかのブール値) を取得します。
5.  **`mfa_enabled`**:
    *   `mfa_enabled` (そのユーザーが現在2段階認証を有効にしているかどうかのブール値) を取得します。セキュリティ水準の高いアプリで役立ちます。

---

## 5. PKCE (Proof Key for Code Exchange) 対応

ネイティブアプリやSPA (シングルページアプリケーション) 向けに、クライアントシークレットを持たなくても安全にコードを引き換えられる **PKCE** をサポートしています。

*   **サポート方式**: `S256` (SHA-256) のみ。※脆弱な `plain` アルゴリズムによるリクエストはセキュリティ上の理由で拒否されます。
*   ディスカバリーの `code_challenge_methods_supported` は自動で検出されます。
*   Public Client (公開クライアント / クライアントシークレットが不明なアプリ) で OAuth を利用する場合、PKCEの利用は**必須**となります。

---

## 6. セキュリティと通知機能

*   **強固なセッションリセット**: ユーザーが外部アプリとの連携を「解除（Revoke）」した瞬間、そのアプリに関連するすべての有効な Access Token と Refresh Token は即座に無効化されます。
*   **通知システム**: 新しいサードパーティのアプリケーションと連携した場合、または新しい権限（スコープ）を付与した場合、アカウントの持ち主のメールアドレスに「新しいアプリと連携しました」という確認メールが送信され、不正な連携を防止します。
