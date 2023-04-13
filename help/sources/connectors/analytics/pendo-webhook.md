---
title: ペンドソースの概要
description: Web フックを活用して API またはユーザーインターフェイスを使用して Pendo をAdobe Experience Platformに接続する方法を説明します
badge: ベータ
last-substantial-update: 2023-03-29T00:00:00Z
exl-id: 376f18ef-1eea-4c42-8041-6fadb5906e9b
source-git-commit: 0cc4eab97dcd56d2b1d679cf5f35932976d22634
workflow-type: tm+mt
source-wordcount: '370'
ht-degree: 20%

---

# [!DNL Pendo]

>[!NOTE]
>
>[!DNL Pendo] ソースはベータ版です。詳しくは、 [ソースの概要](../../home.md#terms-and-conditions) ベータラベル付きのソースの使用に関する詳細

Adobe Experience Platform を使用すると、外部ソースからデータを取り込みながら、Platform サービスを使用して受信データの構造化、ラベル付けおよび拡張を行うことができます。アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取り込むことができます。

Experience Platformは、サードパーティの分析アプリケーションからデータを取り込む機能を提供しています。 Analytics プロバイダーのサポートには以下が含まれます。 [!DNL Pendo].

[[!DNL Pendo]](https://pendo.io/) は、ソフトウェア会社が顧客の共感を呼ぶ製品を開発するのに役立つように構築された、製品分析アプリです。 このアプリを使用すると、ソフトウェアメーカーは様々なツールを使用して製品を埋め込むことができ、ユーザーにとっての製品エクスペリエンスの向上と、製品チームの新しいインサイトの向上の両方につながります。

この [!DNL Pendo] ソースを使用すると、サポートされる webhook イベントスキーマとその関連イベントデータを [!DNL Pendo.io] の使用 [[!DNL Pendo] ウェブフック](https://support.pendo.io/hc/en-us/articles/360032285012-Webhooks). この [!DNL Pendo] ソースは [!DNL Pendo] URL Web フック。

次の Web フックがサポートされています。

* [ガイド](https://support.pendo.io/hc/en-us/articles/8146679315867-Creating-a-Guide) 表示済み
* [投票](https://support.pendo.io/hc/en-us/articles/360031867152-Polls-Classic-) 表示済み/送信済み
* [ネットプロモータースコア](https://support.pendo.io/hc/en-us/articles/360033527151-Set-up-an-NPS-Survey) 表示済み/送信済み

## 前提条件 {#prerequisites}

事前に [!DNL Pendo] ソース接続を使用する場合は、まず次の点を確認する必要があります。

A [!DNL Pendo] アカウント まだ [[!DNL Pendo] 登録](https://app.pendo.io/register) 登録してアカウントを作成するページ。

### 設定 [!DNL Pendo] ウェブフック {#set-up-webhook}

データフローを正常に作成したら、次の情報を Platform に知らせる Webhook を設定する必要があります。 [!DNL Pendo] イベント。 [!DNL Pendo] Web フックは、特定のイベントが発生した場合にリアルタイム通知を他のサービスにプッシュし、この情報を [!DNL Pendo] ソース。 詳しくは、 [ストリーミングエンドポイント URL の取得](../../tutorials/ui/create/analytics/pendo-webhook.md#get-streaming-endpoint) および [設定 [!DNL Pendo] ウェブフック](../../tutorials/ui/create/analytics/pendo-webhook.md#set-up-webhook).

## 接続中 [!DNL Pendo] Platform へ {#connect-to-platform}

以下のドキュメントでは、 [!DNL Pendo] 接続するストリーミングコネクタ [!DNL Platform] API またはユーザーインターフェイスを使用する場合：

### API を使用して [!DNL Pendo] と Platform を接続する {#connect-to-platform-using-api}

* [ソース接続を作成し [!DNL Pendo] 、API を使用して Platform にデータを取得します。](../../tutorials/api/create/analytics/pendo-webhook.md)

### UI を使用した [!DNL Pendo] の Platform への接続 {#connect-to-platform-using-ui}

* [ソース接続を作成して [!DNL Pendo] ユーザーインターフェイスを使用した Platform へのデータの取得](../../tutorials/ui/create/analytics/pendo-webhook.md)
