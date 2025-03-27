---
title: Adobe Experience Platform リリースノート（2025年3月）
description: Adobe Experience Platform の 2025年3月のリリースノート。
exl-id: f854f9e5-71be-4d56-a598-cfeb036716cb
source-git-commit: 445bf302baadf478a39b0c11a31ccfe25d5dd726
workflow-type: tm+mt
source-wordcount: '1185'
ht-degree: 22%

---

# Adobe Experience Platform リリースノート

**リリース日：2025年3月26日**

Adobe Experience Platformの既存の機能およびドキュメントのアップデート：

- [Adobe Experience Platform リリースノート](#adobe-experience-platform-release-notes)

   - [ダッシュボード](#dashboards)
   - [宛先](#destinations)
   - [セグメント化サービス](#segmentation-service)
   - [ソース](#sources)

## ダッシュボード {#dashboards}

Experience Platformでは、毎日のスナップショットで得られた、組織のデータに関する重要なインサイトを表示できる複数のダッシュボードを提供しています。

**新機能または更新された機能**

| 機能 | 説明 |
| ------- | ----------- |
| 指標ベースのライセンス使用状況ダッシュボード | ライセンス使用状況ダッシュボードに、合理化された UI が 2 つのタブ（**指標** および **製品** で追加されました。 新しい **指標** タブには、購入した製品全体の追跡可能なすべてのライセンス指標がまとめて表示されます。 各指標には、説明と関連製品を表示するインライン情報アイコンが含まれています。 ユーザーは、実稼動用または開発用サンドボックスを選択し、インタラクティブグラフで過去の使用トレンドを表示し、サンドボックス固有のデータを CSV ファイルとして書き出すことができます。 これらの更新により、ライセンスの追跡が合理化され、より明確なインサイトが得られます。 詳しくは、[ ライセンス使用状況ダッシュボードガイド ](../../dashboards/guides/license-usage.md) を参照してください。 |
| 更新された予測頻度 | ライセンス使用状況ダッシュボードでは、毎月ではなく **毎週** 使用状況の予測を更新することで、見込み消費に関するより正確なインサイトを提供するようになりました。 これらの予測は、最近の傾向に基づいて、今後 6 週間の予想使用量を示しています。 この変更により、意思決定の迅速化、以前の介入、ライセンス計画の改善が可能になります。 詳しくは、[ ライセンス使用状況ダッシュボードガイド ](../../dashboards/guides/license-usage.md#predicted-usage) を参照してください。 |
| UI の指標の説明を更新しました | ライセンス使用状況ダッシュボードの指標定義が改訂され、明確になり一貫性が保たれました。 「**指標**」タブの各指標の横にあるインライン情報アイコンを使用して、更新された説明をダッシュボードで直接表示できるようになりました。 これらの更新により、指標の追跡方法と、指標が適用される製品を理解しやすくなります。 詳しくは、[ ライセンス使用状況ダッシュボードガイド ](../../dashboards/guides/license-usage.md#available-metrics) を参照してください。 |

{style="table-layout:auto"}

アクセス権限の付与方法やカスタムウィジェットの作成方法など、ダッシュボードの詳細については、まず[ダッシュボードの概要](../../dashboards/home.md)を参照してください。

## 宛先 {#destinations}

[!DNL Destinations] は、Adobe Experience Platform からのデータの円滑なアクティベーションを可能にする、事前定義済みの出力先プラットフォームとの統合です。宛先を使用して、クロスチャネルマーケティングキャンペーン、メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

**新規宛先または更新された宛先** {#new-updated-destinations}

| 宛先 | 説明 |
| --- | --- |
| [ 飛行船属性 ](/help/destinations/catalog/mobile-engagement/airship-attributes.md) のアップグレード | 2025 年 3 月 25 日（PT）以降、2 つの **[!UICONTROL 飛行船属性]** カードを宛先カタログに並べて表示できます。 これは、宛先サービスの内部アップグレードが原因です。 既存の **[!UICONTROL Airship Attributes]** 宛先コネクタの名前が **[!UICONTROL （非推奨） Airship Attributes に変更され]** 新しいカードが **[!UICONTROL Airship Attributes]** として使用できるようになりました。 <br> 新しいアクティベーションデータフローには、カタログの **[!UICONTROL Airship Attributes]** 接続を使用します。 [!DNL (Deprecated) Airship Attributes] の宛先へのアクティブなデータフローがある場合、それらは自動的に更新されるので、ユーザーからのアクションは必要ありません。 <br> [Flow Service API](https://developer.adobe.com/experience-platform-apis/references/destinations/) を使用してデータフローを作成する場合は、[!DNL flow spec ID] を更新し、次の値に [!DNL connection spec ID] す必要があります。 <ul><li> フロー仕様 ID: `a862e0be-966e-4e5a-80d3-1bb566461986`</li><li> 接続仕様 ID: `594bc002-4a47-49b7-8a98-ac0d21045502`</li> </ul> |

{style="table-layout:auto"}

**新機能または更新された機能** {#destinations-new-updated-functionality}

| 機能 | 説明 |
| --- | --- |
| [ ストリーミング宛先のレポート精度の強化 ](../../dataflows/ui/monitor-destinations.md) | 2025 年 3 月より、Adobeは、ストリーミング宛先のレポート精度を高めるためのアップデートを展開しています。 この機能強化により、Experience Platformのレポートと出力先プラットフォームの間の整合性が向上します。 <br> この更新の前は、すべてのアクティベーションの再試行が **[!UICONTROL ID 失敗]** に含まれていました。 この更新後は、最後のアクティベーションの再試行のみが合計数に含まれます。 <br> この機能強化は、すべてのストリーミング宛先に適用されます。 この機能強化に <br> り、ストリーミング宛先のユーザーでは、**[!UICONTROL 失敗した ID]** 数が予期された低下する場合があります。 |
| [ エンタープライズ宛先とエッジ宛先でのマップタイプのフィールド書き出しのサポート ](/help/destinations/ui/export-arrays-maps-objects.md) | [Amazon Kinesis](/help/destinations/catalog/cloud-storage/amazon-kinesis.md)、[HTTP API](/help/destinations/catalog/streaming/http-destination.md)、{Azure Event Hubs](/help/destinations/catalog/cloud-storage/azure-event-hubs.md) および [6}Adobe Target](/help/destinations/catalog/personalization/adobe-target-connection.md) の宛先にデータを書き出す際に、アクティベーションワークフローのマッピング手順で、書き出し用のマップタイプフィールドを選択できるようになりました。<br>[ ![ マップタイプ フィールドをエンタープライズ宛先に書き出します。](../2025/assets/march/export-map.png " マップタイプフィールドをエンタープライズ宛先に書き出します。"){width="250" align="center" zoomable="yes"} |

{style="table-layout:auto"}

詳しくは、[宛先の概要](../../destinations/home.md)を参照してください。

## セグメント化サービス {#segmentation-service}

[!DNL Segmentation Service] は、顧客ベース内のマーケティング可能なユーザーグループを区別する基準を記述することで、プロファイルの特定のサブセットを定義します。セグメントは、レコードデータ（人口統計情報など）や、顧客によるブランドとのやり取りを表す時系列イベントに基づいて作成できます。

| 機能 | 説明 |
| ------- | ----------- |
| アカウントオーディエンスビルダーの機能強化 | Audience Builder 内で、属性をフィルタリングして、入力された属性のみを表示し、これらの入力された属性の概要データを表示できるようになりました。 これらの機能強化について詳しくは、[Audience Builder](../../rtcdp/segmentation/audience-builder.md) ドキュメントを参照してください。 |
| 柔軟なオーディエンス評価の一般提供 | 柔軟なオーディエンス評価が一般公開されました。 柔軟なオーディエンス評価を使用すると、時間依存の通信に対して、オンデマンドで新しいオーディエンスを作成できます。 柔軟なオーディエンス評価について詳しくは、[ 柔軟なオーディエンス評価の概要 ](../../segmentation/methods/flexible-audience-evaluation.md) を参照してください。 |

[!DNL Segmentation Service] について詳しくは、[セグメント化の概要](../../segmentation/home.md)を参照してください。

## ソース {#sources}

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

Experience Platform のソースを使用して、Adobe アプリケーションまたはサードパーティのデータソースからデータを取り込みます。

**新しいソース**

| 機能 | 説明 |
| --- | --- |
| [!DNL Bombora Intent] | [!DNL Bombora Intent] ソースを、ソースカタログで使用できるようになりました。 このソースを使用して、次の操作を行います。 <ul><li>Bombora の Company Surge Intent データを統合して、製品やサービスを積極的に調査しているアカウントを特定します。</li><li>正確なセグメントを作成し、高度にターゲットを絞った ABM キャンペーンを実行するために市場内アカウントに優先順位を付け、マーケティング活動がコンバージョンする可能性が最も高いアカウントに集中できるようにします。</li><li>インテントドリブン戦略を活用して、広告費用を最適化し、エンゲージメントを強化し、ROI を最大化します。</li></ul> 詳しくは、[ アカウントのExperience Platformへの接続 ](../../sources/tutorials/ui/create/data-partners/bombora.md) に関するガイドを参照し  [!DNL Bombora]  ください。 |
| [!DNL Demandbase Intent] | [!DNL Demandbase Intent] のデータソースを、ソースカタログから使用できるようになりました。 このソースを使用して、次の操作を行います。 <ul><li>Demandbase のアカウントインテントデータを統合し、リアルタイムエンゲージメントに基づいて高金利のアカウントを識別します。</li><li>最も強力なインテントシグナルに優先順位を付けることで、正確なセグメントを作成し、ターゲットを絞ったキャンペーンを配信して、マーケティング活動がコンバージョンする可能性が最も高いアカウントに集中できるようにします。</li><li>インテントドリブン戦略をアクティブ化して、広告費用の最適化、エンゲージメントの向上、ROI の向上を可能にします。</li></ul> 詳しくは、[ アカウントのExperience Platformへの接続 ](../../sources/tutorials/ui/create/data-partners/demandbase.md) に関するガイドを参照し  [!DNL Demandbase]  ください。 |

{style="table-layout:auto"}

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| [!DNL Google Ads] ソースの機能強化 | [[!DNL Google Ads] source](../../sources/connectors/advertising/ads.md) を使用して集計データを取り込むことができるようになりました。 [!DNL Google Ads Query Builder] を使用して、Experience Platformに取り込む属性、セグメント、リソースを指定できます。 詳しくは、[ アカウントのExperience Platformへの接続 ](../../sources/tutorials/ui/create/advertising/ads.md) に関するガイドを参照し  [!DNL Google Ads]  ください。 |
| [!DNL Microsoft Dynamics] ソースの機能強化 | データの内容や構造を調べる際に、特定の [!DNL Microsoft Dynamics] テーブルのプライマリキーを指定できるようになりました。 この機能を使用して、[!DNL Microsoft Dynamics] ソースとのクエリを最適化します。 詳しくは、[API を使用したソースとExperience Platformの接続 ](../../sources/tutorials/api/create/crm/ms-dynamics.md) に関するガイドを参照し  [!DNL Microsoft Dynamics]  ください。 |
| セルフサービスソースでの API キー認証のサポート（Batch SDK） | 新しいソースをセルフサービスソースと統合する際に、API キー認証を認証タイプとして使用できるようになりました（バッチ SDK）。 詳しくは、[ バッチ SDKでの認証仕様の設定 ](../../sources/sources-sdk/config/authspec.md) に関するガイドを参照してください。 |
| ソースの属性ベースのアクセス制御のサポート | ソースデータフローに対して属性ベースのアクセス制御機能を使用できるようになりました。 詳しくは、次のガイドを参照してください。 <ul><li>[API を使用したソースデータフローへのラベルの適用 ](../../sources/tutorials/api/labels.md)</li><li>[UI を使用したソースデータフローへのラベルの適用 ](../../sources/tutorials/ui/labels.md)。 |

{style="table-layout:auto"}

詳しくは、[ソースの概要](../../sources/home.md)を参照してください。
