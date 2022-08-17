# インセプションデッキ

## Why We Are Here?

ギター練習の際、練習したいフレーズを繰り返し再生することのできるアプリが欲しかったため。

## Elevator Pitch

ギター練習の効率を上げたい人向けの re-music というプロダクトは、楽器練習補助ツールである。このプロダクトでは、練習曲をフレーズごとに分割し、繰り返し再生することができる。また、簡単な操作が音声入力によって実行できるため、楽器の練習で手が埋まっていて操作できることを強みとする。

## Solution

まず、WEB の技術で実現できないか検討する。
WEB ベースで、音声の録音と編集、再生を実施する。

### 実現方法

- 音声録音
  - https://qiita.com/matsuyamasyou/items/73ff1936b8747dc4d539
- 音声再生
  - https://mdstage.com/html-css/html-senior/audio
- 音声編集
  - サーバサイドでなんとかする
    - JS
      - https://www.petitmonte.com/javascript/wave_cut.html

### 構成

- API

- モバイル
  - フレームワーク: flutter
