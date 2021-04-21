---
keywords: Experience Platform；ホーム；人気のあるトピック；クエリサービス；クエリサービス；ポスティコ；ポスティコ；クエリサービスに接続；
solution: Experience Platform
title: Posticoをクエリサービスに接続
topic-legacy: connect
description: このドキュメントには、Adobe Experience Platformクエリサービス用のバックアップクライアントPosticoをインストールするためのリンクが含まれています。
exl-id: a19abfc8-b431-4e57-b44d-c6130041af4a
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '253'
ht-degree: 16%

---

# [!DNL Postico]をクエリサービス(Mac)に接続

このドキュメントでは、[!DNL Postico]をAdobe Experience Platform[!DNL Query Service]に接続する手順を説明します。

>[!NOTE]
>
> このガイドは、[!DNL Postico]へのアクセス権が既にあり、そのインターフェイスの操作方法に精通していることを前提としています。 [!DNL Postico]に関する詳細は、[正式な [!DNL Postico] ドキュメント](https://eggerapps.at/postico/docs)を参照してください。
> 
> さらに、[!DNL Postico]は&#x200B;****&#x200B;のみで、macOSデバイスで使用できます。

[!DNL Postico]をクエリサービスに接続するには、[!DNL Postico]を開いて&#x200B;**[!DNL New Favorite]**&#x200B;を選択します。

![](../images/clients/postico/open-postico.png)

現在は、値を入力してAdobe Experience Platformと接続できます。

データベース名、ホスト、ポート、ログイン資格情報の検索について詳しくは、[Platform の資格情報ページ](https://platform.adobe.com/query/configuration)を参照してください。資格情報を探すには、[!DNL Platform]にログインし、**[!UICONTROL クエリ]**&#x200B;を選択してから、**[!UICONTROL 資格情報]**&#x200B;を選択します。

資格情報を挿入した後、**[!DNL Connect]**&#x200B;を選択してクエリサービスに接続します。

![](../images/clients/postico/authentication-details.png)

プラットフォームに接続すると、クエリサービスとの関係がすべて、以前にリストされたことを確認できます。

![](../images/clients/postico/show-queries.png)

## SQL文の作成

新しいSQLクエリを作成するには、「SQLクエリ」を選択して開きます。

![](../images/clients/postico/create-query.png)

ボックスが表示され、ここから実行するクエリを入力できます。 終了したら、**[!DNL Execute Statement]**&#x200B;を選択してクエリを実行します。

![](../images/clients/postico/run-statement.png)

完了したクエリの実行結果を示すテーブルが表示されます。

![](../images/clients/postico/query-results.png)

## 次の手順

[!DNL Query Service]に接続したので、[!DNL Postico]を使ってクエリを書くことができます。 クエリの書き込みおよび実行方法について詳しくは、『[クエリ実行ガイド](../best-practices/writing-queries.md)』を参照してください。
