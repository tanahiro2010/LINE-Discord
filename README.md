# Discord-LINE連携
このソフトウェアは、LINE Message APIとDiscordAPIを使用し、LINEからDiscordのチャット機能を使用できるようにしたものです<br>
どちらかのAPIアップデートにより、使用できなくなることがございます

## 概要
### 言語: Ruby
### 使用モジュール: Discord.rb sinatra line
### rootフォルダ: ./
### APIなどの情報ファイル: config.json

言語にRubyを指定した理由は、Discord.rbの処理が簡単にバックグラウンドでできるからです

## config.jsonの書き方
```json
{
  "server": {
    "host": "0.0.0.0",
    "port": 3333
  },
  "discord": {
    "token": "",
    "client_id": "",
    "template": {
      "guild": null,
      "channel": null,
      "message": "メッセージを受信しました\nGuild\nName: {guild_name}\nId: {guild_id}\nChannel\nName: {channel_name}\nId: {channel_id}\nAuthor\nName: {author_name}\nDisplayName: {author_display_name}\nId: {author_id}\nContent\b{content}"
    }
  },
  "line": {
    "channel_secret": "",
    "access_token": "",
    "send_user_id": ""
  }
}

```

これに必要な情報を書き込んでください<br>
`config['discord']['template']['message']`は、DiscordのBotがメッセージを受信したときにLINEに送信するときの送信用templateです<br>
変数は以下が使えます。また、すべて`{変数名}`と記述して使用してください<br>

| 変数名 | 内容 |  
| - | - |
| guild_name | 受信したサーバーの名前 |  |
| guild_id | 受信したサーバーのID |  |
| channel_name | 受信したチャンネルの名前 |  |
| channel_id | 受信したチャンネルのID |  |
| author_name | 送信者の名前 |  |
| author_display_name | 送信者のそのサーバーでの名前 |  |
| author_id | 送信者のID (メンション時に使用) |  |
| content | 受信したメッセージの内容 |  |

templateのguildとchannelは、bot起動時のメッセージ受信、送信チャンネルです<br>
両方ともint型でIDを入力してください

## 実行
実行するときは、以下のコマンドを実行してください
```bash
gem install sinatra
gem install discordrb

git clone "https://github.com/teamzisty/Discord-LINE-linkage/"
cd "Discord-Line-linkage"
ruby src/main.rb
```

## 仕組み
LINEからメッセージの送信イベントを受信したら、指定したDiscordのサーバーのチャンネルにメッセージを送信するプログラムです<br>
Discord.rbの方でメッセージ取得イベントが発火したらLINEのAPIに送信するという仕組みです

## LINEからのコマンド
| 変数名        | 内容                                                                     |  
|------------|------------------------------------------------------------------------|
| !ready     | botが起動しているか確認します                                                       |  |
| !info      | 現在指定しているサーバー情報、チャンネル情報を確認する                                            |  |
| !set       | メッセージ送信先サーバー、チャンネルを指定する                                                |  |
| !list      | 参加しているサーバーリストを表示する                                                     |  |
| !fetch     | サーバーIDからそのサーバーの名称、チャンネル一覧などを取得する                                       |  |
| !catch     | LINE Message APIの使用制限対策のため、メッセージの受信を一時的に制限する.もう一度このコマンドを使用することで制限を解除する |  |
