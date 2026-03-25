---
title: Adobe Experience Platform リリースノート（2026年3月）
description: Adobe Experience Platform の 2026年3月のリリースノート。
exl-id: f854f9e5-71be-4d56-a598-cfeb036716cb
source-git-commit: 8c55aebcb65327394ffbdf59db1d2a203182ed18
workflow-type: tm+mt
source-wordcount: '1161'
ht-degree: 21%

---

# Adobe Experience Platform リリースノート

>[!TIP]
>
>他のAdobe Experience Platform アプリケーションのリリースノートについては、次のドキュメントを参照してください。
>
>- [Adobe Journey Optimizer](https://experienceleague.adobe.com/ja/docs/journey-optimizer/using/whats-new/release-notes)
>- [Adobe Journey Optimizer B2B](https://experienceleague.adobe.com/ja/docs/journey-optimizer-b2b/user/release-notes)
>- [Customer Journey Analytics](https://experienceleague.adobe.com/ja/docs/analytics-platform/using/releases/latest)
>- [連合オーディエンス構成](https://experienceleague.adobe.com/ja/docs/federated-audience-composition/using/release-notes)
>- [Real-Time CDP Collaboration](https://experienceleague.adobe.com/ja/docs/real-time-cdp-collaboration/using/latest)

**リリース日：2026年3月24日（PT）**

Adobe Experience Platformの新機能と既存の機能の更新：

- [詳細なデータライフサイクル管理](#advanced-data-lifecycle-management)
- [Agent Orchestrator](#agent-orchestrator)
- [宛先](#destinations)
- [エクスペリエンスデータモデル（XDM）](#xdm)
- [セグメント化サービス](#segmentation-service)
- [ソース](#sources)

## 詳細なデータライフサイクル管理 {#advanced-data-lifecycle-management}

Experience Platformには、消費者のレコードやデータセットをプログラムによって削除することにより、保存されたデータを管理できるデータハイジーン機能が揃っています。 UIのデータライフサイクルワークスペースまたはData Hygiene APIへの呼び出しを使用すると、データストアを効果的に管理できます。 これらの機能を使用して、情報が期待どおりに使用され、不正確なデータの修正が必要な場合に更新され、組織のポリシーで必要と判断された場合に削除されるようにします。

| 機能 | 説明 |
| --- | --- |
| マルチデータセットおよびプロファイルのみのレコード削除（APIのみ） | 1つのデータセット ID、データセット IDのコンマ区切りリスト、または`ALL`のリテラル `datasetId`を送信して、1つ、多く、またはすべてのデータセットのIDを削除できます。 また、`targetServices`を`["identity","profile","ajo"]`に設定してプロファイル関連のサービスに削除を制限することもできます。これにより、データレイクは変更されません。この機能は、Data Hygiene API経由でのみ使用できます。 詳細については、[&#x200B; レコードの削除作業指示ガイド &#x200B;](../../hygiene/api/workorder.md)を参照してください。 |

{style="table-layout:auto"}

詳しくは、[高度なデータライフサイクル管理の概要](../../hygiene/home.md)を参照してください。

## Agent Orchestrator {#agent-orchestrator}

Agent Orchestratorなら、ワークフローを自動化し、複数のチャネルをまたいで顧客とやり取りできる、AIを活用したエージェントを構築し、展開できます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| [&#x200B; [!DNL Microsoft 365 Copilot]の](https://experienceleague.adobe.com/ja/docs/experience-cloud-ai/experience-cloud-ai/agents/ama-ms)Adobe Marketing Agent | [!DNL Microsoft 365 Copilot]用Adobe Marketing Agentは、Adobeのマーケティングインテリジェンスを、[!DNL Teams]、[!DNL Word]、[!DNL PowerPoint]およびその他[!DNL Microsoft 365] アプリなどの日常的なツールに直接取り込む組み込みエージェントです。 このエージェントを使用すると、施策の計画中にAdobe アプリケーションから信頼できるキャンペーンインサイトを取得したり、オーディエンスを確認したり、他のユーザーと協力してお客様の質問に答えたり、[!DNL Microsoft 365] ワークフローから離れることなく、データに基づいた意思決定を行ったりできます。 |

{style="table-layout:auto"}

詳しくは、[Agent Orchestrator ドキュメント &#x200B;](https://experienceleague.adobe.com/ja/docs/experience-cloud-ai/experience-cloud-ai/agents/agent-orchestrator)を参照してください。

## 宛先 {#destinations}

[!DNL Destinations]は、Experience Platformからのデータのシームレスなアクティベーションを可能にする、宛先プラットフォームとの事前定義済みの統合です。 宛先を使用して、クロスチャネルマーケティングキャンペーン、メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

**新規宛先または更新された宛先**

| 宛先 | 説明 |
| --- | --- |
| [Adobe Advertising DSP](../../destinations/catalog/advertising/adobe-advertising-cloud-connection.md)接続 | 新しいAdobe Advertising DSP接続は、従来の接続と同じ機能に加えて、追加のIDのサポートを提供します。 新しいコネクタを使用すると、Cookie ベースのIDをAdobe Advertising DSPに書き出すこともできます。 |
| [FreeWheel](../../destinations/catalog/advertising/freewheel.md)接続 | [!DNL Real-Time CDP]人のオーディエンスを毎日のバッチファイルとしてFreeWheelに送信すると、CTV、ビデオ、ディスプレイをまたいでFreeWheelのお得な情報やキャンペーンでターゲットにすることができます。 アクセスについては、Adobe アカウントチームにお問い合わせください。 |
| [The Trade Desk CRM](../../destinations/catalog/advertising/tradedesk-emails.md)および[Pinterest](../../destinations/catalog/advertising/pinterest.md)の外部オーディエンスのサポート | セグメンテーションサービスを超えたオリジンのオーディエンスを、カスタムアップロードオーディエンス（CSVからインポート）、類似オーディエンス、連合オーディエンス、[!DNL Adobe Journey Optimizer]などの他のExperience Platform アプリケーションで作成されたオーディエンスを含む、The Trade Desk CRM、Criteo、およびPinterestにアクティベートできるようになりました。 この更新は3月末までロールアウトされます。 詳しくは、各宛先のカタログページの「[&#x200B; サポートされているオーディエンス &#x200B;](../../destinations/catalog/advertising/criteo.md#supported-audiences)」セクションを参照してください。 |
| カスタムアップロードオーディエンスの制限の増加 | 宛先インスタンスごとに最大20個のカスタムアップロードオーディエンスをアクティブ化できるようになりました。 以前は、この制限は10でした。 詳しくは、[宛先ガードレール &#x200B;](../../destinations/guardrails.md#batch-file-based-activation)を参照してください。 |
| 外部オーディエンスに対する[&#x200B; ファイルの書き出し](../../destinations/ui/export-file-now.md)および[&#x200B; アドホックアクティベーション API](../../destinations/api/ad-hoc-activation-api.md)のサポート | バッチファイルベースの宛先に対してアクティブ化する際に、今すぐファイルを書き出し（UI）とアドホックアクティベーション APIを外部オーディエンス（カスタムアップロード、類似、フェデレーション、他のExperience Platform アプリからのオーディエンスなど）と共に使用できるようになりました。 この更新は3月末までロールアウトされます。 |

{style="table-layout:auto"}

**修正点および改善点**

| 修正 | 説明 |
| --- | --- |
| [TikTok](../../destinations/catalog/social/tiktok.md) コネクタの電話番号のハッシュ化 | 宛先カードの設定ミスにより、電話番号のキーオフされたIDがTikTokにアクティベートされない問題を修正しました。 この修正の利点を得るには、新しいアクティベーションフローを設定するか、既存のフローから電話番号マッピングを削除して保存し、もう一度追加します。 |

{style="table-layout:auto"}

詳しくは、[宛先の概要](../../destinations/home.md)を参照してください。

## エクスペリエンスデータモデル（XDM） {#xdm}

XDMは、Experience Platformに取り込まれるデータに共通の構造と定義（スキーマ）を提供するオープンソース仕様です。 XDM 標準規格に準拠しているので、すべての顧客体験データを共通の表現に反映させて、迅速かつ統合的な方法でインサイトを提供できます。顧客アクションから有益なインサイトを得たり、セグメントを通じて顧客オーディエンスを定義したり、パーソナライズ機能のために顧客属性を使用したりできます。

| 機能 | 説明 |
| --- | --- |
| XDM エンティティのアクションと削除サポート | インラインテーブルメニューと詳細ページヘッダーメニューから、スキーマ、クラス、フィールドグループ、データタイプのアクションに直接アクセスできます。 必要な権限を持っている場合は、組織のエンティティがデータセットで使用されておらず、プロファイルに対して有効になっていない場合に、そのエンティティを削除することもできます。 詳しくは、[XDM UI ガイド &#x200B;](../../xdm/ui/explore.md)を参照してください。 |

詳しくは、[XDMの概要](../../xdm/home.md)を参照してください。

<!-- 
## Run and Operate {#run-and-operate}

Inspect, troubleshoot, and optimize your Experience Platform implementations with the Run and Operate tools. Gain visibility into scheduled batch activations, identify configuration issues, and improve system reliability.

**New or updated features**

| Feature | Description |
| --- | --- |
| [Job Schedules](../../run-and-operate/job-schedules.md) general availability | [!DNL Job Schedules] provides a unified view of all scheduled batch processing jobs across your data pipeline, from ingestion through destination activation. Inspect execution status, identify scheduling conflicts, and diagnose configuration issues before they impact your business operations. |
| [Health Checks](../../run-and-operate/health-checks.md) general availability | Poor schema and identity configurations lead to significant downstream issues, including incorrect profile creation, failed segment qualification, and inaccurate activation. <br>Health checks shift your approach from reactive troubleshooting to proactive, preventative maintenance. Health checks are always-on scans of your schemas and identities used in your sandbox and provide a summary of issues that you can use to explore and troubleshoot. |

{style="table-layout:auto"}

For more information, read the [Run and Operate overview](../run-and-operate/overview.md), [Inspect job schedules](../run-and-operate/job-schedules.md), and the [Platform UI guide](../landing/ui-guide.md). -->

## セグメント化サービス {#segmentation-service}

[!DNL Segmentation Service] は、顧客ベース内のマーケティング可能なユーザーグループを区別する基準を記述することで、プロファイルの特定のサブセットを定義します。オーディエンスは、レコードデータ（デモグラフィック情報など）や、顧客と企業とのインタラクションを表す時系列イベントにもとづいておこなうことができます。

**新機能または更新された機能**

| 機能 | 説明 |
| ------- | ----------- |
| 取り込みタイプ | 属性の取り込みタイプを表示できるようになりました。 これにより、データの出所を把握して、より優れたオーディエンスを構築できるようになります。 この機能について詳しくは、[&#x200B; セグメントビルダーガイド &#x200B;](/help/segmentation/ui/segment-builder.md)を参照してください。 |
| 概要データ | アカウントおよびピープルベースのオーディエンスの属性の概要データを表示できるようになりました。 アカウントオーディエンスのこの機能について詳しくは、アカウント [&#x200B; オーディエンスビルダーガイド &#x200B;](/help/rtcdp/segmentation/audience-builder.md)を参照してください。 人物ベースのオーディエンスのこの機能について詳しくは、[&#x200B; セグメントビルダーガイド &#x200B;](/help/segmentation/ui/segment-builder.md)を参照してください。 |

詳しくは、[[!DNL Segmentation Service] 概要](../../segmentation/home.md)を参照してください。

## ソース

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

**新規または更新されたソース**

| ソース | 説明 |
| --- | --- |
| [!DNL Talon.One] | 新しい[!DNL Talon.One] [!DNL Talon.One] バッチ [および](../../sources/tutorials/ui/create/loyalty/talon-one-batch.md) ストリーミング [&#x200B; ソースを使用して、Experience Platformを](../../sources/tutorials/ui/create/loyalty/talon-one-streaming.md)に接続できるようになりました。 新しいソースを使用して、ロイヤルティプロファイルデータとトランザクションおよびロイヤルティアクティビティイベントをExperience Platformに取り込みます。 |
| 新しいIP アドレスを許可リストに加える | GBR9の新しいIP アドレス：Azure上のExperience Platformへのバッチソース接続を正常に行うために許可リストに加えるする必要があるアドレスのリストに、イギリスが追加されました。 詳しくは、[IP アドレスの許可リストに加えるガイド &#x200B;](../../sources/ip-address-allow-list.md#gbr9-united-kingdom)を参照してください。 |
| Change Data Captureの拡張サポート | [!DNL Marketo Engage]、[!DNL Microsoft Dynamics]、[!DNL Salesforce CRM]のソースでChange Data Captureを使用できるようになりました。 |
| [[!DNL Google BigQuery]](../../sources/connectors/databases/bigquery.md)の認証ガイドが改善されました | [!DNL Google BigQuery] ソースの認証ガイドが次の情報で拡張されました： <ul><li>更新トークンに必要なスコープ。</li><li>[!DNL Google] IDに必要なIAM役割。</li><li>`largeResultsDataSetId`の使用に関する追加のガイダンス。</li></ul> |

{style="table-layout:auto"}

詳しくは、[ソースの概要](../../sources/home.md)を参照してください。

<!--

NOTE FOR VLAD, CRITEO WAS REMOVED FROM EXTERNAL AUDIENCE SUPPORT

| Destination | Description |
| --- | --- |
| [Snowflake Batch](../../destinations/catalog/warehouses/snowflake-batch.md) region selector | You can now find your region more easily with the new searchable dropdown, which combines search and dropdown into one control. |
| New table structure for [Snowflake Batch](../../destinations/catalog/warehouses/snowflake-batch.md) destinations | Tables shared into your Snowflake account now have a new structure which includes separate audience name and audience origin columns. The new table structure applies to all new destination connections set up moving forward. For any new connections that you set up, an old format and new format table are created. The old table structure will be kept for another three months before being deprecated. Read more in the [Exported data](../../destinations/catalog/warehouses/snowflake-batch.md#exported-data) section of the Snowflake Batch documentation. |
| [HTTP API](../../destinations/catalog/streaming/http-destination.md) destinations with OAuth 2 and mTLS | You can now create and authenticate HTTP API destinations that use OAuth 2 when the authentication endpoint requires mutual TLS (mTLS); token retrieval during destination setup now supports mTLS. |

| Fix | Description |
| --- | --- |
| [Snowflake Streaming](../../destinations/catalog/warehouses/snowflake.md) and [Snowflake Batch](../../destinations/catalog/warehouses/snowflake-batch.md) account ID validation | A regular expression validator has been added to the Account ID step. When you enter your ID, it is now validated to ensure organization ID and account ID are in the correct format (separated by a dot). |

-->