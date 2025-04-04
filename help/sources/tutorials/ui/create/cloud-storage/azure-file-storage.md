---
keywords: Experience Platform；ホーム；人気のトピック；Azure File Storage;Azure File Storage コネクタ
solution: Experience Platform
title: UI での Azure File Storage Source接続の作成
type: Tutorial
description: Adobe Experience Platform UI を使用して Azure ファイルストレージソース接続を作成する方法を説明します。
exl-id: 25d483b6-3975-4e80-9dbe-28b7b91cb063
source-git-commit: b48c24ac032cbf785a26a86b50a669d7fcae5d97
workflow-type: tm+mt
source-wordcount: '463'
ht-degree: 46%

---

# UI での [!DNL Azure File Storage] ソース接続の作成

Adobe Experience PlatformのSource コネクタには、外部ソースのデータをスケジュールに従って取り込む機能が用意されています。 このチュートリアルでは、[!DNL Experience Platform] ユーザーインターフェイスを使用して [!DNL Azure File Storage] ソースコネクタを認証する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

- [[!DNL Experience Data Model (XDM)]  システム](../../../../../xdm/home.md)：[!DNL Experience Platform] が顧客体験データの整理に使用する標準化されたフレームワーク。
   - [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   - [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
- [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に有効な [!DNL Azure File Storage] 接続がある場合は、このドキュメントの残りの部分をスキップして、[データフローの設定](../../dataflow/batch/cloud-storage.md)に関するチュートリアルに進むことができます。

### 必要な資格情報の収集

[!DNL Azure File Storage] ソースコネクタを認証するには、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `host` | アクセスする [!DNL Azure File Storage] インスタンスのエンドポイント。 |
| `userId` | [!DNL Azure File Storage] エンドポイントに十分なアクセス権を持つユーザー。 |
| `password` | [!DNL Azure File Storage] アクセスキー。 |

基本について詳しくは、[ このドキュメント  [!DNL Azure File Storage]  を参照してください ](https://docs.microsoft.com/en-us/azure/storage/files/storage-how-to-use-files-windows)。

## [!DNL Azure File Storage] アカウントの接続

必要な資格情報が揃ったら、次の手順に従って、[!DNL Azure File Storage] アカウントを [!DNL Experience Platform] にリンクします。

[Adobe Experience Platform](https://platform.adobe.com) にログインし、左側のナビゲーションバーから **[!UICONTROL ソース]** を選択して **[!UICONTROL ソース]** ワークスペースにアクセスします。 **[!UICONTROL カタログ]**&#x200B;画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

**[!UICONTROL データベース]** カテゴリで、「**[!UICONTROL Azure File Storage]**」を選択します。 このコネクタを初めて使用する場合は、「**[!UICONTROL 設定]**」を選択します。 それ以外の場合は、「**[!UICONTROL データを追加]**」を選択して、新しい [!DNL Azure File Storage] コネクタを作成します。

![カタログ](../../../../images/tutorials/create/azure-file-storage/catalog.png)

**[!UICONTROL Azure ファイルストレージに接続]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 新しいアカウント

新しい資格情報を使用している場合は、「**[!UICONTROL 新しいアカウント]**」を選択します。表示される入力フォームで、名前、説明（オプション）、[!DNL Azure File Storage] 資格情報を入力します。 終了したら「**[!UICONTROL 接続]**」を選択し、新しい接続が確立されるまでしばらく待ちます。

![ 接続 ](../../../../images/tutorials/create/azure-file-storage/new.png)

### 既存のアカウント

既存のアカウントに接続するには、接続する [!DNL Azure File Storage] アカウントを選択し、「**[!UICONTROL 次へ]**」を選択して続行します。

![既存](../../../../images/tutorials/create/azure-file-storage/existing.png)

## 次の手順

このチュートリアルでは、[!DNL Azure File Storage] アカウントとの接続を確立しました。次のチュートリアルに進み、[ クラウドストレージからデータをに取り込むためのデータフローの設定  [!DNL Experience Platform]](../../dataflow/batch/cloud-storage.md) を行いましょう。
