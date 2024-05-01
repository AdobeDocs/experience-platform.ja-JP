---
title: UI での Azure Event Hubs ソース接続の作成
description: Adobe Experience Platform UI を使用して Azure Event Hubs ソース接続を作成する方法について説明します。
badgeUltimate: label="Ultimate" type="Positive"
exl-id: 7e67e213-8ccb-4fa5-b09f-ae77aba8614c
source-git-commit: 22f3b76c02e641d2f4c0dd7c0e5cc93038782836
workflow-type: tm+mt
source-wordcount: '1096'
ht-degree: 17%

---

# を作成 [!DNL Azure Event Hubs] ui のソース接続

>[!IMPORTANT]
>
>この [!DNL Azure Event Hubs] ソースは、Real-time Customer Data Platform Ultimate を購入したユーザーがソースカタログから利用できます。

を作成する方法については、このチュートリアルをお読みください。 [!DNL Azure Event Hubs] Adobe Experience Platform ユーザーインターフェイスを使用したアカウント。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)]  システム](../../../../../xdm/home.md)：[!DNL Experience Platform] が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に有効な [!DNL Event Hubs] 接続がある場合は、このドキュメントの残りの部分をスキップして、[データフローの設定](../../dataflow/streaming/cloud-storage-streaming.md)に関するチュートリアルに進むことができます。

### 必要な資格情報の収集

を認証するために [!DNL Event Hubs] ソースコネクタの場合、次の接続プロパティの値を指定する必要があります。

>[!BEGINTABS]

>[!TAB 標準認証]

| 資格情報 | 説明 |
| --- | --- |
| SAS キー名 | 承認ルールの名前。SAS キー名とも呼ばれます。 |
| SAS キー | のプライマリキー [!DNL Event Hubs] 名前空間。 この `sasPolicy` その `sasKey` 次に対応する必要があります `manage` 用に設定された権限 [!DNL Event Hubs] 入力するリスト。 |
| 名前空間 | の名前空間 [!DNL Event Hubs] 現在アクセス中。 An [!DNL Event Hubs] 名前空間は、1 つ以上の作成できる一意のスコーピングコンテナを提供します [!DNL Event Hubs]. |

>[!TAB SAS 認証]

| 資格情報 | 説明 |
| --- | --- |
| SAS キー名 | 承認ルールの名前。SAS キー名とも呼ばれます。 |
| SAS キー | のプライマリキー [!DNL Event Hubs] 名前空間。 この `sasPolicy` その `sasKey` 次に対応する必要があります `manage` 用に設定された権限 [!DNL Event Hubs] 入力するリスト。 |
| 名前空間 | の名前空間 [!DNL Event Hubs] 現在アクセス中。 An [!DNL Event Hubs] 名前空間は、1 つ以上の作成できる一意のスコーピングコンテナを提供します [!DNL Event Hubs]. |
| イベントハブ名 | の名前 [!DNL Event Hubs] ソース。 |

の共有アクセス署名（SAS）認証について詳しくは、を参照してください [!DNL Event Hubs]、を読み取ります [[!DNL Azure] sas の使用に関するガイド](https://docs.microsoft.com/en-us/azure/event-hubs/authenticate-shared-access-signature).

>[!TAB イベント ハブ Azure Active Directory 認証]

| 資格情報 | 説明 |
| --- | --- |
| テナント ID | 権限をリクエストするテナント ID。 テナント ID は、GUID またはわかりやすい名前として書式設定できます。 **注意**：テナント ID は、では「ディレクトリ ID」と呼ばれます [!DNL Microsoft Azure] インターフェイス。 |
| クライアント ID | アプリに割り当てられたアプリケーション ID。 この ID は、から取得できます [!DNL Microsoft Entra ID] を登録したポータル [!DNL Azure Active Directory]. |
| クライアントシークレットの値 | アプリを認証するためにクライアント ID と共に使用されるクライアント秘密鍵。 クライアント秘密鍵は、から取得できます。 [!DNL Microsoft Entra ID] を登録したポータル [!DNL Azure Active Directory]. |
| 名前空間 | の名前空間 [!DNL Event Hubs] 現在アクセス中。 An [!DNL Event Hubs] 名前空間は、1 つ以上の作成できる一意のスコーピングコンテナを提供します [!DNL Event Hubs]. |

について詳しくは、 [!DNL Azure Active Directory]、を読み取ります [Microsoft Entra ID の使用に関する Azure ガイド](https://learn.microsoft.com/en-us/azure/healthcare-apis/register-application).

>[!TAB Azure Active Directory 認証を範囲とするイベント ハブ]

| 資格情報 | 説明 |
| --- | --- |
| テナント ID | 権限をリクエストするテナント ID。 テナント ID は、GUID またはわかりやすい名前として書式設定できます。 **注意**：テナント ID は、では「ディレクトリ ID」と呼ばれます [!DNL Microsoft Azure] インターフェイス。 |
| クライアント ID | アプリに割り当てられたアプリケーション ID。 この ID は、から取得できます [!DNL Microsoft Entra ID] を登録したポータル [!DNL Azure Active Directory]. |
| クライアントシークレットの値 | アプリを認証するためにクライアント ID と共に使用されるクライアント秘密鍵。 クライアント秘密鍵は、から取得できます。 [!DNL Microsoft Entra ID] を登録したポータル [!DNL Azure Active Directory]. |
| 名前空間 | の名前空間 [!DNL Event Hubs] 現在アクセス中。 An [!DNL Event Hubs] 名前空間は、1 つ以上の作成できる一意のスコーピングコンテナを提供します [!DNL Event Hubs]. |
| イベントハブ名 | の名前 [!DNL Event Hubs] ソース。 |

について詳しくは、 [!DNL Azure Active Directory]、を読み取ります [Microsoft Entra ID の使用に関する Azure ガイド](https://learn.microsoft.com/en-us/azure/healthcare-apis/register-application).

>[!ENDTABS]

必要な資格情報を収集したら、次の手順に従ってをリンクできます [!DNL Event Hubs] Experience Platformするアカウント。

## [!DNL Event Hubs] アカウントを接続

Platform UI の左側のナビゲーションバーで「**[!UICONTROL ソース]**」を選択して、[!UICONTROL ソース]ワークスペースにアクセスします。この [!UICONTROL カタログ] 画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

の下 [!UICONTROL クラウドストレージ] カテゴリ、選択 **[!UICONTROL Azure Event Hubs]**&#x200B;を選択してから、 **[!UICONTROL データを追加]**.

![Azure Event Hubs が選択されているソースカタログ。](../../../../images/tutorials/create/eventhub/catalog.png)

この **[!UICONTROL Azure Event Hubs への接続]** ダイアログが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 既存のアカウント

既存のアカウントを使用するには、 [!DNL Event Hubs] 使用するアカウントを選択してから、 **[!UICONTROL 次]** をクリックして続行します。

![既存の Azure Event Hubs ソースアカウントのリスト。](../../../../images/tutorials/create/eventhub/existing.png)

### 新規アカウント

>[!TIP]
>
>作成後は、の認証タイプを変更できません [!DNL Event Hubs] ベース接続。 認証タイプを変更するには、新しいベース接続を作成する必要があります。

新しいアカウントを作成するには、を選択します **[!UICONTROL 新しいアカウント]**&#x200B;を選択し、新しいの名前と説明（オプション）を入力します [!DNL Event Hubs] アカウント。

![Azure Event Hubs の新しいアカウント作成インターフェイス。](../../../../images/tutorials/create/eventhub/new.png)

>[!BEGINTABS]

>[!TAB 標準認証]

を作成するには [!DNL Event Hubs] 標準認証を使用するアカウントでは、 [!UICONTROL アカウント認証] ドロップダウンメニューのを選択し、 **[!UICONTROL 標準認証]**. 次に、の値を指定します [!UICONTROL SAS キー名], [!UICONTROL SAS キー]、および [!UICONTROL 名前空間].

認証資格情報を入力したら、 **[!UICONTROL ソースに接続]**.

![Azure Event Hubs の標準認証インターフェイス。](../../../../images/tutorials/create/eventhub/standard.png)

>[!TAB SAS 認証]

を作成するには [!DNL Event Hubs] sas 認証を使用するアカウントでは、 [!UICONTROL アカウント認証] ドロップダウンメニューのを選択し、 **[!UICONTROL SAS 認証]**. 次に、の値を指定します [!UICONTROL SAS キー名], [!UICONTROL SAS キー], [!UICONTROL 名前空間]、および [!UICONTROL Event Hubs 名].

認証資格情報を入力したら、 **[!UICONTROL ソースに接続]**.

![Azure Event Hubs の SAS 認証インターフェイス。](../../../../images/tutorials/create/eventhub/sas.png)

>[!TAB イベント ハブ Azure Active Directory 認証]

を作成するには [!DNL Event Hubs] event Hub Azure Active Directory 認証を持つアカウントは、 [!UICONTROL アカウント認証] ドロップダウンメニューのを選択し、 **[!UICONTROL Event Hub Azure Active Directory]**. 次に、の値を指定します [!UICONTROL テナント ID], [!UICONTROL クライアント ID], [!UICONTROL クライアントシークレットの値]、および [!UICONTROL 名前空間].

![Azure Event Hub Azure Active Directory 認証](../../../../images/tutorials/create/eventhub/active-directory.png)

>[!TAB Azure Active Directory 認証を範囲とするイベント ハブ]

を作成するには [!DNL Event Hubs] event Hub スコープの Azure Active Directory 認証を持つアカウントは、 [!UICONTROL アカウント認証] ドロップダウンメニューのを選択し、 **[!UICONTROL Azure Active Directory を範囲とするイベント ハブ]**. 次に、の値を指定します [!UICONTROL テナント ID], [!UICONTROL クライアント ID], [!UICONTROL クライアントシークレットの値], [!UICONTROL 名前空間]、および [!UICONTROL イベントハブ名].

![Azure Activity Directory 認証を範囲とする Azure イベントハブ](../../../../images/tutorials/create/eventhub/scoped.png)

>[!ENDTABS]


## 次の手順

このチュートリアルでは、を接続しました [!DNL Event Hubs] Experience Platformするアカウント。 次のチュートリアルに進むことができます。 [データフローを設定して、クラウドストレージからExperience Platformにデータを取り込みます](../../dataflow/streaming/cloud-storage-streaming.md).
