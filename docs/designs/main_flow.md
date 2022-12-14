# メインフロー

音声ファイルの録音->トリミング->再生までのメインの処理フローを設計する。

```mermaid
sequenceDiagram
  autonumber
  participant Browser
  participant API
  participant S3
  participant RDB
  participant SQS
  participant Lambda

  # 切り取り要求
  Browser ->> API: 音声ファイルアップロード(練習曲作成 API)
  API ->> S3: 音声ファイル保存
  Browser ->>+ API: 切り取り要求送信(音声フレーズ作成 API)
  Note left of API: 切り取り位置
  API ->> RDB: ステータス登録
  API ->> SQS: 切り取り要求をキューイング
  API ->>- Browser: レスポンス

  # 切り取り実行
  SQS ->> Lambda: 切り取り実行要求
  Lambda ->> S3: 音声ファイル取得
  Lambda ->> Lambda: 切り取り実行
  Lambda ->> S3: 切り取り済み音声ファイル保存
  Lambda ->> API: ステータス更新をリクエスト(音声フレーズ作成ステータス更新 API)
  API ->> RDB: ステータス更新

  # 切り取りファイル再生
  Browser ->> API: ステータス参照(音声フレーズ作成ステータス取得 API)
  Browser ->> API: 切り取りファイル取得(音声フレーズ取得 API)
  API ->> S3: 切り取りファイル取得
  Browser ->> Browser: 切り取りファイル再生
```

## 認証

1st リリースでは、ユーザベースの認証認可機能は作らない。最低限の Basic 認証のみでリリースする。

### 処理

- API へのリクエストでは、固定 Token のヘッダー情報をもとに認可する
- フロント側では、ログイン済みかのステータスを管理し、ログインしていない場合、認証用のポップアップを表示して、Basic 認証によるチェックを実行する

## 要検討

- S3 へのアクセスは、Browser から直接ではなく API を経由した方が単純かもしれない
  - 将来的に、モバイルアプリとかを考えると API 側に諸々寄せておいた方が都合が良さそう
  - ただ、大きな静的ファイルをサーバ内で処理するのは、負荷的な面で心配
    - 初期スコープでは、API を経由する方に寄せる
- ユーザ管理は、初期スコープに入れるか？
