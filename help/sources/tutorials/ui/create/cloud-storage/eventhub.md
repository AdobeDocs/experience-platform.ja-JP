---
title: UI での Azure Event Hubs Source接続の作成
description: Adobe Experience Platform UI を使用して Azure Event Hubs ソース接続を作成する方法について説明します。
badgeUltimate: label="Ultimate" type="Positive"
exl-id: 7e67e213-8ccb-4fa5-b09f-ae77aba8614c
source-git-commit: 1256f0c76b29edad4808fc4be1d61399bfbae8fa
workflow-type: tm+mt
source-wordcount: '1097'
ht-degree: 17%

---

# UI での [!DNL Azure Event Hubs] ソース接続の作成

>[!IMPORTANT]
>
>Real-time Customer Data Platform Ultimate を購入したユーザーは、ソースカタログで [!DNL Azure Event Hubs] ソースを利用できます。

Adobe Experience Platform ユーザーインターフェイスを使用して [!DNL Azure Event Hubs] アカウントを作成する方法については、このチュートリアルをお読みください。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)]  システム](../../../../../xdm/home.md)：[!DNL Experience Platform] が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に有効な [!DNL Event Hubs] 接続がある場合は、このドキュメントの残りの部分をスキップして、[データフローの設定](../../dataflow/streaming/cloud-storage-streaming.md)に関するチュートリアルに進むことができます。

### 必要な資格情報の収集

[!DNL Event Hubs] ソースコネクタを認証するには、次の接続プロパティの値を指定する必要があります。

>[!BEGINTABS]

>[!TAB  標準認証 ]

| 資格情報 | 説明 |
| --- | --- |
| SAS キー名 | 承認ルールの名前。SAS キー名とも呼ばれます。 |
| SAS キー | [!DNL Event Hubs] 名前空間のプライマリキー。 [!DNL Event Hubs] リストを入力するには、`sasKey` が対応する `sasPolicy` に `manage` 権限が設定されている必要があります。 |
| 名前空間 | アクセスする [!DNL Event Hub] の名前空間。 [!DNL Event Hub] 名前空間は、一意のスコーピングコンテナを提供し、このコンテナでは 1 つ以上の [!DNL Event Hubs] を作成できます。 |

>[!TAB SAS 認証 ]

| 資格情報 | 説明 |
| --- | --- |
| SAS キー名 | 承認ルールの名前。SAS キー名とも呼ばれます。 |
| SAS キー | [!DNL Event Hub] 名前空間のプライマリキー。 [!DNL Event Hubs] リストを入力するには、`sasKey` が対応する `sasPolicy` に `manage` 権限が設定されている必要があります。 |
| 名前空間 | アクセスする [!DNL Event Hub] の名前空間。 [!DNL Event Hub] 名前空間は、一意のスコーピングコンテナを提供し、このコンテナでは 1 つ以上の [!DNL Event Hubs] を作成できます。 |
| イベントハブ名 | [!DNL Azure Event Hub] 名を入力します。 [!DNL Event Hub] の名前について詳しくは、[Microsoft ドキュメント ](https://learn.microsoft.com/en-us/azure/event-hubs/event-hubs-create#create-an-event-hub) を参照してください。 |

>[!TAB Event Hub Azure Active Directory Auth]

| 資格情報 | 説明 |
| --- | --- |
| テナント ID | 権限をリクエストするテナント ID。 テナント ID は、GUID またはわかりやすい名前として書式設定できます。 **注意**：テナント ID は、[!DNL Microsoft Azure] インターフェイスでは「ディレクトリ ID」と呼ばれます。 |
| クライアント ID | アプリに割り当てられたアプリケーション ID。 この ID は、[!DNL Azure Active Directory] ーザーを登録した [!DNL Microsoft Entra ID] ポータルから取得できます。 |
| クライアントシークレットの値 | アプリを認証するためにクライアント ID と共に使用されるクライアント秘密鍵。 クライアントの秘密鍵は、クライアントを登録した [!DNL Microsoft Entra ID] ポータルから取得でき [!DNL Azure Active Directory] す。 |
| 名前空間 | アクセスする [!DNL Event Hub] の名前空間。 [!DNL Event Hub] 名前空間は、一意のスコーピングコンテナを提供し、このコンテナでは 1 つ以上の [!DNL Event Hubs] を作成できます。 |

[!DNL Azure Active Directory] について詳しくは、[Microsoft Entra ID の使用に関する Azure ガイド ](https://learn.microsoft.com/en-us/azure/healthcare-apis/register-application) を参照してください。

>[!TAB Azure Active Directory 認証をスコープとするイベント ハブ ]

| 資格情報 | 説明 |
| --- | --- |
| テナント ID | 権限をリクエストするテナント ID。 テナント ID は、GUID またはわかりやすい名前として書式設定できます。 **注意**：テナント ID は、[!DNL Microsoft Azure] インターフェイスでは「ディレクトリ ID」と呼ばれます。 |
| クライアント ID | アプリに割り当てられたアプリケーション ID。 この ID は、[!DNL Azure Active Directory] ーザーを登録した [!DNL Microsoft Entra ID] ポータルから取得できます。 |
| クライアントシークレットの値 | アプリを認証するためにクライアント ID と共に使用されるクライアント秘密鍵。 クライアントの秘密鍵は、クライアントを登録した [!DNL Microsoft Entra ID] ポータルから取得でき [!DNL Azure Active Directory] す。 |
| 名前空間 | アクセスする [!DNL Event Hub] の名前空間。 [!DNL Event Hub] 名前空間は、一意のスコーピングコンテナを提供し、このコンテナでは 1 つ以上の [!DNL Event Hubs] を作成できます。 |
| イベントハブ名 | [!DNL Azure Event Hub] 名を入力します。 [!DNL Event Hub] の名前について詳しくは、[Microsoft ドキュメント ](https://learn.microsoft.com/en-us/azure/event-hubs/event-hubs-create#create-an-event-hub) を参照してください。 |

[!DNL Azure Active Directory] について詳しくは、[Microsoft Entra ID の使用に関する Azure ガイド ](https://learn.microsoft.com/en-us/azure/healthcare-apis/register-application) を参照してください。

>[!ENDTABS]

必要な資格情報を収集したら、次の手順に従って [!DNL Event Hubs] アカウントをExperience Platformにリンクできます。

## [!DNL Event Hubs] アカウントを接続

Platform UI の左側のナビゲーションバーで「**[!UICONTROL ソース]**」を選択して、[!UICONTROL ソース]ワークスペースにアクセスします。[!UICONTROL  カタログ ] 画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

[!UICONTROL  クラウドストレージ ] カテゴリで、「**[!UICONTROL Azure Event Hubs]**」を選択し、次に「**[!UICONTROL データを追加]**」を選択します。

![Azure Event Hubs が選択されているソースカタログ ](../../../../images/tutorials/create/eventhub/catalog.png)

**[!UICONTROL Azure Event Hubs に接続]** ダイアログが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 既存のアカウント

既存のアカウントを使用するには、使用する [!DNL Event Hubs] アカウントを選択し、「**[!UICONTROL 次へ]**」を選択して続行します。

![ 既存の Azure Event Hubs ソースアカウントのリスト。](../../../../images/tutorials/create/eventhub/existing.png)

### 新規アカウント

>[!TIP]
>
>作成した後は、[!DNL Event Hubs] ベース接続の認証タイプを変更できません。 認証タイプを変更するには、新しいベース接続を作成する必要があります。

新しいアカウントを作成するには、「**[!UICONTROL 新しいアカウント]**」を選択し、新しい [!DNL Event Hubs] アカウントの名前と説明（オプション）を入力します。

![Azure Event Hubs の新しいアカウント作成インターフェイス ](../../../../images/tutorials/create/eventhub/new.png)

>[!BEGINTABS]

>[!TAB  標準認証 ]

標準認証で [!DNL Event Hubs] アカウントを作成するには、[!UICONTROL  アカウント認証 ] ドロップダウンメニューを使用して、**[!UICONTROL 標準認証]** を選択します。 次に、[!UICONTROL SAS キー名 ]、[!UICONTROL SAS キー ]、[!UICONTROL  名前空間 ] の値を指定します。

認証資格情報を入力したら、「**[!UICONTROL ソースに接続]**」を選択します。

![Azure Event Hubs の標準認証インターフェイス。](../../../../images/tutorials/create/eventhub/standard.png)

>[!TAB SAS 認証 ]

SAS 認証を使用して [!DNL Event Hubs] アカウントを作成するには、「[!UICONTROL  アカウント認証 ]」ドロップダウンメニューを使用し、「**[!UICONTROL SAS 認証]**」を選択します。 次に、[!UICONTROL SAS キー名 ]、[!UICONTROL SAS キー ]、[!UICONTROL  名前空間 ]、および [!UICONTROL Event Hubs 名 ] の値を指定します。

認証資格情報を入力したら、「**[!UICONTROL ソースに接続]**」を選択します。

![Azure Event Hubs の SAS 認証インターフェイス ](../../../../images/tutorials/create/eventhub/sas.png)

>[!TAB Event Hub Azure Active Directory Auth]

Event Hub Azure Active Directory 認証で [!DNL Event Hubs] アカウントを作成するには、「[!UICONTROL  アカウント認証 ]」ドロップダウンメニューを使用して、「**[!UICONTROL Event Hub Azure Active Directory]**」を選択します。 次に、[!UICONTROL  テナント ID]、[!UICONTROL  クライアント ID]、[!UICONTROL  クライアントシークレット値 ]、および [!UICONTROL  名前空間 ] の値を指定します。

![Azure Event Hub Azure Active Directory 認証 ](../../../../images/tutorials/create/eventhub/active-directory.png)

>[!TAB Azure Active Directory 認証をスコープとするイベント ハブ ]

Event Hub スコープ Azure Active Directory 認証の [!DNL Event Hubs] アカウントを作成するには、[[!UICONTROL  アカウント認証 ]] ドロップダウン メニューを使用し、[**[!UICONTROL Event Hub スコープ Azure Active Directory]**] を選択します。 次に、[!UICONTROL  テナント ID]、[!UICONTROL  クライアント ID]、[!UICONTROL  クライアントシークレット値 ]、[!UICONTROL  名前空間 ]、[!UICONTROL  イベントハブ名 ] の値を指定します。

![Azure Activity Directory 認証を範囲とする Azure イベントハブ ](../../../../images/tutorials/create/eventhub/scoped.png)

>[!ENDTABS]


## 次の手順

このチュートリアルでは、[!DNL Event Hubs] アカウントをExperience Platformに接続しました。 次のチュートリアルに進み、[ データフローを設定して、クラウドストレージからExperience Platformにデータを取り込む ](../../dataflow/streaming/cloud-storage-streaming.md) ことができます。
