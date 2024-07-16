---
title: Chatlio Sourceの概要
description: API や Webhook を活用したユーザーインターフェイスを使用して Chatlio をAdobe Experience Platformに接続する方法について説明します
badge: ベータ版
exl-id: 4a71d1dc-e0eb-443e-a956-8caa0e82fa18
source-git-commit: 8de45a54607bed17fd79bbed693666beb09c0502
workflow-type: tm+mt
source-wordcount: '355'
ht-degree: 18%

---

# [!DNL Chatlio]

>[!NOTE]
>
>[!DNL Chatlio] ソースはベータ版です。ベータラベル付きソースの使用について詳しくは、[ ソースの概要 ](../../home.md#terms-and-conditions) を参照してください。

Adobe Experience Platform を使用すると、外部ソースからデータを取り込みながら、Platform サービスを使用して受信データの構造化、ラベル付けおよび拡張を行うことができます。アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取り込むことができます。

Experience Platformは、ストリーミングアプリケーションからのデータ取り込みをサポートしています。 ストリーミングプロバイダーのサポートには、[!DNL Chatlio] が含まれます。

[[!DNL Chatlio]](https://chatlio.com/) は、[!DNL Slack] と完全に統合され、複数のサポートエージェントが個々のサイト訪問者を同時に支援できるようにするライブチャットアプリです。 [!DNL Chatlio] は [!DNL Chatio Zapier App] を使用して、2,000 を超える異なるアプリやサービスと [!DNL Chatlio] を接続します。

[!DNL Chatlio] ソースを使用すると、[[!DNL Chatlio] Webhook](https://chatlio.com/docs/webhooks/) を使用して、サポートされている Webhook イベントスキーマとそれに関連するイベントデータを [!DNL Chatlio.com] から取り込むことができます。

サポートされている Webhook は次の通りです。

* chat トランスクリプトのエクスポート
* オフラインメッセージのエクスポート
* 新しい会話が開始されました
* 訪問者は、時間内に回答を得られませんでした
* 訪問者がチャット後にフィードバックを残しました

## 前提条件 {#prerequisites}

[!DNL Chatlio] ソース接続を作成する前に、次の点を確認する必要があります。

* [!DNL Chatlio] アカウント。 アカウントをまだお持ちでない場合は、[[!DNL Chatlio]  サインアップページ ](https://chatlio.com/app/#/signup) にアクセスし、アカウントを登録、作成してください。
* アカウントの登録が完了したら、[[!DNL Chatlio]  設定ドキュメント ](https://chatlio.com/docs/setup/) に従ってアカウントの設定を完了します。

### Webhook[!DNL Chatlio] 設定 {#set-up-webhook}

データフローが正常に作成されたら、イベントについて Platform に通知する Webhook を設定する必要 [!DNL Chatlio] あります。 Webhook は、顧客属性が変更されたとき、またはユーザーがメッセージを開いてこの情報を [!DNL Chatlio] ソースに送信したときにすぐに通知します。

詳しくは、[ ストリーミングエンドポイント URL の取得 ](../../tutorials/ui/create/marketing-automation/chatlio-webhook.md#get-streaming-endpoint) および [Webhook の設定 ](../../tutorials/ui/create/marketing-automation/chatlio-webhook.md#set-up-webhook) に関するチュ  [!DNL Chatlio]  トリアルを参照してください。

## [!DNL Chatlio] を Platform に接続 {#connect-to-platform}

以下のドキュメントでは、API またはユーザーインターフェイスを使用してと接続する、[!DNL Chatlio] ストリーミングコネクタを作成する方法に関する情報を提供し [!DNL Platform] す。

### API を使用して [!DNL Chatlio] と Platform を接続する {#connect-to-platform-using-api}

* [API を使用してデータを Platform に取り込む  [!DNL Chatlio]  めのソース接続を作成します。](../../tutorials/api/create/marketing-automation/chatlio-webhook.md)

### UI を使用した [!DNL Chatlio] の Platform への接続 {#connect-to-platform-using-ui}

* [ユーザーインターフェイスを使用した、Platform にデータを取り込むソ  [!DNL Chatlio]  ス接続の作成](../../tutorials/ui/create/marketing-automation/chatlio-webhook.md)
