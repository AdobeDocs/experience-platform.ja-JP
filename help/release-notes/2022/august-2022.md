---
title: Adobe Experience Platform リリースノート 2022年8月
description: Adobe Experience Platform の 2022年8月のリリースノート。
exl-id: dbf1e7a3-8599-4991-8932-f57d3b1c640d
source-git-commit: edd285c3d0638b606876c015dffb18309887dfb5
workflow-type: tm+mt
source-wordcount: '2082'
ht-degree: 100%

---

# Adobe Experience Platform リリースノート

**リリース日：2022年8月24日（PT）**

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

AI／ML サービスは、マーケティングアナリストや実務担当者に対して、顧客体験のユースケースで人工知能とマシンラーニングの機能を活用する機能を提供します。これにより、マーケティングアナリストは、データサイエンスの専門知識がなくても、ビジネスレベルの設定を使用して、会社のニーズに固有のモデルを設定できます。

### アトリビューション AI

アトリビューション AI は、コンバージョンイベントにつながるタッチポイントの貢献度を明らかにするために使用します。カスタマージャーニーをまたいだ個別マーケティングタッチポイントのマーケティング効果を、マーケターが定量化する際に役立ちます。

**更新された機能**

| 機能 | 説明 |
| ------- | ----------- |
| プライバシーのサポート | <ul><li> アトリビューション AI は、製品アプリケーション内の機能とオブジェクトの[権限](../../../help/access-control/abac/ui/permissions.md)を管理するユーザーの役割およびアクセスポリシーの定義をサポートするようになりました。 </li><li>監査ログのリソースは、アクティビティが発生すると自動的に記録されます。</li><li> [属性ベースのアクセス制御](../../access-control/abac/overview.md)により、管理者は特定の属性に基づいて、特定のオブジェクトや機能へのアクセスを制御できます。この属性は、ラベルなどのオブジェクトに追加されるメタデータにすることができます。管理者は、特定のフィールドと、これらのフィールドに対応するデータにのみアクセスできるユーザーの役割を定義することもできます。</li><li>アトリビューション AI では Platform データセットを活用します。 ブランドが受け取る可能性のある消費者の権利リクエストをサポートするには、Platform Privacy Service を使用して、アクセスおよび削除に対する消費者のリクエストを送信し、データレイク、ID サービス、リアルタイム顧客プロファイルをまたいでデータを削除する必要があります。  </li><li>モデルの入出力に使用されるすべてのデータセットは、Platform のガイドラインに従います。Platform データ暗号化は、保存中および送信中のデータに適用されます。[データ暗号化](../../../help/landing/governance-privacy-security/encryption.md)について詳しくは、ドキュメントを参照してください。</li></ul> |

{style="table-layout:auto"}

**メモ**：既存のヘルスケアシールドのお客様は、追加の通知が届くまでアトリビューション AI を利用できません。

アトリビューション AI について詳しくは、[アトリビューション AI](../../intelligent-services/attribution-ai/overview.md) の概要を参照してください。

### 顧客 AI

Real-Time Customer Data Platform で使用できる顧客 AI は、個々のプロファイルのチャーンやコンバージョンなどのカスタム傾向スコアを大規模に生成するために使用します。

**更新された機能**

| 機能 | 説明 |
| ------- | ----------- |
| プライバシーのサポート | <ul><li> 顧客 AI は、製品アプリケーション内の機能とオブジェクトの[権限](../../../help/access-control/abac/ui/permissions.md)を管理するユーザーの役割とアクセスポリシーの定義をサポートするようになりました。 </li><li>監査ログのリソースは、アクティビティが発生すると自動的に記録されます。</li><li> [属性ベースのアクセス制御](../../access-control/abac/overview.md)により、管理者は特定の属性に基づいて、特定のオブジェクトや機能へのアクセスを制御できます。 これらの属性は、オブジェクトに追加されるメタデータ（ラベルなど）にすることができます。 管理者は、特定のフィールドと、これらのフィールドに対応するデータにのみアクセスできるユーザーの役割を定義することもできます。</li><li>顧客 AI では Platform データセットを活用します。 ブランドが受け取る可能性のある消費者の権利リクエストをサポートするには、Platform Privacy Service を使用して、アクセスおよび削除に対する消費者のリクエストを送信し、データレイク、ID サービス、リアルタイム顧客プロファイルをまたいでデータを削除する必要があります。 </li><li>モデルの入出力に使用されるすべてのデータセットは、Platform のガイドラインに従います。Platform データ暗号化は、保存中および送信中のデータに適用されます。[データ暗号化](../../../help/landing/governance-privacy-security/encryption.md)について詳しくは、ドキュメントを参照してください。</li></ul> |

{style="table-layout:auto"}

**メモ**：既存の Healthcare Shield のお客様は、追加の通知が届くまで顧客 AI を利用できません。

顧客 AI について詳しくは、[顧客 AI](../../intelligent-services/customer-ai/overview.md) の概要を参照してください。

## [!DNL Dashboards] {#dashboards}

Adobe Experience Platform では、複数の [!DNL dashboards] を提供しており、毎日のスナップショットでキャプチャされた、組織のデータに関する重要な情報を表示できます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| 予定されているアクティベーションウィジェット | [!UICONTROL 予定されているアクティベーション]ウィジェットは、最近アクティブ化された宛先を表形式で表示します。セグメントごとに、名前、宛先プラットフォーム、アクティベーションの開始日と終了日が含まれます。このウィジェットを使用すると、オーディエンスがアクティブ化されている場所とタイミングを一目で見つけ、重複したアクティベーションや不要なアクティベーションをより透過的にすることができます。この累積情報は、アクティベーションが中断された場所も示します。 |

[!DNL Dashboards] について詳しくは、[[!DNL Dashboards] 概要](../../dashboards/home.md)を参照してください。

## [!DNL Data Prep] {#data-prep}

[!DNL Data Prep] を使用すると、データエンジニアは Experience Data Model（XDM）との間でデータをマッピング、変換および検証できます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| 警告を含むレコードの取り込みのサポート | Data Prep では、警告（重要でないエラー）をフィールドにローカライズし、残りの行を取り込めるようになりました。すべてのマッパー変換エラーが警告としてレポートされるようになりました。また、部分的に取り込んだ行は成功と見なされ、警告が表示されるようになりました。モニタリングは、警告および診断の詳細を含むレコードに対してもサポートされます。警告を含むレコードの部分的な取り込みは、現在、ストリーミングデータでのみ使用できます。詳しくは、[警告を含むレコードの取り込み](../../sources/tutorials/ui/monitor-streaming.md)に関するドキュメントを確認してください。 |

{style="table-layout:auto"}

[!DNL Data Prep] について詳しくは、[[!DNL Data Prep] 概要](../../data-prep/home.md)を参照してください。

## [!DNL Destinations] {#destinations}

[!DNL Destinations] は、Adobe Experience Platform からのデータの円滑なアクティベーションを可能にする、事前定義済みの出力先プラットフォームとの統合です。宛先を使用して、クロスチャネルマーケティングキャンペーン、電子メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

**新機能または更新された機能**

| 機能 | 説明 |
| ----------- | ----------- |
| （ベータ版）パーソナライゼーションの宛先に対する属性ベースのパーソナライゼーションのサポート | 属性ベースのパーソナライゼーションのベータ版リリースでは、[宛先カタログ](../../destinations/catalog/overview.md)に次の 2 つの新しいカードが表示されます。 <ul><li>**[!UICONTROL Adobe Target V2]**：このコネクタは現在ベータ版で、一部のお客様のみご利用いただけます。Adobe Target V1 カードが提供する機能に加えて、Target V2 コネクタにより、[マッピング手順](/help/destinations/ui/activate-profile-request-destinations.md#map-attributes)をアクティベーションワークフローに追加します。これにより、プロファイル属性を Adobe Target にマッピングし、属性ベースの同じページおよび次のページのパーソナライゼーションを有効にできます。</li><li>**[!UICONTROL 属性を含むカスタムパーソナライゼーション]**：このコネクタは現在ベータ版で、一部のお客様のみご利用いただけます。**[!UICONTROL カスタムパーソナライゼーション]**&#x200B;が提供する機能に加えて、 **[!UICONTROL 属性を含むカスタムパーソナライゼーション]**&#x200B;コネクタにより、オプションの[マッピング手順](../../destinations/ui/activate-profile-request-destinations.md#map-attributes)をアクティベーションワークフローに追加します。これにより、プロファイル属性をカスタムパーソナライゼーションの宛先にマッピングし、属性ベースの同じページおよび次のページのパーソナライゼーションを有効にできます。</li></ul> <br> プロファイル属性には、機密データが含まれている場合があります。 このデータを保護するために、**[!UICONTROL 属性を含むカスタムパーソナライゼーション]**&#x200B;の宛先では、データ収集に [Edge Network Server API](../../server-api/overview.md) を使用する必要があります。さらに、すべての Server API 呼び出しは、[認証済みコンテキスト](../../server-api/authentication.md)で行う必要があります。 |

{style="table-layout:auto"}

**新しい宛先**

| 宛先 | 説明 |
| ----------- | ----------- |
| [[!DNL Outreach]](../..//destinations/catalog/crm/outreach.md) | [[!DNL Outreach]](https://www.outreach.io/) は、世界で最も B2B のバイヤーとセラーのインタラクションデータを扱う Sales Execution Platform で、販売データをインテリジェンスに変換するための独自の AI テクノロジーへの大量の投資を行っています。[!DNL Outreach] は、組織がセールスエンゲージメントを自動化、収益インテリジェンスに基づいて行動し、効率、予測可能性、成長を向上させるのに役立ちます。 |

{style="table-layout:auto"}

宛先の一般的な情報については、[宛先の概要](../../destinations/home.md)を参照してください。

## エクスペリエンスデータモデル（XDM） {#xdm}

XDM は、Adobe Experience Platform に取り込むデータの共通構造および定義（スキーマ）を提供するオープンソース仕様です。XDM 標準規格に準拠しているため、すべての顧客体験データを共通の表現に反映させて、迅速かつ統合的な方法でインサイトを提供できます。顧客行動から有益なインサイトを得たり、セグメントを通じて顧客オーディエンスを定義したり、パーソナライゼーションのために顧客属性を使用したりできます。

**新しい XDM コンポーネント**

| コンポーネントのタイプ | 名前 | 説明 |
| --- | --- | --- |
| クラス | [[!UICONTROL AJO エンティティクラス]](https://github.com/adobe/xdm/blob/master/extensions/adobe/experience/customerJourneyManagement/ajo-entity.schema.json) | Adobe Journey Optimizer のルックアップスキーマを作成するためのレコードベースのクラス。 |
| フィールドグループ | [[!UICONTROL Workfront 作業オブジェクト]](https://github.com/adobe/xdm/blob/master/extensions/adobe/experience/workfront/workobjects-all.schema.json) | Adobe Workfront のすべての下位レベルのオブジェクト固有のフィールドグループを参照するラッパーフィールドグループ。 |

{style="table-layout:auto"}

**更新された XDM コンポーネント**

| コンポーネントのタイプ | 名前 | 説明 |
| --- | --- | --- |
| フィールドグループ | [[!UICONTROL Journey Orchestration ステップイベントの共通フィールド]](https://github.com/adobe/xdm/blob/master/extensions/adobe/experience/journeyOrchestration/stepEvents/journeyStepEventCommonFieldsMixin.schema.json) | `origTimeStamp` および `experienceID` の 2 つの新しいプロパティが追加されました。 |
| フィールドグループ | [[!UICONTROL セグメントメンバーシップの詳細]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/shared/segmentation.schema.json) | [!UICONTROL XDM 個人プロファイル]に加えて、このフィールドグループは、XDM ビジネスアカウントクラスに基づくスキーマでも使用できるようになりました。 |
| フィールドグループ | （複数） | Marketo B2B アクティビティに関連するいくつかのフィールドグループが、安定したステータスに更新されました。詳しくは、次の[プルリクエスト](https://github.com/adobe/xdm/pull/1593/files)を参照してください。 |
| フィールドグループ | （複数） | `uvIndex` および `sunsetTime` で発生していたエラーを修正するために、複数の天候関連のフィールドグループが更新されました。詳しくは、次の[プルリクエスト](https://github.com/adobe/xdm/pull/1602/files)を参照してください。 |
| データタイプ | [[!UICONTROL 製品リスト項目]](https://github.com/adobe/xdm/blob/master/components/datatypes/productlistitem.schema.json) | 新しいプロパティ `productImageUrl` が追加されました。 |
| データタイプ | [[!UICONTROL QoE データの詳細情報]](https://github.com/adobe/xdm/blob/master/components/datatypes/qoedatadetails.schema.json) | 新しいプロパティ `framesPerSecond` が追加されました。 |
| データタイプ | [[!UICONTROL セッションの詳細情報]](https://github.com/adobe/xdm/blob/master/components/datatypes/sessiondetails.schema.json) | `sdkVersion` の名前は `appVersion` に変更されました。`meta:enum` および `description` フィールドも更新されました。 |
| データタイプとフィールドグループ | （複数） | 複数のメディアデータタイプとフィールドグループに、新しいフィールドと説明が更新されました。詳しくは、次の[プルリクエスト](https://github.com/adobe/xdm/pull/1582/files)を参照してください。 |
| (すべて) | （複数） | `enum` フィールドを含むすべてのスキーマオブジェクトには、対応する `meta:enum` フィールドも含まれるようになり、各制約の表示値を示します。詳しくは、次の[プルリクエスト](https://github.com/adobe/xdm/pull/1601/files)を参照してください。 |

{style="table-layout:auto"}

Platform の XDM について詳しくは、[XDM システムの概要](../../xdm/home.md)を参照してください。

## リアルタイム顧客プロファイル {#profile}

Adobe Experience Platform を使用すると、顧客がいつどこからブランドとやり取りしても、顧客に合わせて調整された、一貫性と関連性のある体験を提供できます。リアルタイム顧客プロファイルでは、オンライン、オフライン、CRM、サードパーティデータなど、複数のチャネルのデータを組み合わせて、各顧客の全体像を確認できます。プロファイルを使用すると、顧客データを統合ビューに統合して、すべての顧客インタラクションの実用的なタイムスタンプ付きのアカウントを提供できます。

| 機能 | 説明 |
| ------- | ----------- |
| 結合ポリシーのハード制限 | Platform は、サンドボックスごとに **5** つの結合ポリシーのハード制限を適用するようになりました。サンドボックスに現在 5 つを超える結合ポリシーがある場合、サンドボックスの結合ポリシーが 5 つ未満になるまで、新しい結合ポリシーを作成できま&#x200B;**せん**。 |
| 孤立したプロファイルエッジ属性のクリーンアップ | すべての組織で、プロファイルサービスは、ユーザーアクティビティ領域の残りのエッジ属性を毎日削除して、システム内のプロファイルをより正確に表示できるようになりました。このクリーンアップは、特定のプロファイルのすべてのプロファイルフラグメントが削除された後に発生し、`com_adobe_aep_profile_region_dataset` が `true` とマークされているデータセットから結合されるプロファイルに影響を与えます。このリリース以前の残りのエッジ属性フラグメントはこの指標に含まれていたため、クリーンアップによってライセンス使用状況ダッシュボードの「アドレス可能なオーディエンス」指標が低下したり、プロファイルダッシュボードの「プロファイル数」指標が低下したりする場合があります。 |

{style="table-layout:auto"}

プロファイルデータを操作するためのチュートリアルやベストプラクティスなど、リアルタイム顧客プロファイルについて詳しくは、[リアルタイム顧客プロファイルの概要](../../profile/home.md)を参照してください。

## セグメント化サービス {#segmentation}

[!DNL Segmentation Service] は、顧客ベース内のマーケティング可能なユーザーグループを区別する基準を記述することで、プロファイルの特定のサブセットを定義します。セグメントは、レコードデータ（人口統計情報など）や、顧客によるブランドとのやり取りを表す時系列イベントに基づいて作成できます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| 4000 セグメントのサポート | Platform を持つすべての組織は、最大 4,000 個のセグメント定義をサポートできるようになりました。 この変更がセグメントジョブ API に与える影響について詳しくは、[セグメントジョブエンドポイントガイド](../../segmentation/api/segment-jobs.md)を参照してください。 |

[!DNL Segmentation Service] について詳しくは、[セグメント化の概要](../../segmentation/home.md)を参照してください。

## ソース {#sources}

Adobe Experience Platform では、外部ソースからデータを取り込むときに、Platform サービスを使用して、そのデータの構造化、ラベル付け、拡張を行うことができます。アドビのアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM システムなど、様々なソースからデータを取り込むことができます。

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

**新機能**

| 機能 | 説明 |
| --- | --- |
| セルフサービスソース（Batch SDK）の一般提供 | REST API ベースのデータソースを開発、テスト、統合し、簡単にソース仕様を設定できる方法で、バッチデータを Experience Platform に取り込みます。Sources SDK を使用すると、次のことができます。 <ul><li>新しいソースを Experience Platform カタログに設定する。</li><li>サポートされる認証タイプ、スケジュール、リソースデータの取得方法など、ソースの仕様を定義する。</li><li>新しいソースに関するユーザー向けドキュメントを作成する。</li></ul> 詳しくは、[セルフサービスソース（Batch SDK）](../../sources/sources-sdk/overview.md)に関するドキュメントを参照してください。 |
| [!DNL Google BigQuery] ソースの一般提供 | [!DNL Google BigQuery] ソースを使用すると、[!DNL Google BigQuery] データウェアハウスから Experience Platform にデータを取り込むことができます。詳しくは、[[!DNL Google BigQuery]  ソース](../../sources/connectors/databases/bigquery.md)に関するドキュメントを参照してください。 |
| [!DNL Teradata Vantage] ソース（ベータ版） | [!DNL Teradata Vantage] ソースを使用すると、ハイブリッドマルチクラウド環境から Experience Platform にデータを取り込むことができます。詳しくは、[[!DNL Teradata Vantage]  ソース](../../sources/connectors/databases/teradata-vantage.md)に関するドキュメントを参照してください。 |
| Adobe Analytics ソースのクロスリージョンサポート | 任意の地域（米国、英国またはシンガポール）からレポートスイートを取り込めるようになりました。レポートスイートは、ソース接続が作成されている Experience Platform サンドボックスインスタンスと同じ組織にマッピングする必要があります。詳しくは、[UI での Adobe Analytics ソース接続の作成](../../sources/tutorials/ui/create/adobe-applications/analytics.md)に関するガイドを参照してください。 |

{style="table-layout:auto"}

ソースについて詳しくは、[ソースの概要](../../sources/home.md)を参照してください。
