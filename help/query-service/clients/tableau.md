---
keywords: Experience Platform；ホーム；人気の高いトピック；Tableau;Tableau;クエリサービス；クエリサービス；クエリサービスに接続；
solution: Experience Platform
title: Tableauをクエリサービスに接続
topic-legacy: connect
description: このドキュメントでは、TableauとAdobe Experience Platformクエリサービスを接続する手順について説明します。
exl-id: f380aacd-5091-41bc-97ca-593e0b1670fd
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '213'
ht-degree: 11%

---

# [!DNL Tableau]をクエリサービスに接続

このドキュメントでは、TableauとAdobe Experience Platform[!DNL Query Service]を接続する手順を説明します。

>[!NOTE]
>
> このガイドは、[!DNL Tableau]へのアクセス権が既にあり、そのインターフェイスの操作方法に精通していることを前提としています。 [!DNL Tableau]に関する詳細は、[正式な [!DNL Tableau] ドキュメント](https://help.tableau.com/current/pro/desktop/en-us/default.htm)を参照してください。

[!DNL Tableau]を[!DNL Query Service]に接続するには、[!DNL Tableau]を開き、**[!DNL To a Server]**&#x200B;セクションで&#x200B;**[!DNL More]**&#x200B;を選択し、次に&#x200B;**[!DNL PostgreSQL]**&#x200B;を選択します

![](../images/clients/tableau/open-connection.png)

現在は、値を入力してAdobe Experience Platformと接続できます。 データベース名、ホスト、ポート、ログイン資格情報の検索について詳しくは、[Platform の資格情報ページ](https://platform.adobe.com/query/configuration)を参照してください。資格情報を探すには、[!DNL Platform]にログインし、**[!UICONTROL クエリ]**&#x200B;を選択してから、**[!UICONTROL 資格情報]**&#x200B;を選択します。

接続を試みる前に、「**[!UICONTROL SSL Required]**」ボックスをオンにしていることを確認してください。

すべての資格情報を入力した後、**[!DNL Sign In]**&#x200B;を選択して続行します。

![](../images/clients/tableau/sign-in.png)

これでAdobe Experience Platformとつながり、テーブルのリストが横に表示されました。

![](../images/clients/tableau/connected.png)

## 次の手順

[!DNL Query Service]に接続したので、[!DNL Tableau]を使ってクエリを書くことができます。 クエリの書き込みと実行の方法の詳細については、[実行中のクエリ](../best-practices/writing-queries.md)のガイドを参照してください。
