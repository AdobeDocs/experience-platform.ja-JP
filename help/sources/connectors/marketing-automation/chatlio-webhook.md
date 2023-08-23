---
title: Chatlio ソースの概要
description: Web フックを活用して API またはユーザーインターフェイスを使用して Chatlio をAdobe Experience Platformに接続する方法を説明します
exl-id: 4a71d1dc-e0eb-443e-a956-8caa0e82fa18
source-git-commit: 68c14d7b187075b4af6b019a8bd1ca2625beabde
workflow-type: tm+mt
source-wordcount: '346'
ht-degree: 20%

---

# [!DNL Chatlio]

Adobe Experience Platform を使用すると、外部ソースからデータを取り込みながら、Platform サービスを使用して受信データの構造化、ラベル付けおよび拡張を行うことができます。アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取り込むことができます。

Experience Platformは、ストリーミングアプリケーションからデータを取り込む機能を備えています。 ストリーミングプロバイダーのサポートには以下が含まれます。 [!DNL Chatlio].

[[!DNL Chatlio]](https://chatlio.com/) は、と完全に統合されたライブチャットアプリです。 [!DNL Slack] およびは、複数のサポートエージェントが個々のサイト訪問者を同時に支援するのを容易にします。 [!DNL Chatlio] は [!DNL Chatio Zapier App] 接続する [!DNL Chatlio] 2,000 を超える異なるアプリやサービスを使用して

The [!DNL Chatlio] ソースを使用すると、サポートされる webhook イベントスキーマとそれに関連するイベントデータを [!DNL Chatlio.com] の使用 [[!DNL Chatlio] ウェブフック](https://chatlio.com/docs/webhooks/).

次の Web フックがサポートされています。

* チャットトランスクリプトの書き出し
* オフラインメッセージの書き出し
* 新しい会話が開始されました
* 訪問者は時間内に回答を受け取りませんでした
* 訪問者がチャットの後にフィードバックを残しました

## 前提条件 {#prerequisites}

事前に [!DNL Chatlio] ソース接続を使用する場合は、まず次の点を確認する必要があります。

* A [!DNL Chatlio] アカウント。 まだ [[!DNL Chatlio] 登録ページ](https://chatlio.com/app/#/signup) をクリックして、アカウントを登録および作成します。
* アカウントの登録が完了したら、 [[!DNL Chatlio] 設定ドキュメント](https://chatlio.com/docs/setup/) をクリックして、アカウントの設定を完了します。

### 設定 [!DNL Chatlio] webhook {#set-up-webhook}

データフローを正常に作成したら、次の情報を Platform に知らせる Webhook を設定する必要があります。 [!DNL Chatlio] イベント。 顧客属性が変更されたときや、ユーザーがメッセージを開いてこの情報をユーザーに送信したときに、Web フックから即座に通知される場合があります [!DNL Chatlio] ソース。

詳しくは、 [ストリーミングエンドポイント URL の取得](../../tutorials/ui/create/marketing-automation/chatlio-webhook.md#get-streaming-endpoint) および [設定 [!DNL Chatlio] ウェブフック](../../tutorials/ui/create/marketing-automation/chatlio-webhook.md#set-up-webhook).

## [!DNL Chatlio] を Platform に接続 {#connect-to-platform}

以下のドキュメントでは、 [!DNL Chatlio] 接続するストリーミングコネクタ [!DNL Platform] API またはユーザーインターフェイスを使用する場合：

### API を使用して [!DNL Chatlio] と Platform を接続する {#connect-to-platform-using-api}

* [ソース接続を作成し [!DNL Chatlio] 、API を使用して Platform にデータを取得します。](../../tutorials/api/create/marketing-automation/chatlio-webhook.md)

### UI を使用した [!DNL Chatlio] の Platform への接続 {#connect-to-platform-using-ui}

* [ソース接続を作成して [!DNL Chatlio] ユーザーインターフェイスを使用した Platform へのデータの取得](../../tutorials/ui/create/marketing-automation/chatlio-webhook.md)
