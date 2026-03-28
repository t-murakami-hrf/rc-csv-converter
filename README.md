# RC応募者CSV変換ツール

ビズリーチ・マイナビ等の求人媒体からエクスポートしたCSVを、RecruitingCloud（RC）の応募者インポート形式（`oubosya.csv`）に変換し、ZIP出力するブラウザツール。

## 特徴

- **ブラウザ完結** — サーバーにデータが送信されない（個人情報安全）
- **Shift_JIS / UTF-8 自動判定** — 媒体CSVの文字コードを気にしなくてOK
- **自動カラムマッピング** — ヘッダー名からRC項目へ自動対応
- **簡易アクセス制御** — パスフレーズを知っている人だけ利用可能
- **GitHub Pages** — pushで即反映、URL共有で展開完了

## セットアップ

### 1. リポジトリ作成

```bash
# 社内GitHubで新規リポジトリを作成後
git clone <your-repo-url>
cd rc-csv-converter
```

### 2. このフォルダの中身をコピー

```
rc-csv-converter/
├── docs/
│   └── index.html    ← ツール本体（GitHub Pagesで公開）
├── README.md
└── CHANGELOG.md
```

### 3. GitHub Pages を有効化

1. リポジトリの Settings → Pages
2. Source: **Deploy from a branch**
3. Branch: `main` / `/docs`
4. Save

### 4. URLを共有

`https://<org>.github.io/rc-csv-converter/`

## アクセス制御

初回アクセス時にパスフレーズ入力画面が表示されます。

**デフォルトパスフレーズ:** `rcimport2026`

変更する場合は `docs/index.html` 内の以下の行を編集:

```javascript
const ACCESS_PHRASE = 'rcimport2026';
```

> ※ フロントエンドのみの簡易認証です。機密レベルの高いデータを扱う場合は社内VPNとの併用を推奨。

## 更新方法

```bash
# index.html を編集
git add docs/index.html
git commit -m "v1.1: マイナビ中途のプリセット追加"
git push origin main
# → 数分でGitHub Pages に反映
```

## 媒体プリセットの追加方法

`docs/index.html` 内の `MEDIA_PRESETS` オブジェクトに追加:

```javascript
MEDIA_PRESETS['新媒体名'] = {
  last_name: '氏名の列名',
  first_name: '名の列名',
  phone: '電話番号の列名',
  // ...
};
```

## セキュリティ

- 全処理がブラウザ内完結（サーバー送信なし）
- 数式インジェクション防止（=, +, -, @ で始まるセルを無害化）
- 必須項目バリデーション
- パスフレーズによる簡易アクセス制御
