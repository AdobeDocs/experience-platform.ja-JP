---
keywords: Experience Platform；ホーム；人気のあるトピック；クエリサービス；クエリサービス；ポスティコ；ポスティコ；クエリサービスへの接続；
solution: Experience Platform
title: Posticoをクエリサービスに接続
topic-legacy: connect
description: このドキュメントには、Adobe Experience Platform Query Service用のバックアップクライアントPosticoをインストールするためのリンクが含まれています。
exl-id: a19abfc8-b431-4e57-b44d-c6130041af4a
source-git-commit: 910a38ccb556ec427584d9b522e29f6877d1c987
workflow-type: tm+mt
source-wordcount: '248'
ht-degree: 7%

---

# [!DNL Postico]をクエリサービス(Mac)に接続

このドキュメントでは、[!DNL Postico]をAdobe Experience Platform [!DNL Query Service]に接続する手順を説明します。

>[!NOTE]
>
> このガイドは、[!DNL Postico]へのアクセス権を既に持っており、インターフェイスの操作方法に精通していることを前提としています。 [!DNL Postico]に関する詳細は、[公式の [!DNL Postico] ドキュメント](https://eggerapps.at/postico/docs)を参照してください。
> 
> また、[!DNL Postico]は&#x200B;**のみ**&#x200B;で、macOSデバイスで使用できます。

[!DNL Postico]をクエリサービスに接続するには、[!DNL Postico]を開き、**[!DNL New Favorite]**&#x200B;を選択します。

![](../images/clients/postico/open-postico.png)

Adobe Experience Platformに接続する値を入力できるようになりました。

データベース名、ホスト、ポート、ログイン資格情報の検索について詳しくは、『[資格情報ガイド](../ui/credentials.md)』を参照してください。 資格情報を探すには、[!DNL Platform]にログインし、**[!UICONTROL クエリ]**&#x200B;を選択し、**[!UICONTROL 資格情報]**&#x200B;を選択します。

資格情報を挿入したら、**[!DNL Connect]**&#x200B;を選択してクエリサービスに接続します。

![](../images/clients/postico/authentication-details.png)

Platformに接続すると、以前にクエリサービスでおこなわれたすべての関係のリストを表示できます。

![](../images/clients/postico/show-queries.png)

## SQL文の作成

新しいSQLクエリを作成するには、「SQLクエリ」を選択して開きます。

![](../images/clients/postico/create-query.png)

ボックスが表示され、ここから実行するクエリを入力できます。 終了したら、**[!DNL Execute Statement]**&#x200B;を選択してクエリを実行します。

![](../images/clients/postico/run-statement.png)

完了したクエリの実行結果を示すテーブルが表示されます。

![](../images/clients/postico/query-results.png)

## 次の手順

[!DNL Query Service]と接続したので、[!DNL Postico]を使用してクエリを記述できます。 クエリの書き込みおよび実行方法について詳しくは、『[クエリ実行ガイド](../best-practices/writing-queries.md)』を参照してください。
