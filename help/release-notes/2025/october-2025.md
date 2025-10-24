---
title: Adobe Experience Platform リリースノート 2025年10月
description: Adobe Experience Platform の 2025年10月のリリースノート。
source-git-commit: 7f37ba35111f6fa96d1889d74a66e32302b8ab85
workflow-type: tm+mt
source-wordcount: '1068'
ht-degree: 13%

---

# Adobe Experience Platform リリースノート

>[!TIP]
>
>その他のAdobe Experience Platform アプリケーションのリリースノートについては、次のドキュメントを参照してください。
>
>- [Adobe Journey Optimizer](https://experienceleague.adobe.com/ja/docs/journey-optimizer/using/whats-new/release-notes)
>- [Adobe Journey Optimizer B2B](https://experienceleague.adobe.com/ja/docs/journey-optimizer-b2b/user/release-notes)
>- [Customer Journey Analytics](https://experienceleague.adobe.com/en/docs/analytics-platform/using/releases/pre-release-notes)
>- [連合オーディエンス構成](https://experienceleague.adobe.com/en/docs/federated-audience-composition/using/e-release-notes)
>- [Real-Time CDP Collaboration](https://experienceleague.adobe.com/en/docs/real-time-cdp-collaboration/using/latest)

**リリース日：2025 年 10 月 22 日**

Adobe Experience Platformの既存の機能に対する新機能とアップデート：

- [Agent Orchestrator](#agent-orchestrator)
- [アラート](#alerts)
- [宛先](#destinations)
- [ソース](#sources)

## Agent Orchestrator {#agent-orchestrator}

Adobe Experience Platform Agent OrchestratorはAdobe Experience Platformの新しいアジェンティック層です。

**更新された機能**

| 機能 | 説明 |
| ------- | ----------- |
| Audience Agent | Audience Agentでは、対話型オーディエンス探索と重複オーディエンス検出のために、アカウントベースのオーディエンスをサポートするようになりました。 詳しくは、[Audience Agent ドキュメント ](https://experienceleague.adobe.com/en/docs/experience-cloud-ai/experience-cloud-ai/agents/audience) を参照してください。 |

エージェントについて詳しくは、[Agent Orchestrator ドキュメント ](https://experienceleague.adobe.com/en/docs/experience-cloud-ai/experience-cloud-ai/home) を参照してください。

## アラート {#alerts}

Experience Platformでは、様々なExperience Platform アクティビティに関するイベントベースのアラートを登録できます。 Experience Platform ユーザーインターフェイスの「[!UICONTROL Alerts]」タブを使用して、様々なアラートルールを購読し、UI 内またはメール通知を通じてアラートメッセージを受け取るように選択できます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| アクティブ化失敗率アラート | 宛先に対して新しいアラートが追加されました：**アクティベーションの失敗率がしきい値を超えています**。 このアラートは、データのアクティベーション中に失敗したレコードの数が許可されているしきい値を超えた場合に通知するので、アクティベーションの問題に迅速に対応できます。 詳しくは、[ 標準アラートルール ](../../observability/alerts/rules.md) に関するドキュメントを参照してください。 |

{style="table-layout:auto"}

アラートについて詳しくは、[[!DNL Observability Insights]  概要 ](../../observability/home.md) を参照してください。

## 宛先 {#destinations}

Experience Platformから [!DNL Destinations] データの円滑なアクティベーションを可能にする、事前定義済みの出力先プラットフォームとの統合です。 宛先を使用して、クロスチャネルマーケティングキャンペーン、メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

**新規宛先または更新された宛先**

| 宛先 | 説明 |
| --- | --- |
| [!DNL Adform] | この宛先を使用して、Adobe Real-Time CDP オーディエンスを [!DNL Adform] に送信し、Experience Cloud ID （ECID）と [!DNL Adform] の ID Fusion に基づいてアクティブ化します。 [!DNL Adform] の ID Fusion は、Experience Cloud ID （ECID）に基づいてファーストパーティオーディエンスをアクティブ化できる ID 解決サービスです。 詳しくは、[[!DNL Adform]  ドキュメント ](../../destinations/catalog/advertising/adform.md) を参照してください |
| [!DNL Amazon Ads] | ID のサポートが追加されました。 これには、`firstName`、`lastName`、`street`、`city`、`state`、`zip` および `country` などのフィールドが含まれます。 これらのフィールドをターゲット ID としてマッピングすると、オーディエンスの一致率を向上させることができます。 詳しくは、[[!DNL Amazon Ads]  ドキュメント ](../../destinations/catalog/advertising/amazon-ads.md) を参照してください。 |
| [!DNL Snowflake Batch] （限定提供） | ライブ [!DNL Snowflake] データ共有を作成して、毎日のオーディエンスの更新を共有テーブルとして直接アカウントに受け取ります。 この統合は、現在、VA7 地域でプロビジョニングされたお客様の組織で利用できます。 詳しくは、[[!DNL Snowflake Batch]  ドキュメント ](../../destinations/catalog/warehouses/snowflake-batch.md) を参照してください。 |
| [!DNL Snowflake Streaming] （限定提供） | ライブオーディ [!DNL Snowflake] ンスデータ共有を作成して、ストリーミングオーディエンスの更新を共有テーブルとして直接アカウントに受け取ります。 この統合は、現在、VA7 地域でプロビジョニングされたお客様の組織で利用できます。 詳しくは、[[!DNL Snowflake Streaming]  ドキュメント ](../../destinations/catalog/warehouses/snowflake.md) を参照してください。 |

{style="table-layout:auto"}

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| [ オーディエンスレベルの監視をサポートする新しい宛先 ](../../dataflows/ui/monitor-destinations.md#audience-level-view) | 以下の宛先で、オーディエンスレベルの監視がサポートされるようになりました。 <ul><li>[!DNL Airship Tags]</li><li>（API） [!DNL Salesforce Marketing Cloud]</li><li>[!DNL Marketo Engage]</li><li>[!DNL Microsoft Bing]</li><li>（V1） [!DNL Pega CDH Realtime Audience]</li><li>（V2） [!DNL Pega CDH Realtime Audience]</li><li>[!DNL Salesforce Marketing Cloud] Account Engagement</li><li>[!DNL The Trade Desk]</li></ul> |
| データセット書き出しガードレールの修正 | データセット書き出しガードレールの修正が実装されました。 以前は、XDM エクスペリエンスイベントスキーマに基づい _タイムスタンプ列を含むが_ 含まない）一部のデータセットがエクスペリエンスイベントデータセットとして誤って扱われ、書き出しが 365 日のルックバックウィンドウに制限されていました。 ドキュメント化された 365 日間のルックバックガードレールは、エクスペリエンスイベントデータセットにのみ適用されるようになりました。 XDM エクスペリエンスイベントスキーマ以外のスキーマを使用するデータセットは、100 億レコードのガードレールで管理されるようになりました。 一部のお客様では、データセットの書き出し数が増加し、365 日間のルックバックウィンドウで誤って失敗する場合があります。 これにより、ルックバックウィンドウが長い予測ワークフローのデータセットを書き出すことができます。 詳しくは、[ データセット書き出しガードレール ](../../destinations/guardrails.md#dataset-exports) を参照してください。 |
| エンタープライズ宛先に関するオーディエンスレベルのレポートの強化 | このリリース以降、選択した宛先に関連するオーディエンスのみを含んだ、より正確なオーディエンスレポート番号が表示されるようになります。 この監視の調整により、データフローでマッピングされたオーディエンスのみがレポートに含まれるようになり、実際のデータのアクティブ化に関するインサイトが明確になります。 これは、アクティブ化されるデータの量には影響しません。純粋に、レポートの精度を向上させるための監視機能の強化にすぎません。 |
| アクセスラベルによる UI のグレーアウトされたデータフロー | アクセス権を持たない宛先データフローが完全に非表示になっていたため、一部のユーザーに空白のページが表示される問題に対処するために、UI では、これらの制限されたデータフローを完全に省略するのではなく、グレー表示の状態で表示するようになりました。 詳しくは、[ アクセスラベルを使用した宛先データフローへのユーザーアクセスの管理 ](../../access-control/abac/apply-access-labels-destinations.md#important-callouts-and-items-to-know) に関するドキュメントを参照してください。 |

{style="table-layout:auto"}

詳しくは、[ 宛先の概要 ](../../destinations/home.md) を参照してください。

## ソース {#sources}

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| Adobe Analytics ソースのデータセット作成の変更 | Adobe AnalyticsとExperience Platform間のデータフロー作成プロセスの一環として、カタログサービスを使用してデータセットが作成されます。 このデータセットは、データを格納するコンテナとして機能します。 現在、このプロセスには、Analytics レポートスイートから取得されてカタログサービスに送信され、新しく作成されたデータセットに関連付けられるデータソース ID が含まれます。 変更後、データソース ID を指定するオプションは、データセットの作成時には使用できなくなります。 したがって、Analytics ソースによって作成された新しいデータセットには、カタログサービスで関連付けられたデータソース ID がなくなります。 この変更はメタデータにのみ適用され、データセット内のデータのストレージには一切変更されません。 ただし、カタログサービスで提供されるデータソース ID は、Adobe Analytics用に新しく作成されたデータセットでは使用できなくなることを知っておくことが重要です。 Adobe Analytics ソースコネクタについて詳しくは [](../../sources/connectors/adobe-applications/analytics.md)Adobe Analytics ソースドキュメント } を参照してください。 |
| [!DNL Google Ads] ソースの一般提供（API のみ） | [ ソースの  [!DNL Google Ads]](../../sources/tutorials/api/create/advertising/ads.md)API バージョンが一般提供されるようになりました。 API に関するドキュメントを更新し、最新バージョンが `v21` 新されたことを反映しました。また、Experience Platformでは v19 以降のすべてのバージョンをサポートしています。 [UI バージョン ](../../sources/tutorials/ui/create/advertising/ads.md) はベータ版のままで、1 回限りの取り込みをサポートします。 増分データ取り込みを使用するには、API ルートを使用します。 |
| [!DNL Azure Event Hubs] 仮想ネットワークのサポート | Adobeは、[[!DNL Azure Event Hubs]](../../sources/connectors/cloud-storage/eventhub.md) への仮想ネットワーク接続を明示的にサポートするようになりました。これにより、パブリックネットワークではなくプライベートネットワーク経由でのデータ転送が可能になります。 お客様は、*Experience Platform VNet を許可リストに加えるして、Azure プライベートバックボーンを通じて Event Hubs トラフィックを非公開でルーティングし、データ取得ワークフローのセキュリティとコンプライアンスを強化できます。 |

{style="table-layout:auto"}

詳しくは、[ソースの概要](../../sources/home.md)を参照してください。

<!--
| Source | Description |
| --- | --- |
| [!BADGE Beta]{type=Informative} [!DNL Talon.one] sources for loyalty data | Use the [[!DNL Talon.One] sources](../../sources/connectors/loyalty/talon-one.md) to ingest batch and streaming loyalty data into Experience Platform. The connector supports streaming of profile data, transaction data, and loyalty data including points earned, points redeemed, points expired, and tier data. For more information, read the [!DNL Talon.One] [batch](../../sources/tutorials/ui/create/loyalty/talon-one-batch.md) and [streaming](../../sources/tutorials/ui/create/loyalty/talon-one-streaming.md) documentation. |
-->