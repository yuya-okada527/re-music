# インタフェース定義

## API 一覧

- ~~練習曲取得 API~~ (一旦、必要なしだが多分その後必要)
  - GET /v1/songs/{song_id}
  - 練習曲を取得する
- 練習曲作成 API
  - POST /v1/songs
  - 音声ファイルを S3 にアップロードする
- 音声フレーズ作成 API
  - POST /v1/songs/{song_id}/phrases
  - 音声ファイルの切り取り要求を受け付ける
- 音声フレーズ取得 API
  - GET /v1/songs/{song_id}/phrases/{phrase_id}
  - 音声フレーズを取得する
- 音声フレーズ作成ステータス取得 API
  - GET /v1/songs/{song_id}/phrases/{phrase_id}/status
  - 音声フレーズ作成ステータスを取得する
- 音声フレーズ作成ステータス更新 API
  - PUT /v1/songs/{song_id}/phrases/{phrase_id}/status
  - 音声フレーズ作成ステータスを更新する

## API 詳細

### 練習曲作成 API

- Path: /v1/songs
- Method: POST
- Content-Type: multipart/form-data
- form-data:

```
data: xxx
name: song_name
```

### 音声フレーズ作成 API

- Path: /v1/songs/{song_id}/phrases
- Content-Type: application/json
- data:

```
{
  "name": "phrase_name",
  "start": 123,
  "end": 321
}
```
