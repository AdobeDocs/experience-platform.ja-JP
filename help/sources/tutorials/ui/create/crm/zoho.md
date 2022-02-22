---
keywords: Experience Platform；ホーム；人気のトピック；Zoho CRM;Zoho crm;Zoho;zoho
solution: Experience Platform
title: UI での Zoho CRM ソース接続の作成
topic-legacy: overview
type: Tutorial
description: Adobe Experience Platform UI を使用して Zoho CRM ソース接続を作成する方法を説明します。
exl-id: c648fc3e-beea-4030-8d36-dd8a7e2c281e
source-git-commit: 46b2fd6bc715bf1d8ccfeed576a2a2d193f92edd
workflow-type: tm+mt
source-wordcount: '551'
ht-degree: 11%

---

# の作成 [!DNL Zoho CRM] UI のソース接続

Adobe Experience Platformのソースコネクタは、外部ソースの CRM データをスケジュールに従って取り込む機能を提供します。 このチュートリアルでは、 [!DNL Zoho CRM] を使用したソースコネクタ [!DNL Platform] ユーザーインターフェイス。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md):標準化されたフレームワーク [!DNL Experience Platform] は顧客体験データを整理します。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md):スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に有効な [!DNL Zoho CRM] アカウントを使用する場合は、このドキュメントの残りの部分をスキップし、次のチュートリアルに進んでください： [データフローの設定](../../dataflow/crm.md).

### 必要な資格情報の収集

接続するには [!DNL Zoho CRM] を Platform に対して、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| --- | --- |
| エンドポイント | のエンドポイント [!DNL Zoho CRM] リクエスト先のサーバー。 |
| アカウント URL | アカウント URL は、アクセスおよび更新トークンの生成に使用されます。 URL は、ドメイン固有である必要があります。 |
| クライアント ID | に対応するクライアント ID [!DNL Zoho CRM] ユーザーアカウント。 |
| クライアント秘密鍵 | に対応するクライアントの秘密鍵 [!DNL Zoho CRM] ユーザーアカウント。 |
| アクセストークン | アクセストークンは、 [!DNL Zoho CRM] アカウント |
| 更新トークン | 更新トークンは、アクセストークンの有効期限が切れた後に、新しいアクセストークンの生成に使用されるトークンです。 |

これらの資格情報について詳しくは、 [[!DNL Zoho CRM] 認証](https://www.zoho.com/crm/developer/docs/api/v2/oauth-overview.html).

## 接続 [!DNL Zoho CRM] アカウント

必要な資格情報を収集したら、次の手順に従って、 [!DNL Zoho CRM] アカウント [!DNL Platform].

Platform UI で、「 **[!UICONTROL ソース]** 左側のナビゲーションバーから [!UICONTROL ソース] ワークスペース。 この [!UICONTROL カタログ] 画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、使用する特定のソースを検索オプションを使用して探すこともできます。

以下 [!UICONTROL CRM] カテゴリ、選択 **[!UICONTROL Zoho CRM]**&#x200B;を選択し、 **[!UICONTROL データを追加]**.

![カタログ](../../../../images/tutorials/create/zoho/catalog.png)

この **[!UICONTROL Zoho CRM アカウントを接続]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 既存のアカウント

既存のアカウントを使用するには、 [!DNL Zoho CRM] 新しいデータフローを作成するアカウントを選択し、 **[!UICONTROL 次へ]** をクリックして続行します。

![既存](../../../../images/tutorials/create/zoho/existing.png)

### 新しいアカウント

新しいアカウントを作成する場合は、「 **[!UICONTROL 新しいアカウント]**&#x200B;をクリックし、名前、説明（オプション）、 [!DNL Zoho CRM] 資格情報。 終了したら、「 」を選択します。 **[!UICONTROL ソースに接続]** その後、新しい接続が確立されるまでしばらく時間をかけます。

>[!TIP]
>
>アカウント URL ドメインは、適切なドメインの場所に対応している必要があります。 様々なドメインと対応するアカウント URL を次に示します。<ul><li>米国：https://accounts.zoho.com</li><li>オーストラリア：https://accounts.zoho.com.au</li><li>ヨーロッパ：https://accounts.zoho.eu</li><li>インド：https://accounts.zoho.in</li><li>中国：https://accounts.zoho.com.cn</li></ul>

![新規](../../../../images/tutorials/create/zoho/new.png)

## 次の手順

このチュートリアルに従うことで、 [!DNL Zoho CRM] アカウント 次のチュートリアルに進み、 [データを Platform に取り込むためのデータフローの設定](../../dataflow/crm.md).
