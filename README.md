# typescript 5.2.2 / react
TypeScriptのバージョン5.2.2の学習用に作ります。

## 前提条件
- MacOS / Windows環境
- Gitコマンド
- Docker for Desktop

## ミドルウェア
- Node 20.19
- Typescript 4.9.5
- React.js 19.2

## ファイル構成
```
┗ src
  ┣ js                ← tsファイルjsファイルにトランスパイルされる
  ┣ node_modules      ← npm installでインストールされるライブラリ
  ┣ src               ← Typescriptのコード
  ┣ package.json
  ┣ package-lock.json
  ┗ tsconfig.json     ← Typescriptの設定ファイル
```
## 準備
必要となるコードをリポジトリからclonseして配置します。

```bash
$ mkdir -p ~/workspace/typescript5.2.2-react/ && cd ~/workspace/typescript5.2.2-react
$ git clone https://github.com/reflet/typescript5.2.2-react.git .
```

## docker構築
dockerイメージを作成し、バージョン確認してみる。

```bash
# dockerイメージ作成
$ docker compose build
```

## サーバ起動
dockerコンテナを起動します。
```bash
$ docker compose up -d
```

## バージョン確認
```bash
$ docker compose exec node npm --version
9.5.1
```

```bash
$ docker compose exec node npx --version                     
9.5.1

$ docker compose exec node npx tsc --version
Version 4.9.5
```

```bash
$ docker compose exec node npm list react

<出力結果>
├─ react-dom@19.2.0
├─ react-scripts@5.0.1
└─ react@19.2.0
```

## 動作確認
開発用にReactを起動します。

```bash
$ docker compose exec node npm start
```

ブラウザでページを開いてみる。

```bash
$ open http://localhost:3000
```

## プロジェクト作成
プロジェクトを新規作成する場合は、既存ファイルをまず削除ください。
```bash
rm -rf ./src/*
```

Dockerfileの下記場所を一時的にコメントアウトします。
```Dockerfile
# Install application dependencies.
#RUN npm install

# start react.js.
# ※ docker環境の場合、--hostのオプションが必要となる
#CMD ["npm", "run", "dev", "--", "--host"]
```

compose.yamlの下記場所を一時的にコメントアウトします。
```yaml
    volumes:
      - ./src:/usr/src/app
      #- node_modules:/usr/src/app/node_modules/
```

その後、下記コマンドを実行することで、Reactのファイルが作成されます。
```bash
$ docker compose run --rm node npm create vite@latest . --template react

◇  Select a framework:
│  React
│
◇  Select a variant:
│  TypeScript
│
◇  Use rolldown-vite (Experimental)?:
│  No
│
◇  Install with npm and start now?
│  No
│
◇  Scaffolding project in /usr/src/app...
│
└  Done. Now run:

  npm install
  npm run dev
```

Dockerfileとcompose.yamlのコメントアウトを解除する。

dockerイメージを作成して、dockerコンテナを起動します。

```bash
$ docker compose build
$ docker compose up -d
```

以上
