---
title: Adobe Experience Platformリリースノート 2022 年 9 月
description: Adobe Experience Platformの 2022 年 9 月のリリースノート。
source-git-commit: dd17e0903f69a6d639582d83bcd22b269aedb5d0
workflow-type: tm+mt
source-wordcount: '3107'
ht-degree: 29%

---

# Adobe Experience Platform リリースノート

**リリース日：2022 年 9 月 28 日（PT）**

Adobe Experience Platform の新機能：

- [属性ベースのアクセス制御](#abac)
- [データハイジーン](#data-hygiene)
- [[!UICONTROL プライバシーコンソール]](#privacy-console)

Adobe Experience Platform の既存の機能に対するアップデート：

- [[!DNL Artificial Intelligence and Machine Learning Services]](#ai-and-ml-services)
- [監査ログ](#audit-logs)
- [[!DNL Dashboards]](#dashboards)
- [データ収集](#data-collection)
- [宛先](#destinations)
- [エクスペリエンスデータモデル（XDM）](#xdm)
- [ID サービス](#identity-service)
- [クエリサービス](#query-service)
- [ソース](#sources)

## 属性ベースのアクセス制御 {#abac}

>[!IMPORTANT]
>
>属性ベースのアクセス制御は、2022 年 10 月から有効になります。 アーリーアダプターになりたい場合は、Adobe担当者にお問い合わせください。

属性ベースのアクセス制御は、Adobe Experience Platformの機能で、プライバシーを意識するブランドがユーザーアクセスを柔軟に管理できるようにします。 スキーマフィールドやセグメントなどの個々のオブジェクトを、ユーザーの役割に割り当てることができます。 この機能を使用すると、組織内の特定の Platform ユーザーに対する個々のオブジェクトへのアクセスを許可または取り消すことができます。

属性ベースのアクセス制御を通じて、組織の管理者は、すべての Platform ワークフローとリソースにわたって、ユーザーのアクセス、機密個人データ (SPD)、個人識別情報 (PII)、その他のカスタマイズされた種類のデータを制御できます。 管理者は、特定のフィールドと、それらのフィールドに対応するデータにのみアクセスできるユーザーの役割を定義できます。

| 機能 | 説明 |
| --- | --- |
| 属性ベースのアクセス制御 | 属性ベースのアクセス制御を使用すると、エクスペリエンスデータモデル (XDM) スキーマフィールドとセグメントに、組織またはデータの使用範囲を定義するラベルを付けることができます。 同時に、管理者は、ユーザーと役割の管理インターフェイスを使用して、XDM スキーマフィールドとセグメントに関するアクセスポリシーを定義し、ユーザーまたはユーザーのグループ（内部、外部、サードパーティのユーザー）に与えるアクセスを管理できます。 詳しくは、[属性ベースのアクセス制御の概要](../../access-control/abac/overview.md)を参照してください。 |
| 権限 | 権限は、管理者がユーザーの役割およびアクセスポリシーを定義し、製品アプリケーション内の機能およびオブジェクトのアクセス権限を管理できる、Experience Cloud の領域です。権限を使用すると、役割の作成と管理、これらの役割に必要なリソース権限の割り当て、ラベルを活用し、特定の Platform リソースにアクセスできるユーザーの役割を定義するポリシーを作成できます。 また、権限では、特定の役割に関連付けられたラベル、サンドボックス、ユーザーを管理することもできます。詳しくは、[権限 UI ガイド](../../access-control/abac/ui/browse.md)を参照してください。 |

属性ベースのアクセス制御の詳細については、[属性ベースのアクセス制御の概要](../../access-control/abac/overview.md)を参照してください。属性ベースのアクセス制御ワークフローの包括的なガイドについては、 [属性ベースのアクセス制御エンドツーエンドガイド](../../access-control/abac/end-to-end-guide.md).

## データハイジーン {#data-hygiene}

Adobe Experience Platform では、カスタマーエクスペリエンスを調整するために、大規模で複雑なデータ操作を管理するための堅牢なツールのセットを提供しています。長い期間をかけてデータがシステムに取り込まれるにつれて、データが期待通りに使用され、間違ったデータを修正する必要がある場合は更新され、組織のポリシーで必要と判断された場合は削除されるように、データストアを管理することがますます重要になります。

Adobe Experience Platformのデータ衛生機能を使用すると、自動データセット有効期限をスケジュールし、ID に基づいて消費者データをプログラムで削除することで、データをクレンジングできます。

>[!IMPORTANT]
>
>データ衛生機能は、Adobe・ヘルスケア・シールドまたはプライバシー・シールドを購入した組織でのみ利用できます。

データの衛生状態に関する基礎知識を習得するには、次のドキュメントを参照してください。

- [データの衛生状態の概要](../../hygiene/home.md):Platform のデータ衛生機能の基本について説明します。
- [[!UICONTROL データの衛生状態] UI ガイド](../../hygiene/ui/overview.md):Platform ユーザーインターフェイス内でデータセットの有効期限と消費者の削除リクエストをスケジュールする方法について説明します。
- [データ衛生 API ガイド](../../hygiene/api/overview.md):UI で実行できるすべてのデータ衛生アクティビティは、プログラムによっても実行できます

## [!UICONTROL プライバシーコンソール] {#privacy-console}

この [!UICONTROL プライバシーコンソール] 「 」タブを使用すると、Experience PlatformUI に表示される、プライバシー関連の機能 ( [Privacy Serviceからのデータ主体のリクエスト](../../privacy-service/home.md), [データ衛生作業指示](../../hygiene/home.md)、および [監査ログ](../../landing/governance-privacy-security/audit-logs/overview.md). また、コンソールには、一般的なプライバシーワークフローの手順をガイドする、製品内の使用例ガイドも用意されています。

詳しくは、 [プライバシーコンソールの概要](../../landing/governance-privacy-security/privacy-console.md) を参照してください。

## [!DNL Artificial Intelligence/Machine Learning services] {#ai-and-ml-services}

AI／ML サービスは、マーケティングアナリストや実務担当者に対して、顧客体験のユースケースで人工知能とマシンラーニングの機能を活用する機能を提供します。これにより、マーケティングアナリストは、データサイエンスの専門知識がなくても、ビジネスレベルの設定を使用して、会社のニーズに固有のモデルを設定できます。

### アトリビューション AI

アトリビューション AI は、コンバージョンイベントにつながるタッチポイントの貢献度を明らかにするために使用します。これは、マーケターが、カスタマージャーニーをまたいだ個別マーケティングタッチポイントのマーケティング効果を、マーケターが、定量化する際に役立ちます。

| 機能 | 説明 |
| --- | --- |
| ドラフトインスタンスを保存 | この新機能を使用すると、マーケティングアナリストは、モデル設定をドラフトインスタンスとして保存し、トレーニングとスコアリングの前にモデルが完了するまで編集を続行できます。 この機能が役立つシナリオには、ユーザーがワークフローに定義する複数のフィールドを持っているが、時間の制約が原因でそれを完了できない場合などがあります。 別のシナリオとしては、1 つ以上のデータセット統計が処理され、まだ使用できない場合があります。 詳しくは、 [Attribution AIユーザーガイド](../../intelligent-services/attribution-ai/user-guide.md#governance-policies) を参照してください。 |
| ガバナンスポリシー | ユーザーが設定ワークフローを使用してインスタンスを作成するように送信すると、新しいポリシー強制サービスは、データ使用に対するポリシー違反があるかどうかを確認し、詳細をポップオーバーに表示します。 これにより、データ操作とマーケティングアクションが、Adobe Experience Platformで設定されたデータ使用ポリシーに確実に準拠するようにします。 |

Attribution AI [Attribution AIの概要](../../intelligent-services/attribution-ai/overview.md). データガバナンスポリシーの詳細については、 [ポリシーの概要](../../data-governance/policies/overview.md).

### 顧客 AI

Real-time Customer Data Platform で使用できる顧客 AI は、個々のプロファイルのチャーンやコンバージョンなどのカスタム傾向スコアを大規模に生成するために使用します。

| 機能 | 説明 |
| --- | --- |
| ドラフトインスタンスを保存 | この新機能を使用すると、マーケティングアナリストは、モデル設定をドラフトインスタンスとして保存し、トレーニングとスコアリングの前にモデルが完了するまで編集を続行できます。 この機能が役立つシナリオには、ユーザーがワークフローに定義する複数のフィールドを持っているが、時間の制約が原因でそれを完了できない場合などがあります。 別のシナリオとしては、1 つ以上のデータセット統計が処理され、まだ使用できない場合があります。 詳しくは、 [顧客 AI ユーザーガイド](../../intelligent-services/customer-ai/user-guide/configure.md#governance-policies) を参照してください。 |
| ガバナンスポリシー | ユーザーが設定ワークフローを使用してインスタンスを作成するように送信すると、新しいポリシー強制サービスは、データ使用に対するポリシー違反があるかどうかを確認し、詳細をポップオーバーに表示します。 これにより、データ操作とマーケティングアクションが、Adobe Experience Platformで設定されたデータ使用ポリシーに確実に準拠するようにします。 |

顧客 AI について詳しくは、 [顧客 AI の概要](../../intelligent-services/customer-ai/overview.md). データガバナンスポリシーの詳細については、 [ポリシーの概要](../../data-governance/policies/overview.md).

## 監査ログ {#audit-logs}

Experience Platform を使用すると、様々なサービスおよび機能についてユーザーアクティビティを監査できます。監査ログは、誰がいつ何をしたかに関する情報を提供します。

**更新された機能**

| 機能 | 名前 | 説明 |
| --- | --- | --- |
| 追加されたリソース | <ul><li>Attribution AIインスタンス</li><li>顧客 AI インスタンス</li><li>Datastream</li></ul> | 監査ログのリソースは、アクティビティが発生すると自動的に記録されます。この機能が有効な場合、ログ収集を手動で有効にする必要はありません。 |

{style=&quot;table-layout:auto&quot;}

Platform の監査ログで追跡される様々なリソース固有のイベントタイプについて詳しくは、 [監査ログの概要](../../landing/governance-privacy-security/audit-logs/overview.md).

## [!DNL Dashboards] {#dashboards}

Adobe Experience Platformは、毎日のスナップショットで取得した、組織のデータに関する重要なインサイトを表示できる、複数のダッシュボードを提供します。

| 機能 | 説明 |
| --- | --- |
| 使用中のラベル | 使用中のラベルをウィジェットライブラリで表示すると、ダッシュボードに既存のウィジェットが存在するかを簡単に識別できます。 これにより、同じウィジェットを複数回追加することができますが、重複を避けるのが簡単です。 |
| ユーザー定義ダッシュボード | ユーザー定義ダッシュボードでは、カスタムダッシュボードの作成と管理を可能にして、インサイトを効率化し、ビジュアライゼーションをカスタマイズするのに役立ちます。 ユーザー定義のダッシュボードを使用すると、カスタムウィジェットを作成、追加および編集して、組織に関連する主要指標を視覚化できます。 詳しくは、 [機能ガイド](../../dashboards/user-defined-dashboards.md) を参照してください。 |
| 顧客データプラットフォームインサイトデータモデル | 顧客データプラットフォーム (CDP) インサイトデータモデル機能は、データモデルと SQL を公開し、様々なプロファイル、宛先、セグメント化ウィジェットに関するインサイトを強化します。 これらの SQL クエリテンプレートをカスタマイズして、マーケティングおよび主要業績評価指標の使用例に関する CDP レポートを作成できます。 これらのインサイトは、ユーザー定義のダッシュボードのカスタムウィジェットとして使用できます。 詳しくは、 [CDP インサイトデータモデル機能ガイド](../../dashboards/cdp-insights-data-model.md) を参照してください。 |
| オーディエンス重複レポートウィジェット | このウィジェットは両方で使用できます [!UICONTROL プロファイル] および [!UICONTROL セグメント] ダッシュボード。 レポートには、選択したセグメントの重複が最も多い、または最も少ない割合でランク付けされた、オーディエンスの順番付きリストが表示されます。 次の [!UICONTROL プロファイル] ダッシュボードは、使用可能なすべてのセグメントから、結合ポリシーに基づいてオーディエンスの重複をフィルタリングして表示できます。 この [!UICONTROL セグメント] ダッシュボードでは、特定のセグメントでオーディエンスの重複をフィルタリングできます。<br>この分析を使用して、新しい高パフォーマンスのセグメントを作成し、同じオーディエンスが異なる宛先に送信されるのを防ぎます。 また、このレポートは、隠れたインサイトを特定して、セグメント化を改善したり、追跡する固有のプロファイルを見つけたりするのに役立ちます。 各 [プロファイル](../../dashboards/guides/profiles.md#audience-overlap-report) および [セグメント](../../dashboards/guides/segments.md#audience-overlap-report) ウィジェットガイドを参照してください。 |

[!DNL Dashboards] について詳しくは、[[!DNL Dashboards] 概要](../../dashboards/home.md)を参照してください。

## データ収集 {#data-collection}

Adobe Experience Platform は、クライアントサイドのカスタマーエクスペリエンスデータを収集し、Adobe Experience Platform Edge Network に送信します。そこでデータを強化、変換、アドビまたはアドビ以外の宛先への配信を可能にする一連のテクノロジーを提供します。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| Platform UI での左ナビゲーション統合 | 以前はデータ収集 UI 専用であったすべての機能（タグ、イベント転送、データストリームなど）も、Experience Platformの左側のナビゲーションから「 」カテゴリで使用できるようになりました。 **[!UICONTROL データ収集]**. これにより、Platform でデータ収集機能を使用する際に、UI を切り替える必要がなくなります。 |
| タグとイベント転送のユーザー属性 | 使用可能なリストを表示する場合 [!UICONTROL プロパティ] タグとイベント転送で、リストされた各プロパティが最終更新日時と、更新をおこなったユーザーが表示されるようになりました。 |
| [[!DNL User-Agent Client Hints] Web SDK の](../../edge/fundamentals/user-agent-client-hints.md) | Web SDK は、 [[!DNL User-Agent Client Hints]](https://developer.chrome.com/docs/privacy-sandbox/user-agent/). クライアントヒントを使用すると、Web サイトの所有者は、 [!DNL User-Agent] 文字列に基づいて書き込まれますが、よりプライバシーを保持する方法で書き込まれます。 |
| [ページ別の Web SDK の移行](../../edge/home.md#migrating-to-web-sdk) | 既存の Web プロパティを、次のような他のExperience Cloudライブラリから移行できるようになりました。 [!DNL at.js]を Web SDK に、一度に 1 ページずつ追加します。 これにより、すべてのページを一度に移行する必要なく、Web SDK の移行に対する段階的なアプローチを可能にします。 |

{style=&quot;table-layout:auto&quot;}

<!-- | [[!DNL Adobe Journey Optimizer] support for datastreams](../../edge/datastreams/overview.md#aep)| The Adobe Experience Platform service for datastreams now supports [!DNL Adobe Journey Optimizer]. This option allows you to use web and app-based inbound channels in [!DNL Adobe Journey Optimizer].|
-->

Platform のデータ収集について詳しくは、[データ収集の概要](../../collection/home.md)を参照してください。

## [!DNL Destinations] {#destinations}

[!DNL Destinations] は、Adobe Experience Platform からのデータの円滑なアクティベーションを可能にする、事前定義済みの出力先プラットフォームとの統合です。宛先を使用して、クロスチャネルマーケティングキャンペーン、電子メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

**新機能または更新された機能**

| 機能 | 説明 |
| ----------- | ----------- |
| Destination SDK | Destination SDKは、製品化されたバッチ（またはファイルベース）の宛先やプライベートの宛先を作成する、パートナーやお客様に対して完全なサポートを提供するようになりました。 詳しくは、次のドキュメントページを参照してください。 <ul><li>[Destination SDKの概要](/help/destinations/destination-sdk/overview.md)</li><li>[ファイルベースの宛先の設定](/help/destinations/destination-sdk/configure-file-based-destination-instructions.md)</li><li>[ファイルベースの宛先のファイル形式オプションの設定](/help/destinations/destination-sdk/configure-file-based-destination-instructions.md)</li><li>[ファイルベースの宛先のテスト](/help/destinations/destination-sdk/file-based-destination-testing-overview.md)</li></ul> |

{style=&quot;table-layout:auto&quot;}

**新しい宛先または更新された宛先**

| 宛先 | 説明 |
| ----------- | ----------- |
| [[!DNL Adobe Campaign Managed Cloud Services]](../../destinations/catalog/email-marketing/adobe-campaign-managed-services.md) | Adobe Campaign Managed Cloud Servicesは、クロスチャネルの顧客体験を設計するためのプラットフォームと、視覚的なキャンペーン編成、リアルタイムのインタラクション管理、クロスチャネルの実行のための環境を提供します。 [Campaign の概要](https://experienceleague.adobe.com/docs/campaign/campaign-v8/start/get-started.html). |
| [[!DNL Salesforce CRM]](../../destinations/catalog/crm/salesforce.md) | この [!DNL Salesforce CRM] の宛先が更新され、連絡先とリードの両方の更新に加え、迅速な更新を実現するパフォーマンスの向上がサポートされるようになりました。 |

{style=&quot;table-layout:auto&quot;}

**新規または更新されたドキュメント**

| ドキュメント | 説明 |
| ----------- | ----------- |
| 宛先フローサービス API ドキュメント | この [宛先 API リファレンスドキュメント](https://developer.adobe.com/experience-platform-apis/references/destinations/) は、ファイルベースの宛先での操作の実行方法に関するガイダンスを含むように更新されました。 ストリーミング先の操作は、後で追加されます。 |

宛先の一般的な情報については、[宛先の概要](../../destinations/home.md)を参照してください。

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

## クエリサービス {#query-service}

クエリサービスを使用すると、標準 SQL を使用して Adobe Experience Platform [!DNL Data Lake] でデータに対してクエリを実行できます。任意のデータセットを [!DNL Data Lake] から結合し、クエリの結果を新しいデータセットとして取得することで、レポートや Data Science Workspace で使用したり、リアルタイム顧客プロファイルに取り込んだりできます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| アラート購読 API | Adobe Experience Platformクエリサービスを使用すると、アドホッククエリとスケジュールされたクエリの両方でアラートを購読できます。 アラートは、E メール、Platform UI 内、またはその両方で受け取ることができます。 現在、クエリアラートは、 [クエリサービス API](https://developer.adobe.com/experience-platform-apis/references/query-service/). 詳しくは、 [クエリアラートドキュメント](../../query-service/api/alert-subscriptions.md) を参照してください。 |
| データセットのサンプル | クエリサービスのデータセットサンプルを使用すると、クエリの精度を犠牲にして、処理時間を大幅に短縮し、ビッグデータに関する探索的なクエリを実行できます。 詳しくは、 [データセットサンプルガイド](../../query-service/sql/dataset-samples.md) を参照してください。 |

[!DNL Query Service] について詳しくは、[[!DNL Query Service] 概要](../../query-service/home.md)を参照してください。

## ソース {#sources}

Adobe Experience Platform では、外部ソースからデータを取り込むときに、Platform サービスを使用して、そのデータの構造化、ラベル付け、拡張を行うことができます。アドビのアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM システムなど、様々なソースからデータを取り込むことができます。

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| Audience Managerセグメント母集団がリアルタイム顧客プロファイルに与える影響 | サイズの大きいAudience Managerセグメント母集団の取り込みは、Audience Managerソースを使用して初めてプラットフォームにAudience Managerセグメントを送信する際に、合計プロファイル数に直接影響します。 つまり、すべてのセグメントを選択すると、ライセンス使用権限を超えてプロファイル数がカウントされる可能性があります。 詳しくは、 [Audience Managerソースの概要](../../sources/connectors/adobe-applications/audience-manager.md). ライセンスの使用方法については、 [ライセンス使用状況ダッシュボードの使用](../../dashboards/guides/license-usage.md). |
| Adobe Campaign ManagedCloud Serviceのサポート | Adobe Campaign ManagedCloud Serviceソースを使用して、Adobe Campaign v8.4 の配信およびトラッキングログのデータをExperience Platformに取り込みます。 次のガイドを読む： [UI でのAdobe Campaign ManagedCloud Serviceソース接続の作成](../../sources/tutorials/ui/create/adobe-applications/campaign.md) を参照してください。 |
| バッチソースのオンデマンド取り込みの API サポート | オンデマンド取り込みを使用して、 [!DNL Flow Service] API 作成されたフロー実行は、1 回の取り込みに設定する必要があります。 詳しくは、 [API を使用したオンデマンド取り込み用のフロー実行の作成](../../sources/tutorials/api/on-demand-ingestion.md) を参照してください。 |
| バッチソースの失敗したデータフロー実行の再試行に対する API のサポート | 以下を使用： `re-trigger` 操作を使用して、API を介して失敗したデータフローを再試行する必要があります。 次のガイドを読む： [API を使用した、失敗したデータフローの実行の再試行](../../sources/tutorials/api/retry-flows.md) を参照してください。 |
| の行レベルのデータをフィルタリングするための API のサポート [!DNL Google BigQuery] および [!DNL Snowflake] ソース | 論理演算子と比較演算子を使用して、 [!DNL Google BigQuery] および [!DNL Snowflake] ソース。 次のガイドを読む： [API を使用したソースのデータのフィルタリング](../../sources/tutorials/api/filter.md) を参照してください。 |

ソースの詳細については、 [ソースの概要](../../sources/home.md).