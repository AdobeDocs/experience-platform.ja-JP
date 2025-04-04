---
keywords: Experience Platform；ホーム；人気のトピック；Microsoft Dynamics;microsoft dynamics;Dynamics;dynamics
solution: Experience Platform
title: UI でのMicrosoft Dynamics Source接続の作成
type: Tutorial
description: Adobe Experience Platform UI を使用してMicrosoft Dynamics ソース接続を作成する方法を説明します。
exl-id: 1a7a66de-dc57-4a72-8fdd-5fd80175db69
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '617'
ht-degree: 25%

---

# UI での [!DNL Microsoft Dynamics] ソース接続の作成

このチュートリアルでは、Adobe Experience Platform UI を使用して [!DNL Microsoft Dynamics] （以下「[!DNL Dynamics]」）ソース接続を作成する手順について説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md)：Experience Platform が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

有効な [!DNL Dynamics] アカウントを既にお持ちの場合は、このドキュメントの残りの部分をスキップし、[CRM ソースのデータフローの設定 ](../../dataflow/crm.md) に関するチュートリアルに進んでください。

### 必要な資格情報の収集

[!DNL Dynamics] ソースを認証するには、次の接続プロパティの値を指定する必要があります。

>[!BEGINTABS]

>[!TAB  基本認証 ]

| 資格情報 | 説明 |
| --- | --- |
| `serviceUri` | [!DNL Dynamics] インスタンスのサービス URL。 |
| `username` | [!DNL Dynamics] ユーザーアカウントのユーザー名。 |
| `password` | [!DNL Dynamics] アカウントのパスワード。 |

>[!TAB  サービスプリンシパルとキーの認証 ]

| 資格情報 | 説明 |
| --- | --- |
| `servicePrincipalId` | [!DNL Dynamics] アカウントのクライアント ID。 この ID は、サービスプリンシパルおよびキーベースの認証を使用する場合に必要です。 |
| `servicePrincipalKey` | サービスプリンシパルの秘密鍵。 この資格情報は、サービスプリンシパルおよびキーベースの認証を使用する場合に必要です。 |

>[!ENDTABS]

基本について詳しくは、[ このドキュメント  [!DNL Dynamics]  を参照してください ](https://docs.microsoft.com/en-us/powerapps/developer/common-data-service/authenticate-oauth)。

## [!DNL Dynamics] アカウントを接続

Experience Platformの UI で、左側のナビゲーションから **[!UICONTROL Sources]** を選択し、[!UICONTROL Sources] ワークスペースにアクセスします。 [!UICONTROL  カタログ ] 画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

[!UICONTROL CRM] カテゴリ内で、「**[!UICONTROL Microsoft Dynamics]**」を選択し、次に「**[!UICONTROL データを追加]**」を選択します。

![Microsoft Dynamicsが選択されているソースカタログ ](../../../../images/tutorials/create/ms-dynamics/catalog.png)

**[!UICONTROL Microsoft Dynamics アカウントの接続]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 既存のアカウント

既存のアカウントを使用するには、使用する [!DNL Dynamics] アカウントを選択し、右上隅にある **[!UICONTROL 次へ]** を選択して続行します。

![既存のアカウントインターフェイス。](../../../../images/tutorials/create/ms-dynamics/existing.png)

### 新しいアカウント

>[!TIP]
>
>作成した後は、[!DNL Dynamics] ベース接続の認証タイプを変更できません。 認証タイプを変更するには、新しいベース接続を作成する必要があります。

新しいアカウントを作成するには、「**[!UICONTROL 新しいアカウント]**」を選択し、新しい [!DNL Dynamics] アカウントの名前と説明（オプション）を入力します。

![ 新しいアカウント作成インターフェイス ](../../../../images/tutorials/create/ms-dynamics/new.png)

[!DNL Dynamics] アカウントを作成する際は、基本認証またはサービスプリンシパルおよびキー認証のいずれかを使用できます。

>[!BEGINTABS]

>[!TAB  基本認証 ]

基本認証を持つ [!DNL Dynamics] アカウントを作成するには、「[!UICONTROL  基本認証 ]」を選択し、「[!UICONTROL  サービス URI]」、「[!UICONTROL  ユーザー名 ]」、「[!UICONTROL  パスワード ] の値を指定します。 **注意**:[!DNL Dynamics] での基本認証は、現在Experience Platformでサポートされていない二要素認証でブロックされる場合があります。 この場合、[!DNL Dynamics] を使用してソースコネクタを作成するには、キーベースの認証を使用することをお勧めします。

完了したら、「**[!UICONTROL ソースに接続]**」を選択し、新しいアカウントが確立されるまでしばらく待ちます。

![ 基本認証インターフェイス ](../../../../images/tutorials/create/ms-dynamics/basic-authentication.png)

>[!TAB  サービスプリンシパルとキーの認証 ]

サービスプリンシパルとキーの認証を持つ [!DNL Dynamics] アカウントを作成するには、「**[!UICONTROL サービスプリンシパルとキーの認証]** を選択してから、「[!UICONTROL  サービスプリンシパル ID] と [!UICONTROL  サービスプリンシパルキー ] の値を指定します。

完了したら、「**[!UICONTROL ソースに接続]**」を選択し、新しいアカウントが確立されるまでしばらく待ちます。

![ サービスプリンシパルキー認証インターフェイス ](../../../../images/tutorials/create/ms-dynamics/service-principal.png)

>[!ENDTABS]

## 次の手順

このチュートリアルでは、[!DNL Dynamics] アカウントとの接続を確立しました。次のチュートリアルに進み、[ データをExperience Platformに取り込むためのデータフローの設定 ](../../dataflow/crm.md) を行いましょう。
