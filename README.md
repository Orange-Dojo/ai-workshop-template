# 🛠️ AIプログラミングワークショップ

企画（ノンエンジニア）向けのプログラミング体験ワークショップ用テンプレートです。

## 🎯 ゴール

要求を考えたとき「それが開発上どの程度の改修が必要か」をざっくりイメージできるようになること。

## 🚀 始め方

1. 「**Use this template**」→「**Create a new repository**」
2. 作成したリポジトリで「**Create codespace on main**」
3. ターミナルに `✅ セットアップ完了！` と表示されるまで待つ
4. ターミナルで `python app.py`（WARNING が出ても無視してOK）
5. ブラウザで掲示板アプリが表示されます

### セットアップの確認（完了メッセージが出ない場合）

ターミナルで以下を1つずつ実行して確認してください：

```bash
flask --version          # Flask のバージョンが表示されればOK
ls posts.db              # ファイルが表示されればOK
claude --version         # バージョン番号が表示されればOK
```

表示されない場合は手動で再インストール：

```bash
pip install -r requirements.txt     # Flask の再インストール
python init_db.py                   # DB の再作成
npm install -g @anthropic-ai/claude-code  # Claude Code の再インストール
```

---

## 📁 ファイル構成と役割

| ファイル | 役割 | 変更すると何が変わるか |
|---------|------|---------------------|
| `app.py` | サーバー側の処理。URLごとの動作やデータベースの読み書きを定義 | データの保存・取得のルールが変わる（**Backend**） |
| `init_db.py` | データベースのテーブル構造を定義。起動時に自動実行される | 保存できる項目（カラム）が変わる（**DB**） |
| `posts.db` | データベースファイル。投稿データが保存されている | 直接は触らない（`app.py` 経由で操作する） |
| `templates/base.html` | 全ページ共通のHTML枠（ヘッダー、タイトルなど） | 全画面の共通部分の見た目が変わる（**UI**） |
| `templates/index.html` | 投稿一覧画面のHTML | 一覧の表示方法が変わる（**Frontend**） |
| `templates/create.html` | 新規投稿フォームのHTML | 入力フォームの内容が変わる（**Frontend**） |
| `templates/edit.html` | 編集フォームのHTML | 編集画面の内容が変わる（**Frontend**） |
| `static/style.css` | 色・フォント・余白などのデザイン設定 | 見た目のデザインが変わる（**UI**） |

### 要求と改修ファイルの関係

| 要求の例 | 変更するファイル | 改修レベル |
|---------|----------------|-----------|
| 「ボタンの色を変えて」 | `style.css` のみ | **UI**（小） |
| 「入力チェックを追加して」 | `templates/` | **Frontend**（中） |
| 「投稿にカテゴリ項目を追加して」 | `app.py` + `init_db.py` + `templates/` | **Backend + DB**（大） |

> **ポイント**: 変更するファイルの数が多いほど、改修の規模が大きい

---

## 🤖 Claude Code

ターミナルで `claude` と入力すると、AIコーディングアシスタントが起動します。

### Codespace 上での初回セットアップ

1. ターミナルで `claude` を実行
2. テーマ選択 → **Light** / **Dark** お好みで
3. 「Do you trust the files in this folder?」→ **Yes**
4. 認証方法の選択 → **「Use an API key」** を選択
5. 講師から共有された **API キー**（`sk-ant-` で始まる文字列）を貼り付けて Enter

> API キーは貼り付けても画面に表示されませんが、正常です。そのまま Enter を押してください。

2回目以降は `claude` だけで直接起動します。

### `/estimate` コマンド（改修見積もり）⭐

要求に対して **改修レベル・変更ファイル・理由** を見積もる専用コマンドです。  
実装はせず、見積もりだけを返します。

```
/estimate ボタンの色を赤に変えたい
→ 改修レベル: UI（小）、変更ファイル: style.css のみ

/estimate 投稿にカテゴリ項目を追加して
→ 改修レベル: Backend + DB（大）、変更ファイル: 5つ
```

見積もりに納得したら：
```
> OK、この内容で実装して
```

### 基本的な使い方

```
> このプロジェクトの構成を教えて
> 投稿一覧をカード形式のUIに変更して
> 投稿に「いいね」機能を追加して
> app.py の index 関数が何をしているか、初心者にもわかるように説明して
```

> ファイルを変更する際は確認プロンプトが表示されます。「Yes」で承認すると変更が適用されます。

### Plan モード（計画だけ立てる）

`Shift + Tab` で Plan モードに切り替え。実装前に「何をどう変えるか」を確認できます。

```
> 投稿に「いいね」機能を追加したい。まずは計画だけ立てて、実装はまだしないで
```

計画に納得したら：

```
> OK、この計画で実装して
```

### よく使う操作

| 操作 | 方法 |
|------|------|
| 起動 | `claude` |
| **改修見積もり** | `/estimate 要求内容` |
| Plan モード切替 | `Shift + Tab` |
| 変更を承認 | `Yes` または `y` |
| 変更を拒否 | `No` または `n` |
| 終了 | `/exit` または `Ctrl + C` |
| 会話リセット | `/clear` |
| ファイル指定 | `@ファイル名` |

---

## ⚠️ トラブルシューティング

| 症状 | 対処法 |
|------|--------|
| `ModuleNotFoundError: No module named 'flask'` | `pip install flask` を実行 |
| `OperationalError: no such table: posts` | `python init_db.py` を実行 |
| `Address already in use` | `Ctrl + C` で停止してから `python app.py` |
| `command not found: claude` | `npm install -g @anthropic-ai/claude-code` を実行 |
| `API key not found` | 講師から共有された API キーを再入力。`/login` で再認証できます |
| ポップアップが出ない | 画面下部「ポート」タブ → 5000番の地球アイコンをクリック |

---

## 🛑 終了時

ワークショップ終了後は Codespace を停止または削除してください。

- **停止**: 画面左下「Codespaces: xxxxxx」→「Stop Current Codespace」
- **削除**: https://github.com/codespaces →「…」→「Delete」
