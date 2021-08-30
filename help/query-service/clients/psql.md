---
keywords: Experience Platform；ホーム；人気のあるトピック；PSQL;psqlconnect to query service；クエリサービス；クエリサービス；
solution: Experience Platform
title: PSQLをクエリサービスに接続
topic-legacy: connect
description: PSQLは、PostgreSQLをマシンにインストールする際に提供されるコマンドラインインターフェイスです。 次の手順に従ってインストールできます。
exl-id: ceb07128-409e-42be-8143-0cf681d435de
source-git-commit: 910a38ccb556ec427584d9b522e29f6877d1c987
workflow-type: tm+mt
source-wordcount: '275'
ht-degree: 12%

---

# PSQLをクエリサービスに接続

PSQLは、[!DNL PostgreSQL]をコンピュータにインストールする際にインストールされるコマンドラインインターフェイスです。 このドキュメントでは、PSQLをAdobe Experience Platform [!DNL Query Service]に接続する手順を説明します。

>[!NOTE]
>
> このガイドは、[!DNL PSQL]へのアクセス権を既に持っており、使い方に精通していることを前提としています。 [!DNL PSQL]に関する詳細は、[公式の[!DNL PSQL]ドキュメント](https://www.postgresql.org/docs/current/app-psql.html)を参照してください。

PSQLをコンピューターにインストールした後、PSQLをクエリサービスに接続する準備が整いました。 [!DNL Platform] UIに戻り、「**[!UICONTROL クエリ]**」を選択し、次に「**[!UICONTROL 資格情報]**」を選択します。

![画像](../images/clients/psql/connect-bi.png)

**[!UICONTROL PSQL Command]**&#x200B;というラベルのセクションをコピーするアイコンを選択し、Enterキーを押す前に、ターミナルまたはコマンドラインウィンドウにコマンド文字列を貼り付けます。

>[!IMPORTANT]
>
>PCの場合は、テキストエディタを使用してコマンド文字列の改行を削除し、文字列をコピーします。 バージョン12.0以降を使用している場合は、接続文字列に`PGGSSENCMODE=disable`を追加する必要があります。 また、期限切れにならない資格情報を使用する場合は、「password」フィールドを期限切れにならない資格情報パスワードに置き換えます。 期限が切れない資格情報の詳細については、[資格情報ガイド](../ui/credentials.md)を参照してください。

次のような結果が表示されます。

```shell
psql (10.5, server 0.1.0)
SSL connection (protocol: TLSv1.2, cipher: ECDHE-RSA-AES256-GCM-SHA384, bits: 256, compression: off)
Type "help" for help.
all=>
```

バージョン 10.5 以降が表示されない場合は、バージョン 10.5 以降をダウンロードする必要があります。

## 次の手順

[!DNL Query Service]と接続したので、PSQLを使用してクエリを記述できます。 クエリの書き込みと実行の方法の詳細については、[クエリ](../best-practices/writing-queries.md)の実行に関するガイドを参照してください。
