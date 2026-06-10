# FastOn GitHub Pages サイト

このディレクトリは https://naname0109.github.io/faston/ にデプロイされる静的サイトです。
**Apple 審査でプライバシー/利用規約 URL が 404 だと Guideline 5.1.1 でリジェクトされる**ため、提出前に必ずデプロイすること。

## ファイル

- `index.html` — ランディング
- `privacy.html` — プライバシーポリシー（メタデータの `privacy_url` から参照）
- `terms.html` — 利用規約
- `share/index.html` — 共有リストビューア（Firestore Web SDK で取得）

## デプロイ手順

リポジトリ名は `faston`（小文字）であること（`constants.dart:shareBaseUrl` と一致が必要）。

### 1. リポジトリ作成（未作成なら）

```bash
gh repo create naname0109/faston --public --description "FastOn GitHub Pages site"
```

### 2. share/index.html の Firebase 設定を埋める

Firebase Console → プロジェクト設定 → アプリ → **ウェブアプリを追加**（iOS アプリと別途）
→ 表示された `firebaseConfig` の値（apiKey、authDomain、projectId 等）を `share/index.html` の同名フィールドに貼り付け。

```js
const firebaseConfig = {
  apiKey: "AIza...",
  authDomain: "your-project.firebaseapp.com",
  projectId: "your-project",
  storageBucket: "your-project.appspot.com",
  messagingSenderId: "123456",
  appId: "1:123456:web:abcdef",
};
```

> Firebase の web apiKey は **公開して問題ない値**です（セキュリティは Firestore ルールで担保）。

### 3. デプロイ

```bash
# このディレクトリの中身を gh-pages ブランチに push
cd docs/site
git init
git remote add origin git@github.com:naname0109/faston.git
git checkout -b gh-pages
git add -A
git commit -m "Initial GitHub Pages site"
git push -u origin gh-pages
```

GitHub リポジトリの **Settings → Pages** で:
- Source: `Deploy from a branch`
- Branch: `gh-pages` / `(root)`

数分後に `https://naname0109.github.io/faston/` で確認。

### 4. 動作確認

```bash
curl -I https://naname0109.github.io/faston/
curl -I https://naname0109.github.io/faston/privacy.html
curl -I https://naname0109.github.io/faston/terms.html
curl -I https://naname0109.github.io/faston/share/   # 200 (共有 ID なしでも 200 を返す)
```

すべて 200 になれば Apple 審査 OK。
