---
keywords: Experience Platform；ホーム；人気の高いトピック；Google PubSub;google pubsub
solution: Experience Platform
title: UI でのGoogle PubSub ソース接続の作成
topic-legacy: overview
type: Tutorial
description: Platform ユーザーインターフェイスを使用してGoogle PubSub ソースコネクタを作成する方法を説明します。
exl-id: fb8411f2-ccae-4bb5-b1bf-52b1144534ed
source-git-commit: da7b6fe8f9d274b8e5f27138a1baf8caf63a0c01
workflow-type: tm+mt
source-wordcount: '493'
ht-degree: 9%

---

# の作成 [!DNL Google PubSub] UI のソース接続

このチュートリアルでは、 [!DNL Google PubSub] （以下「」という。）[!DNL PubSub]」) を使用して、Platform ユーザーインターフェイスを使用します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [ソース](../../../../home.md):Experience Platformを使用すると、様々なソースからデータを取り込みながら、Platform サービスを使用して、受信データの構造化、ラベル付け、拡張をおこなうことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：Experience Platform は、単一の Platform インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

既に有効な [!DNL PubSub] 接続する場合は、このドキュメントの残りの部分をスキップして、 [データフローの設定](../../dataflow/batch/cloud-storage.md).

### 必要な資格情報の収集

接続するには [!DNL PubSub] を Platform に設定するには、次の資格情報に対して有効な値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `projectId` | 認証に必要なプロジェクト ID [!DNL PubSub]. |
| `credentials` | 認証に必要な秘密鍵または秘密鍵の ID [!DNL PubSub]. |

これらの値の詳細については、次を参照してください。 [PubSub 認証](https://cloud.google.com/pubsub/docs/authentication) 文書。 サービスアカウントベースの認証を使用している場合は、次を参照してください [PubSub ガイド](https://cloud.google.com/docs/authentication/production#create_service_account) 資格情報の生成手順については、を参照してください。

>[!TIP]
>
>サービスアカウントベースの認証を使用している場合は、資格情報をコピー&amp;ペーストする際に、サービスアカウントへの十分なユーザーアクセス権が付与され、JSON に余分な空白がないことを確認します。

必要な資格情報を収集したら、次の手順に従って、 [!DNL PubSub] アカウントを Platform に送信します。

## 接続 [!DNL PubSub] アカウント

内 [Platform UI](https://platform.adobe.com)を選択します。 **[!UICONTROL ソース]** 左側のナビゲーションバーから [!UICONTROL ソース] ワークスペース。 この [!UICONTROL カタログ] 画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、検索バーを使用して、作業対象の特定のソースを見つけることもできます。

以下 [!UICONTROL クラウドストレージ] カテゴリ、選択 **[!UICONTROL Google PubSub]**&#x200B;を選択し、 **[!UICONTROL データを追加]**.

![カタログ](../../../../images/tutorials/create/google-pubsub/catalog.png)

この **[!UICONTROL Google PubSub に接続]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 既存のアカウント

既存のアカウントを使用するには、 [!DNL PubSub] 新しいデータフローを作成するアカウントを選択し、 **[!UICONTROL 次へ]** をクリックして続行します。

![既存](../../../../images/tutorials/create/google-pubsub/existing.png)

### 新しいアカウント

新しいアカウントを作成する場合は、「 **[!UICONTROL 新しいアカウント]**&#x200B;をクリックし、名前、説明（オプション）、 [!DNL PubSub] 認証資格情報を入力フォームに設定します。 終了したら、「 」を選択します。 **[!UICONTROL ソースに接続]** その後、新しい接続が確立されるまでしばらく時間をかけます。

![新規](../../../../images/tutorials/create/google-pubsub/new.png)

## 次の手順

このチュートリアルでは、 [!DNL PubSub] アカウントとプラットフォーム。 次のチュートリアルに進み、 [データフローを設定して、クラウドストレージから Platform にストリーミングデータを取り込みます。](../../dataflow/streaming/cloud-storage-streaming.md).
