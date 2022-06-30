---
title: Adobe Experience Platform リリースノート
description: Adobe Experience Platform の最新のリリースノートです。
exl-id: f854f9e5-71be-4d56-a598-cfeb036716cb
source-git-commit: 0a01dd2b0d8a1039178e3593475f9a87639ccdcd
workflow-type: tm+mt
source-wordcount: '1794'
ht-degree: 33%

---

# Adobe Experience Platform リリースノート

**リリース日：2022 年 6 月 22 日**

Adobe Experience Platform の既存の機能に対するアップデート：

- [[!DNL Data Science Workspace]](#dsw)
- [[!DNL Destinations]](#destinations)
- [エクスペリエンスデータモデル（XDM）](#xdm)
- [クエリサービス](#query-service)
- [Real-time Customer Data Platform Connections](#data-collection)
- [ソース](#sources)

## [!DNL Data Science Workspace] {#dsw}

 Data Science Workspace は、機械学習と人工知能を使用して、データからインサイトを引き出します。Adobe Experience Platform に統合された Data Science Workspace は、アドビソリューションでコンテンツやデータアセットを使用して予測をおこなうことを支援します。Data Science Workspace がこれを実現する方法の 1 つは、JupyterLab を使用することです。 JupyterLab は、<a href="https://jupyter.org/" target="_blank">プロジェクト Jupyter</a> の Web ベースのユーザーインターフェイスで、Adobe Experience Platform に緊密に統合されています。これは、データ科学者が Jupyter のノートブック、コード、データを扱うための、インタラクティブな開発環境を提供します。

| 機能 | 説明 |
| --- | --- |
| JupyterLab ランチャー | JupyterLab Launcher に Spark 3.2 ノートブックのスターターが含まれるようになりました。 Spark 2.4 ノートブックスタートは、Spark 3.2 ノートブックに置き換えられ、このリリースに含まれる予定です。 |
| Spark 3.2 | 新しい Scala(Spark) と PySpark のレシピで Spark 3.2 が使用されるようになりました。 |
| カーネル | Scala(Spark) ノートブックは、Scala カーネルを介して作成されるようになりました。 PySpark ノートブックは、Python カーネルを介して作成されるようになりました。 Spark と PySpark カーネルは廃止され、今後のリリースで削除される予定です。 |
| レシピ | 新しい PySpark と Spark のレシピは、Python と R のレシピと同様の Docker ワークフローに従うようになりました。 |

{style=&quot;table-layout:auto&quot;}

Data Science Workspace の一般情報について詳しくは、 [概要ドキュメント](../../data-science-workspace/home.md).

## [!DNL Destinations] {#destinations}

[!DNL Destinations] は、Adobe Experience Platform からのデータの円滑なアクティベーションを可能にする、事前定義済みの出力先プラットフォームとの統合です。宛先を使用して、クロスチャネルマーケティングキャンペーン、電子メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

**新機能または更新された機能**

| 機能 | 説明 |
| ----------- | ----------- |
| （ベータ版）のDestination SDKサポート [[!DNL Google Cloud Storage]](../../destinations/destination-sdk/server-and-file-configuration.md#gcs-example) ファイルベースの宛先と [設定可能なファイル名](../../destinations/destination-sdk/file-based-destination-configuration.md#file-name-configuration). | Destination SDKを使用して、Google Cloud Storage の宛先を作成し、ファイル名マクロを使用して、書き出したファイルのカスタムファイル名を定義できるようになりました。 <br><br>現在、Adobe Experience Platform Destination SDK でのファイルベースの宛先のサポートはベータ版です。ドキュメントと機能は変更される場合があります。 |
| データフローのセグメント列をバッチ保存先に対して実行 | データフローをバッチ宛先に対して実行する場合、UI に各データフローの実行に関連付けられたセグメントの名前が表示されるようになりました。 詳細を表示 [データフローは、バッチ宛先に対して実行されます](/help/dataflows/ui/monitor-destinations.md#dataflow-runs-for-batch-destinations). |

{style=&quot;table-layout:auto&quot;}

**新しい宛先**

| 宛先 | 説明 |
| ----------- | ----------- |
| [（ベータ版）Google Ad Manager 360](../../destinations/catalog/advertising/google-ad-manager-360-connection.md) | この [!DNL Google Ad Manager 360] 接続では、バッチアップロードが有効になります [!DNL publisher provided identifiers] (PPID) を次に移動： [!DNL Google Ad Manager 360]，経由 [!DNL Google Cloud Storage] <br><br>この宛先は現在ベータ版で、限られた数のお客様のみが利用できます。 へのアクセスをリクエストするには、以下を実行します。 [!DNL Google Ad Manager 360] 接続する場合は、Adobe担当者に連絡し、 [!DNL IMS Organization ID]. |
| [[!DNL Medallia]](/help/destinations/catalog/voice/medallia-connector.md) | ターゲットを絞った Medalia の調査とフィードバックの収集のプロファイルをアクティブ化して、顧客のニーズと期待をより深く理解します。 |
| [[!DNL Adobe Advertising Cloud DSP]](../../destinations/catalog/advertising/adobe-advertising-cloud-connection.md) | ザAdobe Advertising Cloud [!DNL Demand-Side Platform] (DSP) の宛先を使用すると、認証済みのファーストパーティセグメントを承認済みの広告主やユーザーと共有し、DSPとキャンペーンのアクティベーション用に共有できます。 |

{style=&quot;table-layout:auto&quot;}

宛先の一般的な情報については、[宛先の概要](../../destinations/home.md)を参照してください。

## エクスペリエンスデータモデル（XDM） {#xdm}

XDM は、Adobe Experience Platform に取り込むデータの共通構造および定義（スキーマ）を提供するオープンソース仕様です。XDM 標準規格に準拠しているため、すべての顧客体験データを共通の表現に反映させて、迅速かつ統合的な方法でインサイトを提供できます。顧客行動から有益なインサイトを得たり、セグメントを通じて顧客オーディエンスを定義したり、パーソナライゼーションのために顧客属性を使用したりできます。

**新しい XDM コンポーネント**

| コンポーネントのタイプ | 名前 | 説明 |
| --- | --- | --- |
| クラス | [[!UICONTROL 薬]](https://github.com/adobe/xdm/blob/master/components/classes/medication.schema.json) | 医療用治療、特に薬剤や薬剤に使用される物質の詳細をキャプチャする医療業界クラス。 |
| クラス | [[!UICONTROL プラン]](https://github.com/adobe/xdm/blob/master/components/classes/plan.schema.json) | 医療プランや保険プランなど、医療プランの詳細を取り込む医療業界クラス。 |
| クラス | [[!UICONTROL プロバイダー]](https://github.com/adobe/xdm/blob/master/components/classes/provider.schema.json) | 医療機関に関する詳細を収集する医療業界クラス。 |
| クラス | [[!UICONTROL 支払者]](https://github.com/adobe/xdm/blob/master/components/classes/payer.schema.json) | 保険会社の詳細をキャプチャする医療業界クラス。 |
| クラス | [[!UICONTROL ライブイベントスケジュール]](https://github.com/adobe/xdm/blob/master/components/classes/live-event-schedule.json) | 旅行コンサートのスケジュールやスポーツチームのスケジュールなど、ライブイベントのスケジュールに関する詳細をキャプチャするスポーツ&amp;エンターテイメント業界クラス。 |
| クラス | [[!UICONTROL ロケーション ]](https://github.com/adobe/xdm/blob/master/components/classes/location.json) | コンサートホールやスポーツアリーナなど、ライブイベントの場所をキャプチャするスポーツ&amp;エンターテイメント業界クラス。 |
| フィールドグループ | [[!UICONTROL 医療薬]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/medication/healthcare-medication.schema.json) | のフィールドグループ [!UICONTROL 薬] ブランド名、ロット番号、数量など、薬物に関する詳細をキャプチャするクラス。 |
| フィールドグループ | [[!UICONTROL 医療プランの詳細]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/plan/healthcare-plan-details.schema.json) | のフィールドグループ [!UICONTROL プラン] ネットワーク、タイプ、アクティブステータスなどの詳細を取得するクラス。 |
| フィールドグループ | [[!UICONTROL 医療機関]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/provider/healthcare-provider-details.schema.json) | のフィールドグループ [!UICONTROL プロバイダー] 医療の診断と治療のサービスを提供するための免許を受けた個人の医療専門家または医療施設組織の詳細をキャプチャするクラス。 |
| フィールドグループ | [[!UICONTROL 医療メンバーの詳細]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/provider/healthcare-provider-details.schema.json) | のフィールドグループ [!UICONTROL XDM 個人プロファイル] 連絡先情報、プライマリケア医師、プラン情報など、サービスまたはケアを受ける人の詳細をキャプチャするクラス。 |
| フィールドグループ | [[!UICONTROL サイトツールの詳細]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/industry-verticals/experienceevent-healthcare-sitetool.schema.json) | のフィールドグループ [!UICONTROL XDM ExperienceEvent] chatbot、survey などのサイトツールで収集された情報をキャプチャするクラス。 |
| フィールドグループ | [[!UICONTROL ライブイベントチケットの購入]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/industry-verticals/experienceevent-live-event-ticket-purchase.json) | のフィールドグループ [!UICONTROL XDM ExperienceEvent] ライブイベントのチケットの購入履歴をキャプチャするクラス（コンサートやスポーツゲームなど）。 |
| フィールドグループ | [[!UICONTROL スポーツ&amp;エンターテイメントイベントのスケジュール]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/live-event-schedule/sports-entertainment-event-schedule.schema.json) | のフィールドグループ [!UICONTROL ライブイベントスケジュール] 引き付け名、ドア開け時間など、スケジュールの詳細をキャプチャするクラス。 |
| フィールドグループ | [[!UICONTROL スポーツエンターテイメントイベント会場]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/location/sports-entertainment-event-venue.schema.json) | のフィールドグループ [!UICONTROL 場所] 座席数や指定販売地域 (DMA) など、イベント会場に関する詳細をキャプチャするクラス。 |
| グローバルスキーマ | （複数） | RTCDP インサイトの宛先指標に対して、新しいグローバルスキーマを使用できます。 次を参照してください。 [プルリクエスト](https://github.com/adobe/xdm/pull/1560) を参照してください。 |

{style=&quot;table-layout:auto&quot;}

**更新された XDM コンポーネント**

| コンポーネントのタイプ | 名前 | 説明のアップデート |
| --- | --- | --- |
| 動作 | [[!UICONTROL 時系列スキーマ]](https://github.com/adobe/xdm/blob/master/components/behaviors/time-series.schema.json) | メディア states-update イベントタイプが追加されました。 |
| フィールドグループ | [[!UICONTROL 宿泊施設の予約]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/industry-verticals/experienceevent-lodging-reservation.schema.json) | 宿泊施設のチェックアウトプロパティを追加しました。 |
| データタイプ | [[!UICONTROL メディア情報]](https://github.com/adobe/xdm/blob/master/components/datatypes/media.schema.json) | states-start フィールドと states-end フィールドを追加しました。 |
| 拡張子 | [[!UICONTROL Workfront 変更イベント]](https://github.com/adobe/xdm/blob/master/extensions/adobe/experience/workfront/changeevent.schema.json) | 作成イベントのユーザーと時刻を識別するのに役立つ、属性の保存に使用する 2 つのフィールドを追加しました。 |
| 拡張子 | [[!UICONTROL AdobeCJM ExperienceEvent — メッセージインタラクションの詳細]](https://github.com/adobe/xdm/blob/master/extensions/adobe/experience/customerJourneyManagement/message-interaction.schema.json) | ランディングページオブジェクトに、購読、同意、カスタム E メールおよび追加のデータ情報を追加しました。 |

{style=&quot;table-layout:auto&quot;}

Platform の XDM について詳しくは、[XDM システムの概要](../../xdm/home.md)を参照してください。

## クエリサービス {#query-service}

クエリサービスを使用すると、標準 SQL を使用して Adobe Experience Platform [!DNL Data Lake] でデータに対してクエリを実行できます。任意のデータセットを [!DNL Data Lake] から結合し、クエリの結果を新しいデータセットとして取得することで、レポートや Data Science Workspace で使用したり、リアルタイム顧客プロファイルに取り込んだりできます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| アドホックスキーマのラベル付け | クエリサービス CTAS クエリを通じて自動的に生成されるアドホックスキーマのデータフィールドにラベルを適用して、機密データへのアクセスを管理します。 アドホックスキーマの特定のフィールド（データセット）の使用を制限して、機密性の高い個人データと個人を特定できる情報の両方へのアクセスを制御できます。 属性ベースのアクセス制御機能を使用すると、Platform UI を通じてアドホックスキーマフィールドにラベルを付けることができます。 |
| `FLATTEN` 設定 | サードパーティの BI ツールを使用してデータベースに接続する場合、 `FLATTEN` を設定すると、ネストされたデータ構造が別々の列に統合され、属性名が行値を保持する列名になります。 これにより、アドホックスキーマの使いやすさが向上し、ネストされたデータ構造をサポートしない BI ツールでデータを取得、分析、変換およびレポートするために必要なワークロードが削減されます。 |

{style=&quot;table-layout:auto&quot;}

クエリサービスの詳細については、 [クエリサービスの概要](../../query-service/home.md).

## Real-time Customer Data Platform Connections {#data-collection}

Real-time Customer Data Platform Connections は、クライアント側の顧客体験データを収集し、Adobe Experience Platform Edge Network に送信して、AdobeやAdobe以外の宛先にエンリッチメント、変換、配布できるテクノロジーのスイートを提供します。

**新機能**

| 機能 | 説明 |
| --- | --- |
| [データストリームのアクセスタイプの設定](../../edge/datastreams/overview.md#create) | 新しいデータストリームを作成する際に、Edge ネットワークで受け入れるリクエストの種類を選択できるようになりました。 <ul><li>**[!UICONTROL 混合認証]**：このオプションを選択すると、Edge Network は認証済みリクエストと未認証リクエストの両方を受け入れます。[Server API](../../server-api/overview.md) と一緒に Web SDK または [Mobile SDK](https://aep-sdks.gitbook.io/docs/) を使用する場合は、このオプションを選択してください。 </li><li>**[!UICONTROL 認証済みのみ]**：このオプションを選択すると、Edge Network は認証済みのリクエストのみを受け入れます。Server API のみを使用する予定で、未認証のリクエストが [!DNL Edge Network] で処理されないようにする場合は、このオプションを選択します。 </li></ul> |
| [提案をレンダリング](../../edge/personalization/rendering-personalization-content.md#applypropositions) 指標を増分せずに単一ページアプリケーションで使用する場合。 | 新しく追加された `applyPropositions` コマンドを使用すると、次の提案の配列をレンダリングまたは実行できます： [!DNL Target] を単一ページのアプリケーションに追加するときに、 [!DNL Analytics] および [!DNL Target] 指標。 これにより、レポートの精度が向上します。 |
| [モバイルから Web への ID とクロスドメイン ID の共有](../../edge/identity/id-sharing.md) | Adobe Experience Platform Web SDK で訪問者 ID 共有機能がサポートされ、モバイルアプリとモバイル Web コンテンツの間、およびドメイン間で、より正確にパーソナライズされたエクスペリエンスを配信できるようになりました。 |

詳しくは、 [Real-Time CDP接続の概要](../../rtcdp-connections/home.md).

## ソース {#sources}

Adobe Experience Platform では、外部ソースからデータを取り込むときに、Platform サービスを使用して、そのデータの構造化、ラベル付け、拡張を行うことができます。アドビのアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM システムなど、様々なソースからデータを取り込むことができます。

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

| 機能 | 説明 |
| --- | --- |
| [!DNL Mixpanel] ソースのベータ版リリース | これで、 [!DNL Mixpanel] 分析データを取り込むソース [!DNL Mixpanel] アカウントからExperience Platformへ。 詳しくは、 [[!DNL Mixpanel] ソースドキュメント](../../sources/connectors/analytics/mixpanel.md) を参照してください。 |

{style=&quot;table-layout:auto&quot;}

ソースについて詳しくは、[ソースの概要](../../sources/home.md)を参照してください。
