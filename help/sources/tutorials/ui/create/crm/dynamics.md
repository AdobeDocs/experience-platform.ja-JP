---
keywords: Experience Platform；ホーム；人気の高いトピック；Microsoft Dynamics;microsoft dynamics;Dynamics;dynamics
solution: Experience Platform
title: UI でのMicrosoft Dynamics ソース接続の作成
type: Tutorial
description: Adobe Experience Platform UI を使用してMicrosoft Dynamics ソース接続を作成する方法を説明します。
exl-id: 1a7a66de-dc57-4a72-8fdd-5fd80175db69
source-git-commit: ed92bdcd965dc13ab83649aad87eddf53f7afd60
workflow-type: tm+mt
source-wordcount: '596'
ht-degree: 37%

---

# UI での [!DNL Microsoft Dynamics] ソース接続の作成

このチュートリアルでは、 [!DNL Microsoft Dynamics] （以下「」という。）[!DNL Dynamics]&quot;) Adobe Experience Platform UI を使用したソース接続

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md)：Experience Platform が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に有効な [!DNL Dynamics] アカウントを使用する場合は、このドキュメントの残りの部分をスキップし、次のチュートリアルに進んでください： [CRM ソースのデータフローの設定](../../dataflow/crm.md).

### 必要な資格情報の収集

| 資格情報 | 説明 |
| ---------- | ----------- |
| `serviceUri` | のサービス URL [!DNL Dynamics] インスタンス。 |
| `username` | のユーザー名 [!DNL Dynamics] ユーザーアカウント。 |
| `password` | ユーザーのパスワード [!DNL Dynamics] アカウント |
| `servicePrincipalId` | のクライアント ID [!DNL Dynamics] アカウント この ID は、サービスプリンシパルとキーベースの認証を使用する場合に必要です。 |
| `servicePrincipalKey` | サービスプリンシパル秘密鍵。 この資格情報は、サービスプリンシパルとキーベースの認証を使用する場合に必要です。 |

の導入について詳しくは、 [この [!DNL Dynamics] 文書](https://docs.microsoft.com/en-us/powerapps/developer/common-data-service/authenticate-oauth).

## [!DNL Dynamics] アカウントを接続

必要な資格情報を収集したら、以下の手順に従って [!DNL Dynamics] アカウントを Platform にリンクできます。

にログインします。 [Adobe Experience Platform](https://platform.adobe.com) 次に、 **[!UICONTROL ソース]** 左側のナビゲーションバーから [!UICONTROL ソース] ワークスペース。 **[!UICONTROL カタログ]**&#x200B;画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

以下 **[!UICONTROL CRM]** カテゴリ、選択 **[!UICONTROL Microsoft Dynamics]**. このコネクタを初めて使用する場合は、「 **[!UICONTROL 設定]**. それ以外の場合は、「 **[!UICONTROL データを追加]** 新しい [!DNL Dynamics] コネクタ。

![カタログ](../../../../images/tutorials/create/ms-dynamics/catalog.png)

この **[!UICONTROL Dynamics に接続]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 新しいアカウント

新しい資格情報を使用している場合は、「**[!UICONTROL 新しいアカウント]**」を選択します。表示される入力フォームで、新しい [!DNL Dynamics] アカウント

この [!DNL Dynamics] コネクタは、アクセス用に様々な認証タイプを提供します。 の下 [!UICONTROL アカウント認証] 選択 **[!UICONTROL 基本認証]** パスワードベースの資格情報を使用する場合。

終了したら、「 」を選択します。 **[!UICONTROL ソースに接続]** そして、新しいアカウントが確立されるまでしばらく待ちます。

![basic-authentication](../../../../images/tutorials/create/ms-dynamics/basic-auth.png)

または、 **[!UICONTROL サービスプリンシパルおよびキー認証]** をクリックし、 [!DNL Dynamics] ～を組み合わせてアカウントを作る [!UICONTROL サービスプリンシパル ID] および [!UICONTROL サービスプリンシパルキー].

>[!IMPORTANT]
>
> での基本認証 [!DNL Dynamics] は、現在 Platform ではサポートされていない 2 要素認証によってブロックされる場合があります。 この場合、キーベースの認証を使用して、 [!DNL Dynamics].

![key-based-authentication](../../../../images/tutorials/create/ms-dynamics/key-based-auth.png)

| 資格情報 | 説明 |
| ---------- | ----------- |
| [!UICONTROL サービスプリンシパル ID] | のクライアント ID [!DNL Dynamics] アカウント この ID は、サービスプリンシパルとキーベースの認証を使用する場合に必要です。 |
| [!UICONTROL サービスプリンシパルキー] | サービスプリンシパル秘密鍵。 この資格情報は、サービスプリンシパルとキーベースの認証を使用する場合に必要です。 |

### 既存のアカウント

既存のアカウントに接続するには、 [!DNL Dynamics] 接続するアカウントを選択し、 **[!UICONTROL 次へ]** をクリックして次に進みます。

![既存](../../../../images/tutorials/create/ms-dynamics/existing.png)

## 次の手順

このチュートリアルでは、[!DNL Dynamics] アカウントとの接続を確立しました。次のチュートリアルに進み、[データを Platform に取り込むためのデータフローの設定](../../dataflow/crm.md)を行いましょう。
