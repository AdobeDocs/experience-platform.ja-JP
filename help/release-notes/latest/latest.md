---
title: Adobe Experience Platform リリースノート
description: Adobe Experience Platform の最新のリリースノートです。
exl-id: f854f9e5-71be-4d56-a598-cfeb036716cb
source-git-commit: f458b3f204e961f849782f26a1563a67d6ea4b60
workflow-type: tm+mt
source-wordcount: '1966'
ht-degree: 36%

---

# Adobe Experience Platform リリースノート

**リリース日：2022年8月24日**

Adobe Experience Platform の既存の機能に対するアップデート：

- [[!DNL Artificial Intelligence and Machine Learning Services]](#ai-and-ml-services)
- [[!DNL Dashboards]](#dashboards)
- [[!DNL Data Prep]](#data-prep)
- [[!DNL Destinations]](#destinations)
- [エクスペリエンスデータモデル（XDM）](#xdm)
- [リアルタイム顧客プロファイル](#profile)
- [セグメント化サービス](#segmentation)
- [ソース](#sources)

## [!DNL Artificial Intelligence/Machine Learning services] {#ai-and-ml-services}

AI/ML サービスは、マーケティングアナリストや従事者に対して、顧客体験のユースケースで人工知能とマシンラーニングの機能を活用する機能を提供します。これにより、マーケティングアナリストは、データサイエンスの専門知識を必要とせずに、ビジネスレベルの設定を使用して、会社のニーズに特化したモデルを設定できます。

### アトリビューション AI

アトリビューション AI は、コンバージョンイベントにつながるタッチポイントの貢献度を明らかにするために使用します。これは、マーケターが、カスタマージャーニーをまたいだ個別マーケティングタッチポイントのマーケティング効果を、マーケターが、定量化する際に役立ちます。

**更新された機能**

| 機能 | 説明 |
| ------- | ----------- |
| プライバシーのサポート | <ul><li>Attribution AIは、ユーザーの役割の定義と、管理するアクセスポリシーの定義をサポートするようになりました。 [権限](../../../help/access-control/abac/ui/permissions.md) 製品アプリケーション内のフィーチャとオブジェクトの場合。</li><li>監査ログのリソースは、アクティビティが発生すると自動的に記録されます。</li><li>～ [属性ベースのアクセス制御](../../../help/access-control/abac/overview.md)管理者は、特定の属性に基づいて、特定のオブジェクトや機能へのアクセスを制御できます。この属性は、ラベルなどのオブジェクトに追加できます。管理者は、これらのフィールドに対応する特定のフィールドやデータにのみアクセスできるユーザーの役割を定義できます。</li><li>[データの衛生状態](../../../help/hygiene/home.md) Attribution AI内の機能では、更新されたデータのみを使用して、さらなるトレーニングとスコアリングをおこなうことができます。 同様に、Attribution AIの削除をリクエストした場合、データは削除されたデータを使用しなくなります。</li><li>Attribution AIは Platform データセットを活用します。 GDPR への準拠を容易にするために、Adobe Experience Platform Privacy Serviceを使用して、データレイク、ID サービス、リアルタイム顧客プロファイルをまたいで、顧客のデータへのアクセスおよび削除要求に従うプロトコルを設定できます。 すべてのデータは送信時および保存時に暗号化されます。</li></ul> |

{style=&quot;table-layout:auto&quot;}

**注意**:Attribution AIは、既存のヘルスケアシールドまたはプライバシーシールドのお客様が追加の通知を受けるまで利用できません。

Attribution AIの詳細については、 [Attribution AI](../../intelligent-services/attribution-ai/overview.md) 概要。

### 顧客 AI

Real-time Customer Data Platform で使用できる顧客 AI は、個々のプロファイルのチャーンやコンバージョンなどのカスタム傾向スコアを大規模に生成するために使用します。

**更新された機能**

| 機能 | 説明 |
| ------- | ----------- |
| プライバシーのサポート | <ul><li>顧客 AI で、管理するユーザーの役割とアクセスポリシーの定義がサポートされるようになりました [権限](../../../help/access-control/abac/ui/permissions.md) 製品アプリケーション内のフィーチャとオブジェクトの場合。</li><li>監査ログのリソースは、アクティビティが発生すると自動的に記録されます。</li><li> ～ [属性ベースのアクセス制御](../../access-control/abac/overview.md)の管理者は、特定の属性に基づいて、特定のオブジェクトや機能へのアクセスを制御できます。 これらの属性は、ラベルなど、オブジェクトに追加されるメタデータにすることができます。 また、管理者は、特定のフィールドおよびそれらのフィールドに対応するデータのみにアクセスできるユーザーの役割を定義できます。</li><li>[データの衛生状態](../../../help/hygiene/home.md) 顧客 AI 内のの機能では、更新されたデータのみを使用して、さらなるトレーニングとスコアリングをおこなうことができます。 同様に、データの削除をリクエストした場合、顧客 AI は削除されたデータを使用しません。</li><li>顧客 AI は Platform データセットを活用します。 GDPR への準拠を容易にするために、Adobe Experience Platform Privacy Serviceを使用して、データレイク、ID サービス、リアルタイム顧客プロファイルをまたいで、顧客のデータへのアクセスおよび削除要求に従うプロトコルを設定できます。 すべてのデータは送信時および保存時に暗号化されます。</li></ul> |

{style=&quot;table-layout:auto&quot;}

**注意**:顧客 AI は、追加の通知を受けるまで、既存のヘルスケアシールドまたはプライバシーシールドのお客様では利用できません。

顧客 AI について詳しくは、 [顧客 AI](../../intelligent-services/customer-ai/overview.md) 概要。

## [!DNL Dashboards] {#dashboards}

Adobe Experience Platform [!DNL dashboards] 毎日のスナップショットでキャプチャされた、組織のデータに関する重要なインサイトを表示できます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| スケジュールされたアクティベーションウィジェット | この [!UICONTROL 予定されているアクティベーション] ウィジェットは、最近アクティブ化された宛先を表形式で表示します。 セグメントごとに、名前、宛先プラットフォーム、アクティベーションの開始日と終了日が含まれます。 このウィジェットを使用すると、オーディエンスがアクティブ化されている場所とタイミングを一目で見つけ、重複したアクティベーションや不要なアクティベーションをより透明にすることができます。 この累積情報は、アクティベーションが中断された場所も示します。 |

[!DNL Dashboards] について詳しくは、[[!DNL Dashboards] 概要](../../dashboards/home.md)を参照してください。

## [!DNL Data Prep] {#data-prep}

[!DNL Data Prep] を使用すると、データエンジニアは Experience Data Model（XDM）との間でデータをマッピング、変換および検証できます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| 警告を含むレコードの取り込みのサポート | Data Prep は、警告（重要でないエラー）をフィールドにローカライズし、残りの行を取り込めるようになりました。 すべてのマッパー変換エラーが、警告と共に、部分的に取り込まれた行が成功したと見なされ、警告と共にレポートされるようになりました。  監視は、警告および診断の詳細を含むレコードに対してもサポートされます。 警告を含むレコードの部分取り込みは、現在、データのストリーミングでのみ使用できます。 次のドキュメントを確認します。 [警告を含むレコードの取り込み](../../sources/tutorials/ui/monitor-streaming.md) を参照してください。 |

{style=&quot;table-layout:auto&quot;}

[!DNL Data Prep] について詳しくは、[[!DNL Data Prep] 概要](../../data-prep/home.md)を参照してください。

## [!DNL Destinations] {#destinations}

[!DNL Destinations] は、Adobe Experience Platform からのデータの円滑なアクティベーションを可能にする、事前定義済みの出力先プラットフォームとの統合です。宛先を使用して、クロスチャネルマーケティングキャンペーン、電子メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

<!--

**New or updated features**

| Feature | Description |
| ----------- | ----------- |
|  ||

{style="table-layout:auto"}

-->

**新しい宛先**

| 宛先 | 説明 |
| ----------- | ----------- |
| [[!DNL Outreach]](../..//destinations/catalog/crm/outreach.md) | [[!DNL Outreach]](https://www.outreach.io/) は、世界で最も B2B のバイヤーとセラーのインタラクションデータを扱う Sales Execution Platform で、販売データをインテリジェンスに変換するための独自の AI テクノロジーへの大量の投資を行っています。 [!DNL Outreach] は、組織がセールスエンゲージメントを自動化し、収益インテリジェンスに基づいて行動し、効率、予測可能性、成長を向上させるのに役立ちます。 |

{style=&quot;table-layout:auto&quot;}

宛先の一般的な情報については、[宛先の概要](../../destinations/home.md)を参照してください。

## エクスペリエンスデータモデル（XDM） {#xdm}

XDM は、Adobe Experience Platform に取り込むデータの共通構造および定義（スキーマ）を提供するオープンソース仕様です。XDM 標準規格に準拠しているため、すべての顧客体験データを共通の表現に反映させて、迅速かつ統合的な方法でインサイトを提供できます。顧客行動から有益なインサイトを得たり、セグメントを通じて顧客オーディエンスを定義したり、パーソナライゼーションのために顧客属性を使用したりできます。

**新しい XDM コンポーネント**

| コンポーネントのタイプ | 名前 | 説明 |
| --- | --- | --- |
| グローバルスキーマ | [[!UICONTROL AJO エンティティスキーマ]](https://github.com/adobe/xdm/blob/master/extensions/adobe/experience/customerJourneyManagement/ajo-entity.schema.json) | Adobe Journey Optimizerの非正規化エンティティを表します。 |
| クラス | [[!UICONTROL AJO 実行エンティティ]](https://github.com/adobe/xdm/blob/master/extensions/adobe/experience/customerJourneyManagement/ajo-execution-entity.schema.json) | セグメント化で使用するAdobe Journey Optimizer実行エンティティを表します。 |
| フィールドグループ | [[!UICONTROL Workfront Work Objects]](https://github.com/adobe/xdm/blob/master/extensions/adobe/experience/workfront/workobjects-all.schema.json) | Adobe Workfrontのすべての下位レベルのオブジェクト固有のフィールドグループを参照するラッパーフィールドグループ。 |

{style=&quot;table-layout:auto&quot;}

**更新された XDM コンポーネント**

| コンポーネントのタイプ | 名前 | 説明 |
| --- | --- | --- |
| フィールドグループ | [[!UICONTROL Journey Orchestrationステップイベントの共通フィールド]](https://github.com/adobe/xdm/blob/master/extensions/adobe/experience/journeyOrchestration/stepEvents/journeyStepEventCommonFieldsMixin.schema.json) | 2 つの新しいプロパティが追加されました。 `origTimeStamp` および `experienceID`. |
| フィールドグループ | [[!UICONTROL セグメントメンバーシップの詳細]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/shared/segmentation.schema.json) | に加えて [!UICONTROL XDM 個人プロファイル]の場合、このフィールドグループは、XDM ビジネスアカウントクラスに基づくスキーマでも使用できるようになりました。 |
| フィールドグループ | （複数） | Marketo B2B アクティビティに関連するいくつかのフィールドグループが、安定したステータスに更新されました。 次を参照してください。 [プルリクエスト](https://github.com/adobe/xdm/pull/1593/files) 」を参照してください。 |
| フィールドグループ | （複数） | 発生していたエラーを修正するために、複数の天気関連フィールドグループが更新されました。 `uvIndex` および `sunsetTime`. 次を参照してください。 [プルリクエスト](https://github.com/adobe/xdm/pull/1602/files) 」を参照してください。 |
| データタイプ | [[!UICONTROL 製品リスト項目]](https://github.com/adobe/xdm/blob/master/components/datatypes/productlistitem.schema.json) | 新しいプロパティ `productImageUrl` が追加されました。 |
| データタイプ | [[!UICONTROL QoE データの詳細情報]](https://github.com/adobe/xdm/blob/master/components/datatypes/qoedatadetails.schema.json) | 新しいプロパティ `framesPerSecond` が追加されました。 |
| データタイプ | [[!UICONTROL セッションの詳細情報]](https://github.com/adobe/xdm/blob/master/components/datatypes/sessiondetails.schema.json) | `sdkVersion` の名前は `appVersion` に変更されました。`meta:enum` および `description` フィールドも更新されました。 |
| データタイプとフィールドグループ | （複数） | 複数のメディアデータタイプとフィールドグループに、新しいフィールドと説明が更新されました。 次を参照してください。 [プルリクエスト](https://github.com/adobe/xdm/pull/1582/files) 」を参照してください。 |
| (すべて) | （複数） | を含むすべてのスキーマオブジェクト `enum` フィールドに、対応する `meta:enum` 各制約の表示値を示すフィールド。 次を参照してください。 [プルリクエスト](https://github.com/adobe/xdm/pull/1601/files) 」を参照してください。 |

{style=&quot;table-layout:auto&quot;}

Platform の XDM について詳しくは、[XDM システムの概要](../../xdm/home.md)を参照してください。

## リアルタイム顧客プロファイル {#profile}

Adobe Experience Platform を使用すると、いつでもどこでもブランドとのやり取りが顧客に対して調整され、一貫性と関連性のあるエクスペリエンスを提供できます。リアルタイム顧客プロファイルでは、オンライン、オフライン、CRM、サードパーティデータなど、複数のチャネルのデータを組み合わせた、各顧客の全体像を確認できます。プロファイルを使用すると、顧客データを統合ビューに統合して、すべての顧客インタラクションの実用的なタイムスタンプ付きのアカウントを提供できます。

| 機能 | 説明 |
| ------- | ----------- |
| 結合ポリシーのハード制限 | Platform は、次のハード制限を適用するようになりました： **5** サンドボックスごとのポリシーの結合 現在、サンドボックスに結合ポリシーが 6 つ以上ある場合、 **not** サンドボックスに結合ポリシーが 5 つ未満になるまで、新しい結合ポリシーを作成できます。 |
| 孤立したプロファイルエッジ属性のクリーンアップ | すべての組織で、プロファイルサービスが、ユーザーアクティビティ領域の残りのエッジ属性を毎日削除して、システム内のプロファイルをより正確に表現できるようになりました。 このクリーンアップは、特定のプロファイルのすべてのプロファイルフラグメントが削除された後に発生し、が存在するデータセットから結合されるプロファイルに影響を与えます `com_adobe_aep_profile_region_dataset` は `true`. このリリース以前の残りのエッジ属性フラグメントがこれらの指標に含まれていたので、ライセンス使用状況ダッシュボードの「アドレス可能なオーディエンス」指標にドロップが表示されたり、プロファイルダッシュボードの「プロファイル数」指標にドロップが表示されたりします。 |

{style=&quot;table-layout:auto&quot;}

プロファイルデータを操作するためのチュートリアルやベストプラクティスなど、リアルタイム顧客プロファイルの詳細については、[リアルタイム顧客プロファイルの概要](../../profile/home.md)を参照してください。

## セグメント化サービス {#segmentation}

[!DNL Segmentation Service] は、顧客ベース内のマーケティング可能なユーザーグループを区別する基準を記述することで、プロファイルの特定のサブセットを定義します。セグメントは、レコードデータ（人口統計情報など）や、顧客によるブランドとのやり取りを表す時系列イベントに基づいて作成できます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| 4000 セグメントのサポート | Platform を持つすべての組織は、最大 4,000 個のセグメント定義をサポートできるようになりました。 この変更がセグメントジョブ API に与える影響について詳しくは、 [セグメントジョブエンドポイントガイド](../../segmentation/api/segment-jobs.md) |

[!DNL Segmentation Service] について詳しくは、[セグメント化の概要](../../segmentation/home.md)を参照してください。

## ソース {#sources}

Adobe Experience Platform では、外部ソースからデータを取り込むときに、Platform サービスを使用して、そのデータの構造化、ラベル付け、拡張を行うことができます。アドビのアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM システムなど、様々なソースからデータを取り込むことができます。

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

**新機能**

| 機能 | 説明 |
| --- | --- |
| セルフサービスソース（バッチ SDK）の一般リリース | REST API ベースのデータソースを開発、テスト、統合し、簡単にソース仕様を設定できる方法で、バッチデータをExperience Platformに取り込みます。 ソース SDK を使用すると、次のことができます。 <ul><li>新しいソースをExperience Platformカタログに設定します。</li><li>サポートされる認証タイプ、スケジュール、リソースデータの取得方法など、ソースの仕様を定義します。</li><li>新しいソースに関するユーザー向けドキュメントを作成します。</li></ul> 詳しくは、 [セルフサービスソース（バッチ SDK）](../../sources/sources-sdk/overview.md). |
| [!DNL Google BigQuery] ソースの一般提供 | 以下を使用： [!DNL Google BigQuery] データを取り込むソース [!DNL Google BigQuery] data warehouse からExperience Platformへ。 詳しくは、 [[!DNL Google BigQuery] ソース](../../sources/connectors/databases/bigquery.md). |
| [!DNL Teradata Vantage] ソース（ベータ版） | 以下を使用： [!DNL Teradata Vantage] ハイブリッドマルチクラウド環境からExperience Platformにデータを取り込むソース。 詳しくは、 [[!DNL Teradata Vantage] ソース](../../sources/connectors/databases/teradata-vantage.md). |
| Adobe Analyticsソースのクロスリージョンサポート | 任意の地域（米国、英国またはシンガポール）からレポートスイートを取り込めるようになりました。レポートスイートは、ソース接続が作成されるExperience Platformサンドボックスインスタンスと同じ組織にマッピングする必要があります。 詳しくは、 [UI でのAdobe Analyticsソース接続の作成](../../sources/tutorials/ui/create/adobe-applications/analytics.md). |

{style=&quot;table-layout:auto&quot;}

ソースについて詳しくは、[ソースの概要](../../sources/home.md)を参照してください。