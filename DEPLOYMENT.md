# GitHub Pages デプロイメント手順

このドキュメントでは、HTMLプロジェクトをGitHub Pagesで公開するための手順を説明します。

## 前提条件

- Git がインストールされていること
- GitHub CLI (`gh`) がインストールされ、認証済みであること
- HTMLファイルが準備できていること

## デプロイメント手順

### 1. Gitリポジトリの初期化

プロジェクトディレクトリに移動して、Gitリポジトリを初期化します：

```bash
cd /path/to/your/project
git init
```

### 2. ファイルの準備

GitHub Pagesで公開するメインファイルを `index.html` という名前にします：

```bash
# もし別名のHTMLファイルがある場合はリネーム
mv your-file.html index.html
```

### 3. 初回コミット

すべてのファイルをステージングしてコミット：

```bash
git add -A
git commit -m "Initial commit - プロジェクトの説明"
```

### 4. GitHubリポジトリの作成とプッシュ

GitHub CLIを使用してリポジトリを作成し、コードをプッシュ：

```bash
gh repo create your-repo-name --public --source=. --push
```

オプション説明：
- `your-repo-name`: リポジトリ名
- `--public`: 公開リポジトリとして作成
- `--source=.`: 現在のディレクトリをソースとして使用
- `--push`: 作成後自動的にプッシュ

### 5. GitHub Pages の有効化

GitHub APIを使用してPages機能を有効化：

```bash
# mainブランチを使用する場合
gh api repos/YOUR_USERNAME/your-repo-name/pages \
  --method POST \
  --field source[branch]=main \
  --field source[path]=/

# masterブランチを使用する場合
gh api repos/YOUR_USERNAME/your-repo-name/pages \
  --method POST \
  --field source[branch]=master \
  --field source[path]=/
```

### 6. デプロイメント確認

ビルド状況を確認：

```bash
gh api repos/YOUR_USERNAME/your-repo-name/pages/builds/latest
```

## 公開URL

GitHub Pagesが有効になると、以下のURLでアクセス可能になります：

```
https://YOUR_USERNAME.github.io/your-repo-name/
```

**注意**: 初回デプロイメントには数分かかることがあります。

## よくある問題と解決策

### 404エラーが表示される場合

1. **ビルドが完了していない可能性**
   - 初回デプロイには最大10分程度かかることがあります
   - ビルド状況を確認してください

2. **ブランチ名の問題**
   - GitHubの新規リポジトリは `main` がデフォルトブランチです
   - 古いリポジトリは `master` を使用している場合があります
   - 正しいブランチ名を確認してください

3. **index.htmlが存在しない**
   - メインファイルが `index.html` という名前であることを確認
   - ルートディレクトリに配置されていることを確認

### ブランチをmainに変更する場合

```bash
# ローカルブランチ名を変更
git branch -m master main

# リモートにプッシュ
git push origin main

# デフォルトブランチを変更
gh api repos/YOUR_USERNAME/your-repo-name \
  --method PATCH \
  --field default_branch=main

# Pages設定を更新
gh api repos/YOUR_USERNAME/your-repo-name/pages --method DELETE
gh api repos/YOUR_USERNAME/your-repo-name/pages \
  --method POST \
  --field source[branch]=main \
  --field source[path]=/
```

## 更新時の手順

コンテンツを更新した場合：

```bash
git add -A
git commit -m "Update: 変更内容の説明"
git push
```

GitHub Pagesは自動的に更新をデプロイします（通常1-2分以内）。

## このプロジェクトの実際の設定

- **リポジトリ名**: tap-counter-grid
- **公開URL**: https://muumuu8181.github.io/tap-counter-grid/
- **ブランチ**: main
- **パス**: / (ルートディレクトリ)

---

作成日: 2024-09-24