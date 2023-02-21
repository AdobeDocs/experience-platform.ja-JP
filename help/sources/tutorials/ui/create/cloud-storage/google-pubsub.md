---
title: UI で Google PubSub ソース接続を作成
description: Platform ユーザーインターフェイスを使用して、Google PubSub ソースコネクタを作成する方法を説明します。
exl-id: fb8411f2-ccae-4bb5-b1bf-52b1144534ed
source-git-commit: 2b72d384e8edd91c662364dfac31ce4edff79172
workflow-type: tm+mt
source-wordcount: '658'
ht-degree: 73%

---

# UI での [!DNL Google PubSub] ソース接続の作成

このチュートリアルでは、Platform ユーザーインターフェイスを使用して、[!DNL Google PubSub]（以下「[!DNL PubSub]」と呼びます）を作成する手順について説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [ソース](../../../../home.md)：Experience Platform を使用すると、データを様々なソースから取得しながら、Platform サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：Experience Platform には、単一の Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

既に有効な [!DNL PubSub] 接続がある場合は、このドキュメントの残りの部分をスキップして、[データフローの設定](../../dataflow/batch/cloud-storage.md)に関するチュートリアルに進むことができます。

### 必要な認証情報の収集

[!DNL PubSub] を Platform に接続するには、次の資格情報に対する有効な値を指定する必要があります。

| 認証情報 | 説明 |
| ---------- | ----------- |
| プロジェクト ID | [!DNL PubSub] の認証に必要なプロジェクト ID。 |
| 認証情報 | [!DNL PubSub] の認証に必要な資格情報または秘密鍵の ID。 |
| トピック ID | の ID [!DNL PubSub] メッセージのフィードを表すリソース。 トピック ID を指定する必要があるのは、 [!DNL Google PubSub] ソース。 |
| サブスクリプション ID | の ID [!DNL PubSub] 購読。 In [!DNL PubSub]を使用すると、購読を使用して、メッセージの公開先のトピックを購読することでメッセージを受け取ることができます。 |

これらの値について詳しくは、次の [PubSub 認証](https://cloud.google.com/pubsub/docs/authentication)ドキュメントを参照してください。サービスアカウントベースの認証を使用している場合、資格情報の生成手順については、次の [PubSub ガイド](https://cloud.google.com/docs/authentication/production#create_service_account)を参照してください。

>[!TIP]
>
>サービスアカウントベースの認証を使用している場合は、サービスアカウントに十分なユーザーアクセス権が付与され、資格情報をコピー＆ペーストする際に、JSON 内に余分な空白がないことを確認してください。

必要な資格情報を収集したら、以下の手順に従って [!DNL PubSub] アカウントを Platform にリンクできます。

## [!DNL PubSub] アカウントを接続

Platform UI の左側のナビゲーションバーで「**[!UICONTROL ソース]**」を選択し、[!UICONTROL ソース]ワークスペースにアクセスします。[!UICONTROL カタログ]画面には、アカウントを作成できる様々なソースが表示されます。。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

[!UICONTROL クラウドストレージ]カテゴリで、**[!UICONTROL Google PubSub]** を選択し、次に&#x200B;**[!UICONTROL データを追加]**&#x200B;を選択します。

![Experience PlatformUI のソースカタログ。](../../../../images/tutorials/create/google-pubsub/catalog.png)

**[!UICONTROL Google PubSub に接続]**&#x200B;ページが表示されます。このページでは、新しい資格情報または既存の資格情報を使用できます。

### 既存のアカウント

既存のアカウントを使用するには、新しいデータフローを作成する [!DNL PubSub] アカウントを選択し、「**[!UICONTROL 次へ]**」を選択して続行します。

![ソースワークフロー内の既存のアカウント選択。](../../../../images/tutorials/create/google-pubsub/existing.png)

### 新しいアカウント

新しいアカウントを作成する場合は、**[!UICONTROL 新しいアカウント]**&#x200B;を選択し、入力フォームに名前、説明（オプション）、[!DNL PubSub] 認証資格情報を入力します。この手順では、トピック ID を指定して、アカウントがアクセスできるデータを定義できます。 そのトピック ID に関連付けられている購読のみにアクセスできます。

>[!NOTE]
>
>パブサブプロジェクトに割り当てられたプリンシパル（ロール）は、 [!DNL PubSub] プロジェクト。 特定のトピックに対するアクセス権を持つプリンシパル（役割）を追加する場合は、そのプリンシパル（役割）もトピックの対応するサブスクリプションに追加する必要があります。 詳しくは、 [[!DNL PubSub] アクセス制御に関するドキュメント](https://cloud.google.com/pubsub/docs/access-control).

終了したら「**[!UICONTROL ソースに接続]**」を選択し、新しい接続が確立されるまでしばらく待ちます。

![ソースワークフローの新しいアカウントインターフェイス。](../../../../images/tutorials/create/google-pubsub/new.png)

## 次の手順

このチュートリアルでは、[!DNL PubSub] アカウントと Platform の間に接続を作成しました。次のチュートリアルに進み、[データフローを設定して、クラウドストレージから Platform にストリーミングデータを取り込みます](../../dataflow/streaming/cloud-storage-streaming.md)。
