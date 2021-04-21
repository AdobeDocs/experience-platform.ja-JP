---
keywords: Experience Platform；ホーム；人気のあるトピック；Google PubSub;google pubsub
solution: Experience Platform
title: UIでのGoogle PubSub Source Connectionの作成
topic-legacy: overview
type: Tutorial
description: プラットフォームユーザーインターフェイスを使用してGoogle PubSubソースコネクタを作成する方法を説明します。
exl-id: fb8411f2-ccae-4bb5-b1bf-52b1144534ed
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '509'
ht-degree: 9%

---

# UIに[!DNL Google PubSub]ソース接続を作成する

>[!NOTE]
>
> [!DNL Google PubSub]コネクタはベータ版です。 ベータラベル付きコネクタの使用方法の詳細については、[ソースの概要](../../../../home.md#terms-and-conditions)を参照してください。

このチュートリアルでは、プラットフォームのユーザーインターフェイスを使用して[!DNL Google PubSub]（以下「[!DNL PubSub]」と呼びます）を作成する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [ソース](../../../../home.md):Experience Platformを使用すると、様々なソースからデータを取り込むことができ、Platform Servicesを使用して、データの構造化、ラベル付け、および入力データの拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：Experience Platform は、単一の Platform インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

既に有効な[!DNL PubSub]接続がある場合は、このドキュメントの残りの部分をスキップして、[データフローの設定](../../dataflow/batch/cloud-storage.md)のチュートリアルに進むことができます。

### 必要な資格情報の収集

[!DNL PubSub]をプラットフォームに接続するには、次の資格情報に対して有効な値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `projectId` | [!DNL PubSub]の認証に必要なプロジェクトID。 |
| `credentials` | [!DNL PubSub]の認証に必要な秘密鍵または秘密鍵ID。 |

これらの値について詳しくは、次の[PubSub authentication](https://cloud.google.com/pubsub/docs/authentication)ドキュメントを参照してください。 サービスアカウントベースの認証を使用している場合は、資格情報の生成方法に関する手順について、次の[PubSubガイド](https://cloud.google.com/docs/authentication/production#create_service_account)を参照してください。

>[!TIP]
>
>サービスアカウントベースの認証を使用している場合は、秘密鍵証明書をコピーして貼り付ける際に、サービスアカウントへの十分なユーザーアクセス権限を付与済みで、JSONに余分な空白がないことを確認してください。

必要な資格情報を収集したら、次の手順に従って[!DNL PubSub]アカウントをプラットフォームにリンクできます。

## [!DNL PubSub]アカウントに接続

[プラットフォームUI](https://platform.adobe.com)で、左のナビゲーションバーから「**[!UICONTROL ソース]**」を選択して、[!UICONTROL ソース]ワークスペースにアクセスします。 [!UICONTROL カタログ]画面には様々なソースが表示され、このソースを使用してアカウントを作成できます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、検索バーを使用して、使用する特定のソースを見つけることもできます。

[!UICONTROL クラウドストレージ]カテゴリの下で、**[!UICONTROL Google PubSub]**&#x200B;を選択し、**[!UICONTROL 追加data]**&#x200B;を選択します。

![カタログ](../../../../images/tutorials/create/google-pubsub/catalog.png)

**[!UICONTROL Google PubSubに接続]**&#x200B;ページが表示されます。 このページでは、新しい秘密鍵証明書または既存の秘密鍵証明書を使用できます。

### 既存のアカウント

既存のアカウントを使用するには、新しいデータフローを作成する[!DNL PubSub]アカウントを選択し、**[!UICONTROL 次へ]**&#x200B;を選択して次に進みます。

![既存の](../../../../images/tutorials/create/google-pubsub/existing.png)

### 新しいアカウント

新しいアカウントを作成する場合は、「**[!UICONTROL 新しいアカウント]**」を選択し、名前、オプションの説明、および入力フォームでの[!DNL PubSub]認証資格情報を入力します。 終了したら、[**[!UICONTROL ソースに接続]**]を選択し、新しい接続が確立されるまでの時間を許可します。

![新規](../../../../images/tutorials/create/google-pubsub/new.png)

## 次の手順

このチュートリアルに従うと、[!DNL PubSub]アカウントとプラットフォームの間に接続を作成できます。 次のチュートリアルに進み、[データフローを設定して、クラウドストレージからのストリーミングデータをプラットフォーム](../../dataflow/streaming/cloud-storage-streaming.md)に取り込むことができます。
