---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: PSQLとの接続
topic: connect
translation-type: tm+mt
source-git-commit: 8310204071375a55329f661c9ac678f96979a594

---


# PSQLとの接続

PSQLは、Postgresをマシンにインストールする際に提供されるコマンドラインインターフェイスです。 次の手順に従ってインストールできます。

## MacでのPostgresのインストール

ターミナルウィンドウを開き、次の3つのコマンドを実行します。

```shell
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

```shell
brew install postgres
```

```shell
which psql
```

これらのコマンドを発行すると、次の内容が表示されます。

```shell
/usr/local/bin/psql
```

## PCへのPostgresのインストール

この場所からPostgresをダウンロードしてインスト [ールしま](https://www.postgresql.org/download/windows/)す。

パス変数の編集：

![画像](../images/clients/psql/path.png)

「追加Postgres」を含む2行が表示されています。

更新を保存し、コマンドプロンプトを開いて、次のように入力します。

```shell
psql -V
```

次のように表示されます。

```shell
psql (PostgreSQL) 9.5.14
```

## PSQLとクエリサービス

「Connect BIツール」ページのプラットフォームUIに戻ります。

「PSQLコ **マンド** 」の「copy」をクリックします。

![画像](../images/clients/psql/connect-bi.png)

> [!IMPORTANT]:PCを使用している場合は、テキストエディタを使用してコマンド文字列の改行を削除し、文字列をコピーします。

コマンド文字列をターミナルまたはコマンドウィンドウに貼り付け、Enterキーを押します。

次のような結果が表示されます。

```shell
psql (10.5, server 0.1.0)
SSL connection (protocol: TLSv1.2, cipher: ECDHE-RSA-AES256-GCM-SHA384, bits: 256, compression: off)
Type "help" for help.
all=>
```

バージョン10.5が表示されない場合は、そのバージョン以降をダウンロードする必要があります。