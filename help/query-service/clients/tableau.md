---
keywords: Experience Platform；ホーム；人気のあるトピック；tableau;Tableau；クエリサービス；クエリサービス；クエリサービス；クエリサービスへの接続；
solution: Experience Platform
title: Tableau のクエリーサービスへの接続
topic-legacy: connect
description: このドキュメントでは、Tableau とAdobe Experience Platform Query Service を接続する手順について説明します。
exl-id: f380aacd-5091-41bc-97ca-593e0b1670fd
source-git-commit: 910a38ccb556ec427584d9b522e29f6877d1c987
workflow-type: tm+mt
source-wordcount: '208'
ht-degree: 3%

---

# [!DNL Tableau] をクエリサービスに接続

このドキュメントでは、Tableau をAdobe Experience Platform [!DNL Query Service] に接続する手順を説明します。

>[!NOTE]
>
> このガイドは、既に [!DNL Tableau] にアクセスでき、インターフェイスの操作方法に精通していることを前提としています。 [!DNL Tableau] の詳細については、[ 公式の  [!DNL Tableau]  ドキュメント ](https://help.tableau.com/current/pro/desktop/en-us/default.htm) を参照してください。

[!DNL Tableau] を [!DNL Query Service] に接続するには、[!DNL Tableau] を開き、**[!DNL To a Server]** セクションで **[!DNL More]** を選択し、**[!DNL PostgreSQL]** を選択します。

![](../images/clients/tableau/open-connection.png)

Adobe Experience Platformに接続する値を入力できるようになりました。 データベース名、ホスト、ポート、ログイン資格情報の検索の詳細については、[ 資格情報ガイド ](../ui/credentials.md) を参照してください。 資格情報を探すには、[!DNL Platform] にログインし、**[!UICONTROL クエリ]** を選択してから、**[!UICONTROL 資格情報]** を選択します。

接続を試みる前に、「**[!UICONTROL SSL Required]**」ボックスをオンにしておく必要があります。

すべての資格情報を入力した後、**[!DNL Sign In]** を選択して続行します。

![](../images/clients/tableau/sign-in.png)

これで、Adobe Experience Platformに接続し、テーブルのリストが横に表示されます。

![](../images/clients/tableau/connected.png)

## 次の手順

[!DNL Query Service] に接続したら、[!DNL Tableau] を使用してクエリを記述できます。 クエリの書き込みと実行の詳細については、[ クエリの実行 ](../best-practices/writing-queries.md) に関するガイドを参照してください。
