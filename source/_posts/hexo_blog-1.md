---
title: hexoでブログ作成 その１ ～セットアップ編～
date: 2023-08-17 02:12:31
tags: 
    - blog
    - hexo
---
hexoプロジェクト作成～localでの起動まで.

## 初めに
このブログを hexo ＆ github pages で作ったのでその記録。
今回はhexoプロジェクトフォルダを立ち上げるところまで。

---
## 環境
- OS: dockerコンテナ (amazonlinux:2)
- Node: 16.x
- hexo: 6.3.0 (実施時に指定なしでインストールされたもの)

---
## 結論
以下のコマンドを実行する
```bash
# Nodeのインストール
curl -fsSL https://rpm.nodesource.com/setup_16.x | bash -
yum install -y nodejs

# hexoのインストール
npm install -g hexo-cli

# プロジェクトの作成
cd /opt
npx hexo init blog
# ローカル起動
cd blog
# npx hexo clean
npx hexo generate
npx hexo server
```

---
## 詳細
### インストール
hexoはNodeで動いているらしいのでNodeもインストールする。
```bash
# nodeのインストール
curl -fsSL https://rpm.nodesource.com/setup_16.x | bash -
yum install -y nodejs

# hexoのインストール
npm install -g hexo-cli
```
本当は18.xを使いたかったけど amazonlinux:2 上ではうまくインストールできなかったので16.xを使うことに...
詳しくは[このブログ](https://itneko.com/amazon-linux2-nodejs18/)を参照。

### プロジェクトの作成
適当なフォルダで`init`すると初期プロジェクトフォルダを構築してくれる。
```bash
npx hexo init blog
```
以下のような構成でフォルダが作成される。
※sourceフォルダだけ少し深く出力してます。
```
blog
|-- _config.landscape.yml
|-- _config.yml
|-- node_modules
|-- package-lock.json
|-- package.json
|-- scaffolds
|-- source
|   `-- _posts
|       `-- hello-world.md
`-- themes
```
source > _posts 配下に`hello-world.md`というのがあるが、これがサンプルブログファイル。
こいつは削除して、ブログ記事毎にここにmdファイルを追加していくイメージ。

とりあえずは何もせずに先に進む。

### ローカル起動
プロジェクトフォルダに移動してローカルサーバーを起動。
```bash
cd blog
npx hexo generate
npx hexo server
```
- `generate`でマークダウンファイルから静的なhtmlやらcssやらを作成する。
コマンドの実行が完了すると`public`というフォルダが作成されており、その中に静的コンテンツが配置されている。
- `server`で作成した静的コンテンツにアクセスする動作確認用サーバーを起動してくれる。

以下のようなログが出るので、そこのURLからアクセスしてみると...ブログが起動してる！
```
INFO  Hexo is running at http://localhost:4000/blog/ . Press Ctrl+C to stop.
```

デフォルトポートは`4000`だが、オプションで変更可能。
```bash
npx hexo server -p 4000
```
コンテナで実行している場合はホスト側とポートマッピングしておかないとアクセスできない点に注意。

---
## 所感
ぶっちゃけ、公式チュートリアルの一部を焼きなおしただけみたいな感じになっちゃったな。
ひとまずは自分が思い出せるようにしておくのが第一の目的だし、まあいいか。
しかしいい加減ブログタイトルくらいは変えないとな...

---
## 参考URL
https://hexo.io/docs/
https://itneko.com/amazon-linux2-nodejs18/

<!-- おわり -->