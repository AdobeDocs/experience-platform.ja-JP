---
keywords: Experience Platform；ホーム；人気のトピック；Azure Data Explorer;Azure Data Explorer;Data Explorer;Data Explorer
solution: Experience Platform
title: UI での Azure Data ExplorerSource接続の作成
type: Tutorial
description: Adobe Experience Platform UI を使用して Azure Data Explorerソース接続を作成する方法を説明します。
exl-id: 561bf948-fc92-4401-8631-e2a408667507
source-git-commit: ed92bdcd965dc13ab83649aad87eddf53f7afd60
workflow-type: tm+mt
source-wordcount: '487'
ht-degree: 41%

---

# UI での [!DNL Azure Data Explorer] ソース接続の作成

Adobe Experience PlatformのSource コネクタには、外部ソースのデータをスケジュールに従って取り込む機能が用意されています。 このチュートリアルでは、[!DNL Platform] ユーザーインターフェイスを使用して [!DNL Azure Data Explorer] （以下「[!DNL Data Explorer]」）ソースコネクタを作成する手順について説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)]  システム](../../../../../xdm/home.md)：[!DNL Experience Platform] が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に有効な [!DNL Data Explorer] 接続がある場合は、このドキュメントの残りの部分をスキップして、[データフローの設定](../../dataflow/databases.md)に関するチュートリアルに進むことができます。

### 必要な資格情報の収集

[!DNL Platform] で [!DNL Data Explorer] アカウントにアクセスするには、次の値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `endpoint` | [!DNL Data Explorer] サーバーのエンドポイント。 |
| `database` | [!DNL Data Explorer] データベースの名前。 |
| `tenant` | [!DNL Data Explorer] データベースへの接続に使用する一意のテナント ID。 |
| `servicePrincipalId` | [!DNL Data Explorer] データベースへの接続に使用する一意のサービスプリンシパル ID。 |
| `servicePrincipalKey` | [!DNL Data Explorer] データベースへの接続に使用する一意のサービスプリンシパルキー。 |

基本について詳しくは、[ このドキュメント  [!DNL Data Explorer]  を参照してください ](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/management/access-control/how-to-authenticate-with-aad)。

## [!DNL Azure Data Explorer] アカウントの接続

必要な資格情報が揃ったら、次の手順に従って、[!DNL Data Explorer] アカウントを [!DNL Platform] にリンクします。

[Adobe Experience Platform](https://platform.adobe.com) にログインし、左側のナビゲーションバーから **[!UICONTROL ソース]** を選択して **[!UICONTROL ソース]** ワークスペースにアクセスします。 **[!UICONTROL カタログ]** 画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

**[!UICONTROL Databases]** カテゴリで、「**[!UICONTROL Azure Data Explorer]**」を選択します。 このコネクタを初めて使用する場合は、「**[!UICONTROL 設定]**」を選択します。 それ以外の場合は、「**[!UICONTROL データを追加]**」を選択して、新しいData Explorerコネクタを作成します。

![カタログ](../../../../images/tutorials/create/data-explorer/catalog.png)

**[!UICONTROL Azure に接続Data Explorer]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 新しいアカウント

新しい資格情報を使用している場合は、「**[!UICONTROL 新しいアカウント]**」を選択します。表示される入力フォームで、名前、説明（オプション）、[!DNL Data Explorer] 資格情報を入力します。 終了したら「**[!UICONTROL 接続]**」を選択し、新しい接続が確立されるまでしばらく待ちます。

![ 接続 ](../../../../images/tutorials/create/data-explorer/new.png)

### 既存のアカウント

既存のアカウントに接続するには、接続する [!DNL Data Explorer] アカウントを選択し、「**[!UICONTROL 次へ]**」を選択して続行します。

![既存](../../../../images/tutorials/create/data-explorer/existing.png)

## 次の手順

このチュートリアルでは、[!DNL Data Explorer] アカウントとの接続を確立しました。次のチュートリアルに進み、[ データをに取り込むためのデータフローの設定  [!DNL Platform]](../../dataflow/databases.md) を行いましょう。
