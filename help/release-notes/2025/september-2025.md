---
title: Adobe Experience Platform リリースノート 2025年9月
description: Adobe Experience Platform の 2025年9月のリリースノート。
exl-id: 9c5ab487-22b8-4590-b4ea-abec0f377703
source-git-commit: bd5611b23740f16e41048f3bc65f62312593a075
workflow-type: tm+mt
source-wordcount: '1415'
ht-degree: 22%

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

**リリース日：2025年9月23日（PT）**

Adobe Experience Platformの既存の機能に対する新機能とアップデート：

- [Agent Orchestrator](#agent-orchestrator)
- [アラート](#alerts)
- [宛先](#destinations)
- [エクスペリエンスデータモデル（XDM）](#xdm)
- [リアルタイム顧客プロファイル](#profile)
- [セグメント化サービス](#segmentation-service)
- [ソース](#sources)

## Agent Orchestrator {#agent-orchestrator}

Adobe Experience Platform Agent OrchestratorはAdobe Experience Platformの新しいアジェンティック層です。

**新機能**

| 機能 | 説明 |
| --- | --- |
| Agent Orchestrator | Adobe Experience Platform Agent OrchestratorはAdobe Experience Platformの新しいアジェンティック層です。 Experience Platform Agent Orchestratorは、プラットフォームの豊富なデータとカスタマーナレッジを活用するように設計されており、専用のエキスパートであるAdobe Experience Platform Agents の背後にあるインテリジェンスと推論を強化し、複雑な意思決定や問題解決のタスクを人間の監督によって迅速かつ大規模に実行できるようにします。 AI アシスタントなどの会話型インターフェイスで自然言語を使用して質問したり、ヘルプをリクエストしたりすると、Agent Orchestratorは自動的に専門のエージェントを呼び出して、適切な回答を得ます。 Agent Orchestratorは、会話履歴を記憶しているので、コンテキストを繰り返すことなく自然に前の質問に基づいて構築でき、複数のエージェントからのインサイトを組み合わせて、明確で統一された回答を提供します。 詳しくは、[Agent Orchestrator ドキュメント ](https://experienceleague.adobe.com/en/docs/experience-cloud-ai/experience-cloud-ai/agents/agent-orchestrator) を参照してください。 |
| Audience Agent | Audience Agentを使用すると、オーディエンスサイズの大きな変化の検出、重複オーディエンスの検出、オーディエンスインベントリの調査、オーディエンスのサイズの取得など、オーディエンスに関するインサイトを表示できます。 詳しくは、[Audience Agent ドキュメント ](https://experienceleague.adobe.com/en/docs/experience-cloud-ai/experience-cloud-ai/agents/audience) を参照してください。 |

詳しくは、[Agent Orchestrator ドキュメント ](https://experienceleague.adobe.com/en/docs/experience-cloud-ai/experience-cloud-ai/home) を参照してください。

## アラート {#alerts}

Experience Platformでは、様々なExperience Platform アクティビティに関するイベントベースのアラートを登録できます。 Experience Platform ユーザーインターフェイスの「[!UICONTROL &#x200B; アラート &#x200B;]」タブを使用して、様々なアラートルールを購読し、UI 内またはメール通知を通じてアラートメッセージを受け取るように選択できます。

**新機能**

| 機能 | 説明 |
| --- | --- |
| ストリーミングプロファイル取り込みアラート | データフローレベルでのストリーミング取り込みに関する 2 つの新しいアラートを登録できるようになりました。 <ul><li>ストリーミング取得の失敗率を超えています</li><li>ストリーミング取り込みスキップ率が超過しました</li></ul> プラットフォーム内またはメールのアラートは、デフォルトのしきい値または定義したカスタムのしきい値を超えたときに通知します。 詳しくは、[ プロファイルアラート ](../../observability/alerts/rules.md#profile) ガイドを参照してください。 |

{style="table-layout:auto"}

アラートについて詳しくは、[[!DNL Observability Insights]  概要 ](../../observability/home.md) を参照してください。

## 宛先 {#destinations}

Experience Platformから [!DNL Destinations] データの円滑なアクティベーションを可能にする、事前定義済みの出力先プラットフォームとの統合です。 宛先を使用して、クロスチャネルマーケティングキャンペーン、メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

**新規宛先または更新された宛先**

| 宛先 | 説明 |
| --- | --- |
| [!BADGE Beta]{type=Informative} [[!DNL Snowflake Batch]](../../destinations/catalog/cloud-storage/snowflake-batch.md) コネクタ | 新しい [!DNL Snowflake Batch] コネクタが使用できるようになり、特定のユースケースに対してストリーミングコネクタの代わりに使用できます。 |
| [[!DNL Data Landing Zone]](../../destinations/catalog/cloud-storage/data-landing-zone.md) 暗号化のサポート | RSA 形式の公開鍵を添付して、書き出したファイルを暗号化できるようになりました。これにより、他のクラウドストレージの宛先が機密情報に提供するのと同じレベルのセキュリティが提供されます。 |
| [[!DNL Pinterest]](../../destinations/catalog/advertising/pinterest.md) 宛先の認証有効期限の詳細 | [!DNL Pinterest] の宛先の認証の有効期限に関する情報がExperience Platform インターフェイスに直接表示されるようになり、認証の有効期限が切れるタイミングを確認し、データフローが中断される前に更新できるようになりました。 トークンの有効期限は、「**[!UICONTROL アカウント]**&#x200B;**[[!UICONTROL または]](../../destinations/ui/destinations-workspace.md#accounts)** 参照 **[[!UICONTROL タブの「アカウントの有効期限]](../../destinations/ui/destinations-workspace.md#browse)** 列から監視できます。 |

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| Experience Platform UI での宛先管理機能の強化 | 「参照 [[!UICONTROL &#x200B; タブと &#x200B;]](../../destinations/ui/destinations-workspace.md#browse) アカウント [[!UICONTROL &#x200B; タブの新しい並べ替え機能により、宛先管理ワークフローが向上し &#x200B;]](../../destinations/ui/destinations-workspace.md#accounts) す。 また、アカウント認証の有効期限が近づくと、視覚的なインジケーターも表示されるようになりました。<br> ![](../../destinations/assets/ui/workspace/expired-accounts.png){width="100" zoomable="yes"} |
| 列の幅の永続的な設定 | ページから移動して戻る際に、列の幅の設定が保持されるようになりました。 例えば、「[[!UICONTROL &#x200B; 参照 &#x200B;]](../../destinations/ui/destinations-workspace.md#browse) タブの列幅を調整した場合、他の場所に移動してそのタブに戻っても、カスタムの列幅は変わりません。 |

詳しくは、[ 宛先の概要 ](../../destinations/home.md) を参照してください。

## エクスペリエンスデータモデル（XDM） {#xdm}

XDM は、Experience Platformに取り込むデータの共通の構造と定義（スキーマ）を提供するオープンソース仕様です。 XDM 標準規格に準拠しているので、すべての顧客体験データを共通の表現に反映させて、迅速かつ統合的な方法でインサイトを提供できます。顧客アクションから有益なインサイトを得たり、セグメントを通じて顧客オーディエンスを定義したり、パーソナライズ機能のために顧客属性を使用したりできます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| モデルベースのスキーマ | モデルベースのスキーマを使用して、データモデリングを簡略化できます。 包括的なハウツー例やガイダンスを使用して、スキーマをより簡単に作成できるようになりました。 この機能は、現在、Campaign Orchestration のライセンスホルダーが使用でき、GA の Data Distillerのお客様にも拡大される予定で、データモデリングがよりアクセスしやすく効率的になります。 この機能には、時系列データと変更データのキャプチャ機能のサポートが含まれています。 |

詳しくは、[XDM の概要 ](../../xdm/home.md) を参照してください。

<!--

| Data Mirror | Ingest row-level changes from cloud data warehouses (e.g., Snowflake, Databricks, BigQuery) into Adobe Experience Platform using model-based schemas. Data Mirror eliminates upstream ETL and preserves relationships, versioning, and deletions by mirroring existing database structures directly into the data lake. Time-series and record event schema behavior with change data capture capabilities are all supported. This feature is currently available for Campaign Orchestration license holders and will expand through this limited release, also including Customer Journey Analytics customers. See the [Data Mirror documentation](../../xdm/data-mirror/overview.md) for more details. Contact your Adobe representative for access. |
-->

## リアルタイム顧客プロファイル {#profile}

Adobe Experience Platform を使用すると、顧客がいつどこからブランドとやり取りしても、顧客に合わせて調整された、一貫性と関連性のある体験を提供できます。リアルタイム顧客プロファイルでは、オンライン、オフライン、CRM、サードパーティデータなど、複数のチャネルのデータを組み合わせて、各顧客の全体像を確認できます。プロファイルを使用すると、顧客データを統合ビューに統合して、すべての顧客インタラクションの実用的なタイムスタンプ付きのアカウントを提供できます。

**更新された機能**

| 機能 | 説明 |
| ------- | ----------- |
| プロファイルビューアの機能強化 | 2025 年 9 月リリースには、プロファイルビューアに対する次の機能強化が含まれています。 <ul><li>**組み合わせビュー**：属性、イベント、インサイトが単一のビューに組み合わされました。</li><li>**AI で生成されたインサイト**：プロファイルの詳細ページに、AI で生成されたインサイトが表示され、プロファイルから生成された詳細を知ることができるようになりました。 これらのインサイトには、傾向スコアやトレンド分析などの情報を含めることができます。</li><li>**スタイルの更新**：プロファイルの詳細ページが視覚的に更新されました。</li><li>**参照**：検索とカスタマイズ機能を備えたインタラクティブカードベースのカルーセルを使用して、プロファイルを参照できるようになりました。</li></ul> |

詳しくは、[ リアルタイム顧客プロファイルの概要 ](../../profile/home.md) を参照してください。

## セグメント化サービス {#segmentation-service}

[!DNL Segmentation Service] は、顧客ベース内のマーケティング可能なユーザーグループを区別する基準を記述することで、プロファイルの特定のサブセットを定義します。オーディエンスは、レコードデータ（人口統計情報など）や、顧客によるブランドとのやり取りを表す時系列イベントに基づいて設定できます。

**新機能または更新された機能**

| 機能 | 説明 |
| ------- | ----------- |
| エクスペリエンスイベントの廃止を伴うアカウントオーディエンス | B2B アーキテクチャのアップグレード後、エクスペリエンスイベントを含むアカウントオーディエンスはサポートされなくなります。 代わりに、新しいセグメントセグメントのアプローチを使用します。エクスペリエンスイベントで People オーディエンスを作成してから、アカウントオーディエンスの作成時にその People オーディエンスを参照します。 これにより、B2B オーディエンスを作成するための、より柔軟で保守可能なアプローチが提供されます。 |

**重要な更新**

| 更新 | 説明 |
| ------- | ----------- |
| オーディエンス予測自動更新の復元 | オーディエンス予測の自動更新の機能強化は元に戻されました。 オーディエンスの推定は、引き続きセグメントビルダー内で生成されますが、自動更新機能は削除されています。 |
| 外部オーディエンス | 9 月 30 日（PT）以降、外部オーディエンスはセグメントビルダーの統合検索で取得されます。 Segment Match を使用している場合は、セグメントビルダー内で従来のエクスペリエンスを有効にできます。 |

詳しくは、[[!DNL Segmentation Service] 概要](../../segmentation/home.md)を参照してください。

## ソース {#sources}

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| 一般公開の新しいソース | 次のソースが一般提供されるようになりました。複数のソースコネクタがBetaから GA に更新されました。 <ul><li>[Acxiom のデータ取り込み ](../../sources/connectors/data-partners/acxiom-data-ingestion.md)</li><li>[Acxiom 見込み客データの取り込み ](../../sources/connectors/data-partners/acxiom-prospecting-data-import.md)</li><li>[Merkury エンタープライズ ](../../sources/connectors/data-partners/merkury.md)</li><li>[SAP Commerce](../../sources/connectors/ecommerce/sap-commerce.md)</li></ul>。これらのソースが完全にサポートされ、実稼動で使用できる状態になりました。 |
| [!DNL Snowflake] キーペア認証のサポート | キーペア認証をサポートし、Snowflake接続のセキュリティを強化しました。 基本認証（ユーザー名/パスワード）は 2025 年 11 月までに廃止される予定です。セキュリティを向上させるには、キーペア認証に移行することをお勧めします。 詳しくは、[[!DNL Snowflake]  ドキュメント ](../../sources/connectors/databases/snowflake.md) を参照してください。 |
| [!BADGE Beta]{type=Informative} [!DNL Capillary Streaming Events] | [[!DNL Capillary Streaming Events]  ソース ](../../sources/connectors/loyalty/capillary.md) を使用すると、[!DNL Capillary] アカウントからExperience Platformにロイヤルティデータをストリーミングできます。 |
| [!BADGE Beta]{type=Informative} [!DNL Relay Connector] | [[!DNL Relay Connector]](../../sources/tutorials/ui/create/marketing-automation/relay-connector.md) を使用して、[!DNL Relay Network] 統合からExperience Platformにイベントデータをストリーミングします。 |
| ソースでのプライベートリンクのサポートの一般提供 | 選択したソースグループに対して **プライベートリンク** を使用できるようになりました。 この機能を使用して、ソースが接続できるプライベートエンドポイントを作成します。 プライベートエンドポイントを使用すると、パブリックインターネットをバイパスする接続とデータフローを設定して、機密データのセキュリティとネットワーク分離を強化できます。 プライベートリンクのサポートは、次のソースで利用できます。 <ul><li>[[!DNL Azure Blob Storage]](../../sources/connectors/cloud-storage/blob.md)</li><li>[[!DNL ADLS Gen2]](../../sources/connectors/cloud-storage/adls-gen2.md)</li><li>[[!DNL Azure File Storage]](../../sources/connectors/cloud-storage/azure-file-storage.md)</li></ul>。詳しくは、（API での [ プライベートリンクの作成および ](../../sources/tutorials/api/private-link.md)UI での [ プライベートリンクの作成に関するガイドを参照し ](../../sources/tutorials/ui/private-link.md) ください。 |

詳しくは、[ソースの概要](../../sources/home.md)を参照してください。