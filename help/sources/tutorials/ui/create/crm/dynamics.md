---
keywords: Experience Platform；ホーム；人気のあるトピック；Microsoft Dynamics;microsoft dynamics;Dynamics;Dynamics
solution: Experience Platform
title: UI での Microsoft Dynamics ソース接続の作成
topic-legacy: overview
type: Tutorial
description: Adobe Experience Platform UI を使用して Microsoft Dynamics ソース接続を作成する方法を説明します。
exl-id: 1a7a66de-dc57-4a72-8fdd-5fd80175db69
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '596'
ht-degree: 10%

---

# UI での [!DNL Microsoft Dynamics] ソース接続の作成

このチュートリアルでは、Adobe Experience Platform UI を使用して [!DNL Microsoft Dynamics]（以下「[!DNL Dynamics]」と呼びます）ソース接続を作成する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md):顧客体験データを整理する際に使用する標準化されたExperience Platformフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md):スキーマエディターの UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に有効な [!DNL Dynamics] アカウントがある場合は、このドキュメントの残りの部分をスキップし、[CRM ソースのデータフローの設定 ](../../dataflow/crm.md) に関するチュートリアルに進んでください。

### 必要な資格情報の収集

| 資格情報 | 説明 |
| ---------- | ----------- |
| `serviceUri` | [!DNL Dynamics] インスタンスのサービス URL。 |
| `username` | [!DNL Dynamics] ユーザーアカウントのユーザー名。 |
| `password` | [!DNL Dynamics] アカウントのパスワード。 |
| `servicePrincipalId` | [!DNL Dynamics] アカウントのクライアント ID。 この ID は、サービスプリンシパルとキーベースの認証を使用する場合に必要です。 |
| `servicePrincipalKey` | サービスプリンシパル秘密鍵 この資格情報は、サービスプリンシパルとキーベースの認証を使用する場合に必要です。 |

開始方法の詳細については、[ この  [!DNL Dynamics]  ドキュメント ](https://docs.microsoft.com/en-us/powerapps/developer/common-data-service/authenticate-oauth) を参照してください。

## [!DNL Dynamics] アカウントに接続

必要な資格情報を収集したら、以下の手順に従って [!DNL Dynamics] アカウントを Platform にリンクできます。

[Adobe Experience Platform](https://platform.adobe.com) にログインし、左側のナビゲーションバーから「**[!UICONTROL ソース]**」を選択して、「[!UICONTROL  ソース ]」ワークスペースにアクセスします。 **[!UICONTROL カタログ]** 画面には、アカウントを作成するための様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、検索オプションを使用して、目的の特定のソースを見つけることもできます。

「**[!UICONTROL CRM]**」カテゴリで、「**[!UICONTROL Microsoft Dynamics]**」を選択します。 このコネクタを初めて使用する場合は、「**[!UICONTROL 設定]**」を選択します。 それ以外の場合は、「**[!UICONTROL データを追加]**」を選択して新しい [!DNL Dynamics] コネクタを作成します。

![カタログ](../../../../images/tutorials/create/ms-dynamics/catalog.png)

[**[!UICONTROL Dynamics に接続]**] ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 新規アカウント

新しい資格情報を使用する場合は、「**[!UICONTROL 新しいアカウント]**」を選択します。 表示される入力フォームで、新しい [!DNL Dynamics] アカウントの名前と説明（オプション）を入力します。

[!DNL Dynamics] コネクタは、アクセス用に様々な認証タイプを提供します。 「[!UICONTROL  アカウント認証 ]」で、「**[!UICONTROL 基本認証]**」を選択して、パスワードベースの資格情報を使用します。

終了したら、[**[!UICONTROL ソースに接続]**] を選択し、新しいアカウントの確立に時間をかけます。

![basic-authentication](../../../../images/tutorials/create/ms-dynamics/basic-auth.png)

または、**[!UICONTROL サービスプリンシパルとキー認証]** を選択し、[!UICONTROL  サービスプリンシパル ID] と [!UICONTROL  サービスプリンシパルキー ] を組み合わせて [!DNL Dynamics] アカウントに接続することもできます。

>[!IMPORTANT]
>
> [!DNL Dynamics] の基本認証は、現在 Platform ではサポートされていない 2 要素認証でブロックされる場合があります。 この場合、キーベースの認証を使用して [!DNL Dynamics] を使用してソースコネクタを作成することをお勧めします。

![key-based-authentication](../../../../images/tutorials/create/ms-dynamics/key-based-auth.png)

| 資格情報 | 説明 |
| ---------- | ----------- |
| [!UICONTROL サービスプリンシパル ID] | [!DNL Dynamics] アカウントのクライアント ID。 この ID は、サービスプリンシパルとキーベースの認証を使用する場合に必要です。 |
| [!UICONTROL サービスプリンシパルキー] | サービスプリンシパル秘密鍵 この資格情報は、サービスプリンシパルとキーベースの認証を使用する場合に必要です。 |

### 既存のアカウント

既存のアカウントに接続するには、接続する [!DNL Dynamics] アカウントを選択し、右上隅の「**[!UICONTROL 次へ]**」を選択して次に進みます。

![既存](../../../../images/tutorials/create/ms-dynamics/existing.png)

## 次の手順

このチュートリアルに従って、[!DNL Dynamics] アカウントへの接続を確立しました。 次のチュートリアルに進み、データを Platform に取り込むように [ データフローを設定します。](../../dataflow/crm.md)
