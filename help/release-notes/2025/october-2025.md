---
title: Adobe Experience Platform リリースノート 2025年10月
description: Adobe Experience Platform の 2025年10月のリリースノート。
exl-id: 93feff2b-d998-41f1-8d93-332238a1d88d
source-git-commit: 58f69a78fb3c622c8741d7a1618f15509c160a5b
workflow-type: tm+mt
source-wordcount: '1159'
ht-degree: 13%

---

# Adobe Experience Platform リリースノート

>[!TIP]
>
>他のAdobe Experience Platform アプリケーションのリリースノートについては、次のドキュメントを参照してください。
>
>- [Adobe Journey Optimizer](https://experienceleague.adobe.com/ja/docs/journey-optimizer/using/whats-new/release-notes)
>- [Adobe Journey Optimizer B2B](https://experienceleague.adobe.com/ja/docs/journey-optimizer-b2b/user/release-notes)
>- [Customer Journey Analytics](https://experienceleague.adobe.com/en/docs/analytics-platform/using/releases/pre-release-notes)
>- [連合オーディエンス構成](https://experienceleague.adobe.com/en/docs/federated-audience-composition/using/e-release-notes)
>- [Real-Time CDP Collaboration](https://experienceleague.adobe.com/en/docs/real-time-cdp-collaboration/using/latest)

**リリース日：2025年10月22日（PT）**

Adobe Experience Platformの新機能と既存の機能の更新：

- [Agent Orchestrator](#agent-orchestrator)
- [アラート](#alerts)
- [宛先](#destinations)
- [Real-Time CDP B2B エディション](#b2b)
- [ソース](#sources)

## Agent Orchestrator {#agent-orchestrator}

Adobe Experience Platform Agent Orchestratorは、Adobe Experience Platformの新しいエージェント層です。

**更新された機能**

| 機能 | 説明 |
| ------- | ----------- |
| Audience Agent | Audience Agentは、アカウントベースのオーディエンスをサポートし、会話形式のオーディエンスを検索して、重複するオーディエンスを検出できるようになりました。 詳しくは、[Audience Agent ドキュメント ](https://experienceleague.adobe.com/ja/docs/experience-cloud-ai/experience-cloud-ai/agents/audience)を参照してください。 |

エージェントについて詳しくは、[Agent Orchestrator ドキュメント ](https://experienceleague.adobe.com/en/docs/experience-cloud-ai/experience-cloud-ai/home)を参照してください。

## アラート {#alerts}

Experience Platformでは、様々なExperience Platform アクティビティのイベントベースのアラートを購読できます。 Experience Platform ユーザーインターフェイスの「[!UICONTROL Alerts]」タブを使用して、様々なアラートルールを購読できます。また、UI自体またはメール通知を使用してアラートメッセージを受信することもできます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| アクティベーション失敗率アラート | 宛先に対して新しいアラートが追加されました：**アクティベーション失敗率がしきい値**&#x200B;を超えています。 このアラートは、データアクティベーション中に失敗したレコードの数が許可されたしきい値を超えたときに通知し、アクティベーションの問題に迅速に対応できるようにします。 詳しくは、[標準アラートルール ](../../observability/alerts/rules.md)に関するドキュメントを参照してください。 |

{style="table-layout:auto"}

アラートについて詳しくは、[[!DNL Observability Insights] 概要](../../observability/home.md)を参照してください。

## 宛先 {#destinations}

[!DNL Destinations]は、Experience Platformからのデータのシームレスなアクティベーションを可能にする、宛先プラットフォームとの事前定義済みの統合です。 宛先を使用して、クロスチャネルマーケティングキャンペーン、メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

**新規宛先または更新された宛先**

| 宛先 | 説明 |
| --- | --- |
| [!DNL Adform] | この宛先を使用して、Experience Cloud ID （ECID）と[!DNL Adform]のID Fusionに基づいてアクティブ化するためにAdobe Real-Time CDP オーディエンスを[!DNL Adform]に送信します。 [!DNL Adform]のID Fusionは、Experience Cloud ID （ECID）に基づいて1st パーティオーディエンスをアクティブ化できるID解決サービスです。 詳しくは、[[!DNL Adform]  ドキュメント ](../../destinations/catalog/advertising/adform.md)を参照してください |
| [!DNL Amazon Ads] | 追加の個人識別子のサポートが追加されました。 これには、`firstName`、`lastName`、`street`、`city`、`state`、`zip`、`country`などのフィールドが含まれます。 これらのフィールドをターゲット IDとしてマッピングすることで、オーディエンスの一致率を向上させることができます。 詳しくは、[[!DNL Amazon Ads]  ドキュメント ](../../destinations/catalog/advertising/amazon-ads.md)を参照してください。 |
| [!DNL Snowflake Batch] （利用制限あり） | ライブ [!DNL Snowflake] データ共有を作成して、毎日のオーディエンス更新をアカウントの共有テーブルとして直接受信します。 この統合は現在、VA7 リージョンでプロビジョニングされているお客様組織で使用できます。 詳しくは、[[!DNL Snowflake Batch]  ドキュメント ](../../destinations/catalog/warehouses/snowflake-batch.md)を参照してください。 |
| [!DNL Snowflake Streaming] （利用制限あり） | ライブ [!DNL Snowflake] データ共有を作成して、ストリーミングオーディエンスの更新をアカウントの共有テーブルとして直接受信します。 この統合は現在、VA7 リージョンでプロビジョニングされているお客様組織で使用できます。 詳しくは、[[!DNL Snowflake Streaming]  ドキュメント ](../../destinations/catalog/warehouses/snowflake.md)を参照してください。 |

{style="table-layout:auto"}

**新しい機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| [ オーディエンスレベルの監視をサポートするいくつかの新しい宛先](../../dataflows/ui/monitor-destinations.md#audience-level-view) | 次の宛先は、オーディエンスレベルの監視をサポートするようになりました。 <ul><li>[!DNL Airship Tags]</li><li>（API） [!DNL Salesforce Marketing Cloud]</li><li>[!DNL Marketo Engage]</li><li>[!DNL Microsoft Bing]</li><li>（V1） [!DNL Pega CDH Realtime Audience]</li><li>（V2） [!DNL Pega CDH Realtime Audience]</li><li>[!DNL Salesforce Marketing Cloud] アカウントのエンゲージメント</li><li>[!DNL The Trade Desk]</li></ul> |
| データセット書き出しのガードレールの修正 | データセット書き出しガードレールに修正が実装されました。 以前は、タイムスタンプ列を含んでいるが、XDM Experience Events スキーマに基づいて&#x200B;_not_&#x200B;であった一部のデータセットは、Experience Events データセットとして誤って処理され、書き出しは365日のルックバックウィンドウに制限されていました。 文書化された365日間のルックバックガードレールは、Experience Events データセットにのみ適用されるようになりました。 XDM Experience Events スキーマ以外の任意のスキーマを使用するデータセットは、100億件のレコードガードレールによって管理されるようになりました。 一部の顧客では、データセットの書き出し数が増加し、365日のルックバックウィンドウに誤って該当する場合があります。 これにより、ルックバックウィンドウが長い予測ワークフローのデータセットを書き出すことができます。 詳しくは、[ データセット書き出しガードレール ](../../destinations/guardrails.md#dataset-exports)を参照してください。 |
| エンタープライズ宛先向けの強化されたオーディエンスレベルのレポート | このリリースの後は、選択した宛先に関連するオーディエンスのみが含まれる、より正確なオーディエンスレポート番号が表示されます。 この監視の調整により、レポートには、データフローにマッピングされたオーディエンスのみが含まれ、実際のデータのアクティベーションに関する明確なインサイトが提供されます。 これは、アクティブ化されるデータの量には影響しません。レポート精度を向上させるための監視の強化です。 |
| アクセスラベルによるUIのデータフローのグレー表示 | 宛先のデータフローにアクセスできない宛先のデータフローが完全に非表示になっているため、一部のユーザーに空白のページが表示される問題に対処するために、UIでは、制限されたデータフローを完全に省略するのではなく、グレー表示の状態で表示するようになりました。 詳しくは、「[ アクセスラベルを使用した宛先データフローへのユーザーアクセスの管理](../../access-control/abac/apply-access-labels-destinations.md#important-callouts-and-items-to-know)」のドキュメントを参照してください。 |

{style="table-layout:auto"}

詳しくは、[宛先の概要](../../destinations/home.md)を参照してください。

## Real-Time CDP B2B エディション {#b2b}

Real-Time CDP B2B editionは、B2Bの顧客データを包括的に管理するための機能を提供します。これにより、統合された顧客プロファイルを構築し、洗練されたB2B オーディエンスを構築して、様々なマーケティングチャネルをまたいでデータを活用できるようになります。

**新しい機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| B2B エンティティ間の非標準的な関係に対するB2B サポートの廃止 | 2026年1月以降、Real-Time CDP B2B editionでは、B2B エンティティ間の&#x200B;**非標準**&#x200B;関係がサポートされなくなります。 したがって、[B2B名前空間およびスキーマガイド ](../../rtcdp/schemas/b2b.md)に記載されている標準の関係を使用するように、B2B エンティティを更新することをお勧めします。 |

{style="table-layout:auto"}

## ソース {#sources}

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| Adobe Analytics ソースのデータセット作成の変更 | Adobe AnalyticsとExperience Platform間のデータフロー作成プロセスの一環として、カタログサービスを介してデータセットが作成されます。 このデータセットは、データを取り込むためのコンテナとして機能します。 現在、このプロセスには、Analytics レポートスイートから取得され、カタログサービスに送信され、新しく作成されたデータセットに関連付けられたDataSource IDが含まれています。 変更後、データセットの作成中にデータソース IDを指定するオプションは使用できなくなります。 したがって、Analytics ソースで作成された新しいデータセットには、カタログサービスでデータソース IDが関連付けられなくなります。 この変更はメタデータにのみ適用され、データセット内のデータの保存は何も変更されません。 ただし、カタログサービスが提供するデータソース IDは、Adobe Analytics用に新しく作成されたデータセットでは使用できなくなります。 Adobe Analytics ソースコネクタについて詳しくは、[Adobe Analytics ソースドキュメント ](../../sources/connectors/adobe-applications/analytics.md)を参照してください。 |
| [!DNL Google Ads] ソースの一般提供（APIのみ） | [ ソースの [!DNL Google Ads]](../../sources/tutorials/api/create/advertising/ads.md)API バージョンが一般提供されました。 API ドキュメントが更新され、最新バージョンが`v21`になり、Experience Platformはv19以降のすべてのバージョンをサポートするようになりました。 [UI バージョン ](../../sources/tutorials/ui/create/advertising/ads.md)はベータ版のままであり、1回限りの取り込みのみをサポートします。 増分データ取り込みを使用するには、API ルートを使用します。 |
| [!DNL Azure Event Hubs]仮想ネットワークのサポート | Adobeでは、[[!DNL Azure Event Hubs]](../../sources/connectors/cloud-storage/eventhub.md)へのバーチャルネットワーク接続が明示的にサポートされるようになりました。これにより、パブリックネットワークではなくプライベートネットワーク経由でのデータ転送が可能になります。 お客様は、Experience Platform VNetを許可リストに加えるして、イベントハブのトラフィックをAzure プライベートバックボーンを通じてプライベートにルーティングし、データ取り込みワークフローのセキュリティとコンプライアンスを強化できます。 |

{style="table-layout:auto"}

詳しくは、[ソースの概要](../../sources/home.md)を参照してください。

<!--
| Source | Description |
| --- | --- |
| [!BADGE Beta]{type=Informative} [!DNL Talon.one] sources for loyalty data | Use the [[!DNL Talon.One] sources](../../sources/connectors/loyalty/talon-one.md) to ingest batch and streaming loyalty data into Experience Platform. The connector supports streaming of profile data, transaction data, and loyalty data including points earned, points redeemed, points expired, and tier data. For more information, read the [!DNL Talon.One] [batch](../../sources/tutorials/ui/create/loyalty/talon-one-batch.md) and [streaming](../../sources/tutorials/ui/create/loyalty/talon-one-streaming.md) documentation. |
-->
