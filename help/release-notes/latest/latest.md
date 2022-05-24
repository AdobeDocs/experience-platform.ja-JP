---
title: Adobe Experience Platform リリースノート
description: Adobe Experience Platform の最新のリリースノートです。
exl-id: f854f9e5-71be-4d56-a598-cfeb036716cb
source-git-commit: 7040a3415ced04035e2a6a73292c2113411df21d
workflow-type: tm+mt
source-wordcount: '2916'
ht-degree: 100%

---

# Adobe Experience Platform リリースノート

**リリース日：2022年4月27日（PT）**

Adobe Experience Platform の既存の機能に対するアップデート：

- [[!DNL Artificial Intelligence and Machine Learning Services]](#ai/ml-services)
- [[!DNL Dashboards]](#dashboards)
- [データフロー](#dataflows)
- [[!DNL Data Prep]](#data-prep)
- [宛先](#destinations)
- [エクスペリエンスデータモデル（XDM）](#xdm)
- [Real-time Customer Data Platform B2B エディション](#B2B)
- [ソース](#sources)

## [!DNL Dashboards] {#dashboards}

Platform は、毎日のスナップショットで取得した、組織のデータに関する重要な情報を表示できる複数のダッシュボードを提供しています。

ダッシュボードは、組織のデータ用に事前設定されたレポートオプションを提供し、Platform 内のマーケターワークフローに直接組み込まれます。これらのダッシュボードを利用するのに、追加の IT サポートや、データウェアハウスの設計と実装を追加してデータをエクスポートおよび処理する時間と労力は、必要ありません。

次のウィジェットは、各ダッシュボードのウィジェットライブラリで使用できます。[ウィジェットライブラリを使用したウィジェットの追加方法](../../dashboards/customize/widget-library.md)について詳しくは、ドキュメントを参照してください。

**新しいウィジェット**

| ウィジェット | ダッシュボード | 説明 |
| ------ | --------- | ----------- |
| [!UICONTROL 追加されたプロファイルのトレンド] | プロファイル | このウィジェットは、折れ線グラフを使用して、過去 30 日、90 日または 12 か月にわたって毎日プロファイルストアに追加された結合プロファイルの合計数を示します。 |
| [!UICONTROL 宛先ステータスにマッピングされたオーディエンス] | プロファイル | このウィジェットは、マッピングされたオーディエンスとマッピングされていないオーディエンスの両方の合計数を単一の指標で表示し、ドーナツグラフを使用して、それぞれの合計の割合の違いを示します。 |
| [!UICONTROL オーディエンスサイズ] | プロファイル | このウィジェットは、最大 20 個のセグメントと各セグメントに含まれるオーディエンスの合計数をリストする 2 列の表を提供します。リストは、適用された結合ポリシーに依存し、オーディエンスの合計数に応じて高いものから低いものへと並べられます。 |
| [!UICONTROL プロファイル数のトレンド] | プロファイル | このウィジェットは、折れ線グラフを使用して、システムに含まれるプロファイルの合計数のトレンドの推移を示します。30 日、90 日および 12 か月の期間のデータを可視化できます。 |
| [!UICONTROL 単一の ID プロファイル (ID 別)] | プロファイル | このウィジェットは、棒グラフを使用して、単一の一意の ID のみで識別されるプロファイルの合計数を示します。このウィジェットは、最も一般的な ID を最大 5 つサポートします。 |
| [!UICONTROL 宛先ステータス] | 宛先 | このウィジェットは、有効な宛先の合計数を単一の指標として表示し、ドーナツグラフを使用して、有効な宛先と無効な宛先の割合の違いを示します。 |
| [!UICONTROL 宛先プラットフォーム別のアクティブな宛先] | 宛先 | このウィジェットは、2 列の表を使用して、アクティブな宛先プラットフォームと各宛先プラットフォームに対するアクティブな宛先の合計数のリストを表示します。 |
| [!UICONTROL すべての宛先にわたってアクティブ化されたオーディエンス] | 宛先 | このウィジェットは、すべての宛先にわたってアクティブ化されたオーディエンスの合計数を単一の指標で提供します。 |
| [!UICONTROL Audience Activation の順序] | セグメント | このウィジェットは、宛先名、プラットフォーム、オーディエンスのアクティベーション日をリストする 3 列の表を提供します。 |
| [!UICONTROL オーディエンスサイズのトレンド] | セグメント | このウィジェットは、30 日、90 日および 12 か月の期間で、任意のセグメント定義の条件を満たすプロファイルの合計数を示す折れ線グラフを提供します。 |
| [!UICONTROL オーディエンスサイズの変更のトレンド] | セグメント | このウィジェットは、最新の日別スナップショット間の特定のセグメントに対して選定されたプロファイルの合計数の違いを示す折れ線グラフを提供します。30 日、90 日および 12 か月の期間のトレンド分析の期間を可視化できます。 |
| [!UICONTROL ID 別のオーディエンスサイズのトレンド] | セグメント | このウィジェットは、選択された ID タイプに基づいて、特定のセグメントに対するオーディエンスサイズのトレンドを示します。30 日、90 日および 12 か月の期間のトレンド分析の期間を可視化できます。 |

**新機能**

| 機能 | ダッシュボード | 説明 |
| ------- | --------- | ----------- |
| 孤立したプロファイルセグメントメンバーシップのクリーンアップ | プロファイルとライセンスの使用法 | プロファイルサービスは、残りのセグメントメンバーを毎日削除して、システム内のプロファイルをより正確に表現するようになりました。このクリーンアップは、特定のプロファイルのすべてのプロファイルフラグメントが削除された後に行われます。このリリース以前の残りのセグメントフラグメントはこの指標に含まれていたため、クリーンアップによってライセンス使用状況ダッシュボードの「アドレス可能なオーディエンス」指標が低下したり、プロファイルダッシュボードの「プロファイル数」指標が低下したりする場合があります。 |

{style=&quot;table-layout:auto&quot;}

[[!DNL Profiles]](../../dashboards/guides/profiles.md)、[[!DNL Destinations]](../../dashboards/guides/destinations.md) および [[!DNL Segments]](../../dashboards/guides/segments.md) ダッシュボードについて詳しくは、ドキュメントを参照してください。

## データフロー {#dataflows}

Platformでは、データは様々なソースから取り込まれ、システム内で分析され、様々な宛先に対してアクティブ化されます。 Platform では、データフローに透明性を提供することで、この非線形の可能性があるデータフローのトラッキングプロセスを容易にします。

データフローは、Platform 間でデータを移動するジョブを表します。これらのデータフローは様々なサービスで構成され、ソースコネクタからターゲットデータセットにデータを移動できます。このデータは、最終的に宛先に対してアクティブ化される前に、ID サービスとリアルタイム顧客プロファイルによって利用されます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| セグメントダッシュボード | 監視ダッシュボードを使用して、セグメントのデータフローを監視できるようになりました。詳しくは、[UI でのセグメントの監視](../../dataflows/ui/monitor-segments.md)に関するガイドを参照してください。 |

データフローの一般的な情報については、[データフローの概要](../../dataflows/home.md)を参照してください。セグメント化について詳しくは、[セグメント化の概要](../../segmentation/home.md)を参照してください。

## [!DNL Data Prep] {#data-prep}

[!DNL Data Prep] を使用すると、データエンジニアは Experience Data Model（XDM）との間でデータをマッピング、変換および検証できます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| Adobe Analytics ソースのサポート | Adobe Analytics ソースは、データフローを作成する際に Analytics レポートスイートデータをターゲット XDM スキーマにマッピングできる、データ準備機能をサポートするようになりました。詳しくは、[Analytics ソース接続の作成](../../sources/tutorials/ui/create/adobe-applications/analytics.md)に関するチュートリアルを参照してください。 |
| 既存のマッピングルールの読み込みのサポート | データフロー設定を加速させ、エラーを制限するために、既存のデータフローからマッピングルールを読み込めるようになりました。詳しくは、[既存のマッピングルールの読み込み](../../data-prep/ui/mapping.md)に関するチュートリアルを参照してください。 |

[!DNL Data Prep] について詳しくは、[[!DNL Data Prep] 概要](../../data-prep/home.md)を参照してください。

## [!DNL Destinations] {#destinations}

[!DNL Destinations] は、Adobe Experience Platform からのデータの円滑なアクティベーションを可能にする、事前定義済みの出力先プラットフォームとの統合です。宛先を使用して、クロスチャネルマーケティングキャンペーン、電子メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

**新機能または更新された機能**

| 機能 | 説明 |
| ----------- | ----------- |
| 高度なエンタープライズ宛先コネクタ | [[!DNL Amazon Kinesis]](../../destinations/catalog/cloud-storage/amazon-kinesis.md)、[[!DNL Azure Event Hubs]](../../destinations/catalog/cloud-storage/azure-event-hubs.md)、[[!DNL HTTP API]](../../destinations/catalog/streaming/http-destination.md) の 3 つのエンタープライズ宛先コネクタが一般入手可能になりました。<br> 一般入手可能なエンタープライズ宛先コネクタには、ベータ段階で以前提供されていたすべての機能に加えて、次の機能が含まれます。 <ul><li>[Azure Event Hubs の共有アクセス署名](../../destinations/catalog/cloud-storage/azure-event-hubs.md#sas-authentication)や HTTP API 宛先のその他の[認証タイプ](../../destinations/catalog/streaming/http-destination.md#authentication-information)（ベアラートークン、OAuth 2）を含む、新しい認証機能。</li><li>[履歴プロファイルデータのバックフィル](../../destinations/catalog/streaming/http-destination.md#historical-data-backfill)（最初にアクティブ化される際にセグメントに対して選定された履歴プロファイルを送信）。</li><li>データフローの実行指標で、これらの宛先をサポート。</li><li>データペイロードに含まれる[追加のセグメントメタデータ](../../destinations/catalog/streaming/http-destination.md#destination-details)（セグメント名およびセグメントタイムスタンプなど）。</li><li>Experience Platform を許可リストに記載する必要があるお客様向けの[静的 IP アドレス](/help/destinations/catalog/streaming/ip-address-allow-list.md)のサポート。</li></ul> |
| 宛先データフローのコンテキスト内アラート | 宛先データフローを作成する際に、[アラートを登録](../../destinations/ui/alerts.md)して、データフローの実行のステータス、成功、失敗に関するアラートメッセージを受信できるようになりました。Experience Platform UI またはメールでアラートを受信することを選択できます。 |

### 高度なエンタープライズ宛先コネクタのリリースプロセス {#release-process-enterprise-destinations}

Amazon Kinesis、Azure Event Hubs および HTTP API 宛先について、リリースプロセス（4月27日開始）の間、以前のベータ版の宛先カードと新しい一般入手可能（GA）な宛先カードの両方が宛先カタログに表示されます。ベータ版の宛先を使用してお客様が設定したデータフローは、今後 2 ～ 3 日のうちに、同じ宛先の GA バージョンに移行されます。この移行は、最終的に4月29日金曜日（PT）の終わりまでに完了されます。ベータ版の宛先は、この短い期間の間、引き続き表示され、**非推奨**&#x200B;としてラベル付けされます。

ベータ段階のこれらの宛先を使用している場合は、次の点に注意してください。

- 過去に 3 つの宛先でベータ版を使用したことがある場合は、何もする必要はありません。ベータ版の一部として設定されたすべてのデータフローは、引き続き機能し、GA バージョンに移行されます。
- 4月27日以降にこれらの宛先を設定する場合は、新しい GA バージョンの宛先で行ってください。
- 非推奨とマークされたベータ版のカードは、リリース作業が完了（4月29日金曜日（PT）の終わりを予定）すると、削除されます。Experience Platform エンジニアリングチームは、リリース作業が成功するよう注意深く監視しています。

**新しい宛先**

| 宛先 | 説明 |
| ----------- | ----------- |
| [!DNL Criteo] | [[!DNL Criteo]](../../destinations/catalog/advertising/criteo.md) 広告プラットフォームにデータを接続してアクティブ化します。 |
| [!DNL Sendgrid] | トランザクションメールおよびマーケティングメール用 [[!DNL Sendgrid]](../../destinations/catalog/email-marketing/sendgrid.md) プラットフォームにデータを接続してアクティブ化します。 |

宛先の一般的な情報については、[宛先の概要](../../destinations/home.md)を参照してください。

## エクスペリエンスデータモデル（XDM） {#xdm}

XDM は、Adobe Experience Platform に取り込むデータの共通構造および定義（スキーマ）を提供するオープンソース仕様です。XDM 標準規格に準拠しているため、すべての顧客体験データを共通の表現に反映させて、迅速かつ統合的な方法でインサイトを提供できます。顧客行動から有益なインサイトを得たり、セグメントを通じて顧客オーディエンスを定義したり、パーソナライズ機能のために顧客属性を使用したりできます。

**新機能**

| 機能 | 説明 |
| --- | --- |
| スキーマの個別の標準フィールドの追加と削除 | スキーマエディター UI を使用して、標準フィールドグループの一部をスキーマに追加できるようになり、最初からカスタムリソースを構築しなくても、含めるフィールドをより柔軟に選択できるようになりました。<br><br>また、スキーマ構造内で直接アドホックカスタムフィールドを定義して、新規または既存のカスタムフィールドグループに割り当てることができるようになり、フィールドグループを事前に作成または編集する必要がなくなりました。<br><br>これらの新しいワークフローについて詳しくは、[UI でのスキーマの作成および編集](../../xdm/ui/resources/schemas.md)に関するガイドを参照してください。 |

{style=&quot;table-layout:auto&quot;}

**新しい XDM コンポーネント**

| コンポーネントのタイプ | 名前 | 説明 |
| --- | --- | --- |
| グローバルスキーマ | [[!UICONTROL データハイジーン操作リクエスト]](https://github.com/adobe/xdm/blob/master/schemas/hygiene/aep-hygiene-ops-record.schema.json) | 指定されたデータセットまたはサンドボックス内のレコードを削除または変更するために、データクレンジングリクエストの詳細を取得します。 |
| 記述子 | [[!UICONTROL 時系列の精度記述子]](https://github.com/adobe/xdm/blob/master/schemas/descriptors/time-series/descriptorTimeSeriesGranularity.schema.json) | 時系列データおよび概要データの精度を示します。スキーマに適用される場合、そのスキーマの `timestamp` フィールドが、この精度の期間の最初のタイムスタンプです。 |
| クラス | [[!UICONTROL XDM 概要指標]](https://github.com/adobe/xdm/blob/master/components/classes/summary_metrics.schema.json) | グループディメンションで事前に要約された指標を提供します（SQL SELECT と GROUP BY の結果など）。 |
| フィールドグループ | [[!UICONTROL 同意ポリシー評価結果マップ]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/experienceevent-site-search.schema.json) | 個人に対する同意ポリシー評価結果を取得します。 |
| フィールドグループ | [[!UICONTROL サイト検索]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/experienceevent-site-search.schema.json) | サイト検索関連情報を取得します（検索クエリ、フィルタリング、順序付けなど）。 |
| フィールドグループ | [[!UICONTROL リードを結合]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/events/merge-leads.schema.json) | 2 人以上のリードが結合されるイベントの詳細を取得します。 |
| フィールドグループ | [[!UICONTROL 送信済み電子メール]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/events/emailsent.schema.json) | メールが受信者に送信されるイベントの詳細を取得します。 |
| フィールドグループ | [[!UICONTROL フィールドのステッチ]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/experienceevent-stitching.schema.json) | イベントに対して ID ステッチプロセスで計算された値を取得します。 |
| フィールドグループ | [[!UICONTROL 監査用のセカンダリ受信者の詳細]](https://github.com/adobe/xdm/blob/master/extensions/adobe/experience/customerJourneyManagement/secondary-recipient-detail.schema.json) | 監査用のセカンダリ受信者の詳細を取得する Adobe Journey Optimizer フィールドグループ。 |
| フィールドグループ | [[!UICONTROL XDM ビジネスアカウント人物関係の詳細]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/account-person/account-person-details.schema.json) | アカウントと人物の関係に関する詳細を取得します。 |
| フィールドグループ | [[!UICONTROL アカウント人物の詳細]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/account-person/account-person-details.schema.json) | アカウントと人物の関係に関する詳細を取得します。 |
| データタイプ | [[!UICONTROL 買い物かご]](https://github.com/adobe/xdm/blob/master/components/datatypes/cart.schema.json) | eコマースの買い物かごに関する情報を取得します。 |
| データタイプ | [[!UICONTROL 送料]](https://github.com/adobe/xdm/blob/master/components/datatypes/shipping.schema.json) | 1 つ以上の製品に関する発送情報を取得します。 |
| データタイプ | [[!UICONTROL サイト検索]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/experienceevent-site-search.schema.json) | サイト検索アクティビティに関する情報を取得します。 |
| 拡張（Workfront） | [[!UICONTROL 操作タスク属性]](https://github.com/adobe/xdm/blob/master/extensions/adobe/experience/workfront/opTask.schema.json) | 操作タスクに関する詳細を取得します。 |
| 拡張（Workfront） | [[!UICONTROL 作業ポートフォリオ属性]](https://github.com/adobe/xdm/blob/master/extensions/adobe/experience/workfront/portfolio.schema.json) | 作業ポートフォリオに関する詳細を取得します。 |
| 拡張（Workfront） | [[!UICONTROL 作業プログラム属性]](https://github.com/adobe/xdm/blob/master/extensions/adobe/experience/workfront/program.schema.json) | 作業プログラムに関する詳細を取得します。 |
| 拡張（Workfront） | [[!UICONTROL 作業プロジェクト属性]](https://github.com/adobe/xdm/blob/master/extensions/adobe/experience/workfront/project.schema.json) | 作業プロジェクトに関する詳細を取得します。 |

{style=&quot;table-layout:auto&quot;}

**更新された XDM コンポーネント**

| コンポーネントのタイプ | 名前 | 説明のアップデート |
| --- | --- | --- |
| グローバルスキーマ | [[!UICONTROL 宛先]](https://github.com/adobe/xdm/blob/master/schemas/destinations/destination.schema.json) | `destinationCategory` の新しい列挙値。 |
| 記述子 | [[!UICONTROL わかりやすい名前記述子]](https://github.com/adobe/xdm/blob/master/schemas/descriptors/display/alternateDisplayInfo.schema.json) | 標準フィールドから不要な、提案された値（`meta:enum`）を削除するサポートが追加されました。 |
| フィールドグループ | [[!UICONTROL ユーザーログインプロセス]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/experienceevent-user-login-details.schema.json) | `createProfile` フィールドが追加されました。 |
| データタイプ | [[!UICONTROL コマース]](https://github.com/adobe/xdm/blob/master/components/datatypes/marketing/commerce.schema.json) | いくつかの買い物かご関連フィールドが追加されました。 |
| データタイプ | [[!UICONTROL 製品リスト項目]](https://github.com/adobe/xdm/blob/master/components/datatypes/productlistitem.schema.json) | 選択されたオプションおよび割引額に、新しいフィールドが追加されました。 |
| 拡張（インテリジェントサービス） | [[!UICONTROL Intelligent Services JourneyAI 送信時間の最適化]](https://github.com/adobe/xdm/blob/master/extensions/adobe/experience/intelligentServices/profile-journeyai-sendtimeoptimization.schema.json) | 送信時間スコアのストレージ形式を最適化します。 |
| 拡張（Workfront） | [[!UICONTROL Workfront 変更イベント]](https://github.com/adobe/xdm/blob/master/extensions/adobe/experience/workfront/changeevent.schema.json) | いくつかのフィールドが、カスタムフォームフィールドの `workfront:customData` フィールドに置き換えられました。 |
| 拡張（Workfront） | [[!UICONTROL 作業タスク属性]](https://github.com/adobe/xdm/blob/master/extensions/adobe/experience/workfront/task.schema.json) | いくつかのフィールドが追加されました。 |
| 拡張（Workfront） | [[!UICONTROL 作業オブジェクト]](https://github.com/adobe/xdm/blob/master/extensions/adobe/experience/workfront/workobject.schema.json) | 親オブジェクトタイプおよびカスタムフォームフィールド用の新しいフィールドです。 |

{style=&quot;table-layout:auto&quot;}

Platform の XDM について詳しくは、[XDM システムの概要](../../xdm/home.md)を参照してください。

## [!DNL Artificial Intelligence/Machine Learning services] {#ai/ml-services}

AI/ML サービスは、マーケティングアナリストや従事者に対して、顧客体験のユースケースで人工知能とマシンラーニングの機能を活用する機能を提供します。これにより、マーケティングアナリストは、データサイエンスの専門知識がなくても、ビジネスレベルの設定を使用して、会社のニーズに固有の予測を設定できます。

### アトリビューション AI

アトリビューション AI は、コンバージョンイベントにつながるタッチポイントの貢献度を明らかにするために使用します。これは、マーケターが、カスタマージャーニーをまたいだ個別マーケティングタッチポイントのマーケティング効果を、マーケターが、定量化する際に役立ちます。

**更新された機能**

| 機能 | 説明 |
| ------- | ----------- |
| マルチデータセットのサポート | マルチデータセット機能では、すべての Experience Event データセットをサポートするようになり、ID マップを ID として選択できるようになりました。お客様は、データセット全体で共通の ID 名前空間がある限り、ID マップおよび関連する任意の ID を選択できます。アトリビューション AI は、Adobe Analytics、Experience Event、Consumer Experience Event のスキーマをサポートします。アトリビューション AI でのマルチデータセットのサポートについて詳しくは、[アトリビューション AI ユーザーガイド](../../intelligent-services/attribution-ai/user-guide.md)を参照してください。 |

[!DNL Intelligent Services] について詳しくは、[[!DNL Intelligent Services] 概要](../../intelligent-services/home.md)を参照してください。

### 顧客 AI

Real-time Customer Data Platform で使用できる顧客 AI は、個々のプロファイルのチャーンやコンバージョンなどのカスタム傾向スコアを大規模に生成するために使用します。これを実現するために、ビジネスニーズを機械学習の問題に変換したり、アルゴリズムを選択したり、トレーニング、またはデプロイする必要はありません。

**更新された機能**

| 機能 | 説明 |
| ------- | ----------- |
| マルチデータセットのサポート | マルチデータセット機能では、すべての Experience Event データセットをサポートするようになり、ID マップを ID として選択できるようになりました。お客様は、データセット全体で共通の ID 名前空間がある限り、ID マップおよび関連する任意の ID を選択できます。顧客 AI は、Adobe Analytics、エクスペリエンスイベント、コンシューマーエクスペリエンスイベントおよび Adobe Audience Manager の各スキーマをサポートします。顧客 AI でのマルチデータセットのサポートについて詳しくは、[顧客 AI ユーザーガイド](../../intelligent-services/customer-ai/user-guide/configure.md)を参照してください。 |
| 顧客 AI の新しいモデル評価指標 | 顧客 AI の新しいゲインチャートを使用すると、マーケターは、予算と ROI 目標に基づいてターゲットとするグループサイズを決定できます。新しいリフトチャートは、モデルの品質を測定し、ランダムターゲティングに対して得られるリフトをよりわかりやすく可視化します。詳しくは、[顧客 AI でインサイトを見つける](../../intelligent-services/customer-ai/user-guide/discover-insights.md)を参照してください。 |

[!DNL Intelligent Services] について詳しくは、[[!DNL Intelligent Services] 概要](../../intelligent-services/home.md)を参照してください。

## Real-time Customer Data Platform B2B エディション {#B2B}

Real-time Customer Data Platform（Real-time CDP）上に構築された Real-time CDP B2B Edition は、B2B サービスを行っているマーケター向けに設計されています。複数のソースからのデータをまとめて、人物とアカウントプロファイルの単一のビューに結合します。この統合されたデータにより、マーケターは特定のオーディエンスを正確にターゲットにして、利用可能なすべてのチャネルでそれらのオーディエンスを惹き付けることができます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| `isDeleted` 機能のサポート | `Activities` を除くすべての [!DNL Marketo] データセットで、`isDeleted` マッピングをサポートするようになりました。新しいマッピングは、既存の B2B データフローに自動的に追加されます。`isDeleted` マッピングを使用して、[!DNL Data Lake] のデータがソースデータと一致するように、削除されたレコードをフィルターで除外できます。`isDeleted` について詳しくは、[[!DNL Marketo]  マッピングフィールドガイド](../../sources/connectors/adobe-applications/mapping/marketo.md)を参照してください。 |

Real-time Customer Data Platform B2B Edition について詳しくは、[B2B の概要](../../rtcdp/b2b-overview.md)を参照してください。

## ソース {#sources}

Adobe Experience Platform では、外部ソースからデータを取り込むときに、Platform サービスを使用して、そのデータの構造化、ラベル付け、拡張を行うことができます。アドビのアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM システムなど、様々なソースからデータを取り込むことができます。

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| [!DNL OneTrust Integration] のサポート | [!DNL OneTrust Integration] ソースを使用して、同意および環境設定データを [!DNL OneTrust] アカウントから Platform に取り込むことができるようになりました。詳しくは、[ [!DNL OneTrust Integration]  ソース接続の作成](../../sources/connectors/consent-and-preferences/onetrust.md)のドキュメントを参照してください。 |
| [!DNL Square] のサポート | [!DNL Square] ソースを使用して、支払いデータを [!DNL Square] アカウントから Platform に取り込むことができるようになりました。 |
| 顧客属性データフローの削除のサポート | 顧客属性ソースコネクタを使用して作成したデータフローを削除できるようになりました。 |

ソースについて詳しくは、[ソースの概要](../../sources/home.md)を参照してください。
