---
keywords: Experience Platform；ホーム；人気のトピック；PSQL;psqlconnect to query service;Query service;query service;
solution: Experience Platform
title: PSQL のクエリサービスへの接続
description: PSQL は、お使いのマシンに PostgreSQL をインストールするときに提供されるコマンドラインインターフェイスです。 次の手順に従ってインストールできます。
exl-id: ceb07128-409e-42be-8143-0cf681d435de
source-git-commit: b48c24ac032cbf785a26a86b50a669d7fcae5d97
workflow-type: tm+mt
source-wordcount: '292'
ht-degree: 11%

---

# PSQL のクエリサービスへの接続

PSQL は、コンピューターに [!DNL PostgreSQL] をインストールするときにインストールされるコマンドラインインターフェイスです。 このドキュメントでは、PSQL とAdobe Experience Platform [!DNL Query Service] を接続する手順を説明します。

>[!NOTE]
>
> このガイドは、[!DNL PSQL] へのアクセス権を既に持ち、使用方法に精通していることを前提としています。 [!DNL PSQL] について詳しくは、[official [!DNL PSQL] documentation](https://www.postgresql.org/docs/current/app-psql.html) を参照してください。

お使いのコンピューターに PSQL をインストールしたら、PSQL をクエリサービスに接続する準備が整います。 [!DNL Experience Platform] UI に戻り、「**[!UICONTROL クエリ]**」、「**[!UICONTROL 資格情報]** の順に選択します。

「**[!UICONTROL PSQL コマンド]**」セクションで、**[!UICONTROL クリップボードにコピー]** アイコン（![ コピーアイコン ](/help/images/icons/copy.png)）を選択して、コマンド文字列をコピーします。

![ 「コピー」アイコンがハイライト表示されたクエリダッシュボードの「資格情報」タブ ](../images/clients/psql/connect-bi.png)

コマンド文字列をターミナルまたはコマンドラインウィンドウに貼り付けて、キーボードの **Enter** を押します。

>[!IMPORTANT]
>
>PC を使用している場合は、テキストエディターを使用してコマンド文字列の改行を削除し、文字列をコピーします。 バージョン 12.0 以降を使用している場合は、接続文字列に `PGGSSENCMODE=disable` を追加する必要があります。 さらに、有効期限のない資格情報を使用している場合は、「パスワード」フィールドを有効期限のない資格情報パスワードに置き換えてください。 有効期限のない資格情報について詳しくは、[ 資格情報ガイド ](../ui/credentials.md) を参照してください。

次のような結果が表示されます。

```shell
psql (10.5, server 0.1.0)
SSL connection (protocol: TLSv1.2, cipher: ECDHE-RSA-AES256-GCM-SHA384, bits: 256, compression: off)
Type "help" for help.
all=>
```

バージョン 10.5 以降が表示されない場合は、バージョン 10.5 以降をダウンロードする必要があります。

## 次の手順

[!DNL Query Service] に接続したので、PSQL を使用してクエリを記述できます。 クエリの作成および実行方法について詳しくは、[ クエリの実行 ](../best-practices/writing-queries.md) に関するガイドを参照してください。
