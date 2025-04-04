---
title: Pendo Sourceの概要
description: API や Webhook を使ったユーザーインターフェイスを使ってAdobe Experience Platformに Pendo を接続する方法を学ぶ
badge: ベータ版
exl-id: 376f18ef-1eea-4c42-8041-6fadb5906e9b
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '354'
ht-degree: 7%

---

# [!DNL Pendo]

>[!NOTE]
>
>[!DNL Pendo] ソースはベータ版です。ベータラベル付きソースの使用について詳しくは、[ ソースの概要 ](../../home.md#terms-and-conditions) を参照してください。

Adobe Experience Platformを使用すると、データを外部ソースから取得しながら、Experience Platform サービスを使用して、受信データの構造化、ラベル付け、拡張を行うことができます。 アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取り込むことができます。

Experience Platformでは、サードパーティの分析アプリケーションからデータを取り込むことができます。 Analytics プロバイダーのサポートには、[!DNL Pendo] が含まれます。

[[!DNL Pendo]](https://pendo.io/) は、ソフトウェア企業が顧客の共感を得られる製品を開発するのに役立つように構築された製品分析アプリです。 このアプリを使用すると、ソフトウェアメーカーは、ユーザーの製品体験の向上と製品チームの新しいインサイトの両方につながる可能性のある幅広いツールを製品に埋め込むことができます。

[!DNL Pendo] ソースを使用すると、[[!DNL Pendo] Webhook](https://support.pendo.io/hc/en-us/articles/360032285012-Webhooks) を使用して、サポートされている Webhook イベントスキーマとそれに関連するイベントデータを [!DNL Pendo.io] から取り込むことができます。 [!DNL Pendo] ソースは、URL Webhook[!DNL Pendo] 使用します。

サポートされている Webhook は次の通りです。

* [ ガイド ](https://support.pendo.io/hc/en-us/articles/8146679315867-Creating-a-Guide) が表示されました
* [ 投票 ](https://support.pendo.io/hc/en-us/articles/360031867152-Polls-Classic-) 表示/送信
* [ ネットプロモータースコア ](https://support.pendo.io/hc/en-us/articles/360033527151-Set-up-an-NPS-Survey) 表示/送信済み

## 前提条件 {#prerequisites}

[!DNL Pendo] ソース接続を作成する前に、次の点を確認する必要があります。

[!DNL Pendo] アカウント。 アカウントをまだお持ちでない場合は、アカウントを登録および作成するための [[!DNL Pendo]  登録 ](https://app.pendo.io/register) ページが表示されます。

### Webhook[!DNL Pendo] 設定 {#set-up-webhook}

データフローが正常に作成されたら、イベントについてExperience Platformに通知する Webhook を設定する必要 [!DNL Pendo] あります。 [!DNL Pendo] Webhook は、特定のイベントが発生したときにリアルタイム通知を他のサービスにプッシュし、この情報を [!DNL Pendo] ソースに送信できます。 詳しくは、[ ストリーミングエンドポイント URL の取得 ](../../tutorials/ui/create/analytics/pendo-webhook.md#get-streaming-endpoint) および [Webhook の設定 ](../../tutorials/ui/create/analytics/pendo-webhook.md#set-up-webhook) に関するチュ  [!DNL Pendo]  トリアルを参照してください。

## Experience Platformへの [!DNL Pendo] の接続 {#connect-to-platform}

以下のドキュメントでは、API またはユーザーインターフェイスを使用してと接続する、[!DNL Pendo] ストリーミングコネクタを作成する方法に関する情報を提供し [!DNL Experience Platform] す。

### API を使用した [!DNL Pendo] のExperience Platformへの接続 {#connect-to-platform-using-api}

* [ソース接続を作成し、API を使用してExperience Platformにデータを取り込みます  [!DNL Pendo] ](../../tutorials/api/create/analytics/pendo-webhook.md)

### UI を使用した [!DNL Pendo] のExperience Platformへの接続 {#connect-to-platform-using-ui}

* [ユーザーインターフェイスを使用した  [!DNL Pendo]  ソース接続を作成し、データをExperience Platformに取り込みます](../../tutorials/ui/create/analytics/pendo-webhook.md)
