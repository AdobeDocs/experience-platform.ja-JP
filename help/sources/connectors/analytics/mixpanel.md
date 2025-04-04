---
title: Mixpanel Source コネクタの概要
description: API またはユーザーインターフェイスを使用して Mixpanel をAdobe Experience Platformに接続する方法について説明します。
last-substantial-update: 2023-06-21T00:00:00Z
exl-id: 7eb605f6-8580-40b7-a9b3-96b9c3444f5d
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '445'
ht-degree: 17%

---

# [!DNL Mixpanel]

Adobe Experience Platformを使用すると、データを外部ソースから取得しながら、Experience Platform サービスを使用して、受信データの構造化、ラベル付け、拡張を行うことができます。 アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取り込むことができます。

Experience Platformは、サードパーティの分析アプリケーションからデータを取り込む機能を備えています。 Analytics プロバイダーのサポートには、[!DNL Mixpanel] が含まれます。

[[!DNL Mixpanel]](https://www.mixpanel.com) は、ユーザーによるデジタル製品とのやり取りに関するデータを取得できる製品分析ツールです。 Mixpanel では、数回クリックするだけでデータをクエリし視覚化できるシンプルでインタラクティブなレポートを使用して、この製品データを分析できます。

ソースは [Mixpanel イベント書き出し API/ダウンロード ](https://developer.mixpanel.com/reference/raw-event-export) を活用して、[!DNL Mixpanel] 内で受信および保存されたイベントデータと、すべてのイベントプロパティ（`distinct_id` を含む）およびイベントがExperience Platformに送信された正確なタイムスタンプをダウンロードします。 Mixpanel は、Mixpanel Event Export API と通信するための認証メカニズムとして Bearer トークンを使用します。

## IP アドレス許可リスト

ソースコネクタを操作する前に、IP アドレスのリストを許可リストに追加する必要があります。 地域固有の IP アドレスを許可リストに追加しないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下する場合があります。 詳しくは、[IP アドレスの許可リスト](../../ip-address-allow-list.md)ページを参照してください。

## [!DNL Mixpanel] アカウントの認証

この節では、アカウントを認証し、[!DNL Mixpanel] データをExperience Platformに取り込むために必要な前提条件の手順について説明します。

[!DNL Mixpanel] ソース接続とデータフローを作成するには、まず有効な [!DNL Mixpanel] アカウントが必要です。 有効な [!DNL Mixpanel] アカウントがない場合は、[Mixpanel 登録 ](https://mixpanel.com/register/) ページを参照してアカウントを作成します。

[!DNL Mixpanel] アカウントを正常に作成したら、[!DNL Mixpanel] UI の [!DNL Project Seettings] ページの「[!DNL Project Details]」タブに移動して、プロジェクト ID を取得し、タイムゾーンを設定します。

![mixpanel-project-settings](../../images/tutorials/create/mixpanel-export-events/mixpanel-project-settings.png)

次に、[!DNL Mixpanel] UI の [!DNL Project Settings] ページの「[!DNL Service Accounts]」タブに移動して、サービスアカウント資格情報を取得します。

>[!TIP]
>
>ベストプラクティスとして、「有効期限なし [ のサービスアカウントを選択し ](https://developer.mixpanel.com/reference/service-accounts#service-account-expiration) す。

![Mixpanel サービスアカウント ](../../images/tutorials/create/mixpanel-export-events/mixpanel-service-account.png)

最後に、[!DNL Mixpanel Event Export API] に必要なExperience Platform[ スキーマ ](../../../xdm/schema/composition.md) を作成します。 スキーマに必要なマッピングについて詳しくは、[UI でのソース接続の作成 ](../../tutorials/ui/create/analytics/mixpanel.md#additional-resources) に関するガイドを参照し  [!DNL Mixpanel]  ください。

![ スキーマを作成 ](../../images/tutorials/create/mixpanel-export-events/schema.png)

## API を使用した [!DNL Mixpanel] のExperience Platformへの接続

以下のドキュメントでは、API やユーザーインターフェイスを使用して [!DNL Mixpanel] をExperience Platformに接続する方法について説明しています。

* [Flow Service API を使用したソース接続とデータフロ  [!DNL Mixpanel]  の作成](../../tutorials/api/create/analytics/mixpanel.md)

## UI を使用した [!DNL Mixpanel] のExperience Platformへの接続

* [UI での  [!DNL Mixpanel]  ソース接続の作成](../../tutorials/ui/create/analytics/mixpanel.md)
* [UI でのカスタマーサクセスソース接続のデータフローの作成](../../tutorials/ui/dataflow/analytics.md)
