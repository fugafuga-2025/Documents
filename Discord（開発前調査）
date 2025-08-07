# **Discord API 調査**

**✅ 実現可能性:できそう**

- DiscordのBot機能（[Botアカウント](https://discord.com/developers/docs/intro)）を利用して、
メッセージの取得 → Webhookで外部サーバーへ送信 → 他プラットフォームへ中継する構成は実現可能。
- ただし、Botをサーバーに招待する必要あり（LINEと同様）

# **🛠 Discordの提供機能概要**

**🤖 Bot（Botアカウント）**

- Botトークンを利用してAPI接続
- MESSAGE_CREATE イベントで以下を取得可能：
    - 発言者（author）
    - メッセージ内容（content）
    - チャンネルID / サーバーID

- 取得データを任意のサーバーへWebhookで送信 → 加工して他プラットフォームに転送可能

📚 [Botドキュメント（Discord公式）](https://discord.com/developers/docs/topics/gateway)

**🌐 Webhook（Outgoing Webhook）**

- Webhook URLを生成し、メッセージを外部サービスへ即時送信できる
- ただし、誰が話したかなどの詳細情報取得には不向き
- Botアカウントによるイベント取得が主流

📚 [Discord Webhookガイド](https://support.discord.com/hc/en-us/articles/228383668-Intro-to-Webhooks)

# **🔄 メッセージ同期フロー（ざっくり）**

1. DiscordでAが発言

2. BotがMESSAGE_CREATEイベントを検知

3. 発言内容＋発言者情報を取得

4. JSONに整形してWebhook POST

5. LINEやSlackで表示：「from A: こんにちは」

# **⚠️ 利用規約・運用上の注意**

**🚫 禁止・注意されている行為**

| **規約上の制限** | **内容** |
| --- | --- |
| ✅ スパム禁止 | 同一ユーザーへの過剰送信（Botが同じ文を連打） |
| ✅ 自動大量送信 | メッセージをループ処理で連投するような動作 |
| ✅ 不正アクセス | 正当なOAuth認証以外でのBot導入は禁止 |

📚 [Discord 開発者向け規約](https://discord.com/developers/docs/legal)

**🧠 レートリミット**

- Discord APIにはレート制限あり（通常やは1秒あたり5～10回程度）
- 超過すると**429エラー（Too Many Requests）**が返る
- 必ずバックオフ・再試行処理を実装すること

📚 [Rate Limits（公式）](https://discord.com/developers/docs/topics/rate-limits)

**🔄 アップデート対応**

- Discord APIはバージョン更新が頻繁（例：v9 → v10）
- 特に [Gateway Intents](https://discord.com/developers/docs/topics/gateway#gateway-intents) や MESSAGE CONTENT の取り扱いに変更がある可能性
- 常に最新のIntent設定・Scopeに注意すること

# **💸 料金について**

- Bot利用自体は無料
- ただし、大規模Botを常時稼働させる場合は以下の費用が必要：
    - サーバーインフラ費（VPS / GCP / AWSなど）
    - ストレージ（ログ保存・画像転送用など）
- 

# **👥 発言者の識別**

| **項目** | **Discord** |
| --- | --- |
| 誰が発言したか | ✅ 明確に取得可能（message.author.username） |
| ニックネーム | ✅ ギルドごとのニックネームも取得可（member.nickname） |
| ユーザーID | ✅ 固有の user_id を取得可能 |
| アバター | ✅ 表示URLを生成可能（CDN経由） |

# **🆚 LINEとの比較表（仕様面）**

| **比較項目** | **LINE** | **Discord** |
| --- | --- | --- |
| Bot導入方法 | グループに招待（要友達追加） | サーバーにBotを招待 |
| 発言者の取得 | 制限あり（匿名的なケースあり） | 明確に取得可能（user_id, username） |
| 通信制限 | 商用・大量送信に厳しい | レートリミットはあるが比較的自由 |
| API操作自由度 | 低め（Messaging APIのみ） | 高め（全イベント／編集／削除対応） |

# **🔧 使用可能ライブラリ（開発言語別）**
