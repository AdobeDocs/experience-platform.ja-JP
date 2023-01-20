---
title: Adobe Experience Platform リリースノート 2022年9月
description: Adobe Experience Platform の 2022年9月のリリースノート。
exl-id: a7a4dcf8-2cf3-4e39-879d-bdfcbacb737a
source-git-commit: 668b2624b7a23b570a3869f87245009379e8257c
workflow-type: tm+mt
source-wordcount: '2934'
ht-degree: 98%

---

# Adobe Experience Platform リリースノート

**リリース日：2022年9月28日（PT）**

Adobe Experience Platform の新機能：

- [属性ベースのアクセス制御](#abac)

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
>属性ベースのアクセス制御は、2022年10月から有効になります。 早期導入をご希望の場合は、アドビ担当者にお問い合わせください。

属性ベースのアクセス制御は、プライバシーを重視するブランドが、ユーザーアクセスをより柔軟に管理できるようにする、Adobe Experience Platform の機能です。ユーザーの役割に、スキーマフィールドやセグメントなどの個々のオブジェクトを割り当てることができます。 この機能を使用すると、組織内の特定の Platform ユーザーに対する個々のオブジェクトへのアクセスを許可または取り消すことができます。

属性ベースのアクセス制御により、組織の管理者は、すべての Platform ワークフローとリソースにわたって、機密性の高い個人データ（SPD）、個人を特定できる情報（PII）、およびその他のカスタマイズされた種類のデータへのユーザーのアクセスを制御できます。管理者は、特定のフィールドと、それらのフィールドに対応するデータにのみアクセスできるユーザーの役割を定義できます。

| 機能 | 説明 |
| --- | --- |
| 属性ベースのアクセス制御 | 属性ベースのアクセス制御を使用すると、エクスペリエンスデータモデル（XDM）スキーマフィールドやセグメントに、組織またはデータの使用範囲を定義するラベルを付けることができます。同時に、管理者は、ユーザーと役割の管理インターフェイスを使用して、XDM スキーマフィールドやセグメントをカバーするアクセスポリシーを定義し、ユーザーまたはユーザーのグループ（内部、外部、またはサードパーティのユーザー）に与えるアクセスをうまく管理できます。詳しくは、[属性ベースのアクセス制御の概要](../../access-control/abac/overview.md)を参照してください。 |
| 権限 | 権限は、管理者がユーザーの役割およびアクセスポリシーを定義し、製品アプリケーション内の機能およびオブジェクトのアクセス権限を管理できる、Experience Cloud の領域です。権限を使用すると、役割の作成と管理、これらの役割に必要なリソース権限の割り当て、ポリシーを作成してラベルを活用し、特定の Platform リソースにアクセスできるユーザーの役割を定義できます。 また、権限では、特定の役割に関連付けられたラベル、サンドボックス、ユーザーを管理することもできます。詳しくは、[権限 UI ガイド](../../access-control/abac/ui/browse.md)を参照してください。 |

属性ベースのアクセス制御の詳細については、[属性ベースのアクセス制御の概要](../../access-control/abac/overview.md)を参照してください。属性ベースのアクセス制御ワークフローの包括的なガイドについては、[属性ベースのアクセス制御エンドツーエンドガイド](../../access-control/abac/end-to-end-guide.md)を参照してください。

## [!DNL Artificial Intelligence/Machine Learning services] {#ai-and-ml-services}

AI／ML サービスは、マーケティングアナリストや実務担当者に対して、顧客体験のユースケースで人工知能とマシンラーニングの機能を活用する機能を提供します。これにより、マーケティングアナリストは、データサイエンスの専門知識がなくても、ビジネスレベルの設定を使用して、会社のニーズに固有のモデルを設定できます。

### アトリビューション AI

アトリビューション AI は、コンバージョンイベントにつながるタッチポイントの貢献度を明らかにするために使用します。これは、マーケターが、カスタマージャーニーをまたいだ個別マーケティングタッチポイントのマーケティング効果を、マーケターが、定量化する際に役立ちます。

| 機能 | 説明 |
| --- | --- |
| ドラフトインスタンスの保存 | この新機能を使用すると、マーケティングアナリストは、モデル設定をドラフトインスタンスとして保存し、トレーニングとスコアリングの前に完了するまで編集を続行できます。この機能が役立つシナリオとして、ユーザーがワークフローに定義する複数のフィールドを持っているが、時間の制約が原因でそれを完了できない場合などが考えられます。別のシナリオとしては、1 つ以上のデータセット統計が処理中で、まだ使用できない場合があります。詳しくは、[アトリビューション AI ユーザーガイド](../../intelligent-services/attribution-ai/user-guide.md#governance-policies)を参照してください。 |
| ガバナンスポリシー | ユーザーが設定ワークフローを使用してインスタンス作成を送信すると、新しいポリシー強制サービスは、データ使用に関するポリシー違反の有無を確認し、その詳細をポップオーバーに表示します。これにより、データ操作とマーケティングアクションが、Adobe Experience Platform で設定されたデータ使用ポリシーに確実に準拠するようにします。 |

アトリビューション AI について詳しくは、[アトリビューション AI の概要](../../intelligent-services/attribution-ai/overview.md)を参照してください。データガバナンスポリシーの詳細については、[ポリシーの概要](../../data-governance/policies/overview.md)を参照してください。

### 顧客 AI

Real-Time Customer Data Platform で使用できる顧客 AI は、個々のプロファイルのチャーンやコンバージョンなどのカスタム傾向スコアを大規模に生成するために使用します。

| 機能 | 説明 |
| --- | --- |
| ドラフトインスタンスの保存 | この新機能を使用すると、マーケティングアナリストは、モデル設定をドラフトインスタンスとして保存し、トレーニングとスコアリングの前に完了するまで編集を続行できます。この機能が役立つシナリオとして、ユーザーがワークフローに定義する複数のフィールドを持っているが、時間の制約が原因でそれを完了できない場合などが考えられます。別のシナリオとしては、1 つ以上のデータセット統計が処理中で、まだ使用できない場合があります。詳しくは、[顧客 AI ユーザーガイド](../../intelligent-services/customer-ai/user-guide/configure.md#governance-policies)を参照してください。 |
| ガバナンスポリシー | ユーザーが設定ワークフローを使用してインスタンス作成を送信すると、新しいポリシー強制サービスは、データ使用に関するポリシー違反の有無を確認し、その詳細をポップオーバーに表示します。これにより、データ操作とマーケティングアクションが、Adobe Experience Platform で設定されたデータ使用ポリシーに確実に準拠するようにします。 |

顧客 AI について詳しくは、[顧客 AI の概要](../../intelligent-services/customer-ai/overview.md)を参照してください。データガバナンスポリシーの詳細については、[ポリシーの概要](../../data-governance/policies/overview.md)を参照してください。

## 監査ログ {#audit-logs}

Experience Platform を使用すると、様々なサービスおよび機能についてユーザーアクティビティを監査できます。監査ログは、誰がいつ何をしたかに関する情報を提供します。

**更新された機能**

| 機能 | 名前 | 説明 |
| --- | --- | --- |
| 追加されたリソース | <ul><li>アトリビューション AI インスタンス</li><li>顧客 AI インスタンス</li><li>データストリーム</li></ul> | 監査ログのリソースは、アクティビティが発生すると自動的に記録されます。この機能が有効な場合、ログ収集を手動で有効にする必要はありません。 |

{style=&quot;table-layout:auto&quot;}

Platform の監査ログで追跡される様々なリソース固有のイベントタイプについて詳しくは、[監査ログの概要](../../landing/governance-privacy-security/audit-logs/overview.md)を参照してください。

## [!DNL Dashboards] {#dashboards}

Adobe Experience Platform では、毎日のスナップショットで得られた、組織のデータに関する重要なインサイトを表示できる複数のダッシュボードを提供しています。

| 機能 | 説明 |
| --- | --- |
| 使用中のラベル | 使用中のラベルをウィジェットライブラリで表示すると、ダッシュボード内の既存のウィジェットの存在を簡単に識別できます。これにより、重複を避けることができますが、必要に応じて同じウィジェットを複数回追加することはできます。 |
| ユーザー定義ダッシュボード | ユーザー定義ダッシュボードでは、カスタムダッシュボードの作成と管理を可能にして、インサイトを効率化し、ビジュアライゼーションをカスタマイズするのに役立ちます。ユーザー定義のダッシュボードを使用すると、カスタムウィジェットを作成、追加および編集して、組織に関連する主要指標を視覚化できます。詳しくは、[機能ガイド](../../dashboards/user-defined-dashboards.md)を参照してください。 |
| 顧客データプラットフォームインサイトデータモデル | 顧客データプラットフォーム（CDP）インサイトデータモデル機能は、データモデルと SQL を公開し、様々なプロファイル、宛先、セグメント化ウィジェットに関するインサイトを強化します。 これらの SQL クエリテンプレートをカスタマイズして、マーケティングおよび主要業績評価指標の使用例に関する CDP レポートを作成できます。 ユーザー定義のダッシュボードのカスタムウィジェットとして、これらのインサイトを使用できます。 詳しくは、[CDP インサイトデータモデル機能ガイド](../../dashboards/cdp-insights-data-model.md)を参照してください。 |
| オーディエンス重複レポートのウィジェット | このウィジェットは、[!UICONTROL プロファイル]および[!UICONTROL セグメント]ダッシュボードの両方で使用できます。 レポートには、選択したセグメントの重複率の高い順または低い順にランク付けされたオーディエンスのリストが表示されます。[!UICONTROL プロファイル]ダッシュボードから、使用可能なすべてのセグメントの結合ポリシーでオーディエンスの重複をフィルタリングして表示できます。 この[!UICONTROL セグメント]ダッシュボードでは、特定のセグメントでオーディエンスの重複をフィルタリングできます。<br>この分析を使用して、新しい高パフォーマンスのセグメントを作成し、同じオーディエンスを別の宛先に送信しないようにします。また、このレポートは、隠れたインサイトを特定して、セグメント化を改善したり、追跡する固有のプロファイルを見つけたりするのに役立ちます。 詳しくは、[プロファイル](../../dashboards/guides/profiles.md#audience-overlap-report)と[セグメント](../../dashboards/guides/segments.md#audience-overlap-report)のウィジェットガイドを参照してください。 |

[!DNL Dashboards] について詳しくは、[[!DNL Dashboards] 概要](../../dashboards/home.md)を参照してください。

## データ収集 {#data-collection}

Adobe Experience Platform では、クライアントサイドのカスタマーエクスペリエンスデータを収集し、Adobe Experience Platform Edge Network に送信できます。そこでデータを補強して変換し、アドビまたはアドビ以外の宛先に配信できます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| Platform UI での左ナビゲーション統合 | 以前はデータ収集 UI 専用であったすべての機能（タグ、イベント転送、データストリームなど）は、カテゴリ&#x200B;**[!UICONTROL データ収集]**&#x200B;下の Experience Platform の左ナビゲーションからも利用できるようになりました。これにより、Platform でデータ収集機能を使用する際に、UI を切り替える必要がなくなります。 |
| タグとイベント転送におけるユーザー属性 | タグとイベント転送で使用可能な[!UICONTROL プロパティ]を一覧表示すると、一覧表示された各プロパティが最終更新日時と、更新を行ったユーザーが表示されるようになりました。 |
| イベント転送用 [[!DNL Snap Conversions API] 拡張機能](https://exchange.adobe.com/apps/ec/108550) | [イベント転送](../../tags/ui/event-forwarding/overview.md)拡張機能を使用して、[!DNL Snapchat Conversions API] にデータを送信できるようになりました。認証方法と API の使用方法について詳しくは、[[!DNL Snapchat Marketing API] ドキュメント](https://marketingapi.snapchat.com/docs/conversion.html)を参照してください。 |
| Web SDK における [[!DNL User-Agent Client Hints] ](../../edge/fundamentals/user-agent-client-hints.md) | Web SDK は、[[!DNL User-Agent Client Hints]](https://developer.chrome.com/docs/privacy-sandbox/user-agent/) をサポートするようになりました。Client Hints は、web サイト所有者に、[!DNL User-Agent] 文字列で利用できるのとほぼ同じ量の情報に、よりプライバシーが保護された方法でアクセスできます。 |
| [Web SDK のページごとの移行](../../edge/home.md#migrating-to-web-sdk) | 既存の web プロパティを、[!DNL at.js] などの他の Experience Cloud ライブラリから Web SDK に一度に 1 ページずつ移行できるようになりました。これにより、すべてのページを一度に移行する必要なく、Web SDK の移行に対する段階的なアプローチを可能にします。 |

{style=&quot;table-layout:auto&quot;}

<!-- | [[!DNL Adobe Journey Optimizer] support for datastreams](../../edge/datastreams/overview.md#aep)| The Adobe Experience Platform service for datastreams now supports [!DNL Adobe Journey Optimizer]. This option allows you to use web and app-based inbound channels in [!DNL Adobe Journey Optimizer].|
-->

Platform のデータ収集について詳しくは、[データ収集の概要](../../collection/home.md)を参照してください。

## [!DNL Destinations] {#destinations}

[!DNL Destinations] は、Adobe Experience Platform からのデータの円滑なアクティベーションを可能にする、事前定義済みの出力先プラットフォームとの統合です。宛先を使用して、クロスチャネルマーケティングキャンペーン、電子メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

**新機能または更新された機能**

| 機能 | 説明 |
| ----------- | ----------- |
| Destination SDK | Destination SDK では、パートナーや顧客が、製品化されたバッチ（またはファイルベース）宛先やプライベートの宛先を作成できるようになりました。詳しくは、次のドキュメントページを参照してください。 <ul><li>[Destination SDK の概要](/help/destinations/destination-sdk/overview.md)</li><li>[ファイルベースの宛先の設定](/help/destinations/destination-sdk/configure-file-based-destination-instructions.md)</li><li>[ファイルベースの宛先のファイル形式オプションの設定](/help/destinations/destination-sdk/configure-file-based-destination-instructions.md)</li><li>[ファイルベースの宛先のテスト](/help/destinations/destination-sdk/file-based-destination-testing-overview.md)</li></ul> |

{style=&quot;table-layout:auto&quot;}

**新規宛先または更新された宛先**

| 宛先 | 説明 |
| ----------- | ----------- |
| [[!DNL Adobe Campaign Managed Cloud Services]](../../destinations/catalog/email-marketing/adobe-campaign-managed-services.md) | Adobe Campaign Managed Cloud Services は、クロスチャネルのカスタマーエクスペリエンスを設計するためのプラットフォームと、視覚的なキャンペーンオーケストレーション、リアルタイムインタラクション管理、クロスチャネル実行のための環境を提供します。[Campaign の基本を学ぶ](https://experienceleague.adobe.com/docs/campaign/campaign-v8/start/get-started.html?lang=ja)を参照してください。この統合は、[Adobe Campaign バージョン 8.4 以降](https://experienceleague.adobe.com/docs/campaign/campaign-v8/new/release-notes.html?lang=ja#release-8-4-1)で機能することに注意してください。 |
| [[!DNL Salesforce CRM]](../../destinations/catalog/crm/salesforce.md) | この [!DNL Salesforce CRM] の宛先が更新され、連絡先とリード両方の更新をサポートするようになりました。また、迅速な更新を実現するようパフォーマンスを向上しました。 |

{style=&quot;table-layout:auto&quot;}

**新規ドキュメントまたは更新されたドキュメント**

| ドキュメント | 説明 |
| ----------- | ----------- |
| Destinations Flow Service API ドキュメント | [Destinations API リファレンスドキュメント](https://developer.adobe.com/experience-platform-apis/references/destinations/)が更新され、ファイルベースの宛先に関する操作方法のガイダンスが追加されました。ストリーミング宛先の操作は、今後追加される予定です。 |

宛先の一般的な情報については、[宛先の概要](../../destinations/home.md)を参照してください。

## エクスペリエンスデータモデル（XDM） {#xdm}

XDM は、Adobe Experience Platform に取り込むデータの共通構造および定義（スキーマ）を提供するオープンソース仕様です。XDM 標準規格に準拠しているため、すべての顧客体験データを共通の表現に反映させて、迅速かつ統合的な方法でインサイトを提供できます。顧客行動から有益なインサイトを得たり、セグメントを通じて顧客オーディエンスを定義したり、パーソナライゼーションのために顧客属性を使用したりできます。

**新機能**

| 機能 | 説明 |
| --- | --- |
| 列挙と推奨値の UI のサポート | Platform ユーザーがセグメントの作成時に、わかりやすい値のリストから選択できるよう、データの検証を有効にする列挙に加えて、標準またはカスタム文字列フィールドに[推奨値を追加または削除](../../xdm/ui/fields/enum.md)ができるようになりました。 |

**新しい XDM コンポーネント**

| コンポーネントのタイプ | 名前 | 説明 |
| --- | --- | --- |
| フィールドグループ | [[!UICONTROL AJO 分類フィールド]](https://github.com/adobe/xdm/blob/master/extensions/adobe/experience/decisioning/proposition-action.schema.json) | 提案イベントがトリガーされる原因となった、操作された特定の要素のプロパティ。 |
| フィールドグループ | [[!UICONTROL Media Analytics インタラクションの詳細]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/experienceevent-media-analytics.schema.json) | メディアとのインタラクションを時系列で追跡します。 |
| フィールドグループ | [[!UICONTROL メディアの詳細情報]](https://github.com/adobe/xdm/blob/master/components/datatypes/mediadetails.schema.json) | メディアの詳細情報を追跡します。 |
| フィールドグループ | [[!UICONTROL Adobe CJM ExperienceEvent - サーフェス]](https://github.com/adobe/xdm/blob/master/extensions/adobe/experience/customerJourneyManagement/surfaces.schema.json) | Adobe Journey Optimizer でのエクスペリエンスイベントのサーフェスについて説明します。 |

{style=&quot;table-layout:auto&quot;}

**更新された XDM コンポーネント**

| コンポーネントのタイプ | 名前 | 説明 |
| --- | --- | --- |
| 動作 | [[!UICONTROL 時系列]](https://github.com/adobe/xdm/blob/master/components/behaviors/time-series.schema.json) | <ul><li>`eventType` の値を追加しました。<ul><li>`decisioning.propositionSend`</li><li>`decisioning.propositionDismiss`</li><li>`decisioning.propositionTrigger`</li><li>`media.downloaded`</li><li>`location.entry`</li><li>`location.exit`</li></ul></li><li>`eventType` の値を削除しました。<ul><li>`decisioning.propositionDeliver`</li><li>`media.stateStart`</li><li>`media.stateEnd`</li></ul></li></ul> |
| フィールドグループ | （複数） | Journey Orchestration コンポーネント全体で[いくつかのフィールドの説明を更新しました。](https://github.com/adobe/xdm/pull/1628/files) |
| フィールドグループ | （複数） | 一貫性を保つため、[いくつかの Adobe Workfront コンポーネントのタイトルを](https://github.com/adobe/xdm/pull/1634/files)更新しました。 |
| フィールドグループ | [[!UICONTROL AJO 分類フィールド]](https://github.com/adobe/xdm/blob/master/extensions/adobe/experience/decisioning/proposition-event-type.schema.json) | 複数のフィールドの名前空間を `xdm` に更新しました。 |
| フィールドグループ | [[!UICONTROL Journey Orchestration ステップイベントの共通フィールド]](https://github.com/adobe/xdm/blob/master/extensions/adobe/experience/journeyOrchestration/stepEvents/journeyStepEventCommonFieldsMixin.schema.json) | 新しいフィールド `isReadSegmentTriggerStartEvent` を追加しました。 |
| フィールドグループ | [[!UICONTROL 予測された天気]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/shared/forecasted-weather.schema.json) | `xdm:uvIndex` フィールドを整数タイプに変更し、欠落していたいくつかのフィールドに `xdm` 名前空間を追加しました。 |
| フィールドグループ | [[!UICONTROL メディアの詳細情報]](https://github.com/adobe/xdm/blob/master/components/datatypes/mediadetails.schema.json) | `xdm:endUserIDs` および `xdm:implementationDetails` がフィールドグループから削除されました。 |
| データタイプ | （複数） | 一貫性を保つため、複数のデータタイプをまたいで[複数のメディアプロパティ名を更新しました。](https://github.com/adobe/xdm/pull/1626/files) |
| データタイプ | [[!UICONTROL 実装の詳細]](https://github.com/adobe/xdm/blob/master/components/datatypes/industry-verticals/implementationdetails.schema.json) | フラッターの既知の名前を追加しました。 |
| データタイプ | [[!UICONTROL POI の詳細]](https://github.com/adobe/xdm/blob/master/components/datatypes/poi-detail.schema.json) | データタイプで、POI に関連付けられたメタデータのキーと値のペアのリストを受け取れるようになりました。 |
| データタイプ | [[!UICONTROL 提案アクション]](https://github.com/adobe/xdm/blob/master/extensions/adobe/experience/decisioning/proposition-action.schema.json) | [!DNL AJO Classification Fields] の名前は、[!UICONTROL 提案アクション]に変更されました。 |
| データタイプ | [[!UICONTROL 提案イベントタイプ]](https://github.com/adobe/xdm/blob/master/extensions/adobe/experience/decisioning/proposition-event-type.schema.json) | [!DNL AJO Classification Fields] の名前は、[!UICONTROL 提案アクション]に変更されました。 |
| （複数） | （複数） | 実験的なプロパティは、[B2B の全コンポーネントにおいて安定化されました](https://github.com/adobe/xdm/pull/1617/files)。 |
| （複数） | （複数） | Adobe Journey Optimizer エンティティは、[安定化](https://github.com/adobe/xdm/pull/1625/files)されました。 |
| （複数） | （複数） | いくつかの実験的コンポーネントをまたいだ特定のフィールドの名前空間は、[一貫性を保つために更新されました](https://github.com/adobe/xdm/pull/1626/files)。 |

{style=&quot;table-layout:auto&quot;}

Platform の XDM について詳しくは、[XDM システムの概要](../../xdm/home.md)を参照してください。

## ID サービス {#identity-service}

適切なデジタルエクスペリエンスを提供するには、顧客を完全に理解する必要があります。顧客データが異なるシステムに分散している場合、各顧客が複数の「ID」を持っているように見えるため、より困難になります。

Adobe Experience Platform ID サービスを利用すると、デバイスやシステム間で ID を橋渡しすることで、顧客とその行動をよりよく把握できます。これによって、インパクトのあるパーソナルなデジタルエクスペリエンスをリアルタイムで提供できます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| データセット削除のサポート | ID サービスで、[Catalog Service API](https://developer.adobe.com/experience-platform-apis/references/catalog/)、UI またはデータハイジーンを通じてリクエストした場合、データセットの削除をサポートするようになりました。詳しくは、[UI でのデータセットの削除](../../catalog/datasets/user-guide.md#delete-a-dataset)に関するガイドを参照してください。 |

ID サービスの詳細については、[ID サービスの概要](../../identity-service/home.md)を参照してください。

## クエリサービス {#query-service}

クエリサービスを使用すると、標準 SQL を使用して Adobe Experience Platform [!DNL Data Lake] でデータに対してクエリを実行できます。任意のデータセットを [!DNL Data Lake] クエリの結果を新しいデータセットとして取り込み、レポートや Data Science Workspace で使用したり、リアルタイム顧客プロファイルに取り込んだりできます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| アラートサブスクリプション API | Adobe Experience Platform クエリサービスを使用すると、アドホッククエリとスケジュールされたクエリの両方でアラートを受け取ることができます。アラートは、メール、Platform UI 内またはその両方で受け取ることができます。現在、クエリアラートは、[Query Service API](https://developer.adobe.com/experience-platform-apis/references/query-service/) を使用してのみ受け取ることができます。 |
| データセットのサンプル | クエリサービスのデータセットサンプルを使用すると、クエリの精度を犠牲にする代わりに、処理時間を大幅に短縮し、ビッグデータに関する探索的なクエリを実行できます。詳しくは、[データセットサンプルに関するガイド](../../query-service/essential-concepts/dataset-samples.md)を参照してください。 |

[!DNL Query Service] について詳しくは、[[!DNL Query Service] 概要](../../query-service/home.md)を参照してください。

詳しくは、[クエリアラートのドキュメント](../../query-service/api/alert-subscriptions.md)を参照してください。

## ソース {#sources}

Adobe Experience Platform では、外部ソースからデータを取り込むときに、Platform サービスを使用して、そのデータの構造化、ラベル付け、拡張を行うことができます。アドビのアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM システムなど、様々なソースからデータを取り込むことができます。

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| Audience Managerセグメント母集団がリアルタイム顧客プロファイルに与える影響 | サイズの大きい Audience Manager セグメント母集団の取り込みは、Audience Manager ソースを使用して初めて Platform に Audience Manager セグメントを送信する際に、合計プロファイル数に直接影響します。つまり、すべてのセグメントを選択すると、ライセンス使用権限を超えるプロファイル数が発生する可能性があります。詳しくは、[Audience Manager ソースの概要](../../sources/connectors/adobe-applications/audience-manager.md)を参照してください。ライセンスの使用方法については、[ライセンス使用状況ダッシュボードの使用](../../dashboards/guides/license-usage.md)に関するドキュメントを参照してください。 |
| Adobe Campaign Managed Cloud Service のサポート | Adobe Campaign Managed Cloud Service ソースを使用して、Adobe Campaign v8.4 の配信およびトラッキングログのデータを Experience Platform に取り込みます。詳しくは、[UI での Adobe Campaign Managed Cloud Service ソース接続の作成](../../sources/tutorials/ui/create/adobe-applications/campaign.md)に関するガイドを参照してください。 |
| バッチソースのオンデマンド取り込みの API サポート | オンデマンド取り込みを使用して、[!DNL Flow Service] API で指定されたデータフローに対してアドホックなフローの実行を作成します。作成されたフロー実行は、1 回のみの取り込みに設定する必要があります。詳しくは、[API を使用したオンデマンド取り込み用のフロー実行の作成](../../sources/tutorials/api/on-demand-ingestion.md)に関するガイドを参照してください。 |
| バッチソースで失敗したデータフロー実行の再試行に対する API のサポート | `re-trigger` 操作を使用して、API を介して失敗したデータフローを再試行します。[API を使用して失敗したデータフロー実行の再試行](../../sources/tutorials/api/retry-flows.md)に関するガイドを参照してください。 |
| [!DNL Google BigQuery] および [!DNL Snowflake] ソースの行レベルのデータをフィルタリングする API のサポート | 論理演算子と比較演算子を使用して、[!DNL Google BigQuery] および [!DNL Snowflake] ソースの行レベルのデータをフィルタリングします。[API を使用してソースのデータのフィルタリング](../../sources/tutorials/api/filter.md)に関するガイドを参照してください。 |

ソースについて詳しくは、[ソースの概要](../../sources/home.md)を参照してください。
