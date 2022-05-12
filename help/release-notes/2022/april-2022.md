---
title: Adobe Experience Platformリリースノート 2022 年 4 月
description: Adobe Experience Platformの 2022 年 4 月のリリースノート。
exl-id: 39233787-3089-4469-8363-b006ae41ae21
source-git-commit: 1f5c21e09e471ac0d6f1d98e219e06e571ede89f
workflow-type: tm+mt
source-wordcount: '2802'
ht-degree: 22%

---

# Adobe Experience Platform リリースノート

**リリース日：2022年4月27日（PT）**

Adobe Experience Platform の既存の機能に対するアップデート：

- [[!DNL Dashboards]](#dashboards)
- [データフロー](#dataflows)
- [[!DNL Data Prep]](#data-prep)
- [宛先](#destinations)
- [エクスペリエンスデータモデル（XDM）](#xdm)
- [[!DNL Intelligent Services]](#intelligent-services)
- [Real-time Customer Data Platform B2B エディション](#B2B)
- [ソース](#sources)

## [!DNL Dashboards] {#dashboards}

 Platform は、毎日のスナップショットで取得した、組織のデータに関する重要な情報を表示できる複数のダッシュボードを提供しています。

ダッシュボードは、組織のデータに事前設定されたレポートオプションを提供し、Platform 内のマーケターワークフローに直接組み込まれます。 これらのダッシュボードは、IT サポートの追加や、データウェアハウスの設計と実装を追加してデータをエクスポートおよび処理する時間と労力の必要なしに利用できます。

以下のウィジェットは、それぞれのダッシュボードのウィジェットライブラリで使用できます。 詳しくは、ドキュメントを参照してください。 [ウィジェットライブラリを使用してウィジェットを追加する方法](../../dashboards/customize/widget-library.md).

**新しいウィジェット**

| ウィジェット | ダッシュボード | 説明 |
| ------ | --------- | ----------- |
| [!UICONTROL Profiles added trend] | プロファイル | このウィジェットは線グラフを使用して、過去 30 日、90 日、12 ヶ月間にプロファイルストアに毎日追加された結合プロファイルの合計数を示します。 |
| [!UICONTROL 宛先ステータスにマッピングされたオーディエンス] | プロファイル | このウィジェットは、マッピングされたオーディエンスとマッピングされていないオーディエンスの合計数を 1 つの指標で表示し、ドーナツグラフを使用して合計の比例差を示します。 |
| [!UICONTROL オーディエンスのサイズ] | プロファイル | This widget provides a two-column table that lists up to 20 segments and the total number of audiences contained in each segment. リストは、適用される結合ポリシーに応じて異なります。また、オーディエンスの合計数に応じて、適用される結合ポリシーの順番が高い順から低い順に並べられます。 |
| [!UICONTROL プロファイル数のトレンド] | プロファイル | このウィジェットは折れ線グラフを使用して、時間の経過に伴うシステムに含まれるプロファイルの総数の傾向を示します。 データは 30 日、90 日、12 か月の期間で視覚化できます。 |
| [!UICONTROL ID 別の単一の ID プロファイル] | プロファイル | This widget uses a bar chart to illustrate the total number of profiles that are identified with only a single unique identifier. The widget supports up to five of the most commonly occurring identities. |
| [!UICONTROL 宛先のステータス] | 宛先 | このウィジェットは、有効な宛先の総数を 1 つの指標として表示し、ドーナツグラフを使用して、有効な宛先と無効な宛先の比例差を示します。 |
| [!UICONTROL Active destinations by destination platform] | 宛先 | This widget uses a two-column table to show a list of active destination platforms and the total number of active destinations for each destination platform. |
| [!UICONTROL すべての宛先でアクティブ化されたオーディエンス] | 宛先 | このウィジェットは、1 つの指標内のすべての宛先でアクティブ化されたオーディエンスの合計数を提供します。 |
| [!UICONTROL オーディエンスのアクティベーションの順序] | セグメント | このウィジェットには、宛先名、プラットフォーム、オーディエンスのアクティベーション日の一覧を示す 3 列のテーブルが表示されます。 |
| [!UICONTROL オーディエンスサイズのトレンド] | セグメント | このウィジェットには、30 日、90 日、12 か月の期間でセグメント定義の条件を満たすプロファイルの合計数の線グラフが表示されます。 |
| [!UICONTROL オーディエンスサイズの変更の傾向] | セグメント | このウィジェットは、最新の日別スナップショット間の特定のセグメントについて認定されたプロファイルの合計数の違いを示す線グラフを提供します。 トレンド分析の期間は、30 日、90 日、12 か月の期間で視覚化できます。 |
| [!UICONTROL ID 別のオーディエンスサイズのトレンド] | セグメント | このウィジェットは、選択した ID タイプに基づいて、特定のセグメントに対するオーディエンスサイズのトレンドを示します。 トレンド分析の期間は、30 日、90 日、12 か月の期間で視覚化できます。 |

**新機能**

| 機能 | ダッシュボード | 説明 |
| ------- | --------- | ----------- |
| 孤立したプロファイルセグメントメンバーシップのクリーンアップ | プロファイルとライセンスの使用 | プロファイルサービスで、残りのセグメントメンバーが毎日削除され、システム内のプロファイルをより正確に表現できるようになりました。 このクリーンアップは、特定のプロファイルのすべてのプロファイルフラグメントが削除された後に発生します。 このリリース以前のセグメントフラグメントはこの指標に含まれていたので、ライセンス使用状況ダッシュボードの「アドレス可能なオーディエンス」指標にドロップが表示されたり、プロファイルダッシュボードの「プロファイル数」指標にドロップが表示されたりします。 |

詳しくは、ドキュメントを参照してください。 [[!DNL Profiles]](../../dashboards/guides/profiles.md), [[!DNL Destinations]](../../dashboards/guides/destinations.md)、および [[!DNL Segments]](../../dashboards/guides/segments.md) ダッシュボード。

## データフロー {#dataflows}

Platformでは、データは様々なソースから取り込まれ、システム内で分析され、様々な宛先に対してアクティブ化されます。 Platform では、データフローに透明性を提供することで、この非線形の可能性があるデータフローのトラッキングプロセスを容易にします。

データフローは、Platform 間でデータを移動するジョブを表します。これらのデータフローは様々なサービスで構成され、ソースコネクタからターゲットデータセットにデータを移動できます。このデータは、最終的に宛先に対してアクティブ化される前に、ID サービスとリアルタイム顧客プロファイルによって利用されます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| セグメントダッシュボード | これで、監視ダッシュボードを使用して、セグメントのデータフローを監視できるようになりました。 詳しくは、 [UI でのセグメントの監視](../../dataflows/ui/monitor-segments.md) |

データフローの一般的な情報については、 [データフローの概要](../../dataflows/home.md) を参照してください。セグメント化について詳しくは、 [セグメントの概要](../../segmentation/home.md).

## [!DNL Data Prep] {#data-prep}

[!DNL Data Prep] を使用すると、データエンジニアは Experience Data Model（XDM）との間でデータをマッピング、変換および検証できます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| Adobe Analyticsソースのサポート | Adobe Analyticsソースで Data Prep 機能がサポートされ、データフローの作成時に Analytics レポートスイートのデータをターゲット XDM スキーマにマッピングできるようになりました。 に関するチュートリアルを参照してください。 [Analytics ソース接続の作成](../../sources/tutorials/ui/create/adobe-applications/analytics.md) を参照してください。 |
| 既存のマッピングルールのインポートのサポート | 既存のデータフローからマッピングルールをインポートして、データフロー設定を高速化し、エラーを制限できるようになりました。 に関するチュートリアルを参照してください。 [既存のマッピング・ルールのインポート](../../data-prep/ui/mapping.md) を参照してください。 |

[!DNL Data Prep] について詳しくは、[[!DNL Data Prep] 概要](../../data-prep/home.md)を参照してください。

## [!DNL Destinations] {#destinations}

[!DNL Destinations] は、Adobe Experience Platform からのデータの円滑なアクティベーションを可能にする、事前定義済みの出力先プラットフォームとの統合です。宛先を使用して、クロスチャネルマーケティングキャンペーン、電子メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

**新機能または更新された機能**

| 機能 | 説明 |
| ----------- | ----------- |
| 高度なエンタープライズ宛先コネクタ | 3 つのエンタープライズ宛先コネクタを一般に使用できるようになりました。 [[!DNL Amazon Kinesis]](../../destinations/catalog/cloud-storage/amazon-kinesis.md), [[!DNL Azure Event Hubs]](../../destinations/catalog/cloud-storage/azure-event-hubs.md)、および [[!DNL HTTP API]](../../destinations/catalog/streaming/http-destination.md). <br> エンタープライズ宛先コネクタの一般リリースには、ベータ段階で提供されていたすべての機能などが含まれます。 <ul><li>次の新しい認証機能が追加されました。 [Azure Event Hubs での共有アクセス署名](../../destinations/catalog/cloud-storage/azure-event-hubs.md#sas-authentication) その他 [認証タイプ](../../destinations/catalog/streaming/http-destination.md#authentication-information) HTTP API 宛先の（bearer トークン、OAuth 2）。</li><li>[履歴プロファイルデータのバックフィル](../../destinations/catalog/streaming/http-destination.md#historical-data-backfill) （最初にアクティブ化されたときにセグメントの対象として認定された過去のプロファイルを送信）。</li><li>データフローの実行指標がこれらの宛先でサポートされるようになりました。</li><li>[Additional segment metadata](../../destinations/catalog/streaming/http-destination.md#destination-details) included in the data payload, including segment names and segment timestamps;</li><li>のサポート [静的 IP アドレス](/help/destinations/catalog/streaming/ip-address-allow-list.md) Experience Platformを設定する必要があるお許可リスト客様向け</li></ul> |
| 宛先データフローのコンテキスト内アラート | You can now [subscribe to alerts](../../destinations/ui/alerts.md) when creating a destination dataflow, to receive alert messages regarding the status, success, or failure of your dataflow run. アラートを受信するには、Experience PlatformUI で、または E メールで選択します。 |

### Release process for advanced enterprise destination connectors {#release-process-enterprise-destinations}

Amazon Kinesis、Azure Event Hubs、HTTP API の宛先については、リリースプロセス（4 月 27 日以降）中に、以前のベータ版の宛先カードと、新しく一般公開された (GA) 宛先カードの両方が宛先カタログに表示されます。 ベータ版の宛先を使用して顧客が設定したデータフローは、今後数日以内に同じ宛先の GA バージョンに移行されます。 この移行は、最終的には 4 月 29 日（金）の終わりまでに完了する必要があります。 The Beta destinations will be continue to be visible during this short time-window and labeled as **Deprecated**.

If you have been utilizing these destinations in the Beta phase, please note the following:

- 以前に 3 つの宛先のいずれかでベータ版になっている場合は、何もする必要はありません。 All dataflows set up as part of Beta will continue to be functional and will be migrated to the GA version.
- 4 月 27 日以降にこれらの宛先を設定する場合は、新しい GA バージョンの宛先で設定してください。
- 非推奨とマークされたベータカードは、リリース操作が完了すると削除され、4 月 29 日（金）の終わりまでに見積もられます。 Experience Platformエンジニアリングチームは、リリース操作が成功するのを詳細に監視しています。

**新しい宛先**

| 宛先 | 説明 |
| ----------- | ----------- |
| [!DNL Criteo] | Connect and activate data to the [[!DNL Criteo]](../../destinations/catalog/advertising/criteo.md) advertising platform. |
| [!DNL Sendgrid] | へのデータの接続とアクティブ化 [[!DNL Sendgrid]](../../destinations/catalog/email-marketing/sendgrid.md) トランザクションメールおよびマーケティングメール用のプラットフォーム。 |

宛先の一般的な情報については、[宛先の概要](../../destinations/home.md)を参照してください。

## エクスペリエンスデータモデル（XDM） {#xdm}

XDM は、Adobe Experience Platformに取り込まれるデータの共通の構造と定義（スキーマ）を提供するオープンソース仕様です。 XDM 標準規格に準拠しているため、すべての顧客体験データを共通の表現に反映させて、迅速かつ統合的な方法でインサイトを提供できます。顧客行動から有益なインサイトを得たり、セグメントを通じて顧客オーディエンスを定義したり、パーソナライズ機能のために顧客属性を使用したりできます。

**新機能**

| 機能 | 説明 |
| --- | --- |
| スキーマの個々の標準フィールドを追加または削除する | スキーマエディターの UI で、標準フィールドグループの一部をスキーマに追加できるようになり、カスタムリソースを最初から作成しなくても、含めるように選択したフィールドの柔軟性が向上しました。<br><br>You can now also define ad-hoc custom fields directly within the schema structure and assign them to a new or existing custom field group without needing to create or edit the field group beforehand.<br><br>See the guide on [creating and editing schemas in the UI](../../xdm/ui/resources/schemas.md) for more information on these new workflows. |

{style=&quot;table-layout:auto&quot;}

**New XDM components**

| コンポーネントの種類 | 名前 | 説明 |
| --- | --- | --- |
| Global schema | [[!UICONTROL Data Hygiene Operation Request]](https://github.com/adobe/xdm/blob/master/schemas/hygiene/aep-hygiene-ops-record.schema.json) | 指定したデータセットまたはサンドボックス内のレコードを削除または変更するためのデータクレンジング要求の詳細をキャプチャします。 |
| 記述子 | [[!UICONTROL 時系列の精度記述子]](https://github.com/adobe/xdm/blob/master/schemas/descriptors/time-series/descriptorTimeSeriesGranularity.schema.json) | 時系列データと概要データの精度を示します。 スキーマに適用される場合、スキーマの `timestamp` フィールドは、この精度の期間での最初のタイムスタンプです。 |
| クラス | [[!UICONTROL XDM 概要指標]](https://github.com/adobe/xdm/blob/master/components/classes/summary_metrics.schema.json) | SQL SELECT と GROUP BY の結果など、グループ化ディメンションと共に事前に要約された指標を提供します。 |
| フィールドグループ | [[!UICONTROL 同意ポリシーの評価結果マップ]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/experienceevent-site-search.schema.json) | 個人の同意ポリシーの評価結果をキャプチャします。 |
| フィールドグループ | [[!UICONTROL サイト検索]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/experienceevent-site-search.schema.json) | 検索クエリ、フィルタリング、並べ替えなど、サイト検索関連の情報をキャプチャします。 |
| フィールドグループ | [[!UICONTROL Merge Leads]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/events/merge-leads.schema.json) | 2 つ以上のリードがマージされるイベントの詳細をキャプチャします。 |
| フィールドグループ | [[!UICONTROL 送信済み電子メール]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/events/emailsent.schema.json) | 電子メールが受信者に送信されるイベントの詳細をキャプチャします。 |
| フィールドグループ | [[!UICONTROL フィールドのステッチ]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/experienceevent-stitching.schema.json) | イベントの ID ステッチプロセスで計算された値をキャプチャします。 |
| フィールドグループ | [[!UICONTROL 監査用のセカンダリ受信者の詳細]](https://github.com/adobe/xdm/blob/master/extensions/adobe/experience/customerJourneyManagement/secondary-recipient-detail.schema.json) | 監査のセカンダリ受信者の詳細を取得するAdobe Journey Optimizerのフィールドグループです。 |
| Field group | [[!UICONTROL XDM Business Account Person Relation Details]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/account-person/account-person-details.schema.json) | アカウントと人物の関係に関連する詳細をキャプチャします。 |
| フィールドグループ | [[!UICONTROL Account Person Details]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/account-person/account-person-details.schema.json) | アカウントと人物の関係に関連する詳細をキャプチャします。 |
| データタイプ | [[!UICONTROL 買い物かご]](https://github.com/adobe/xdm/blob/master/components/datatypes/cart.schema.json) | e コマースの買い物かごに関する情報をキャプチャします。 |
| データタイプ | [[!UICONTROL 送料]](https://github.com/adobe/xdm/blob/master/components/datatypes/shipping.schema.json) | 1 つ以上の製品の配送先情報をキャプチャします。 |
| データタイプ | [[!UICONTROL サイト検索]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/experienceevent-site-search.schema.json) | サイト検索アクティビティの情報をキャプチャします。 |
| 拡張機能 (Workfront) | [[!UICONTROL オペレーショナルタスク属性]](https://github.com/adobe/xdm/blob/master/extensions/adobe/experience/workfront/opTask.schema.json) | Captures details related to an operational task. |
| 拡張機能 (Workfront) | [[!UICONTROL 作業Portfolio属性]](https://github.com/adobe/xdm/blob/master/extensions/adobe/experience/workfront/portfolio.schema.json) | 作業ポートフォリオに関連する詳細をキャプチャします。 |
| Extension (Workfront) | [[!UICONTROL 作業プログラム属性]](https://github.com/adobe/xdm/blob/master/extensions/adobe/experience/workfront/program.schema.json) | 作業プログラムに関連する詳細をキャプチャします。 |
| 拡張機能 (Workfront) | [[!UICONTROL 作業プロジェクト属性]](https://github.com/adobe/xdm/blob/master/extensions/adobe/experience/workfront/project.schema.json) | 作業プロジェクトに関連する詳細をキャプチャします。 |

{style=&quot;table-layout:auto&quot;}

**Updated XDM components**

| コンポーネントの種類 | 名前 | 説明のアップデート |
| --- | --- | --- |
| Global schema | [[!UICONTROL 宛先]](https://github.com/adobe/xdm/blob/master/schemas/destinations/destination.schema.json) | 次の新しい列挙値 `destinationCategory`. |
| 記述子 | [[!UICONTROL わかりやすい名前記述子]](https://github.com/adobe/xdm/blob/master/schemas/descriptors/display/alternateDisplayInfo.schema.json) | 推奨値 (`meta:enum`) は標準フィールドからは不要です。 |
| フィールドグループ | [[!UICONTROL ユーザーログインプロセス]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/experienceevent-user-login-details.schema.json) | `createProfile` フィールドが追加されました。 |
| データタイプ | [[!UICONTROL コマース]](https://github.com/adobe/xdm/blob/master/components/datatypes/marketing/commerce.schema.json) | 買い物かご関連のフィールドがいくつか追加されました。 |
| データタイプ | [[!UICONTROL 製品リスト項目]](https://github.com/adobe/xdm/blob/master/components/datatypes/productlistitem.schema.json) | 選択したオプションと割引額に対して追加される新しいフィールド。 |
| 拡張機能（インテリジェントサービス） | [[!UICONTROL Intelligent Services JourneyAI Send Time Optimization]](https://github.com/adobe/xdm/blob/master/extensions/adobe/experience/intelligentServices/profile-journeyai-sendtimeoptimization.schema.json) | Optimize storage format for send-time scores. |
| 拡張機能 (Workfront) | [[!UICONTROL Workfront Change イベント]](https://github.com/adobe/xdm/blob/master/extensions/adobe/experience/workfront/changeevent.schema.json) | 複数のフィールドが `workfront:customData` カスタムフォームフィールドのフィールド。 |
| 拡張機能 (Workfront) | [[!UICONTROL 作業タスクの属性]](https://github.com/adobe/xdm/blob/master/extensions/adobe/experience/workfront/task.schema.json) | 複数のフィールドが追加されました。 |
| 拡張機能 (Workfront) | [[!UICONTROL 作業用オブジェクト]](https://github.com/adobe/xdm/blob/master/extensions/adobe/experience/workfront/workobject.schema.json) | 親オブジェクトタイプとカスタムフォームフィールド用の新しいフィールドです。 |

{style=&quot;table-layout:auto&quot;}

Platform での XDM について詳しくは、 [XDM システムの概要](../../xdm/home.md).

## [!DNL Intelligent Services] {#intelligent-services}

インテリジェントサービスは、マーケティングアナリストや実践者に対して、顧客体験の使用事例で人工知能と機械学習の機能を活用する機能を提供します。これにより、マーケティングアナリストは、データ科学の専門知識を必要とせずに、ビジネスレベルの設定を使用して、会社のニーズに固有の予測を設定できます。

Attribution AIAI と顧客 AI を使用すると、顧客はマーケティング属性と顧客傾向に対して高度な AI/ML モデルを設定できます。 マルチデータセット機能を使用すると、事前にデータをステッチして準備しなくても、モデル設定時に複数のデータセットを取り込むことができます。

**更新された機能**

| 機能 | 説明 |
| ------- | ----------- |
| 複数データセットのサポート | The Multi Dataset feature now supports all Experience Event datasets as well as the selection of Identity Map as an identity. Customers can select the Identity Map and any associated IDs as long as there is a common identity namespace across datasets. Attribution AI supports the following schemas: Adobe Analytics, Experience Event, Consumer Experience Event. Customer AI supports all of these schemas plus the Adobe Audience Manager schema. Attribution AIAI と顧客 AI でのマルチデータセットのサポートについて詳しくは、 [Attribution AIユーザーガイド](../../intelligent-services/attribution-ai/user-guide.md) および [顧客 AI ユーザーガイド](../../intelligent-services/customer-ai/user-guide/configure.md). |
| 顧客 AI の新しいモデル評価指標 | 顧客 AI の新しい利益のグラフを使用すると、マーケターは予算と ROI の目標に基づいて、ターゲットにするグループサイズを決定できます。 新しいリフトチャートは、モデルの品質を測定し、ランダムターゲティングを超えて得られるリフトをより明確に把握できます。 詳しくは、 [顧客 AI でインサイトを見つける](../../intelligent-services/customer-ai/user-guide/discover-insights.md) 文書。 |

[!DNL Intelligent Services] について詳しくは、[[!DNL Intelligent Services] 概要](../../intelligent-services/home.md)を参照してください。

## Real-time Customer Data Platform B2B エディション {#B2B}

Real-time Customer Data Platform（Real-time CDP）上に構築された Real-time CDP B2B Edition は、B2B サービスを行っているマーケター向けに設計されています。複数のソースからのデータをまとめて、人物とアカウントプロファイルの単一のビューに結合します。この統合されたデータにより、マーケターは特定のオーディエンスを正確にターゲットにして、利用可能なすべてのチャネルでそれらのオーディエンスを惹き付けることができます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| のサポート `isDeleted` 機能 | すべて [!DNL Marketo] 次のデータセット `Activities` 今は支持する `isDeleted` マッピング。 The new mapping is automatically added to your existing B2B dataflows. 以下を使用して、 `isDeleted` 削除されたレコードを除外してデータを [!DNL Data Lake] は、ソースデータと一致します。 See the [[!DNL Marketo] mapping fields guide](../../sources/connectors/adobe-applications/mapping/marketo.md) for more information on `isDeleted`. |

Real-time Customer Data Platform B2B Edition の詳細については、 [B2B の概要](../../rtcdp/b2b-overview.md).

## ソース {#sources}

Adobe Experience Platform では、外部ソースからデータを取り込むときに、Platform サービスを使用して、そのデータの構造化、ラベル付け、拡張を行うことができます。アドビのアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM システムなど、様々なソースからデータを取り込むことができます。

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理をおこなうことができます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| サポート対象 [!DNL OneTrust Integration] | これで、 [!DNL OneTrust Integration] のソースから同意および環境設定データを取り込む [!DNL OneTrust] アカウントを Platform に送信します。 詳しくは、[ [!DNL OneTrust Integration]  ソース接続の作成](../../sources/connectors/consent-and-preferences/onetrust.md)のドキュメントを参照してください。 |
| サポート対象 [!DNL Square] | これで、 [!DNL Square] お客様からの支払いデータの取り込み元 [!DNL Square] アカウントを Platform に送信します。 |
| 顧客属性データフローの削除のサポート | 顧客属性ソースコネクタを使用して作成したデータフローを削除できるようになりました。 |

ソースについて詳しくは、[ソースの概要](../../sources/home.md)を参照してください。
