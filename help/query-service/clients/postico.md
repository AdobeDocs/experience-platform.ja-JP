---
keywords: Experience Platform；ホーム；人気の高いトピック；クエリサービス；クエリサービス；postico;Postico；クエリサービスへの接続；
solution: Experience Platform
title: Postico をクエリサービスに接続
topic-legacy: connect
description: このドキュメントには、Adobe Experience Platform Query Service 用のバックアップクライアント Postico をインストールするためのリンクが含まれています。
exl-id: a19abfc8-b431-4e57-b44d-c6130041af4a
source-git-commit: 910a38ccb556ec427584d9b522e29f6877d1c987
workflow-type: tm+mt
source-wordcount: '248'
ht-degree: 7%

---

# 接続 [!DNL Postico] クエリサービス (Mac) へ

このドキュメントでは、 [!DNL Postico] Adobe Experience Platform [!DNL Query Service].

>[!NOTE]
>
> このガイドは、ユーザーが既に [!DNL Postico] インターフェイスの操作方法を熟知している。 詳細情報： [!DNL Postico] は [公式 [!DNL Postico] ドキュメント](https://eggerapps.at/postico/docs).
> 
> また、 [!DNL Postico] が **のみ** はmacOSデバイスで使用できます。

接続するには [!DNL Postico] クエリサービスにアクセスするには、 [!DNL Postico] を選択し、 **[!DNL New Favorite]**.

![](../images/clients/postico/open-postico.png)

これで、Adobe Experience Platformに接続する値を入力できるようになりました。

データベース名、ホスト、ポート、ログイン資格情報の検索の詳細については、 [資格情報ガイド](../ui/credentials.md). 資格情報を検索するには、にログインします。 [!DNL Platform]を選択し、「 **[!UICONTROL クエリ]**&#x200B;に続いて **[!UICONTROL 資格情報]**.

資格情報を挿入したら、「 」を選択します。 **[!DNL Connect]** をクリックして、クエリサービスに接続します。

![](../images/clients/postico/authentication-details.png)

Platform に接続すると、以前にクエリサービスでおこなわれたすべての関係のリストを表示できます。

![](../images/clients/postico/show-queries.png)

## SQL 文の作成

新しい SQL クエリを作成するには、「SQL クエリ」を選択して開きます。

![](../images/clients/postico/create-query.png)

ボックスが表示され、ここから、実行するクエリを入力できます。 終了したら、「 」を選択します。 **[!DNL Execute Statement]** をクリックしてクエリを実行します。

![](../images/clients/postico/run-statement.png)

完了したクエリの実行結果を示すテーブルが表示されます。

![](../images/clients/postico/query-results.png)

## 次の手順

これで、 [!DNL Query Service]を使用する場合、 [!DNL Postico] クエリを書き込みます。 クエリの書き込みおよび実行方法について詳しくは、『[クエリ実行ガイド](../best-practices/writing-queries.md)』を参照してください。
