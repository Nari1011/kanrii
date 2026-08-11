# カンリイ

![Built with Claude Code](https://img.shields.io/badge/Built%20with-Claude%20Code-orange)
![HTML](https://img.shields.io/badge/HTML-1%20file-e34f26)
![Offline](https://img.shields.io/badge/Offline-Service%20Worker-5a6fd8)
![Deploy](https://img.shields.io/badge/GitHub%20Pages-live-brightgreen)

用事ごとに持ち物リストを作って、出かける前にチェックするだけの **持ち物チェックアプリ**。
旅行・出張・ジムなど「毎回同じものを持っていく用事」を登録しておいて、次からはチェックを外して使い回します。

🔗 **[https://nari1011.github.io/kanrii/](https://nari1011.github.io/kanrii/)**

ファイルは `index.html` 1 枚。ビルドもアカウント登録も不要です。

---

## できること

### 📋 用事ごとにリストを分ける

左のサイドバーに「用事」を並べ、それぞれに持ち物リストを持たせます。

- 用事の追加・名前の編集・削除
- 各用事の進捗（`3 / 8 完了`）が一覧に出る
- 用事名は 20 文字まで

### ✅ チェックする

- タップでチェックの ON / OFF
- 進捗バーが伸び、**100% になると青→緑に変わる**
- **全てリセット** でチェックだけ一括で外す（持ち物は残る）
- 持ち物名は 40 文字まで

### ↕️ 並べ替え

持ち物はドラッグで順番を入れ替えられます。

- デスクトップ: 行をそのままドラッグ（HTML5 Drag & Drop）
- スマホ: 左端のハンドルを長押ししてドラッグ（挿入位置に青いラインが出る）

### 🌙 ダークモード

ヘッダー右のボタンで切り替え。選んだテーマは次回も保持されます。

### 📴 オフライン

Service Worker を登録しているため、**一度開けば電波が無くても使えます**。
HTML はネットワーク優先で取りに行くので、更新があれば即反映されます。

---

## スマホ向けの作り込み

出先で使うアプリなので、モバイルの挙動をかなり詰めてあります。

| 対応 | 内容 |
|---|---|
| 画面切り替え | 640px 未満では「用事一覧」と「持ち物リスト」を1画面ずつ表示し、← で戻る |
| iOS のズーム防止 | 入力欄を 16px 以上にし、`maximum-scale=1` を指定 |
| キーボード追従 | `visualViewport` を監視して、入力欄がキーボードの上に来るよう高さを調整 |
| セーフエリア | `env(safe-area-inset-*)` でノッチ・ホームバーと被らないように |
| タップ領域 | ボタン・チェックボックスをモバイルでは拡大（最小 64px 行） |
| スクロール | リスト部分だけスクロールし、ページ全体は動かさない（`overscroll-behavior`） |

---

## データについて

保存先は**ブラウザの `localStorage` だけ**です。サーバも通信もありません。

| キー | 内容 |
|---|---|
| `kanrii-data` | 用事と持ち物の一覧 |
| `kanrii-dark` | ダークモードの ON / OFF |

> ⚠️ ブラウザのデータを消すとリストも消えます。端末をまたいだ同期はできません。

---

## 開発

ビルド不要。ローカルで動かす場合は Service Worker のためにサーバ経由で開きます。

```sh
python3 -m http.server 8000    # → http://localhost:8000
```

`main` への push で GitHub Pages に自動公開されます。

---

## 構成

| ファイル | 役割 |
|---|---|
| `index.html` | HTML / CSS / JavaScript を全部含む本体 |
| `sw.js` | Service Worker。HTML はネットワーク優先、アセットはキャッシュ優先 |

外部依存は [lucide](https://lucide.dev/)（アイコン）を CDN から読み込むのみ。

---

## 開発について

このプロジェクトは **[Claude Code](https://claude.com/claude-code) のみで開発**しました。
設計・実装・スマホ対応の調整まで、すべて対話ベースで組み立てています。
