# typescript 5.9.3 / vite + react
TypeScriptのバージョン5.2.2の学習用に作ります。

## 前提条件
- MacOS / Windows環境
- Gitコマンド
- Docker for Desktop

## ミドルウェア
- Node 20.19
- Typescript 5.9.3
- React.js ^19.1.1
- Vite ^7.1.7

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
$ mkdir -p ~/workspace/typescript5.9.3-vite-react/ && cd ~/workspace/typescript5.9.3-vite-react
$ git clone https://github.com/reflet/typescript5.9.3-vite-react.git .
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
10.8.2

$ docker compose exec node npx tsc --version
Version 5.9.3
```

```bash
$ docker compose exec node npm list react

<出力結果>
├─ react-dom@19.2.0
└─ react@19.2.0
```

```bash
$ docker compose exec node npm list vite

<出力結果>
├─ vitejs/plugin-react@5.0.4
└─ vite@7.1.9
```

## 動作確認
開発用にReactを起動します。

```bash
$ docker compose exec node npm start
```

ブラウザでページを開いてみる。

```bash
$ open http://localhost:5173
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
create-vite@8.0.2
Ok to proceed? (y) y

◇  Current directory is not empty. Please choose how to proceed:
│  Remove existing files and continue
│
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
