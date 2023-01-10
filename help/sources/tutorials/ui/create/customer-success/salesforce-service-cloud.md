---
keywords: Experience Platform；ホーム；人気の高いトピック；Salesforce Service Cloud;Salesforce サービスクラウド
solution: Experience Platform
title: UI での Salesforce サービスクラウドソース接続の作成
type: Tutorial
description: Adobe Experience Platform UI を使用して Salesforce Service Cloud ソース接続を作成する方法を説明します。
exl-id: 38480a29-7852-46c6-bcea-5dc6bffdbd15
source-git-commit: ed92bdcd965dc13ab83649aad87eddf53f7afd60
workflow-type: tm+mt
source-wordcount: '482'
ht-degree: 38%

---

# UI での [!DNL Salesforce Service Cloud] ソース接続の作成

Adobe Experience Platform のソースコネクタには、外部ソースの データを設定したスケジュールに従って取り込む機能が用意されています。 このチュートリアルでは、 [!DNL Salesforce Service Cloud] （以下「SSC」という） [!DNL Platform] ユーザーインターフェイス。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)]  システム](../../../../../xdm/home.md)：[!DNL Experience Platform] が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に有効な SSC 接続がある場合は、このドキュメントの残りの部分をスキップし、次のドキュメントのチュートリアルに進んでください。 [データフローの設定](../../dataflow/customer-success.md)

### 必要な資格情報の収集

で SSC アカウントにアクセスするには [!DNL Platform]に値を指定する場合は、次の値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `username` | ユーザーアカウントのユーザー名です。 |
| `password` | ユーザーアカウントのパスワードです。 |
| `securityToken` | ユーザーアカウントのセキュリティトークン。 |

の導入について詳しくは、 [この [!DNL Salesforce Service Cloud] 文書](https://developer.salesforce.com/docs/atlas.en-us.api_iot.meta/api_iot/qs_auth_access_token.htm).

## [!DNL Salesforce Service Cloud] アカウントを接続

必要な資格情報を収集したら、次の手順に従って SSC アカウントをにリンクできます。 [!DNL Platform].

にログインします。 [Adobe Experience Platform](https://platform.adobe.com) 次に、 **[!UICONTROL ソース]** 左側のナビゲーションバーから **[!UICONTROL ソース]** ワークスペース。 **[!UICONTROL カタログ]**&#x200B;画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

以下 **[!UICONTROL 顧客の成功]** カテゴリ、選択 **[!UICONTROL Salesforce Service Cloud]**. このコネクタを初めて使用する場合は、「 **[!UICONTROL 設定]**. それ以外の場合は、「 **[!UICONTROL データを追加]** 新しい SSC コネクタを作成します。

![カタログ](../../../../images/tutorials/create/ssc/catalog.png)

この **[!UICONTROL Salesforce Service Cloud に接続]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 新しいアカウント

新しい資格情報を使用している場合は、「**[!UICONTROL 新しいアカウント]**」を選択します。表示される入力フォームで、名前、説明（オプション）、SSC 資格情報を入力します。 終了したら、「 」を選択します。 **[!UICONTROL 接続]** その後、新しい接続が確立されるまでしばらく時間をかけます。

![接続](../../../../images/tutorials/create/ssc/connect.png)

### 既存のアカウント

既存のアカウントに接続するには、接続する SSC アカウントを選択し、 **[!UICONTROL 次へ]** をクリックして続行します。

![既存](../../../../images/tutorials/create/ssc/existing.png)

## 次の手順

このチュートリアルに従って、SSC アカウントへの接続を確立しました。 次のチュートリアルに進み、 [顧客成功データをに取り込むためのデータフローの設定 [!DNL Platform]](../../dataflow/customer-success.md).
