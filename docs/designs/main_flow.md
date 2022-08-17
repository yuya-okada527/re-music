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
  Browser ->> S3: 音声アップロード
  Browser ->>+ API: 切り取り要求送信
  Note left of API: S3パス、切り取り位置
  API ->> RDB: ステータス登録
  API ->> SQS: 切り取り要求をキューイング
  API ->>- Browser: レスポンス

  # 切り取り実行
  SQS ->> Lambda: 切り取り実行要求
  Lambda ->> S3: 音声ファイル取得
  Lambda ->> Lambda: 切り取り実行
  Lambda ->> S3: 切り取り済み音声ファイル保存
  Lambda ->> API: ステータス更新をリクエスト
  API ->> RDB: ステータス更新

  # 切り取りファイル再生
  Browser ->> API: ステータス参照
  Browser ->> S3: 切り取りファイル取得
  Browser ->> Browser: 切り取りファイル再生
```

## 要検討

- S3 へのアクセスは、Browser から直接ではなく API を経由した方が単純かもしれない
- ユーザ管理は、初期スコープに入れるか？