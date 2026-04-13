---
id: gs
sidebar_position: 2
title: /gs コマンドリファレンス
sidebar_label: /gs
---

# /gs コマンドリファレンス

`/gs`（guardian settings）コマンド群は、ポイント・処罰・ブラックリスト等の詳細設定を管理します。  
すべてのコマンドは **サーバーオーナーのみ** 実行できます。

---

## ポイント設定

| コマンド | 説明 |
|---|---|
| `/gs violation-points` | 違反ポイント設定の一覧表示 |
| `/gs violation-points <タイプ> <回数> <ポイント>` | 指定タイプの N 回目違反のポイントを変更 |

- `<回数>` : 1〜10（何回目の違反か）
- `<ポイント>` : 1〜100

**違反タイプ一覧**:
`honeypot` / `spam` / `reaction` / `invite_spam` / `content_limit` / `link_blocked` / `keyword_blocked`

---

## 処罰ルール

| コマンド | 説明 |
|---|---|
| `/gs punishment-rules` | 処罰ルールの一覧表示 |
| `/gs punishment-rules <閾値> <タイプ> [duration]` | 処罰ルールを追加・変更 |

**処罰タイプ**: `timeout` / `kick` / `ban`  
`timeout` の場合、`duration` に分数を指定します。

---

## ポイント減衰

| コマンド | 説明 |
|---|---|
| `/gs decay` | 減衰設定の表示 |
| `/gs decay enabled:true <points_per_interval> [interval_hours]` | 減衰を有効化して設定 |
| `/gs decay enabled:false` | 減衰を無効化 |

- `points_per_interval` : 1回の減衰で減らすポイント数（1〜50）
- `interval_hours` : 減衰周期（時間、1〜720。省略時は 24 時間）

---

## ポイント手動操作

| コマンド | 説明 |
|---|---|
| `/gs add-points <ユーザー> <ポイント>` | ポイントを手動加算（処罰は発動しない）|
| `/gs remove-points <ユーザー> <ポイント>` | ポイントを手動減算 |

---

## ブラックリスト管理

### キーワードブラックリスト

| コマンド | 説明 |
|---|---|
| `/gs blacklist-keyword list` | 登録されているキーワードを一覧表示 |
| `/gs blacklist-keyword add <pattern> [is_regex]` | キーワードを追加（`is_regex=True` で正規表現）|
| `/gs blacklist-keyword remove <pattern>` | キーワードを削除 |

### ドメインブラックリスト

| コマンド | 説明 |
|---|---|
| `/gs blacklist-domain list` | 登録されているドメインを一覧表示 |
| `/gs blacklist-domain add <domain>` | ドメインを追加（`.exe` のような TLD 形式も可）|
| `/gs blacklist-domain remove <domain>` | ドメインを削除 |

---

## 通知プレフィックス

| コマンド | 説明 |
|---|---|
| `/gs notify-prefix` | 現在のプレフィックスを表示 |
| `/gs notify-prefix <text>` | チャンネル通知メッセージの冒頭テキストを設定 |
| `/gs notify-prefix` （引数なし実行で空送信）| プレフィックスを解除 |

---

## 閾値・検知設定

各検知機能の閾値（何回で検知するか）を個別に設定します。  
**すべてのコマンドは引数なしで実行すると現在の設定値を表示します。**

| コマンド | 説明 | 主な引数 |
|---|---|---|
| `/gs set-raid` | Anti-Raid 閾値設定 | `threshold`（人数）, `window`（秒）|
| `/gs set-invite` | 招待バースト検知 閾値設定 | `threshold`, `window` |
| `/gs set-spam` | 協調スパム閾値設定 | `cluster_size`, `window`, `similarity` |
| `/gs set-solo-spam` | 単独スパム閾値設定 | `threshold`, `window` |
| `/gs set-reaction` | リアクションスパム閾値設定 | `threshold`, `window` |
| `/gs set-webhook-spam` | Webhook スパム閾値設定 | `threshold`, `window` |
| `/gs set-bot-spam` | ボットスパム監視閾値設定 | `threshold`, `window` |
| `/gs set-nuke` | Nuke 保護閾値設定 | `threshold`, `window` |
| `/gs set-screening` | 最低アカウント年齢設定 | `min_age_hours` |

---

## コンテンツフィルタ詳細設定

| コマンド | 説明 |
|---|---|
| `/gs set-content` | メッセージの上限値（行数・文字数・メンション・添付数）を設定 |
| `/gs set-attachment` | 添付ファイルのサイズ・拡張子フィルタを設定 |
| `/gs set-link` | リンクフィルタの許可ドメイン管理 |
| `/gs set-keyword` | キーワードフィルタのマッチング設定 |
| `/gs content-action` | コンテンツフィルタ違反時のアクションモードを設定 |
| `/gs spam-action` | スパム検知違反時のアクションモードを設定 |
| `/gs trusted-role` | 信頼済みロールを管理（上限バイパス）|
| `/gs channel-override` | チャンネル別コンテンツフィルタ設定を上書き |

---

## ユーザーポイントの確認

個別ユーザーの現在のポイントを確認する専用コマンドは現時点では存在しません。  
`/gs add-points` または `/gs remove-points` を実行すると結果 Embed に**変更前後のポイント**が表示されます（`points` には 1〜100 の整数が必要です）。

違反発生時にはセキュリティログチャンネルにアラートが送信されるため、ポイント推移はログで追跡できます。

---

## 関連コマンドグループ

| グループ | 説明 | リファレンス |
|---|---|---|
| `/guardian` | セキュリティ全体のトグル・セットアップ | [/guardian コマンドリファレンス](guardian.md) |
| `/cf` | コンテンツフィルタの個別トグル | [/cf コマンドリファレンス](cf.md) |
| `/kb` | サーバーのバックアップ・復元 | [/kb コマンドリファレンス](kb.md) |

機能の依存関係（どのフラグをオンにすれば動くか）については [機能依存関係](../features/dependencies.md) を参照してください。
