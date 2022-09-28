---
title: Adobe Experience Platformリリースノート 2022 年 9 月
description: Adobe Experience Platformの 2022 年 9 月のリリースノート。
source-git-commit: a3f12b9524d393441923cd11e09ed3e406814691
workflow-type: tm+mt
source-wordcount: '1377'
ht-degree: 34%

---

# Adobe Experience Platform リリースノート

**リリース日：2022 年 9 月 28 日（PT）**

Adobe Experience Platform の既存の機能に対するアップデート：

- [エクスペリエンスデータモデル（XDM）](#xdm)
- [ID サービス](#identity-service)
- [[!DNL Artificial Intelligence and Machine Learning Services]](#ai-and-ml-services)
- [ソース](#sources)

## エクスペリエンスデータモデル（XDM） {#xdm}

XDM は、Adobe Experience Platform に取り込むデータの共通構造および定義（スキーマ）を提供するオープンソース仕様です。XDM 標準規格に準拠しているため、すべての顧客体験データを共通の表現に反映させて、迅速かつ統合的な方法でインサイトを提供できます。顧客行動から有益なインサイトを得たり、セグメントを通じて顧客オーディエンスを定義したり、パーソナライゼーションのために顧客属性を使用したりできます。

**新機能**

| 機能 | 説明 |
| --- | --- |
| 列挙と推奨値の UI のサポート | データの検証を有効にする列挙に加えて、次の操作を実行できます。 [推奨値を追加または削除](../../xdm/ui/fields/enum.md) 標準またはカスタム文字列フィールドの場合は、Platform ユーザーがセグメントの作成時に、わかりやすい値のリストから選択できるようにします。 |

**新しい XDM コンポーネント**

| コンポーネントのタイプ | 名前 | 説明 |
| --- | --- | --- |
| フィールドグループ | [[!UICONTROL AJO 分類フィールド]](https://github.com/adobe/xdm/blob/master/extensions/adobe/experience/decisioning/proposition-action.schema.json) | 提案イベントがトリガーされる原因となった、操作された特定の要素のプロパティ。 |
| フィールドグループ | [[!UICONTROL MediaAnalytics インタラクションの詳細]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/experienceevent-media-analytics.schema.json) | 時間の経過と共にメディアの操作を追跡します。 |
| フィールドグループ | [[!UICONTROL メディアの詳細情報]](https://github.com/adobe/xdm/blob/master/components/datatypes/mediadetails.schema.json) | メディアの詳細情報をトラッキングします。 |
| フィールドグループ | [[!UICONTROL AdobeCJM ExperienceEvent — サーフェス]](https://github.com/adobe/xdm/blob/master/extensions/adobe/experience/customerJourneyManagement/surfaces.schema.json) | Adobe Journey Optimizerのエクスペリエンスイベントの表面を表します。 |

{style=&quot;table-layout:auto&quot;}

**更新された XDM コンポーネント**

| コンポーネントのタイプ | 名前 | 説明 |
| --- | --- | --- |
| 動作 | [[!UICONTROL 時系列]](https://github.com/adobe/xdm/blob/master/components/behaviors/time-series.schema.json) | <ul><li>次の値を追加しました： `eventType`:<ul><li>`decisioning.propositionSend`</li><li>`decisioning.propositionDismiss`</li><li>`decisioning.propositionTrigger`</li><li>`media.downloaded`</li><li>`location.entry`</li><li>`location.exit`</li></ul></li><li>次の値を削除しました： `eventType`:<ul><li>`decisioning.propositionDeliver`</li><li>`media.stateStart`</li><li>`media.stateEnd`</li></ul></li></ul> |
| フィールドグループ | （複数） | [いくつかのフィールドの説明を更新しました](https://github.com/adobe/xdm/pull/1628/files) 複数のJourney Orchestrationコンポーネント |
| フィールドグループ | （複数） | [いくつかのAdobe Workfrontコンポーネントのタイトルを更新しました](https://github.com/adobe/xdm/pull/1634/files) 一貫性を保つために。 |
| フィールドグループ | [[!UICONTROL AJO 分類フィールド]](https://github.com/adobe/xdm/blob/master/extensions/adobe/experience/decisioning/proposition-event-type.schema.json) | 複数のフィールドの名前空間をに更新しました。 `xdm`. |
| フィールドグループ | [[!UICONTROL Journey Orchestration ステップイベントの共通フィールド]](https://github.com/adobe/xdm/blob/master/extensions/adobe/experience/journeyOrchestration/stepEvents/journeyStepEventCommonFieldsMixin.schema.json) | 新しいフィールド、 `isReadSegmentTriggerStartEvent`. |
| フィールドグループ | [[!UICONTROL 気晴らし]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/shared/forecasted-weather.schema.json) | 変更された `xdm:uvIndex` フィールドを整数型に変更し、 `xdm` 名前空間が見つからない複数のフィールドに対して追加されました。 |
| フィールドグループ | [[!UICONTROL メディアの詳細情報]](https://github.com/adobe/xdm/blob/master/components/datatypes/mediadetails.schema.json) | `xdm:endUserIDs` および `xdm:implementationDetails` がフィールドグループから削除されました。 |
| データタイプ | （複数） | [複数のメディアプロパティ名を更新しました](https://github.com/adobe/xdm/pull/1626/files) 一貫性を保つために複数のデータタイプにまたがって |
| データタイプ | [[!UICONTROL 実装の詳細]](https://github.com/adobe/xdm/blob/master/components/datatypes/industry-verticals/implementationdetails.schema.json) | フラッター用の既知の名前を追加しました。 |
| データタイプ | [[!UICONTROL 目標地点の詳細]](https://github.com/adobe/xdm/blob/master/components/datatypes/poi-detail.schema.json) | データタイプで、目標地点に関連付けられたメタデータのキーと値のペアのリストを受け取れるようになりました。 |
| データタイプ | [[!UICONTROL 提案アクション]](https://github.com/adobe/xdm/blob/master/extensions/adobe/experience/decisioning/proposition-action.schema.json) | [!DNL AJO Classification Fields] は、 [!UICONTROL 提案アクション]. |
| データタイプ | [[!UICONTROL 提案イベントタイプ]](https://github.com/adobe/xdm/blob/master/extensions/adobe/experience/decisioning/proposition-event-type.schema.json) | [!DNL AJO Classification Fields] の名前はに変更されました。 [!UICONTROL 提案アクション]. |
| （複数） | （複数） | 実験的特性がある [すべての B2B 成分に対して安定化](https://github.com/adobe/xdm/pull/1617/files). |
| （複数） | （複数） | Adobe Journey Optimizerエンティティが [安定化](https://github.com/adobe/xdm/pull/1625/files). |
| （複数） | （複数） | いくつかの実験的コンポーネントをまたいだ特定のフィールドの名前空間が、 [整合性を考慮して更新](https://github.com/adobe/xdm/pull/1626/files). |

{style=&quot;table-layout:auto&quot;}

Platform の XDM について詳しくは、[XDM システムの概要](../../xdm/home.md)を参照してください。

## ID サービス {#identity-service}

関連するデジタルエクスペリエンスを提供するには、顧客を完全に理解する必要があります。これは、顧客データが異なる複数のシステムに断片化され、各顧客が複数の「ID」を持つように見える場合に、より難しくなります。

Adobe Experience Platform ID サービスは、デバイスやシステム間で ID を結び付けることで、顧客とその行動をより良く把握でき、効果的な個人のデジタルエクスペリエンスをリアルタイムで提供します。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| データセット削除のサポート | ID サービスで、 [カタログサービス API](https://developer.adobe.com/experience-platform-apis/references/catalog/)、UI、またはデータの衛生状態。 次のガイドを読む： [UI でのデータセットの削除](../../catalog/datasets/user-guide.md#delete-a-dataset) を参照してください。 |

ID サービスの詳細については、 [ID サービスの概要](../../identity-service/home.md).

## [!DNL Artificial Intelligence/Machine Learning services] {#ai-and-ml-services}

AI／ML サービスは、マーケティングアナリストや実務担当者に対して、顧客体験のユースケースで人工知能とマシンラーニングの機能を活用する機能を提供します。これにより、マーケティングアナリストは、データサイエンスの専門知識がなくても、ビジネスレベルの設定を使用して、会社のニーズに固有のモデルを設定できます。

### アトリビューション AI

アトリビューション AI は、コンバージョンイベントにつながるタッチポイントの貢献度を明らかにするために使用します。これは、マーケターが、カスタマージャーニーをまたいだ個別マーケティングタッチポイントのマーケティング効果を、マーケターが、定量化する際に役立ちます。

| 機能 | 説明 |
| --- | --- |
| ドラフトインスタンスを保存 | この新機能により、マーケティングアナリストは、設定時にモデル設定をドラフトインスタンスとして保存し、トレーニングとスコアリングの前に完了するまでドラフトの編集を続行できます。 この機能が役立つシナリオには、設定ワークフローに複数のフィールドを定義して 1 回で完了できない場合や、データセットの統計（列の完全性など）が 1 つ以上使用可能になる前に処理に時間がかかる場合がありますが、これに限定されません。 詳しくは、 [Attribution AIユーザーガイド](../../intelligent-services/attribution-ai/user-guide.md) を参照してください。 |
| ガバナンスポリシー | ユーザーが設定ワークフローを使用してインスタンスを作成するように送信すると、新しいポリシー強制サービスは、データ使用に対するポリシー違反があるかどうかを確認し、詳細をポップオーバーに表示します。 これにより、データ操作とマーケティングアクションが、Adobe Experience Platformで設定されたデータ使用ポリシーに確実に準拠するようにします。 |

Attribution AI [Attribution AIの概要](../../intelligent-services/attribution-ai/overview.md). データガバナンスポリシーの詳細については、 [ポリシーの概要](../../data-governance/policies/overview.md).

### 顧客 AI

Real-time Customer Data Platform で使用できる顧客 AI は、個々のプロファイルのチャーンやコンバージョンなどのカスタム傾向スコアを大規模に生成するために使用します。

| 機能 | 説明 |
| --- | --- |
| ドラフトインスタンスを保存 | この新機能により、マーケティングアナリストは、設定時にモデル設定をドラフトインスタンスとして保存し、トレーニングとスコアリングの前に完了するまでドラフトの編集を続行できます。 この機能が役立つシナリオには、設定ワークフローに複数のフィールドを定義して 1 回で完了できない場合や、データセットの統計（列の完全性など）が 1 つ以上使用可能になる前に処理に時間がかかる場合がありますが、これに限定されません。 詳しくは、 [顧客 AI ユーザーガイド](../../intelligent-services/customer-ai/user-guide/configure.md) を参照してください。 |
| ガバナンスポリシー | ユーザーが設定ワークフローを使用してインスタンスを作成するように送信すると、新しいポリシー強制サービスは、データ使用に対するポリシー違反があるかどうかを確認し、詳細をポップオーバーに表示します。 これにより、データ操作とマーケティングアクションが、Adobe Experience Platformで設定されたデータ使用ポリシーに確実に準拠するようにします。 |

顧客 AI について詳しくは、 [顧客 AI の概要](../../intelligent-services/customer-ai/overview.md). データガバナンスポリシーの詳細については、 [ポリシーの概要](../../data-governance/policies/overview.md).

## ソース {#sources}

Adobe Experience Platform では、外部ソースからデータを取り込むときに、Platform サービスを使用して、そのデータの構造化、ラベル付け、拡張を行うことができます。アドビのアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM システムなど、様々なソースからデータを取り込むことができます。

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| Audience Managerセグメント母集団がリアルタイム顧客プロファイルに与える影響 | サイズの大きいAudience Managerセグメント母集団の取り込みは、Audience Managerソースを使用して初めてプラットフォームにAudience Managerセグメントを送信する際に、合計プロファイル数に直接影響します。 つまり、すべてのセグメントを選択すると、ライセンス使用権限を超えてプロファイル数がカウントされる可能性があります。 詳しくは、 [Audience Managerソースの概要](../../sources/connectors/adobe-applications/audience-manager.md). ライセンスの使用方法については、 [ライセンス使用状況ダッシュボードの使用](../../dashboards/guides/license-usage.md). |

ソースの詳細については、 [ソースの概要](../../sources/home.md).