---
keywords: Experience Platform；ホーム；人気のあるトピック；Google PubSub;google pubsub
solution: Experience Platform
title: UI でのGoogle PubSub ソース接続の作成
topic-legacy: overview
type: Tutorial
description: Platform ユーザーインターフェイスを使用してGoogle PubSub ソースコネクタを作成する方法を説明します。
exl-id: fb8411f2-ccae-4bb5-b1bf-52b1144534ed
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '509'
ht-degree: 9%

---

# UI での [!DNL Google PubSub] ソース接続の作成

>[!NOTE]
>
> [!DNL Google PubSub] コネクタはベータ版です。 ベータラベルのコネクタの使用について詳しくは、「[ ソースの概要 ](../../../../home.md#terms-and-conditions)」を参照してください。

このチュートリアルでは、Platform のユーザーインターフェイスを使用して [!DNL Google PubSub]（以下「[!DNL PubSub]」と呼びます）を作成する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [ソース](../../../../home.md):Experience Platformを使用すると、様々なソースからデータを取り込みながら、Platform サービスを使用して、受信データの構造化、ラベル付け、拡張をおこなうことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：Experience Platform は、単一の Platform インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

既に有効な [!DNL PubSub] 接続がある場合は、このドキュメントの残りの部分をスキップし、[ データフローの設定 ](../../dataflow/batch/cloud-storage.md) に関するチュートリアルに進んでください。

### 必要な資格情報の収集

[!DNL PubSub] を Platform に接続するには、次の資格情報の有効な値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `projectId` | [!DNL PubSub] の認証に必要なプロジェクト ID です。 |
| `credentials` | [!DNL PubSub] の認証に必要な秘密鍵証明書または秘密鍵の ID。 |

これらの値の詳細については、次の [PubSub 認証 ](https://cloud.google.com/pubsub/docs/authentication) ドキュメントを参照してください。 サービスアカウントベースの認証を使用している場合、資格情報の生成手順については、次の [PubSub ガイド ](https://cloud.google.com/docs/authentication/production#create_service_account) を参照してください。

>[!TIP]
>
>サービスアカウントベースの認証を使用している場合、資格情報をコピー&amp;ペーストする際に、サービスアカウントへの十分なユーザーアクセス権が付与され、JSON に余分な空白がないことを確認します。

必要な資格情報を収集したら、以下の手順に従って [!DNL PubSub] アカウントを Platform にリンクできます。

## [!DNL PubSub] アカウントに接続

[Platform UI](https://platform.adobe.com) で、左側のナビゲーションバーから「**[!UICONTROL ソース]**」を選択して、「[!UICONTROL  ソース ]」ワークスペースにアクセスします。 [!UICONTROL  カタログ ] 画面には、アカウントを作成するための様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、検索バーを使用して、作業対象の特定のソースを見つけることもできます。

「[!UICONTROL  クラウドストレージ ]」カテゴリで、「**[!UICONTROL Google PubSub]**」を選択し、「**[!UICONTROL データを追加]**」を選択します。

![カタログ](../../../../images/tutorials/create/google-pubsub/catalog.png)

「**[!UICONTROL Google PubSub に接続]**」ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 既存のアカウント

既存のアカウントを使用するには、新しいデータフローを作成する [!DNL PubSub] アカウントを選択し、「**[!UICONTROL 次へ]**」を選択して次に進みます。

![既存](../../../../images/tutorials/create/google-pubsub/existing.png)

### 新規アカウント

新しいアカウントを作成する場合は、「**[!UICONTROL 新しいアカウント]**」を選択し、名前、説明（オプション）、[!DNL PubSub] 認証資格情報を入力フォームに入力します。 終了したら、[**[!UICONTROL ソースに接続]**] を選択し、新しい接続が確立されるまでしばらく時間をかけます。

![新規](../../../../images/tutorials/create/google-pubsub/new.png)

## 次の手順

このチュートリアルでは、[!DNL PubSub] アカウントと Platform の間に接続を作成します。 次のチュートリアルに進み、クラウドストレージから Platform](../../dataflow/streaming/cloud-storage-streaming.md) にストリーミングデータを取り込むように [ データフローを設定します。
