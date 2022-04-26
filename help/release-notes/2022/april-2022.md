---
title: Adobe Experience Platformリリースノート 2022 年 4 月
description: Adobe Experience Platformの 2022 年 4 月のリリースノート。
source-git-commit: 820fba06d3460408640c816c80f05bff1de227cd
workflow-type: tm+mt
source-wordcount: '1999'
ht-degree: 24%

---

# Adobe Experience Platform リリースノート

**リリース日：2022 年 4 月 27 日（PT）**

Adobe Experience Platform の既存の機能に対するアップデート：

- [[!DNL Dashboards]](#dashboards)
- [データフロー](#dataflows)
- [[!DNL Data Prep]](#data-prep)
- [エクスペリエンスデータモデル（XDM）](#xdm)
- [Real-time Customer Data Platform B2B エディション](#B2B)
- [ソース](#sources)

## [!DNL Dashboards] {#dashboards}

 Platform は、毎日のスナップショットで取得した、組織のデータに関する重要な情報を表示できる複数のダッシュボードを提供しています。

ダッシュボードは、組織のデータに事前設定されたレポートオプションを提供し、Platform 内のマーケターワークフローに直接組み込まれます。 これらのダッシュボードは、IT サポートの追加や、データウェアハウスの設計と実装を追加してデータをエクスポートおよび処理する時間と労力の必要なしに利用できます。

以下のウィジェットは、それぞれのダッシュボードのウィジェットライブラリで使用できます。 詳しくは、ドキュメントを参照してください。 [ウィジェットライブラリを使用してウィジェットを追加する方法](../../dashboards/customize/widget-library.md).

| 機能 | ダッシュボード | 説明 |
| --------------------------------------------------------- | ------------- | ----------- |
| [!UICONTROL 追加されたプロファイルのトレンド] | プロファイル | このウィジェットは線グラフを使用して、過去 30 日、90 日、12 ヶ月間にプロファイルストアに毎日追加された結合プロファイルの合計数を示します。 |
| [!UICONTROL 宛先ステータスにマッピングされたオーディエンス] | プロファイル | このウィジェットは、マッピングされたオーディエンスとマッピングされていないオーディエンスの合計数を 1 つの指標で表示し、ドーナツグラフを使用して合計の比例差を示します。 |
| [!UICONTROL オーディエンスのサイズ] | プロファイル | このウィジェットには 2 列の表が表示され、最大 20 個のセグメントと、各セグメントに含まれるオーディエンスの総数が一覧表示されます。 リストは、適用される結合ポリシーに応じて異なります。また、オーディエンスの合計数に応じて、適用される結合ポリシーの順番が高い順から低い順に並べられます。 |
| [!UICONTROL プロファイル数のトレンド] | プロファイル | このウィジェットは折れ線グラフを使用して、時間の経過に伴うシステムに含まれるプロファイルの総数の傾向を示します。 データは 30 日、90 日、12 か月の期間で視覚化できます。 |
| [!UICONTROL ID 別の単一の ID プロファイル] | プロファイル | このウィジェットは棒グラフを使用して、1 つの一意の識別子のみで識別されるプロファイルの合計数を示します。 ウィジェットは、最も一般的に発生する ID のうち 5 つまでをサポートします。 |
| [!UICONTROL 宛先のステータス] | 宛先 | このウィジェットは、有効な宛先の総数を 1 つの指標として表示し、ドーナツグラフを使用して、有効な宛先と無効な宛先の比例差を示します。 |
| [!UICONTROL 宛先プラットフォーム別のアクティブな宛先] | 宛先 | このウィジェットは 2 列のテーブルを使用して、アクティブな宛先プラットフォームのリストと、各宛先プラットフォームのアクティブな宛先の合計数を表示します。 |
| [!UICONTROL すべての宛先でアクティブ化されたオーディエンス] | 宛先 | このウィジェットは、1 つの指標内のすべての宛先でアクティブ化されたオーディエンスの合計数を提供します。 |
| [!UICONTROL オーディエンスのアクティベーションの順序] | セグメント | このウィジェットには、宛先名、プラットフォーム、オーディエンスのアクティベーション日の一覧を示す 3 列のテーブルが表示されます。 |
| [!UICONTROL オーディエンスサイズのトレンド] | セグメント | このウィジェットには、30 日、90 日、12 か月の期間でセグメント定義の条件を満たすプロファイルの合計数の線グラフが表示されます。 |
| [!UICONTROL オーディエンスサイズの変更の傾向] | セグメント | このウィジェットは、最新の日別スナップショット間の特定のセグメントについて認定されたプロファイルの合計数の違いを示す線グラフを提供します。 トレンド分析の期間は、30 日、90 日、12 か月の期間で視覚化できます。 |
| [!UICONTROL ID 別のオーディエンスサイズのトレンド] | セグメント | このウィジェットは、選択した ID タイプに基づいて、特定のセグメントに対するオーディエンスサイズのトレンドを示します。 トレンド分析の期間は、30 日、90 日、12 か月の期間で視覚化できます。 |

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

## エクスペリエンスデータモデル（XDM） {#xdm}

XDM は、Adobe Experience Platformに取り込まれるデータの共通の構造と定義（スキーマ）を提供するオープンソース仕様です。 XDM 標準規格に準拠しているため、すべての顧客体験データを共通の表現に反映させて、迅速かつ統合的な方法でインサイトを提供できます。顧客行動から有益なインサイトを得たり、セグメントを通じて顧客オーディエンスを定義したり、パーソナライズ機能のために顧客属性を使用したりできます。

**新機能**

| 機能 | 説明 |
| --- | --- |
| スキーマの個々の標準フィールドを追加または削除する | スキーマエディターの UI で、標準フィールドグループの一部をスキーマに追加できるようになり、カスタムリソースを最初から作成しなくても、含めるように選択したフィールドの柔軟性が向上しました。<br><br>また、スキーマ構造内で直接アドホックカスタムフィールドを定義し、事前にフィールドグループを作成または編集しなくても、新しいカスタムフィールドグループまたは既存のカスタムフィールドグループに割り当てることができるようになりました。<br><br>詳しくは、 [UI でのスキーマの作成と編集](../../xdm/ui/resources/schemas.md) を参照してください。 |

{style=&quot;table-layout:auto&quot;}

**新しい XDM コンポーネント**

| コンポーネントの種類 | 名前 | 説明 |
| --- | --- | --- |
| グローバルスキーマ | [[!UICONTROL データ衛生操作リクエスト]](https://github.com/adobe/xdm/blob/master/schemas/hygiene/aep-hygiene-ops-record.schema.json) | 指定したデータセットまたはサンドボックス内のレコードを削除または変更するためのデータクレンジング要求の詳細をキャプチャします。 |
| 記述子 | [[!UICONTROL 時系列の精度記述子]](https://github.com/adobe/xdm/blob/master/schemas/descriptors/time-series/descriptorTimeSeriesGranularity.schema.json) | 時系列データと概要データの精度を示します。 スキーマに適用される場合、スキーマの `timestamp` フィールドは、この精度の期間での最初のタイムスタンプです。 |
| クラス | [[!UICONTROL XDM 概要指標]](https://github.com/adobe/xdm/blob/master/components/classes/summary_metrics.schema.json) | SQL SELECT と GROUP BY の結果など、グループ化ディメンションと共に事前に要約された指標を提供します。 |
| フィールドグループ | [[!UICONTROL 同意ポリシーの評価結果マップ]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/experienceevent-site-search.schema.json) | 個人の同意ポリシーの評価結果をキャプチャします。 |
| フィールドグループ | [[!UICONTROL サイト検索]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/experienceevent-site-search.schema.json) | 検索クエリ、フィルタリング、並べ替えなど、サイト検索関連の情報をキャプチャします。 |
| フィールドグループ | [[!UICONTROL リードのマージ]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/events/merge-leads.schema.json) | 2 つ以上のリードがマージされるイベントの詳細をキャプチャします。 |
| フィールドグループ | [[!UICONTROL 送信済み電子メール]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/events/emailsent.schema.json) | 電子メールが受信者に送信されるイベントの詳細をキャプチャします。 |
| フィールドグループ | [[!UICONTROL フィールドのステッチ]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/experienceevent-stitching.schema.json) | イベントの ID ステッチプロセスで計算された値をキャプチャします。 |
| フィールドグループ | [[!UICONTROL 監査用のセカンダリ受信者の詳細]](https://github.com/adobe/xdm/blob/master/extensions/adobe/experience/customerJourneyManagement/secondary-recipient-detail.schema.json) | 監査のセカンダリ受信者の詳細を取得するAdobe Journey Optimizerのフィールドグループです。 |
| フィールドグループ | [[!UICONTROL XDM ビジネスアカウント人物関係の詳細]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/account-person/account-person-details.schema.json) | アカウントと人物の関係に関連する詳細をキャプチャします。 |
| フィールドグループ | [[!UICONTROL アカウント担当者の詳細]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/account-person/account-person-details.schema.json) | アカウントと人物の関係に関連する詳細をキャプチャします。 |
| データタイプ | [[!UICONTROL 買い物かご]](https://github.com/adobe/xdm/blob/master/components/datatypes/cart.schema.json) | e コマースの買い物かごに関する情報をキャプチャします。 |
| データタイプ | [[!UICONTROL 送料]](https://github.com/adobe/xdm/blob/master/components/datatypes/shipping.schema.json) | 1 つ以上の製品の配送先情報をキャプチャします。 |
| データタイプ | [[!UICONTROL サイト検索]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/experienceevent-site-search.schema.json) | サイト検索アクティビティの情報をキャプチャします。 |
| 拡張機能 (Workfront) | [[!UICONTROL オペレーショナルタスク属性]](https://github.com/adobe/xdm/blob/master/extensions/adobe/experience/workfront/opTask.schema.json) | 操作タスクに関連する詳細をキャプチャします。 |
| 拡張機能 (Workfront) | [[!UICONTROL 作業Portfolio属性]](https://github.com/adobe/xdm/blob/master/extensions/adobe/experience/workfront/portfolio.schema.json) | 作業ポートフォリオに関連する詳細をキャプチャします。 |
| 拡張機能 (Workfront) | [[!UICONTROL 作業プログラム属性]](https://github.com/adobe/xdm/blob/master/extensions/adobe/experience/workfront/program.schema.json) | 作業プログラムに関連する詳細をキャプチャします。 |
| 拡張機能 (Workfront) | [[!UICONTROL 作業プロジェクト属性]](https://github.com/adobe/xdm/blob/master/extensions/adobe/experience/workfront/project.schema.json) | 作業プロジェクトに関連する詳細をキャプチャします。 |

{style=&quot;table-layout:auto&quot;}

**XDM コンポーネントの更新**

| コンポーネントの種類 | 名前 | 説明のアップデート |
| --- | --- | --- |
| グローバルスキーマ | [[!UICONTROL 宛先]](https://github.com/adobe/xdm/blob/master/schemas/destinations/destination.schema.json) | 次の新しい列挙値 `destinationCategory`. |
| 記述子 | [[!UICONTROL わかりやすい名前記述子]](https://github.com/adobe/xdm/blob/master/schemas/descriptors/display/alternateDisplayInfo.schema.json) | 推奨値 (`meta:enum`) は標準フィールドからは不要です。 |
| フィールドグループ | [[!UICONTROL ユーザーログインプロセス]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/experienceevent-user-login-details.schema.json) | `createProfile` フィールドが追加されました。 |
| データタイプ | [[!UICONTROL コマース]](https://github.com/adobe/xdm/blob/master/components/datatypes/marketing/commerce.schema.json) | 買い物かご関連のフィールドがいくつか追加されました。 |
| データタイプ | [[!UICONTROL 製品リスト項目]](https://github.com/adobe/xdm/blob/master/components/datatypes/productlistitem.schema.json) | 選択したオプションと割引額に対して追加される新しいフィールド。 |
| 拡張機能（インテリジェントサービス） | [[!UICONTROL Intelligent Services JourneyAI Send Time Optimization]](https://github.com/adobe/xdm/blob/master/extensions/adobe/experience/intelligentServices/profile-journeyai-sendtimeoptimization.schema.json) | 送信時スコアのストレージ形式を最適化します。 |
| 拡張機能 (Workfront) | [[!UICONTROL Workfront Change イベント]](https://github.com/adobe/xdm/blob/master/extensions/adobe/experience/workfront/changeevent.schema.json) | 複数のフィールドが `workfront:customData` カスタムフォームフィールドのフィールド。 |
| 拡張機能 (Workfront) | [[!UICONTROL 作業タスクの属性]](https://github.com/adobe/xdm/blob/master/extensions/adobe/experience/workfront/task.schema.json) | 複数のフィールドが追加されました。 |
| 拡張機能 (Workfront) | [[!UICONTROL 作業用オブジェクト]](https://github.com/adobe/xdm/blob/master/extensions/adobe/experience/workfront/workobject.schema.json) | 親オブジェクトタイプとカスタムフォームフィールド用の新しいフィールドです。 |

{style=&quot;table-layout:auto&quot;}

Platform での XDM について詳しくは、 [XDM システムの概要](../../xdm/home.md).

### Real-time Customer Data Platform B2B エディション {#B2B}

Real-time Customer Data Platform（Real-time CDP）上に構築された Real-time CDP B2B Edition は、B2B サービスを行っているマーケター向けに設計されています。複数のソースからのデータをまとめて、人物とアカウントプロファイルの単一のビューに結合します。この統合されたデータにより、マーケターは特定のオーディエンスを正確にターゲットにして、利用可能なすべてのチャネルでそれらのオーディエンスを惹き付けることができます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| のサポート `isDeleted` 機能 | すべて [!DNL Marketo] 次のデータセット `Activities` 今は支持する `isDeleted` マッピング。 新しいマッピングは、既存の B2B データフローに自動的に追加されます。 以下を使用して、 `isDeleted` 削除されたレコードを除外してデータを [!DNL Data Lake] は、ソースデータと一致します。 詳しくは、 [[!DNL Marketo] マッピングフィールドガイド](../../sources/connectors/adobe-applications/mapping/marketo.md) 詳しくは、 `isDeleted`. |

Real-time Customer Data Platform B2B Edition の詳細については、 [B2B の概要](../../rtcdp/b2b-overview.md).

## ソース {#sources}

Adobe Experience Platform では、外部ソースからデータを取り込むときに、Platform サービスを使用して、そのデータの構造化、ラベル付け、拡張をおこなうことができます。アドビのアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM システムなど、様々なソースからデータを取り込むことができます。

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理をおこなうことができます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| サポート対象 [!DNL OneTrust Integration] | これで、 [!DNL OneTrust Integration] のソースから同意および環境設定データを取り込む [!DNL OneTrust] アカウントを Platform に送信します。 詳しくは、 [作成 [!DNL OneTrust Integration] ソース接続](../../sources/connectors/consent-and-preferences/onetrust.md) を参照してください。 |
| サポート対象 [!DNL Square] | これで、 [!DNL Square] お客様からの支払いデータの取り込み元 [!DNL Square] アカウントを Platform に送信します。 |
| 顧客属性データフローの削除のサポート | 顧客属性ソースコネクタを使用して作成したデータフローを削除できるようになりました。 |

ソースについて詳しくは、 [ソースの概要](../../sources/home.md) を参照してください。
