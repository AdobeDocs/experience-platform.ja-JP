---
title: Adobe Experience Platform リリースノート
description: Adobe Experience Platform の最新のリリースノートです。
exl-id: f854f9e5-71be-4d56-a598-cfeb036716cb
source-git-commit: 0a01dd2b0d8a1039178e3593475f9a87639ccdcd
workflow-type: tm+mt
source-wordcount: '1794'
ht-degree: 100%

---

# Adobe Experience Platform リリースノート

**リリース日：2022年6月22日（PT）**

Adobe Experience Platform の既存の機能に対するアップデート：

- [[!DNL Data Science Workspace]](#dsw)
- [[!DNL Destinations]](#destinations)
- [エクスペリエンスデータモデル（XDM）](#xdm)
- [クエリサービス](#query-service)
- [Real-Time Customer Data Platform Connections](#data-collection)
- [ソース](#sources)

## [!DNL Data Science Workspace] {#dsw}

Data Science Workspace は、機械学習と人工知能を使用して、データからインサイトを引き出します。Adobe Experience Platform に統合された Data Science Workspace は、アドビソリューションでコンテンツやデータアセットを使用して予測を行うことを支援します。Data Science Workspace がこれを実現する方法の 1 つは、JupyterLab を使用することです。JupyterLab は、<a href="https://jupyter.org/" target="_blank">プロジェクト Jupyter</a> の Web ベースのユーザーインターフェイスで、Adobe Experience Platform に緊密に統合されています。これは、データ科学者が Jupyter のノートブック、コード、データを扱うための、インタラクティブな開発環境を提供します。

| 機能 | 説明 |
| --- | --- |
| JupyterLab ランチャー | JupyterLab ランチャー に Spark 3.2 ノートブックのスターターが含まれるようになりました。 Spark 2.4 ノートブックスターターは、Spark 3.2 ノートブックに置き換えられ、このリリースに含まれる予定です。 |
| Spark 3.2 | 新しい Scala（Spark）と PySpark のレシピで Spark 3.2 が使用されるようになりました |
| カーネル | Scala（Spark）ノートブックは、Scala カーネルを介して作成されるようになりました。 PySpark ノートブックは、Python カーネルを介して作成されるようになりました。Spark と PySpark カーネルは非推奨（廃止予定）で、今後のリリースで削除される予定です。 |
| レシピ | 新しい PySpark と Spark のレシピは、Python と R のレシピと同様の Docker ワークフローに従うようになりました。 |

{style=&quot;table-layout:auto&quot;}

Data Science Workspace の一般情報について詳しくは、[概要ドキュメント](../../data-science-workspace/home.md)を参照してください。

## [!DNL Destinations] {#destinations}

[!DNL Destinations] は、Adobe Experience Platform からのデータの円滑なアクティベーションを可能にする、事前定義済みの出力先プラットフォームとの統合です。宛先を使用して、クロスチャネルマーケティングキャンペーン、電子メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

**新機能または更新された機能**

| 機能 | 説明 |
| ----------- | ----------- |
| （ベータ版）[[!DNL Google Cloud Storage]](../../destinations/destination-sdk/server-and-file-configuration.md#gcs-example) ファイルベースの宛先と[設定可能なファイル名](../../destinations/destination-sdk/file-based-destination-configuration.md#file-name-configuration)の Destination SDK サポート。 | Destination SDK を使用して、Google Cloud Storage の宛先を作成し、ファイル名マクロを使用して、書き出したファイルのカスタムファイル名を定義できるようになりました。<br><br>現在、Adobe Experience Platform Destination SDK でのファイルベースの宛先のサポートはベータ版です。ドキュメントと機能は変更される場合があります。 |
| データフローのセグメント列をバッチ宛先に対して実行 | データフローをバッチ宛先に対して実行する場合、UI に各データフローの実行に関連付けられたセグメントの名前が表示されるようになりました。詳しくは、[データフローのバッチ宛先への実行](/help/dataflows/ui/monitor-destinations.md#dataflow-runs-for-batch-destinations)を参照してください。 |

{style=&quot;table-layout:auto&quot;}

**新しい宛先**

| 宛先 | 説明 |
| ----------- | ----------- |
| [（ベータ版）Google Ad Manager 360](../../destinations/catalog/advertising/google-ad-manager-360-connection.md) | [!DNL Google Ad Manager 360] 接続により、[!DNL Google Cloud Storage] <br><br> 経由で [!DNL publisher provided identifiers]（PPID）を [!DNL Google Ad Manager 360] にバッチでアップロードすることが可能になりました。この宛先は現在ベータ版で、限られた数のお客様のみが利用できます。[!DNL Google Ad Manager 360] 接続へのアクセスをリクエストするには、アドビ担当者に連絡し、[!DNL IMS Organization ID] を提供します。 |
| [[!DNL Medallia]](/help/destinations/catalog/voice/medallia-connector.md) | ターゲットを絞った Medalia の調査とフィードバックの収集のプロファイルを活用して、顧客のニーズと期待をより深く理解します。 |
| [[!DNL Adobe Advertising Cloud DSP]](../../destinations/catalog/advertising/adobe-advertising-cloud-connection.md) | Adobe Advertising Cloud [!DNL Demand-Side Platform]（DSP）の宛先を使用すると、DSP とキャンペーンのアクティベーション用に、認証済みのファーストパーティセグメントを承認済みの広告主やユーザーと共有できます。 |

{style=&quot;table-layout:auto&quot;}

宛先の一般的な情報については、[宛先の概要](../../destinations/home.md)を参照してください。

## エクスペリエンスデータモデル（XDM） {#xdm}

XDM は、Adobe Experience Platform に取り込むデータの共通構造および定義（スキーマ）を提供するオープンソース仕様です。XDM 標準規格に準拠しているため、すべての顧客体験データを共通の表現に反映させて、迅速かつ統合的な方法でインサイトを提供できます。顧客行動から有益なインサイトを得たり、セグメントを通じて顧客オーディエンスを定義したり、パーソナライゼーションのために顧客属性を使用したりできます。

**新しい XDM コンポーネント**

| コンポーネントのタイプ | 名前 | 説明 |
| --- | --- | --- |
| クラス | [[!UICONTROL 医薬品]](https://github.com/adobe/xdm/blob/master/components/classes/medication.schema.json) | 薬物治療、特に投薬に使用される薬剤の詳細を収集する医療業界クラス。 |
| クラス | [[!UICONTROL プラン]](https://github.com/adobe/xdm/blob/master/components/classes/plan.schema.json) | 健康保険や保険制度など、医療プランの詳細を収集する医療業界クラス。 |
| クラス | [[!UICONTROL プロバイダー]](https://github.com/adobe/xdm/blob/master/components/classes/provider.schema.json) | 医療機関に関する詳細を収集する医療業界クラス。 |
| クラス | [[!UICONTROL 支払者]](https://github.com/adobe/xdm/blob/master/components/classes/payer.schema.json) | 保険会社の詳細を収集する医療業界クラス。 |
| クラス | [[!UICONTROL ライブイベントスケジュール]](https://github.com/adobe/xdm/blob/master/components/classes/live-event-schedule.json) | 巡業コンサートのスケジュールやスポーツチームのスケジュールなど、ライブイベントのスケジュールに関する詳細を収集するスポーツエンターテインメント業界クラス。 |
| クラス | [[!UICONTROL ロケーション]](https://github.com/adobe/xdm/blob/master/components/classes/location.json) | コンサートホールや競技場など、ライブイベントの場所を収集するスポーツエンターテインメント業界クラス。 |
| フィールドグループ | [[!UICONTROL 医療薬]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/medication/healthcare-medication.schema.json) | ブランド名、ロット番号、数量など、投薬に関する詳細を収集する[!UICONTROL 医薬品]クラスのフィールドグループ。 |
| フィールドグループ | [[!UICONTROL 医療プランの詳細]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/plan/healthcare-plan-details.schema.json) | ネットワーク、タイプ、アクティブステータスなどの詳細を収集する[!UICONTROL プラン]クラスのフィールドグループ。 |
| フィールドグループ | [[!UICONTROL 医療機関]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/provider/healthcare-provider-details.schema.json) | 診断や治療の医療サービスを提供するための免許を持つ医療従事者個人または医療施設組織の詳細を収集する[!UICONTROL プロバイダー]クラスのフィールドグループ。 |
| フィールドグループ | [[!UICONTROL ヘルスケア会員の詳細]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/provider/healthcare-provider-details.schema.json) | [!UICONTROL XDM 個人プロファイル]クラスのフィールドグループ。連絡先情報、主治医、医療保険情報など、サービスまたはケアを受ける人の詳細を収集します。 |
| フィールドグループ | [[!UICONTROL サイトツールの詳細]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/industry-verticals/experienceevent-healthcare-sitetool.schema.json) | チャットボットやアンケートなどのサイトツールで受け取った情報を収集する、[!UICONTROL XDM ExperienceEvent] クラスのフィールドグループ。 |
| フィールドグループ | [[!UICONTROL ライブイベントチケットの購入]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/industry-verticals/experienceevent-live-event-ticket-purchase.json) | コンサートやスポーツの試合などのライブイベントのチケットの購入履歴を収集する、[!UICONTROL XDM ExperienceEvent] クラスのフィールドグループ。 |
| フィールドグループ | [[!UICONTROL スポーツとエンターテインメントイベントのスケジュール]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/live-event-schedule/sports-entertainment-event-schedule.schema.json) | アトラクション名、開場時刻など、スケジュールの詳細を収集する、[!UICONTROL ライブイベントスケジュール]クラスのフィールドグループ。 |
| フィールドグループ | [[!UICONTROL スポーツとエンターテインメントイベントの会場]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/location/sports-entertainment-event-venue.schema.json) | 座席数や指定販売地域（DMA）など、イベント会場に関する詳細を収集する[!UICONTROL 場所]クラスのフィールドグループ。 |
| グローバルスキーマ | （複数） | RTCDP インサイトの目的地指標として、新しいグローバルスキーマを使用できます。詳しくは、次の[プルリクエスト](https://github.com/adobe/xdm/pull/1560)を参照してください。 |

{style=&quot;table-layout:auto&quot;}

**更新された XDM コンポーネント**

| コンポーネントのタイプ | 名前 | 説明のアップデート |
| --- | --- | --- |
| 動作 | [[!UICONTROL 時系列スキーマ]](https://github.com/adobe/xdm/blob/master/components/behaviors/time-series.schema.json) | メディア states-update イベントタイプを追加しました。 |
| フィールドグループ | [[!UICONTROL 宿泊施設の予約]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/industry-verticals/experienceevent-lodging-reservation.schema.json) | 宿泊施設のチェックアウトプロパティを追加しました。 |
| データタイプ | [[!UICONTROL メディア情報]](https://github.com/adobe/xdm/blob/master/components/datatypes/media.schema.json) | states-start フィールドと states-end フィールドを追加しました。 |
| 拡張機能 | [[!UICONTROL Workfront 変更イベント]](https://github.com/adobe/xdm/blob/master/extensions/adobe/experience/workfront/changeevent.schema.json) | 作成イベントのユーザーと時刻を識別できるように、属性の保存に使用する 2 つのフィールドを追加しました。 |
| 拡張機能 | [[!UICONTROL Adobe CJM ExperienceEvent - メッセージインタラクションの詳細]](https://github.com/adobe/xdm/blob/master/extensions/adobe/experience/customerJourneyManagement/message-interaction.schema.json) | ランディングページオブジェクトに、購読、同意、カスタムメール、追加データ情報を追加しました。 |

{style=&quot;table-layout:auto&quot;}

Platform の XDM について詳しくは、[XDM システムの概要](../../xdm/home.md)を参照してください。

## クエリサービス {#query-service}

クエリサービスを使用すると、標準 SQL を使用して Adobe Experience Platform [!DNL Data Lake] でデータに対してクエリを実行できます。任意のデータセットを [!DNL Data Lake] から結合し、クエリの結果を新しいデータセットとして取得することで、レポートや Data Science Workspace で使用したり、リアルタイム顧客プロファイルに取り込んだりできます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| アドホックスキーマのラベル付け | クエリサービス CTAS クエリを通じて、自動的に生成されるアドホックスキーマのデータフィールドにラベルを適用し、機密データへのアクセスを管理します。アドホックスキーマの特定のフィールドもしくはデータセットの使用を制限して、機密性の高い個人データと個人を特定できる情報の両方へのアクセスを制御できます。属性ベースのアクセス制御機能を使用すると、Platform UI を通じてアドホックスキーマフィールドにラベルを付けることができます。 |
| `FLATTEN` 設定 | サードパーティの BI ツールを使用してデータベースに接続する場合、`FLATTEN` を設定すると、ネストされたデータ構造が別々の列に統合され、属性名が行値を保持する列名になります。これにより、アドホックスキーマの使いやすさが向上し、ネストされたデータ構造をサポートしない BI ツールでデータを取得、分析、変換およびレポートするために必要なワークロードが削減されます。 |

{style=&quot;table-layout:auto&quot;}

クエリサービスについて詳しくは、[クエリサービスの概要](../../query-service/home.md)を参照してください。

## Real-time Customer Data Platform Connections {#data-collection}

Real-Time Customer Data Platform Connections は、クライアントサイドのカスタマーエクスペリエンスに関するデータを収集して Adobe Experience Platform Edge Network に送信し、データを強化したり、変換したり、アドビまたはアドビ以外の宛先に配信したりできるようにする一連のテクノロジーを提供します。

**新機能**

| 機能 | 説明 |
| --- | --- |
| [データストリームのアクセスタイプの設定](../../edge/datastreams/overview.md#create) | 新しいデータストリームを作成する際に、Edge ネットワークで受け入れるリクエストのタイプを選択できるようになりました。 <ul><li>**[!UICONTROL 混合認証]**：このオプションを選択すると、Edge Network は認証済みリクエストと未認証リクエストの両方を受け入れます。[Server API](../../server-api/overview.md) と一緒に Web SDK または [Mobile SDK](https://aep-sdks.gitbook.io/docs/) を使用する場合は、このオプションを選択してください。 </li><li>**[!UICONTROL 認証済みのみ]**：このオプションを選択すると、Edge Network は認証済みのリクエストのみを受け入れます。Server API のみを使用する予定で、未認証のリクエストが [!DNL Edge Network] で処理されないようにする場合は、このオプションを選択します。 </li></ul> |
| 単一ページアプリケーションで、指標を増分することなく、[提案をレンダリング](../../edge/personalization/rendering-personalization-content.md#applypropositions)します。 | 新しく追加された `applyPropositions` コマンドを使用すると、[!DNL Analytics] および [!DNL Target] 指標を増分せずに、[!DNL Target] から単一ページアプリケーションに提案の配列をレンダリングまたは実行できます。これにより、レポートの精度が向上します。 |
| [モバイルから web およびクロスドメイン での ID の共有](../../edge/identity/id-sharing.md) | Adobe Experience Platform Web SDK で訪問者 ID 共有機能がサポートされ、モバイルアプリとモバイル web コンテンツの間、およびドメイン間で、より正確にパーソナライズされたエクスペリエンスを配信できるようになりました。 |

詳しくは、[Real-Time CDP Connections の概要](../../rtcdp-connections/home.md) を参照してください。

## ソース {#sources}

Adobe Experience Platform では、外部ソースからデータを取り込むときに、Platform サービスを使用して、そのデータの構造化、ラベル付け、拡張を行うことができます。アドビのアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM システムなど、様々なソースからデータを取り込むことができます。

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

| 機能 | 説明 |
| --- | --- |
| [!DNL Mixpanel] ソースのベータ版リリース | [!DNL Mixpanel] ソースを使用して、分析データを [!DNL Mixpanel] アカウントから Experience Platform に取り込むことができるようになりました。詳しくは、[[!DNL Mixpanel] ソースドキュメント](../../sources/connectors/analytics/mixpanel.md)を参照してください。 |

{style=&quot;table-layout:auto&quot;}

ソースについて詳しくは、[ソースの概要](../../sources/home.md)を参照してください。
