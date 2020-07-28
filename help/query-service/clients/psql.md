---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: PSQL との接続
topic: connect
translation-type: tm+mt
source-git-commit: 3b710e7a20975880376f7e434ea4d79c01fa0ce5
workflow-type: tm+mt
source-wordcount: '170'
ht-degree: 71%

---


# PSQL との接続

PSQL is a command-line interface that comes when you install [!DNL Postgres] on your machine. 次の手順に従ってインストールできます。

## Mac での Postgres のインストール

ターミナルウィンドウを開き、次の 3 つのコマンドを実行します。

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

## Install [!DNL Postgres] on a PC

Download and install [!DNL Postgres] from this [location](https://www.postgresql.org/download/windows/).

パス変数を編集します。

![画像](../images/clients/psql/path.png)

Add the two lines shown that include &quot;[!DNL Postgres].&quot;

更新を保存し、コマンドプロンプトを開いて、次のように入力します。

```shell
psql -V
```

例えば、以下が表示されます。

```shell
psql (PostgreSQL) 9.5.14
```

## PSQLの接続と [!DNL Query Service]

Return to the [!DNL Platform] UI on the *[!UICONTROL Connect BI Tools]* page.

Click **[!UICONTROL copy]** for *[!UICONTROL PSQL Command]*.

![画像](../images/clients/psql/connect-bi.png)

>[!IMPORTANT]：PC の場合は、テキストエディターを使用してコマンド文字列の改行を削除し、文字列をコピーします。

コマンド文字列をターミナルまたはコマンドウィンドウにペーストし、Enter キーを押します。

次のような結果が表示されます。

```shell
psql (10.5, server 0.1.0)
SSL connection (protocol: TLSv1.2, cipher: ECDHE-RSA-AES256-GCM-SHA384, bits: 256, compression: off)
Type "help" for help.
all=>
```

バージョン 10.5 以降が表示されない場合は、バージョン 10.5 以降をダウンロードする必要があります。