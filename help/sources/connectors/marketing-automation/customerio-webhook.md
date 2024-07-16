---
title: Customer.io Sourceの概要
description: API や Webhook を活用したユーザーインターフェイスを使用して Customer.io をAdobe Experience Platformに接続する方法について説明します
badge: ベータ版
exl-id: 0f4ee106-c22b-465c-9c5e-83709e8424f5
source-git-commit: e37c00863249e677f1645266859bf40fe6451827
workflow-type: tm+mt
source-wordcount: '375'
ht-degree: 17%

---

# [!DNL Customer.io]

>[!NOTE]
>
>[!DNL Customer.io] ソースはベータ版です。ベータラベル付きソースの使用について詳しくは、[ ソースの概要 ](../../home.md#terms-and-conditions) を参照してください。

Adobe Experience Platform を使用すると、外部ソースからデータを取り込みながら、Platform サービスを使用して受信データの構造化、ラベル付けおよび拡張を行うことができます。アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取り込むことができます。

Experience Platformは、ストリーミングアプリケーションからのデータ取り込みをサポートしています。 ストリーミングプロバイダーのサポートには、[!DNL Customer.io] が含まれます。

[[!DNL Customer.io]](https://customer.io/) は、データ駆動型メール、プッシュ通知、アプリ内メッセージ、SMS を作成して送信するコントロールと柔軟性を高めたいマーケター向けの自動メッセージングプラットフォームです。

[!DNL Customer.io] ソースを使用すると、[[!DNL Customer.io]  レポート Webhook](https://customer.io/docs/api/webhooks/) を使用して、サポートされている Webhook イベントスキーマとそれに関連するイベントデータを [!DNL Customer.io] から取り込むことができます。

サポートされている Webhook イベントスキーマは次のとおりです。

* 顧客イベント
* メールイベント
* SMS イベント
* プッシュ通知イベント
* アプリ内メッセージイベント
* Slackイベント
* Webhook イベント

Webhook を通じて使用できるイベントのリストについては、[[!DNL Customer.io] Webhook イベントのレポート ](https://customer.io/docs/webhooks/#events) ドキュメントを参照してください。

## 前提条件 {#prerequisites}

[!DNL Customer.io] ソース接続を作成する前に、次の点を確認する必要があります。

* [!DNL Customer.io] アカウント。 アカウントをお持ちでない場合は [[!DNL Customer.io]  登録ページ ](https://fly.customer.io/signup) を読み、アカウントを登録、作成してください。
* アカウントを作成したら、アカウントを検証する必要もあります。 [[!DNL Customer.io]  アカウントの検証 ](https://customer.io/docs/account-verification/) ページに記載されている手順に従って、プロセスを完了します。

### Webhook[!DNL Customer.io] 設定 {#set-up-webhook}

データフローが正常に作成されたら、イベントについて Platform に通知するレポート Webhook を設定する必要 [!DNL Customer.io] あります。 Webhook は、顧客属性が変更されたとき、またはユーザーがメッセージを開いたときにすぐに通知を受け取り、この情報を [!DNL Customer.io] ソースに送信できます。 詳しくは、[ ストリーミングエンドポイント URL の取得 ](../../tutorials/ui/create/marketing-automation/customerio-webhook.md#get-streaming-endpoint) および [Webhook の設定 ](../../tutorials/ui/create/marketing-automation/customerio-webhook.md#set-up-webhook) に関するチュ  [!DNL Customer.io]  トリアルを参照してください。

## Platform への [!DNL Customer.io] の接続 {#connect-to-platform}

以下のドキュメントでは、API やユーザーインターフェイスを使用してと接続するための [!DNL Customer.io] ストリーミング接続を作成する方法につ [!DNL Platform] て説明します。

### API を使用して [!DNL Customer.io] と Platform を接続する {#connect-to-platform-using-api}

* [ソース接続とデータフローを作成し、API を使用して Platform にデータを取り込みます  [!DNL Customer.io] ](../../tutorials/api/create/marketing-automation/customerio-webhook.md)

### UI を使用した [!DNL Customer.io] の Platform への接続 {#connect-to-platform-using-ui}

* [ユーザーインターフェイスを使用した、Platform にデータを取り込むソ  [!DNL Customer.io]  ス接続とデータフローの作成](../../tutorials/ui/create/marketing-automation/customerio-webhook.md)
