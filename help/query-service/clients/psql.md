---
keywords: Experience Platform；ホーム；人気の高いトピック；PSQL;クエリサービスへのpsqlconnect;クエリサービス；クエリサービス；
solution: Experience Platform
title: PSQLをクエリサービスに接続
topic-legacy: connect
description: PSQLは、PostgreSQLをマシンにインストールする際に提供されるコマンドラインインターフェイスです。 次の手順に従ってインストールできます。
exl-id: ceb07128-409e-42be-8143-0cf681d435de
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '247'
ht-degree: 13%

---

# PSQLをクエリサービスに接続

PSQLは、[!DNL PostgreSQL]をマシンにインストールするときにインストールされるコマンドラインインターフェイスです。 このドキュメントでは、PSQLをAdobe Experience Platform[!DNL Query Service]と接続する手順を説明します。

>[!NOTE]
>
> このガイドは、[!DNL PSQL]へのアクセス権が既にあり、その使い方に精通していることを前提としています。 [!DNL PSQL]について詳しくは、[official [!DNL PSQL] documentation](https://www.postgresql.org/docs/current/app-psql.html.

PSQLをコンピューターにインストールした後、PSQLをクエリサービスに接続する準備が整いました。 [!DNL Platform] UIに戻り、**[!UICONTROL クエリ]**&#x200B;を選択し、次に&#x200B;**[!UICONTROL 資格情報]**&#x200B;を選択します。

![画像](../images/clients/psql/connect-bi.png)

**[!UICONTROL PSQL Command]**&#x200B;というラベルの付いたセクションをコピーするアイコンを選択し、コマンド文字列を端末またはコマンドラインウィンドウに貼り付けてから、enterキーを押します。

>[!IMPORTANT]
>
>PCを使用している場合は、テキストエディタを使用してコマンド文字列の改行を削除し、文字列をコピーします。 さらに、バージョン12.0以降を使用している場合は、接続文字列に`PGGSSENCMODE=disable`を追加する必要があります。

次のような結果が表示されます。

```shell
psql (10.5, server 0.1.0)
SSL connection (protocol: TLSv1.2, cipher: ECDHE-RSA-AES256-GCM-SHA384, bits: 256, compression: off)
Type "help" for help.
all=>
```

バージョン 10.5 以降が表示されない場合は、バージョン 10.5 以降をダウンロードする必要があります。

## 次の手順

[!DNL Query Service]に接続したので、PSQLを使ってクエリを書くことができます。 クエリの書き込みと実行の方法の詳細については、[実行中のクエリ](../best-practices/writing-queries.md)のガイドを参照してください。
