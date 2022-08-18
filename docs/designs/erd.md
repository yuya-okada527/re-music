# ER 図

## リレーション

```mermaid
erDiagram

songs ||--|| song_sounds: ""
songs ||--o{ phrases: ""
phrases ||--|| phrase_sounds: ""
phrases ||--o| phrase_states: ""
```
