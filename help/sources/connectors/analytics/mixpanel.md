---
keywords: Experience Platform;ホーム;人気のトピック;
title: （ベータ版）Mixpanel Source Connector の概要
description: API またはユーザーインターフェイスを使用して Mixpanel をAdobe Experience Platformに接続する方法を説明します。
exl-id: 7eb605f6-8580-40b7-a9b3-96b9c3444f5d
source-git-commit: e37c00863249e677f1645266859bf40fe6451827
workflow-type: tm+mt
source-wordcount: '472'
ht-degree: 30%

---

# （ベータ版）[!DNL Mixpanel]

>[!NOTE]
>
>[!DNL Mixpanel] ソースはベータ版です。詳しくは、 [ソースの概要](../../home.md#terms-and-conditions) ベータラベル付きのソースの使用に関する詳細

Adobe Experience Platform を使用すると、外部ソースからデータを取り込みながら、Platform サービスを使用して受信データの構造化、ラベル付けおよび拡張を行うことができます。アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取り込むことができます。

Experience Platformは、サードパーティの分析アプリケーションからデータを取り込む機能を提供しています。 Analytics プロバイダーのサポートには以下が含まれます。 [!DNL Mixpanel].

[[!DNL Mixpanel]](https://www.mixpanel.com) は、ユーザーがデジタル製品とどのようにやり取りするかに関するデータをキャプチャできる、製品分析ツールです。 Mixpanel を使用すると、数回のクリックでデータをクエリし視覚化できる、シンプルでインタラクティブなレポートでこの製品データを分析できます。

ソースでは [Mixpanel イベント書き出し API > ダウンロード](https://developer.mixpanel.com/reference/raw-event-export) を使用して、イベントデータを受信して保存する際に、 [!DNL Mixpanel]を、すべてのイベントプロパティ ( `distinct_id`) と、イベントが送信された正確なタイムスタンプ (Experience Platform)。 Mixpanel は、bearer トークンを認証メカニズムとして使用し、Mixpanel イベント書き出し API と通信します。

## IP アドレス許可リスト

ソースコネクタを操作する前に、IP アドレスのリストを許可リストに追加する必要があります。 地域固有の IP アドレスを許可リストに追加しないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下する場合があります。 詳しくは、[IP アドレスの許可リスト](../../ip-address-allow-list.md)ページを参照してください。

## の認証 [!DNL Mixpanel] アカウント

この節では、アカウントを認証し、 [!DNL Mixpanel] データを Platform に送信します。

を作成するために、 [!DNL Mixpanel] ソース接続とデータフローの場合、まず有効な [!DNL Mixpanel] アカウント 有効な [!DNL Mixpanel] アカウントについては、 [Mixpanel レジスタ](https://mixpanel.com/register/) ページを開き、アカウントを作成します。

正常に [!DNL Mixpanel] アカウントで、 [!DNL Project Details] 」タブをクリックします。 [!DNL Project Seettings] ページ [!DNL Mixpanel] プロジェクト ID を取得し、タイムゾーンを設定するための UI。

![mixpanel-project-settings](../../images/tutorials/create/mixpanel-export-events/mixpanel-project-settings.png)

次に、 [!DNL Service Accounts] 」タブをクリックします。 [!DNL Project Settings] ページの [!DNL Mixpanel] サービスアカウント資格情報を取得する UI。

>[!TIP]
>
>ベストプラクティスとして、次のサービスアカウントを選択します。 [有効期限なし](https://developer.mixpanel.com/reference/service-accounts#service-account-expiration).

![Mixpanel サービスアカウント](../../images/tutorials/create/mixpanel-export-events/mixpanel-service-account.png)

最後に、プラットフォームの作成 [スキーマ](../../../xdm/schema/composition.md) 必須 [!DNL Mixpanel Event Export API]. スキーマに必要なマッピングについて詳しくは、 [作成 [!DNL Mixpanel] UI のソース接続](../../tutorials/ui/create/analytics/mixpanel.md#additional-resources).

![スキーマを作成](../../images/tutorials/create/mixpanel-export-events/schema.png)

## API を使用して [!DNL Mixpanel] と Platform を接続する

以下のドキュメントでは、API やユーザーインターフェイスを使用して [!DNL Mixpanel] と Platform を接続する方法について説明します。

* [のソース接続とデータフローの作成 [!DNL Mixpanel] フローサービス API の使用](../../tutorials/api/create/analytics/mixpanel.md)

## UI を使用した [!DNL Mixpanel] の Platform への接続

* [UI での  [!DNL Mixpanel]  ソース接続の作成](../../tutorials/ui/create/analytics/mixpanel.md)
* [UI での顧客成功ソース接続のデータフローの作成](../../tutorials/ui/dataflow/analytics.md)
