---
keywords: Experience Platform；ホーム；人気のトピック；Apache Hive;Azure HDInsights;azure hdinsights
solution: Experience Platform
title: UI で Azure HDInsights 上の Apache Hive Source接続を作成
type: Tutorial
description: Adobe Experience Platform UI を使用して、Azure HDInsights 上の Apache Hive ソース接続を作成する方法を説明します。
exl-id: 3eb3cb02-9867-451a-b847-ab895310eedf
source-git-commit: b48c24ac032cbf785a26a86b50a669d7fcae5d97
workflow-type: tm+mt
source-wordcount: '479'
ht-degree: 39%

---

# UI でのソース接続 [!DNL Azure HDInsights] の [!DNL Apache Hive] の作成

>[!NOTE]
>
> [!DNL Azure HDInsights] コネクタの [!DNL Apache Hive] はベータ版です。 ベータ版のコネクタの使用に関して詳しくは、[ ソースの概要 ](../../../../home.md#terms-and-conditions) を参照してください。

Adobe Experience PlatformのSource コネクタには、外部ソースのデータをスケジュールに従って取り込む機能が用意されています。 このチュートリアルでは、[!DNL Experience Platform] ユーザーインターフェイス [!DNL Azure HDInsights] 使用してソースコネクタ上に [!DNL Apache Hive] を作成する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)]  システム](../../../../../xdm/home.md)：[!DNL Experience Platform] が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に有効な [!DNL Hive] 接続がある場合は、このドキュメントの残りの部分をスキップして、[ データフローの設定 ](../../dataflow/databases.md) に関するチュートリアルに進むことができます。

### 必要な資格情報の収集

[!DNL Experience Platform] で [!DNL Hive] アカウントにアクセスするには、次の値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `host` | [!DNL Hive] サーバーの IP アドレスまたはホスト名。 |
| `username` | [!DNL Hive] サーバーへのアクセスに使用するユーザー名。 |
| `password` | ユーザーに対応するパスワード。 |

基本について詳しくは、[ このドキュメント  [!DNL Hive]  を参照してください ](https://cwiki.apache.org/confluence/display/Hive/Tutorial#Tutorial-GettingStarted)。

## [!DNL Hive] アカウントの接続

必要な資格情報が揃ったら、次の手順に従って、[!DNL Hive] アカウントを [!DNL Experience Platform] にリンクします。

[Adobe Experience Platform](https://platform.adobe.com) にログインし、左側のナビゲーションバーから **[!UICONTROL ソース]** を選択して **[!UICONTROL ソース]** ワークスペースにアクセスします。 **[!UICONTROL カタログ]**&#x200B;画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

**[!UICONTROL Databases]** カテゴリの下で **[!UICONTROL Hive]** を選択します。 このコネクタを初めて使用する場合は、「**[!UICONTROL 設定]**」を選択します。 それ以外の場合は、「**[!UICONTROL データを追加]**」を選択して、新しい [!DNL Hive] コネクタを作成します。

![カタログ](../../../../images/tutorials/create/hive/catalog.png)

**[!UICONTROL Hive に接続]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 新しいアカウント

新しい資格情報を使用している場合は、「**[!UICONTROL 新しいアカウント]**」を選択します。表示される入力フォームで、名前、説明（オプション）、[!DNL Hive] 資格情報を入力します。 終了したら「**[!UICONTROL 接続]**」を選択し、新しい接続が確立されるまでしばらく待ちます。

![ 接続 ](../../../../images/tutorials/create/hive/new.png)

### 既存のアカウント

既存のアカウントに接続するには、接続する [!DNL Hive] アカウントを選択し、「**[!UICONTROL 次へ]**」を選択して続行します。

![既存](../../../../images/tutorials/create/hive/existing.png)

## 次の手順

このチュートリアルでは、[!DNL Hive] アカウントとの接続を確立しました。次のチュートリアルに進み、[ データをに取り込むためのデータフローの設定  [!DNL Experience Platform]](../../dataflow/databases.md) を行いましょう。
