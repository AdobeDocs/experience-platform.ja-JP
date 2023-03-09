---
title: Customer.io ソースの概要
description: Web フックを活用して、API またはユーザーインターフェイスを使用して Customer.io をAdobe Experience Platformに接続する方法を説明します
hide: true
hidefromtoc: true
badge: "ベータ"
source-git-commit: f92a42a5d53121cc3338432a3cd975f0aa29b9a8
workflow-type: tm+mt
source-wordcount: '389'
ht-degree: 16%

---

# [!DNL Customer.io]

>[!NOTE]
>
>[!DNL Customer.io] ソースはベータ版です。詳しくは、 [ソースの概要](../../home.md#terms-and-conditions) ベータラベル付きのソースの使用に関する詳細

Adobe Experience Platform を使用すると、外部ソースからデータを取り込みながら、Platform サービスを使用して受信データの構造化、ラベル付けおよび拡張を行うことができます。アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取り込むことができます。

Experience Platformは、ストリーミングアプリケーションからデータを取り込む機能を備えています。 ストリーミングプロバイダーのサポートには以下が含まれます。 [!DNL Customer.io].

[[!DNL Customer.io]](https://customer.io/) は、データ駆動型の E メール、プッシュ通知、アプリ内メッセージおよび SMS をより制御し柔軟に作成して送信できるようにする、マーケター向けの自動メッセージングプラットフォームです。

この [!DNL Customer.io] ソースを使用すると、サポートされる webhook イベントスキーマとその関連イベントデータを [!DNL Customer.io] の使用 [[!DNL Customer.io] ウェブフックのレポート](https://customer.io/docs/api/webhooks/).

サポートされる Webhook イベントスキーマは次のとおりです。

* 顧客イベント
* メールイベント
* SMS イベント
* プッシュ通知イベント
* アプリ内メッセージイベント
* Slackイベント
* Webhook イベント

Web フックで使用できるイベントのリストについては、 [[!DNL Customer.io] Webhook イベントのレポート](https://customer.io/docs/webhooks/#events) ドキュメント。

## 前提条件 {#prerequisites}

事前に [!DNL Customer.io] ソース接続を使用する場合は、まず次の点を確認する必要があります。

* A [!DNL Customer.io] アカウント もし誰も読んでいないなら [[!DNL Customer.io] 登録ページ](https://fly.customer.io/signup) をクリックして、アカウントを登録および作成します。
* アカウントを作成したら、アカウントの検証もおこなう必要があります。 次に示す手順に従います： [[!DNL Customer.io] アカウントの検証](https://customer.io/docs/account-verification/) ページを開き、プロセスを完了します。

### 設定 [!DNL Customer.io] ウェブフック {#set-up-webhook}

データフローを正常に作成したら、次の情報を Platform に知らせるレポート Webhook を設定する必要があります。 [!DNL Customer.io] イベント。 Web フックは、顧客属性が変更されたときや、メッセージが開かれたときに即座に通知し、この情報を [!DNL Customer.io] ソース。 詳しくは、 [ストリーミングエンドポイント URL の取得](../../tutorials/ui/create/marketing-automation/customerio-webhook.md#get-streaming-endpoint) および [設定 [!DNL Customer.io] ウェブフック](../../tutorials/ui/create/marketing-automation/customerio-webhook.md#set-up-webhook).

## 接続中 [!DNL Customer.io] Platform へ {#connect-to-platform}

以下のドキュメントでは、 [!DNL Customer.io] 接続するストリーミング接続 [!DNL Platform] API またはユーザーインターフェイスを使用する場合：

### API を使用して [!DNL Customer.io] と Platform を接続する {#connect-to-platform-using-api}

* [ソース接続とデータフローを作成して、 [!DNL Customer.io] API を使用して Platform にデータを送信する方法について説明します。](../../tutorials/api/create/marketing-automation/customerio-webhook.md)

### UI を使用した [!DNL Customer.io] の Platform への接続 {#connect-to-platform-using-ui}

* [ソース接続とデータフローを作成して、 [!DNL Customer.io] ユーザーインターフェイスを使用した Platform へのデータの取得](../../tutorials/ui/create/marketing-automation/customerio-webhook.md)

