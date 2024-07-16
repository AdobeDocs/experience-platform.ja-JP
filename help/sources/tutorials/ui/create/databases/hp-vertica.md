---
keywords: Experience Platform；ホーム；人気のトピック；HP Vertica
solution: Experience Platform
title: UI での HP Vertica Source接続の作成
type: Tutorial
description: Adobe Experience Platform UI を使用して HP Vertica ソース接続を作成する方法を説明します。
exl-id: d7315ad4-9250-4e66-be33-016efabb512e
source-git-commit: e37c00863249e677f1645266859bf40fe6451827
workflow-type: tm+mt
source-wordcount: '485'
ht-degree: 32%

---

# UI での HP [!DNL Vertica] ソース接続の作成

>[!NOTE]
>
> HP [!DNL Vertica] コネクタはベータ版です。 ベータ版のコネクタの使用に関して詳しくは、[ ソースの概要 ](../../../../home.md#terms-and-conditions) を参照してください。

Adobe Experience PlatformのSource コネクタには、外部ソースのデータをスケジュールに従って取り込む機能が用意されています。 このチュートリアルでは、[!DNL Platform] ユーザーインターフェイスを使用して HP [!DNL Vertica] ソースコネクタを作成する手順について説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)]  システム](../../../../../xdm/home.md)：[!DNL Experience Platform] が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に有効な HP [!DNL Vertica] 接続がある場合は、このドキュメントの残りの部分をスキップして、[ データフローの設定 ](../../dataflow/databases.md) に関するチュートリアルに進むことができます。

### 必要な資格情報の収集

次のセクションでは、[!DNL Flow Service] API を使用して HP [!DNL Vertica] に正常に接続するために必要な追加情報を示します。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `connectionString` | HP [!DNL Vertica] インスタンスへの接続に使用する接続文字列。 HP [!DNL Vertica] の接続文字列パターンは `Server={SERVER};Port={PORT};Database={DATABASE};UID={USERNAME};PWD={PASSWORD}` です |

基本について詳しくは、[ この HP [!DNL Vertica] document](https://www.vertica.com/docs/9.2.x/HTML/Content/Authoring/ConnectingToVertica/ClientJDBC/CreatingAndConfiguringAConnection.htm) を参照してください。

## HP [!DNL Vertica] アカウントの接続

必要な資格情報を収集したら、次の手順に従って HP [!DNL Vertica] アカウントを [!DNL Platform] にリンクできます。

[Adobe Experience Platform](https://platform.adobe.com) にログインし、左側のナビゲーションバーから **[!UICONTROL ソース]** を選択して **[!UICONTROL ソース]** ワークスペースにアクセスします。 **[!UICONTROL カタログ]**&#x200B;画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

**[!UICONTROL Databases]** カテゴリの下で、「**[!UICONTROL HP Vertica]**」を選択します。 このコネクタを初めて使用する場合は、「**[!UICONTROL 設定]**」を選択します。 それ以外の場合は、「**[!UICONTROL データを追加]**」を選択して、新しい HP [!DNL Vertica] コネクタを作成します。

![カタログ](../../../../images/tutorials/create/hp-vertica/catalog.png)

**[!UICONTROL HP Vertica に接続]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 新しいアカウント

新しい資格情報を使用している場合は、「**[!UICONTROL 新しいアカウント]**」を選択します。表示される入力フォームで、名前、説明（オプション）、HP [!DNL Vertica] の資格情報を入力します。 終了したら「**[!UICONTROL 接続]**」を選択し、新しい接続が確立されるまでしばらく待ちます。

![ 接続 ](../../../../images/tutorials/create/hp-vertica/new.png)

### 既存のアカウント

既存のアカウントに接続するには、接続する HP [!DNL Vertica] アカウントを選択し、右上隅にある **[!UICONTROL 次へ]** を選択して続行します。

![既存](../../../../images/tutorials/create/hp-vertica/existing.png)

## 次の手順

このチュートリアルでは、HP [!DNL Vertica] アカウントへの接続を確立しました。 次のチュートリアルに進み、[ データをに取り込むためのデータフローの設定  [!DNL Platform]](../../dataflow/databases.md) を行いましょう。
