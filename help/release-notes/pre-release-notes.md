---
title: Experience Platformのプレリリースノート
description: Adobe Experience Platformの最新のリリースノートのプレビュー。
exl-id: f2c41dc8-9255-4570-b459-4f9fc28ee58b
source-git-commit: 5cbf63cc0a149d54de63e3e1797cae4098498fe8
workflow-type: tm+mt
source-wordcount: '1322'
ht-degree: 18%

---

# Adobe Experience Platformのプレリリースノート

>[!IMPORTANT]
>
>このドキュメントは、今月のリリースノートの **プレビュー** として提供することを目的としています。 リリース項目は変更される場合があり、最終リリースで追加または削除される場合があります。

>[!TIP]
>
>その他のAdobe Experience Platform アプリケーションのリリースノートについては、次のドキュメントを参照してください。
>
>- [Adobe Journey Optimizer](https://experienceleague.adobe.com/ja/docs/journey-optimizer/using/whats-new/release-notes)
>- [Adobe Journey Optimizer B2B](https://experienceleague.adobe.com/ja/docs/journey-optimizer-b2b/user/release-notes)
>- [Customer Journey Analytics](https://experienceleague.adobe.com/ja/docs/analytics-platform/using/releases/latest)
>- [連合オーディエンス構成](https://experienceleague.adobe.com/en/docs/federated-audience-composition/using/release-notes)
>- [Real-Time CDP Collaboration](https://experienceleague.adobe.com/en/docs/real-time-cdp-collaboration/using/latest)

**リリース日：2026 年 3 月**

Adobe Experience Platformの既存の機能に対する新機能とアップデート：

- [詳細なデータライフサイクル管理](#advanced-data-lifecycle-management)
- [Agent Orchestrator](#agent-orchestrator)
- [宛先](#destinations)
- [クエリサービス](#query-service)
- [リアルタイム顧客プロファイル](#profile)
- [実行と操作](#run-and-operate)
- [セグメント化サービス](#segmentation-service)
- [ソース](#sources)

## 詳細なデータライフサイクル管理 {#advanced-data-lifecycle-management}

Experience Platformは、消費者レコードとデータセットをプログラムで削除することで、保存されたデータを管理できる、一連のデータハイジーン機能を提供します。 UI のデータライフサイクルワークスペース、または Data Hygiene API への呼び出しを使用して、データストアを効果的に管理できます。 これらの機能を使用して、情報が期待どおりに使用され、必要な場合は不適切なデータの修正が更新され、組織のポリシーで必要と判断された場合は削除されるようにします。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| マルチデータセットおよびプロファイルのみのレコード削除（API のみ） | 単一のデータセット ID、データセット ID のコンマ区切りリスト、またはのリテラル `ALL` を送信して、1 つ、多数または `datasetId` べてのデータセットの ID を削除できます。 `targetServices` を `["identity","profile","ajo"]` に設定して、削除をプロファイルサービスに制限することもできます。この場合、データレイクは変更されません。 詳しくは、『 [ 作業指示のレコード作成ガイド ](../hygiene/api/workorder.md) を参照してください。 |

{style="table-layout:auto"}

詳しくは、[ 高度なデータライフサイクル管理の概要 ](../hygiene/home.md) を参照してください。

## Agent Orchestrator {#agent-orchestrator}

Agent Orchestratorを使用すると、ワークフローを自動化し、複数のチャネルをまたいで顧客とやり取りできる、AI を活用したエージェントを構築およびデプロイできます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| [!DNL Microsoft 365 Copilot] 用Adobe Marketing Agent | Adobe Marketing Agent for [!DNL Microsoft 365 Copilot] は、Adobeのマーケティングインテリジェンスを [!DNL Teams]、[!DNL Word]、[!DNL PowerPoint] およびその他の [!DNL Microsoft 365] アプリなどの日常的なツールに直接組み込む組み込みエージェントです。 このエージェントを使用すると、キャンペーンの計画中や、オーディエンスのレビューまたは同僚との共同作業の際に、Adobe アプリケーションから信頼できるキャンペーンインサイトを取り込んだり、お客様からの質問に回答したり、[!DNL Microsoft 365] ーザーワークフローを離れることなく、データに基づいた意思決定を行うことができます。 |

{style="table-layout:auto"}

詳しくは、[Agent Orchestrator ドキュメント ](https://experienceleague.adobe.com/ja/docs/experience-cloud-ai/experience-cloud-ai/agents/agent-orchestrator) を参照してください。

## 宛先 {#destinations}

Experience Platformから [!DNL Destinations] データの円滑なアクティベーションを可能にする、事前定義済みの出力先プラットフォームとの統合です。 宛先を使用して、クロスチャネルマーケティングキャンペーン、メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

**新規宛先または更新された宛先**

| 宛先 | 説明 |
| --- | --- |
| [Snowflake バッチ ](../destinations/catalog/warehouses/snowflake-batch.md) 地域の選択 | 検索とドロップダウンを 1 つのコントロールに組み合わせた新しい検索可能なドロップダウンを使用して、地域をより簡単に見つけられるようになりました。 |
| [Snowflake バッチ ](../destinations/catalog/warehouses/snowflake-batch.md) 宛先へのオーディエンスメタデータの書き出し | この宛先に書き出すファイルには、オーディエンスメタデータが含まれるようになりました。 新しいテーブル構造は、今後設定されるすべての新しい宛先接続に適用されます。 古いテーブル構造は、廃止されるまでさらに 3 か月間保持されます。 |
| [!DNL Adobe Advertising Cloud DSP] 接続 | 新しいAdobe Advertising DSP接続は、従来の接続と同じ機能に加えて、追加の ID のサポートを提供します。 |
| [The Trade Desk CRM](../destinations/catalog/advertising/tradedesk-emails.md)、{Criteo[ および ](../destinations/catalog/advertising/criteo.md)4}Pinterest[ に対する外部オーディエンスのサポート](../destinations/catalog/advertising/pinterest.md) | カスタムアップロードオーディエンス（CSV からインポート）、類似オーディエンス、フェデレーティッドオーディエンス、Adobe Journey Optimizerなどの他のExperience Platform アプリで作成されたオーディエンスなど、セグメント化サービスセグメント以外のオーディエンスを、Trade Desk CRM、Criteo およびPinterestに対してアクティブ化できるようになりました。 詳しくは、各宛先のカタログページの [ サポートされるオーディエンス ](../destinations/catalog/advertising/criteo.md#supported-audiences) の節を参照してください。 |
| カスタムアップロードオーディエンス制限の増加 | 宛先インスタンスあたり最大 20 個のカスタムアップロードオーディエンスをアクティブ化できるようになりました。 以前は、この制限は 10 でした。 |
| [ ファイルを今すぐ書き出す ](../destinations/ui/export-file-now.md) と外部オーディエンスの [ アドホックアクティベーション API](../destinations/api/ad-hoc-activation-api.md) サポート | バッチファイルベースの宛先に対してアクティブ化する際に、ファイルを今すぐ書き出し（UI）およびアドホックアクティベーション API を、外部オーディエンス（カスタムアップロード、類似、フェデレーション、他のExperience Platform アプリからのオーディエンスなど）と共に使用できるようになりました。 |
| OAuth 2 および mTLS を使用した HTTP API 宛先 | 認証エンドポイントに相互 TLS （mTLS）が必要な場合に、OAuth 2 を使用する HTTP API 宛先を作成して認証できるようになりました。宛先設定時のトークン取得で、mTLS がサポートされるようになりました。 |
| ZoomInfo アカウントの宛先 | Real-Time Customer Data Platform（B2B）から ZoomInfo にアカウントオーディエンスを送信できるようになりました。 |

{style="table-layout:auto"}

**修正点および改善点**

| 修正 | 説明 |
| --- | --- |
| [Snowflake ストリーミング ](../destinations/catalog/warehouses/snowflake.md) アカウント ID の検証 | アカウント ID 手順に正規表現バリデーターが追加されました。 ID を入力すると、組織 ID とアカウント ID が正しい形式（ドット区切り）であることが検証されるようになりました。 |
| [TikTok](../destinations/catalog/social/tiktok.md) コネクタの電話番号のハッシュ | 電話番号をキーに設定された ID がTikTokに対してアクティブ化されない、宛先カードの設定ミスを修正しました。 |

{style="table-layout:auto"}

詳しくは、[ 宛先の概要 ](../destinations/home.md) を参照してください。

## リアルタイム顧客プロファイル {#profile}

Adobe Experience Platform を使用すると、顧客がいつどこからブランドとやり取りしても、顧客に合わせて調整された、一貫性と関連性のある体験を提供できます。リアルタイム顧客プロファイルを使用すると、オンライン、オフライン、CRM、サードパーティデータなど、複数のチャネルのデータを組み合わせて、各顧客の全体像を確認できます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| プロファイルイベント時間セレクター | 「プロファイルイベント」タブで時間枠を設定して、その範囲内のイベントを表示および分析できるようになりました。 時間枠は最大 30 日まで設定できます。 デフォルトでは、過去 48 時間のイベントが表示されます。 |

{style="table-layout:auto"}

詳しくは、[ リアルタイム顧客プロファイルの概要 ](../profile/home.md) を参照してください。

## クエリサービス {#query-service}

クエリサービスを使用すると、標準 SQL を使用して Adobe Experience Platform [!DNL Data Lake] でデータに対してクエリを実行できます。[!DNL Data Lake] の任意のデータセットを結合したり、クエリ結果を新しいデータセットとして取得したりすることで、それらのデータセットをレポートやデータサイエンスワークスペースで使用したり、リアルタイム顧客プロファイルへの取り込みが可能になります。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| Data Distiller アクセラレーター | 「アクセラレータ」タブからアクセラレータを選択し、必要なパラメーターを入力して、生成された SQL を自分で書くことなく実行またはスケジュールできるようになりました。編集するカスタムテンプレートにアクセラレータをクローンします。 |

{style="table-layout:auto"}

詳しくは、[ クエリサービスの概要 ](../query-service/home.md) を参照してください。

## 実行と操作 {#run-and-operate}

実行ツールと操作ツールを使用して、Experience Platform実装を調べ、トラブルシューティングし、最適化します。 スケジュールされたバッチのアクティベーションを可視化し、設定の問題を特定して、システムの信頼性を向上させます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| [ ジョブ スケジュール ](../run-and-operate/job-schedules.md) 一般提供 | [!DNL Job Schedules] は、取り込みから宛先のアクティベーションまで、データパイプライン全体にわたるすべてのスケジュール済みバッチ処理ジョブの統合ビューを提供します。 実行ステータスを検査し、スケジュールの競合を特定し、ビジネス運用に影響を与える前に設定の問題を診断します。 |
| ヘルスチェックの一般提供 | スキーマと ID の設定が不十分だと、プロファイルの誤った作成、セグメントの選定の失敗、アクティベーションの不正確さなど、ダウンストリームの重大な問題が引き起こされます。 <br> ヘルスチェックにより、事後対応のトラブルシューティングからプロアクティブな予防的メンテナンスへのアプローチが移行します。 ヘルスチェックは、サンドボックスで使用されるスキーマと ID の常時オンスキャンで、調査とトラブルシューティングに使用できる問題の概要を提供します。 |

{style="table-layout:auto"}

詳しくは、[ 実行と操作の概要 ](../run-and-operate/overview.md)、[ ジョブスケジュールの検査 ](../run-and-operate/job-schedules.md)、および [Platform UI ガイド ](../landing/ui-guide.md) を参照してください。

## セグメント化サービス {#segmentation}

Experience Platformでは、顧客データからオーディエンスセグメントを作成でき、これらのオーディエンスをライフサイクル全体で管理できます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| Audience Builder の取り込みソース | 無効なストリーミングオーディエンスや非効率的なストリーミングオーディエンスの構築を回避するために、Audience Builder 内で各属性がバッチ、ストリーミングまたはエッジソースのいずれかから取得されたかを確認できるようになりました。 |
| アカウントオーディエンスビルダーのデータを含むフィールドのみを表示 | アカウントオーディエンスの作成時に、データを含む属性のみを表示するようにフィルタリングできるようになりました。 |

{style="table-layout:auto"}

詳しくは、[ オーディエンスの概要 ](../segmentation/home.md) を参照してください。

## ソース {#sources}

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

**新規または更新されたソース**

| ソース | 説明 |
| --- | --- |
| Change Data Capture のサポートの強化 | [!DNL Marketo Engage]、[!DNL Microsoft Dynamics]、および [!DNL Salesforce CRM] ソースで Change Data Capture を使用できるようになりました。 |

{style="table-layout:auto"}

詳しくは、[ソースの概要](../sources/home.md)を参照してください。

<!--

| [!DNL Deltashare] | The new [!DNL Deltashare] source lets you securely bring live, shared datasets from your partners or internal lakehouse environments directly into Adobe's applications without copying or manually uploading files. You connect to a [!DNL Deltashare] endpoint, choose the tables you need, and you can then use that governed, up-to-date data alongside your existing profiles and insights, so you spend less time on data wrangling and more time activating and analyzing it in your marketing workflows. |
| [!DNL Kobie] | The new [!DNL Kobie] source connector lets you directly ingest rich loyalty data from [!DNL Kobie] into Adobe's applications, so you can activate it alongside your existing customer profiles and insights. You connect your [!DNL Kobie] environment, configure the data objects you want to bring in (such as member status, transactions, and engagement), and then you can use that up-to-date loyalty information to build audiences, personalize experiences, and measure performance without juggling separate systems. |
| [!DNL Talon.One] | The new Talon.One source lets you seamlessly bring promotion and incentive data from Talon.One into Adobe's applications, so you can use it alongside your existing customer profiles and behavioral data. You connect your Talon.One account, select the entities and events you want to ingest (such as campaigns, coupons, and redemptions), and then you can use that real-time promotion context to build smarter audiences, personalize offers, and better understand which incentives are driving performance—without managing separate, disconnected systems. |

-->

<!--

| Data Engineering Agent | The following new and updated skills are available in the Data Engineering Agent:<br><br><ul><li><strong>Data onboarding:</strong> Follow step-by-step workflows and example prompts to connect sources, check data quality, enrich data semantically, and ingest data for B2C and B2B flows, with expected outputs and troubleshooting guidance in the docs.</li><li><strong>Data quality and validation:</strong> Validate data fields and datasets using two new skills (DataField and DataSet).</li><li><strong>Data collection:</strong> Get in-context guidance for complex Data Collection configurations and use conversational insights to explore lineage, dependencies, and relationships across your data collection objects.</li></ul> |

| [Snowflake Streaming](../destinations/catalog/warehouses/snowflake.md) multiregion support | The Snowflake Streaming connector is now available to customers beyond the US VA7 region. Use the region dropdown selector to select which Snowflake region your account is in. The documentation has been updated with the expected data structure for Snowflake streaming tables. |
| Audience filtering in activation workflow | You can now find and filter audiences in the **[!UICONTROL Select audiences]** step with the same experience as the Audiences page; for example, you can filter on audience origin to easily find the audience you are looking for. |

-->