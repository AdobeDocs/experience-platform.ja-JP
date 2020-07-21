---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: PSQLとの接続
topic: connect
translation-type: tm+mt
source-git-commit: 3b710e7a20975880376f7e434ea4d79c01fa0ce5
workflow-type: tm+mt
source-wordcount: '170'
ht-degree: 1%

---


# PSQLとの接続

PSQLは、をマシンにインストールする際に提供されるコマンドラインインターフェイス [!DNL Postgres] です。 以下の手順に従って、インストールできます。

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

これらのコマンドを発行すると、次のように表示されます。

```shell
/usr/local/bin/psql
```

## PC [!DNL Postgres] にインストールする

この [!DNL Postgres] 場所からダウンロードしてインストール [します](https://www.postgresql.org/download/windows/)。

パス変数を編集します。

![画像](../images/clients/psql/path.png)

表示さ追加れている2つの行には「[!DNL Postgres]」が含まれます。

更新を保存し、コマンドプロンプトを開いて次のように入力します。

```shell
psql -V
```

次のように表示されます。

```shell
psql (PostgreSQL) 9.5.14
```

## PSQLの接続と [!DNL Query Service]

[ [!DNL Platform] Connect BI Tools ** ]ページのUIに戻ります。

「 **[!UICONTROL PSQLコマンドの]** コピー *[!UICONTROL 」をクリックし]*&#x200B;ます。

![画像](../images/clients/psql/connect-bi.png)

>[!IMPORTANT]: PCを使用している場合は、テキストエディタを使用してコマンド文字列の改行を削除し、文字列をコピーします。

コマンド文字列をターミナルまたはコマンドウィンドウに貼り付け、Enterキーを押します。

次のような結果になります。

```shell
psql (10.5, server 0.1.0)
SSL connection (protocol: TLSv1.2, cipher: ECDHE-RSA-AES256-GCM-SHA384, bits: 256, compression: off)
Type "help" for help.
all=>
```

バージョン10.5が表示されない場合は、そのバージョン以降をダウンロードする必要があります。