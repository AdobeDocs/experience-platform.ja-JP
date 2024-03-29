---
keywords: Experience Platform；ホーム；人気の高いトピック；Azure File Storage;Azure File Storage コネクタ
solution: Experience Platform
title: UI での Azure ファイルストレージソース接続の作成
type: Tutorial
description: Adobe Experience Platform UI を使用して Azure ファイルストレージソース接続を作成する方法を説明します。
exl-id: 25d483b6-3975-4e80-9dbe-28b7b91cb063
source-git-commit: ed92bdcd965dc13ab83649aad87eddf53f7afd60
workflow-type: tm+mt
source-wordcount: '470'
ht-degree: 49%

---

# の作成 [!DNL Azure File Storage] UI のソース接続

Adobe Experience Platform のソースコネクタには、外部ソースの データを設定したスケジュールに従って取り込む機能が用意されています。 このチュートリアルでは、 [!DNL Azure File Storage] を使用したソースコネクタ [!DNL Platform] ユーザーインターフェイス。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

- [[!DNL Experience Data Model (XDM)]  システム](../../../../../xdm/home.md)：[!DNL Experience Platform] が顧客体験データの整理に使用する標準化されたフレームワーク。
   - [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   - [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
- [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に有効な [!DNL Azure File Storage] 接続がある場合は、このドキュメントの残りの部分をスキップして、[データフローの設定](../../dataflow/batch/cloud-storage.md)に関するチュートリアルに進むことができます。

### 必要な認証情報の収集

を認証するために [!DNL Azure File Storage] ソースコネクタの場合、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `host` | のエンドポイント [!DNL Azure File Storage] アクセスするインスタンス。 |
| `userId` | 十分な [!DNL Azure File Storage] endpoint. |
| `password` | この [!DNL Azure File Storage] アクセスキー。 |

の導入について詳しくは、 [この [!DNL Azure File Storage] 文書](https://docs.microsoft.com/en-us/azure/storage/files/storage-how-to-use-files-windows).

## [!DNL Azure File Storage] アカウントの接続

必要な認証情報が揃ったら、次の手順に従って、[!DNL Azure File Storage] アカウントを [!DNL Platform] にリンクします。

にログインします。 [Adobe Experience Platform](https://platform.adobe.com) 次に、 **[!UICONTROL ソース]** 左側のナビゲーションバーから **[!UICONTROL ソース]** ワークスペース。 **[!UICONTROL カタログ]**&#x200B;画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

以下 **[!UICONTROL データベース]** カテゴリ、選択 **[!UICONTROL Azure ファイルストレージ]**. このコネクタを初めて使用する場合は、「 **[!UICONTROL 設定]**. それ以外の場合は、「 **[!UICONTROL データを追加]** 新しい [!DNL Azure File Storage] コネクタ。

![カタログ](../../../../images/tutorials/create/azure-file-storage/catalog.png)

この **[!UICONTROL Azure ファイルストレージに接続]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 新しいアカウント

新しい資格情報を使用している場合は、「**[!UICONTROL 新しいアカウント]**」を選択します。表示される入力フォームで、名前、説明（オプション）および [!DNL Azure File Storage] 資格情報。 終了したら、「 」を選択します。 **[!UICONTROL 接続]** その後、新しい接続が確立されるまでしばらく時間をかけます。

![接続](../../../../images/tutorials/create/azure-file-storage/new.png)

### 既存のアカウント

既存のアカウントに接続するには、 [!DNL Azure File Storage] 接続するアカウントを選択し、 **[!UICONTROL 次へ]** をクリックして続行します。

![既存](../../../../images/tutorials/create/azure-file-storage/existing.png)

## 次の手順

このチュートリアルでは、[!DNL Azure File Storage] アカウントとの接続を確立しました。次のチュートリアルに進み、 [データフローを設定して、クラウドストレージからにデータを取り込みます。 [!DNL Platform]](../../dataflow/batch/cloud-storage.md).
