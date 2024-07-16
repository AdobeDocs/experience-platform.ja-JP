---
keywords: Experience Platform；ホーム；人気のトピック；Azure Table Storage;azure table storage;ats;ATS
solution: Experience Platform
title: UI での Azure Table Storage Source接続の作成
type: Tutorial
description: Adobe Experience Platform UI を使用して Azure Table Storage ソース接続を作成する方法を説明します。
exl-id: 045cb954-e3e1-439d-a3cd-170d688dfbc8
source-git-commit: ed92bdcd965dc13ab83649aad87eddf53f7afd60
workflow-type: tm+mt
source-wordcount: '473'
ht-degree: 34%

---

# UI での [!DNL Azure Table Storage] ソース接続の作成

Adobe Experience PlatformのSource コネクタには、外部ソースのデータをスケジュールに従って取り込む機能が用意されています。 このチュートリアルでは、[!DNL Platform] ユーザーインターフェイスを使用して [!DNL Azure Table Storage] （以下「ATS」）ソースコネクタを作成する手順について説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)]  システム](../../../../../xdm/home.md)：[!DNL Experience Platform] が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に有効な ATS 接続がある場合は、このドキュメントの残りの部分をスキップして、[ データフローの設定 ](../../dataflow/databases.md) に関するチュートリアルに進むことができます。

### 必要な資格情報の収集

[!DNL Platform] で ATS アカウントにアクセスするには、次の値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `connectionString` | [!DNL Azure Table Storage] インスタンスに接続するための接続文字列。 ATS インスタンスに接続するための接続文字列です。 ATS の接続文字列のパターンは `DefaultEndpointsProtocol=https;AccountName={ACCOUNT_NAME};AccountKey={ACCOUNT_KEY}` です。 |

基本について詳しくは、[ このドキュメント  [!DNL Azure Table Storage]  を参照してください ](https://docs.microsoft.com/en-us/azure/storage/common/storage-introduction)。

## [!DNL Azure Table Storage] アカウントを接続

必要な資格情報を収集したら、次の手順に従って ATS アカウントを [!DNL Platform] にリンクできます。

[Adobe Experience Platform](https://platform.adobe.com) にログインし、左側のナビゲーションバーから **[!UICONTROL ソース]** を選択して **[!UICONTROL ソース]** ワークスペースにアクセスします。 **[!UICONTROL カタログ]**&#x200B;画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

**[!UICONTROL データベース]** カテゴリで、「**[!UICONTROL Azure Table Storage]**」を選択します。 このコネクタを初めて使用する場合は、「**[!UICONTROL 設定]**」を選択します。 それ以外の場合は、「**[!UICONTROL データを追加]**」を選択して、新しい ATS コネクタを作成します。

![カタログ](../../../../images/tutorials/create/ats/catalog.png)

**[!UICONTROL Azure Table Storage に接続]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 新しいアカウント

新しい資格情報を使用している場合は、「**[!UICONTROL 新しいアカウント]**」を選択します。表示される入力フォームで、名前、説明（オプション）、ATS の認証情報を入力します。 終了したら「**[!UICONTROL 接続]**」を選択し、新しい接続が確立されるまでしばらく待ちます。

![ 接続 ](../../../../images/tutorials/create/ats/new.png)

### 既存のアカウント

既存のアカウントに接続するには、接続する ATS アカウントを選択し、**[!UICONTROL 次へ]** を選択して続行します。

![既存](../../../../images/tutorials/create/ats/existing.png)

## 次の手順

このチュートリアルでは、ATS アカウントとの接続を確立しました。 次のチュートリアルに進み、[ データをに取り込むためのデータフローの設定  [!DNL Platform]](../../dataflow/databases.md) を行いましょう。
