---
title: Adobe Experience Platform リリースノート 2024年10月
description: Adobe Experience Platform の 2024年10月のリリースノート。
source-git-commit: a381bdc45ee9c3c7ffb32bb7a7ec43a1233d1556
workflow-type: tm+mt
source-wordcount: '861'
ht-degree: 31%

---

# Adobe Experience Platform リリースノート

**リリース日：2024年10月29日（PT）**

Adobe Experience Platformの既存の機能およびドキュメントのアップデート：

- [データ収集](#data-collection)
- [宛先](#destinations)
- [セグメント化サービス](#segmentation-service)
- [サンドボックス](#sandboxes)
- [ソース](#sources)

<!-- ## Dashboards {#dashboards}

Experience Platform provides multiple dashboards through which you can view important insights about your organization's data, as captured during daily snapshots.

**New or updated features**

| Feature | Description |
| --- | --- |
| Data Distiller Templates | Explore multiple templates to gain structured insights into audience data. Use dashboards like **Advanced [!UICONTROL Audience Overlaps]**, **[!UICONTROL Audience Comparison]**, **[!UICONTROL Audience Trends]**, and **[!UICONTROL Audience Identity Overlaps]** to make data-driven decisions, optimize segmentation, and enhance engagement strategies. See the [Data Distiller Templates guide](../../dashboards/sql-insights-query-pro-mode/templates/overview.md) for more details. |
| Advanced Audience Overlaps | Quickly analyze audience intersections for specific audiences or view all overlaps to uncover valuable insights across your entire audience set. Use these insights to refine segmentation, reduce redundant messaging, and create more targeted campaigns for improved marketing efficiency. See the [Advanced Audience Overlaps guide](../../dashboards/sql-insights-query-pro-mode/templates/overlaps.md) for more details. |
| Audience Comparison enhancements | View a side-by-side comparison of key metrics between different audience groups using the **Audience Comparison** dashboard. With this dashboard you can select specific time frames and KPIs, such as audience size and identity composition, to make more informed decisions about audience segmentation and targeting strategies. Read the [Audience Comparison guide](../../dashboards/sql-insights-query-pro-mode/templates/comparison.md) for more information. |
| Audience Trends Visualization | Analyze audience metrics over time with the **[!UICONTROL Audience Trends]** dashboard. Visualize trends for audience size, number of identities, and number of single identity profiles to help you monitor audience evolution, measure growth, and refine your engagement strategies. See the [Audience Trends guide](../../dashboards/sql-insights-query-pro-mode/templates/trends.md) for more details. |
| Identity Overlaps Analysis | Analyze identity overlaps in selected audiences with the **[!UICONTROL Audience Identity Overlaps]** dashboard. View identity trends and breakdowns to understand how different identity types relate within your audience, enhancing identity stitching and improving customer segmentation accuracy. Refer to the [Audience Identity Overlaps guide](../../dashboards/sql-insights-query-pro-mode/templates/identity-overlaps.md) for more details. |

{style="table-layout:auto"}

For more information on dashboards, including how to grant access permissions and create custom widgets, begin by reading the [dashboards overview](../../dashboards/home.md). -->

## データ収集 {#collection}

Adobe Experience Platformは、クライアントサイドのカスタマーエクスペリエンスデータを収集し、Experience PlatformEdge Networkに送信します。そこでデータを強化、変換、AdobeまたはAdobe以外の宛先への配信を可能にする一連のテクノロジーを提供します。

**新機能**

| タイプ | 機能 | 説明 |
| --- | --- | --- |
| タグと拡張機能 | Adobe Analyticsの JSON ビュー | Adobe Analytics タグ拡張機能を使用して、eVar、prop およびイベント設定を JSON として調べることができるようになりました。これらの情報を Web SDK 拡張機能に含め、編集用に書き出すことができるようになりました。 このデータをアップロードまたはコピーして、デバイスに保存することもできます。 詳しくは、[Adobe Analytics拡張機能のドキュメント ](../../tags/extensions/client/analytics/overview.md) を参照してください。 |

{style="table-layout:auto"}

詳しくは、[ データ収集の概要 ](../../collection/home.md) を参照してください。

## 宛先 {#destinations}

[!DNL Destinations] は、Adobe Experience Platform からのデータの円滑なアクティベーションを可能にする、事前定義済みの出力先プラットフォームとの統合です。宛先を使用して、クロスチャネルマーケティングキャンペーン、メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

**新機能または更新された機能** {#destinations-new-updated-functionality}

| 機能 | 説明 |
| ----------- | ----------- |
| [ アレイのエクスポートのサポートが一般提供されました ](../../destinations/ui/export-arrays-calculated-fields.md) | すべてのお客様は、オーディエンスをアクティブ化する **[!UICONTROL ファイルベースの宛先に対して]***計算フィールドを追加* オプションを使用して、配列全体または配列の要素を書き出すことができるようになりました。 なお、配列をターゲットファイルの文字列にフラット化するには、`array_to_string` 関数を使用する必要があります。<br> ![ 関数とフィールドを使用して計算フィールドの選択を追加します。](../2024/assets/october/array-export.gif "array_to_string 関数と組織の配列を選択した計算フィールドを追加します。"){width="250" align="center" zoomable="yes"} |
| [ ストリーミング宛先のレポート精度の強化 ](/help/destinations/ui/export-datasets.md) | 2024 年 10 月より、Adobeは、ストリーミング宛先のレポート精度を高めるためのアップデートをロールアウトしています。 この機能強化により、宛先とExperience Platformプラットフォームレポートの間の整合性が向上します。 <br> この更新の前は、すべてのアクティベーションの再試行が **[!UICONTROL ID 失敗]** に含まれていました。 この更新後は、最後のアクティベーションの再試行のみが合計数に含まれます。 <br> この強化機能は、現在、[Google カスタマーマッチの宛先に適用されますが ](../../destinations/catalog/advertising/google-customer-match.md) 他のExperience Platformストリーミングの宛先に徐々に展開される予定です。 この機能強化に伴い、[Google カスタマーマッチの宛先 ](../../destinations/catalog/advertising/google-customer-match.md) のユーザーでは、**[!UICONTROL ID 失敗]** カウントが低下する可能性があります。 |
| [ バッチオーディエンスアクティベーション ](../../destinations/ui/activate-batch-profile-destinations.md#export-full-files) に対する柔軟なオーディエンス評価の影響 | セグメント評価後にアクティベートするように既に設定されているオーディエンスに対して [ 柔軟なオーディエンス評価 ](../../segmentation/ui/audience-portal.md#flexible-audience-evaluation) を実行した場合、オーディエンスは、以前の日別アクティベーションジョブに関係なく、柔軟なオーディエンス評価ジョブが終了するとすぐにアクティベートされます。 <br> これにより、オーディエンスがアクションに基づいて 1 日に複数回エクスポートされる可能性があります。 |

{style="table-layout:auto"}

詳しくは、[宛先の概要](../../destinations/home.md)を参照してください。

## セグメント化サービス {#segmentation-service}

[!DNL Segmentation Service] は、顧客ベース内のマーケティング可能なユーザーグループを区別する基準を記述することで、プロファイルの特定のサブセットを定義します。セグメントは、レコードデータ（人口統計情報など）や、顧客によるブランドとのやり取りを表す時系列イベントに基づいて作成できます。

**新機能または更新された機能**

| 機能 | 説明 |
| ------- | ----------- |
| [!BADGE  限定提供 ]{type=Informative} 柔軟なオーディエンス評価 | 柔軟なオーディエンス評価を使用すると、時間依存の通信に対して、オンデマンドで新しいオーディエンスをすばやく作成できます。 この新機能について詳しくは、[Audience Portal ドキュメント ](../../segmentation/ui/audience-portal.md#flexible-audience-evaluation) を参照してください。 |

{style="table-layout:auto"}

[!DNL Segmentation Service] について詳しくは、[ セグメント化の概要 ](../../segmentation/home.md) を参照してください。

## サンドボックス {#sandboxes}

Adobe Experience Platform は、デジタルエクスペリエンスアプリケーションをグローバルな規模で強化するように設計されています。企業ではしばしば複数のデジタルエクスペリエンスアプリケーションを並行して運用し、運用コンプライアンスを確保しながら、アプリケーションの開発、テスト、導入に注力する必要があります。このニーズに対応するために、Experience Platformでは、1 つの Platform インスタンスを別々の仮想環境に分割するサンドボックスを提供し、デジタルエクスペリエンスアプリケーションの開発と発展を支援しています。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| サンドボックスツールパッケージの共有 | サンドボックスツールを使用して、様々な組織のサンドボックス間でサンドボックス設定を簡単に書き出しと読み込みできるようになりました。 これで、2 つのカテゴリの共有パッケージが使用可能になりました。<br><ul><li>**[プライベートパッケージ ](../../sandboxes/ui/sharing-packages-across-orgs.md#private-packages):** ソース組織からの共有リクエストを承認した組織で、プライベートパッケージの共有を使用します。</li><li>**[公開パッケージ ](../../sandboxes/ui/sharing-packages-across-orgs.md#public-packages):** 公開パッケージは、追加の承認を必要とせずに共有でき、パッケージのペイロードを使用して簡単に読み込まれます。</li></ul><br> これらの機能について詳しくは、[ 組織間でのパッケージの共有 ](../../sandboxes/ui/sharing-packages-across-orgs.md) に関するガイドを参照してください。 |
| サンドボックスツール API での [ パッケージ共有 ](https://experienceleague.adobe.com/en/docs/experience-platform/sandbox/sandbox-tooling-api/packages#org-linking) | サンドボックスツール API を使用して、`/handshake` と `/transfer` の 2 つの新しいエンドポイントにリクエストを送信し、組織間での共有、パッケージ共有リクエストの取得と作成を行います。 パッケージのペイロードを取得するための追加リクエストが `/packages` エンドポイントに追加されました。 |

{style="table-layout:auto"}

サンドボックスについて詳しくは、[ サンドボックスの概要 ](../../sandboxes/home.md) を参照してください。

## ソース {#sources}

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

Experience Platform のソースを使用して、Adobe アプリケーションまたはサードパーティのデータソースからデータを取り込みます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| [!DNL Marketo Engage] での標準アクティビティエンティティのフィルタリングのサポート | [!DNL Flow Service] API を使用すると、[!DNL Marketo Engage] ソースからデータを取り込む際に、標準のアクティビティエンティティをフィルタリングできます。 詳しくは、[ フィルタリング  [!DNL Marketo]  標準アクティビティデータ ](../../sources/tutorials/api/filter.md#filter-activity-entities-for-marketo-engage) に関するガイドを参照してください。 |

{style="table-layout:auto"}

詳しくは、[ソースの概要](../../sources/home.md)を参照してください。
