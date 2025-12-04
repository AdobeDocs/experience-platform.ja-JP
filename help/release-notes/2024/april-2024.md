---
title: Adobe Experience Platform リリースノート 2024年4月
description: Adobe Experience Platform の 2024年4月のリリースノート。
exl-id: 86d72fd8-a464-4715-abc9-4177236e423c
source-git-commit: bb90bbddf33bc4b0557026a0f34965ac37475c65
workflow-type: tm+mt
source-wordcount: '1893'
ht-degree: 25%

---

# Adobe Experience Platform リリースノート

**リリース日：2024年4月30日**

>[!TIP]
>
>Real-Time Customer Data PlatformおよびAdobe Experience Platformで使用される用語については [](/help/landing/glossary.md)Adobe Experience Platform用語集 } を参照してください。 探している用語が見つからない場合は、ページのフィードバックオプションを使用して、用語集に新しい用語を追加するようにリクエストします。

Experience Platformの既存の機能に対するアップデート：

- [ダッシュボード](#dashboards)
- [データ収集](#data-collection)
- [宛先](#destinations)
- [ID サービス](#identity-service)
- [監視](#monitoring)
- [クエリサービス](#query-service)
- [サンドボックス](#sandboxes)
- [セグメント化サービス](#segmentation)
- [ソース](#sources)

## ダッシュボード {#dashboards}

Adobe Experience Platform では、毎日のスナップショットで得られた、組織のデータに関する重要なインサイトを確認できる複数のダッシュボードを提供しています。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| Real-Time Customer Data Platform B2B インサイト | アカウントとオポチュニティに関する事前設定済みの [Real-Time CDP B2B データインサイトを調べ ](../../dashboards/insights/account-profiles.md) データを理解し、ビジネス上の意思決定に役立てます。 また、[Real-Time CDP B2B データモデルを使用して独自のインサイトを作成 ](../../dashboards/data-models/cdp-insights-data-model-b2c.md) し、データを視覚化して調査し、ダッシュボードにカスタムビジュアライゼーションを保存することもできます。 |

{style="table-layout:auto"}

アクセス権限の付与方法やカスタムウィジェットの作成方法など、ダッシュボードの詳細については、まず[ダッシュボードの概要](../../dashboards/home.md)を参照してください。

## データ収集 {#data-collection}

Adobe Experience Platformは、クライアントサイドのカスタマーエクスペリエンスデータを収集し、Experience Platform Edge Networkに送信します。そこでデータを強化、変換、AdobeまたはAdobe以外の宛先への配信を可能にする一連のテクノロジーを提供します。

**新機能または更新された機能**

| タイプ | 機能 | 説明 |
| --- | --- | --- |
| 拡張機能 | [!DNL Acxiom Anonymous Visitor Insights] Tags 拡張機能 | [!DNL Acxiom's Visitor Insights] を使用して、web サイトの訪問者の元の場所を特定します。 Acxiom では、地域 IP 検索テクノロジーを利用して、匿名ブラウザの場所を特定できます。 識別されると、組織データベース内の検索によって追加のインサイトが得られ、ブラウザーに送り返されます。 これにより、コンテンツ作成者は、これらのデータポイントに合わせてコンテンツを調整でき、見知らぬ人として出発した訪問者でも、よりパーソナライズされた魅力的なエクスペリエンスを訪問者に提供できます。 |
| データストリーム | [Edge Network ボットの検出 ](../../datastreams/bot-detection.md) | 自動プログラム、Web スクレーパー、スパイダー、スクリプト化されたスキャナーなど、人間以外のエンティティから発生するトラフィックによって、人間の訪問者から発生するイベントの特定がより困難になる場合があります。 このタイプのトラフィックは、重要なビジネス指標に悪影響を与え、誤ったトラフィックレポートにつながる可能性があります。 <br> ボット検出を使用すると、[Adobe Experience Platform Data Collection によって生成されたイベントを ](/help/collection/home.md) 既知のスパイダーやボットによって生成されたものとして識別できます。 データストリームのボット検出を設定することで、ボットイベントとして分類したい特定の IP アドレス、IP 範囲およびリクエストヘッダーを識別できます。 <br> ボットトラフィックの識別によって、サイトまたはモバイルアプリケーション上のユーザーアクティビティをより正確に測定できます。 |
| Mobile SDK | メジャーバージョンリリース | Mobile SDKの新しいメジャーバージョンがリリースされました。対象のプラットフォームは、iOS Mobile Core 5.x と互換性のあるiOS拡張機能、Android Mobile Core 3.x と互換性のあるAndroid拡張機能、React Native Core 6.x と互換性のあるReact Native拡張機能、Flutter Core 4.x と互換性のある Flutter 拡張機能です。 これらのリリースでは、Android SDK for Jetpack Compose のサポート、Adobe Journey Optimizer コードベースのエクスペリエンスのサポート、Flutter 向けAdobe Journey Optimizer Messaging 拡張機能の一般提供など、いくつかの新機能と機能強化が提供されています。 リリースノートについて詳しくは、[Mobile SDK リリースノート ](https://developer.adobe.com/client-sdks/home/release-notes/) を参照してください。 |
| Mobile SDK | プライバシー | Appleのポリシーが更新されたので、2024 年 5 月 1 日（PT）以降、開発者はApp Storeに送信するために新しいプライバシー機能を実装する必要があります。 Mobile SDKを使用するすべてのAdobe ユーザーは、5 月 1 日以降にApp Storeの承認を受けることを希望する場合は、SDKのバージョン 5.x にアップグレードする必要があります。 |
| Roku SDK | Roku SDK | Roku SDKの最初のメジャーバージョンがリリースされ、Experience Platform Edge Networkのストリーミングメディアがサポートされるようになりました。 |
| タグとイベント転送 | 製品内ガイダンス | Experience Platform[ タグ ](../../tags/home.md) および [ イベント転送 ](../../tags/ui/event-forwarding/overview.md) には、すぐに使い始めて価値をすばやく実感するのに役立つ、新しい範囲のエクスペリエンスが用意されています。 これらのエクスペリエンスには、新しいオンボーディング画面、製品内チュートリアル、ツールヒントが含まれます。 <br>![ 製品内ガイダンスがハイライト表示されたイベント転送。](../2024/assets/april/event-forwarding.png " 「タイプ」フィールドと「値タイプをマップ」フィールドがハイライト表示されたスキーマエディター。"){width="100" zoomable="yes"}<br> |
| Web SDK | Audience Managerのお客様向けの Web SDKの導入のシンプル化 | 複数の Web SDKの更新により、Audience Manager、Analytics、Target などのExperience Cloud ソリューションにエクスペリエンスデータモデル（XDM）を使用しなくても、Web SDKの導入を簡単に行えるようになりました。 Audience Manager web SDKの導入について詳しくは、次のガイドを参照してください。 <ul><li><a href="https://experienceleague.adobe.com/en/docs/audience-manager/user-guide/migrate-to-web-sdk/dil-extension-to-web-sdk">Audience Manager タグ拡張機能から Web SDK タグ拡張機能にAudience Managerのデータ収集ライブラリを更新します</li><li><a href="https://experienceleague.adobe.com/en/docs/audience-manager/user-guide/migrate-to-web-sdk/appmeasurement-to-web-sdk">Audience Managerのデータ収集ライブラリをAppMeasurement JavaScript ライブラリから Web SDK JavaScript ライブラリに更新する</li></ul> |

{style="table-layout:auto"}

<!--| Web SDK | [Streaming Media Collection support in Web SDK](/help/collection/js/commands/configure/streamingmedia.md) | You can now use Experience Platform Web SDK to collect data related to media sessions on your website. The collected data can include information about media playbacks, pauses, completions, and other related events. Once collected, you can send this data to Adobe Experience Platform and/or Adobe Analytics, to generate reports. This feature provides a comprehensive solution for tracking and understanding media consumption behavior on your website. <br>See the [Web SDK](/help/collection/js/commands/configure/streamingmedia.md) documentation to learn how to configure the `streamingMedia` component. <br>See the guide on [migrating your Analytics for Streaming Media implementation from Media JS to Web SDK](https://experienceleague.adobe.com/en/docs/media-analytics/using/implementation/edge-recommended/media-edge-sdk/edge-web-sdk) for more details.|-->

データ収集について詳しくは、[ データ収集の概要 ](../../collection/home.md) を参照してください。

## 宛先 {#destinations}

[!DNL Destinations] は、Adobe Experience Platform からのデータの円滑なアクティベーションを可能にする、事前定義済みの出力先プラットフォームとの統合です。宛先を使用して、クロスチャネルマーケティングキャンペーン、メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

**新機能または更新された機能** {#destinations-new-updated-functionality}

| 機能 | 説明 |
| ----------- | ----------- |
| Destination SDK`isRequired` ネストされた顧客データフィールドでパラメーターを使用できるようになりました | Destination SDKで宛先を設定する際に、[ ネストされた顧客データフィールドを必要に応じて設定 ](/help/destinations/destination-sdk/functionality/destination-configuration/customer-data-fields.md#nested-fields) できるようになりました。 これにより、宛先を設定するユーザーは、そのフィールドの値を選択するまでアクティベーションフローを続行できません。 |
| Edgeのセグメント化は、web SDKでAdobe Targetの宛先を設定する場合に必須の要件ではなくなりました | 以前は、Web SDKを使用して [Adobe Targetの宛先を設定する場合 ](/help/destinations/catalog/personalization/adobe-target-connection.md) パーソナライゼーションとエッジのセグメント化のためにデータストリームを有効にする必要がありました。 データストリームでエッジのセグメント化を有効にする必要がありました [ 現在は削除されました ](/help/destinations/ui/activate-edge-personalization-destinations.md#configure-datastream)。 この統合パターンを使用すると、Real-Time CDPでAdobe Targetを使用する際に、パーソナライゼーションのユースケースのサブセットのメリットを得ることができます。 詳しくは、[ 統合タイプで有効になるユースケース ](/help/destinations/catalog/personalization/adobe-target-connection.md#supported-use-cases) を参照してください。 |
| [!BADGE Beta]{type=Informative} アクティブ化フローから複数のオーディエンスとデータセットを削除します | 宛先アクティブ化フローから複数のオーディエンスとデータセットを選択して削除できるようになりました。 詳しくは、[ 宛先の詳細 ](../../destinations/ui/destination-details-page.md#bulk-remove) および [ データセットの書き出し ](../../destinations/ui/export-datasets.md) ドキュメントを参照してください。 |

{style="table-layout:auto"}

宛先の一般的な情報については、[宛先の概要](../../destinations/home.md)を参照してください。

## ID サービス {#identity-service}

Adobe Experience Platform ID サービスを使用すると、デバイスやシステム間で ID を紐付けることで、顧客とその行動の全体像を把握し、インパクトのある、個人的なデジタルエクスペリエンスをリアルタイムで提供できます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| API の `/orgs/{ORG}/` エンドポイントの廃止 | [[!DNL Identity Service] API](https://developer.adobe.com/experience-platform-apis/references/identity-service/) の次のエンドポイントは非推奨（廃止予定）になりました。<ul><li>`https://platform.adobe.io/data/core/idnamespace/orgs/{ORG}/identities`</li><li>`https://platform.adobe.io/data/core/idnamespace/orgs/{ORG}/identities/{ID}`</li></ul> `/idnamespace/identities` および `/idnamespace/identities/{ID}` エンドポイントを使用して、同じタスクを実行し、組織内のすべての名前空間または組織内の特定の名前空間を取得できます。 |

{style="table-layout:auto"}

ID サービスについて詳しくは、[ID サービスの概要](../../identity-service/home.md)を参照してください。

## 監視 {#monitoring}

Experience Platform UI のモニタリングダッシュボードを使用すると、ソース、ID サービス、リアルタイム顧客プロファイル、オーディエンスおよび宛先からのデータのジャーニーをモニタリングできます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| ダッシュボード拡張の監視 | ビジネスの使用例に基づいて、様々なデータタイプに対して監視ダッシュボードを使用できるようになりました。 監視ダッシュボードを使用して、ソース、オーディエンスおよび宛先の人物、アカウントおよび見込み客のデータタイプアクティビティを監視します。 |

{style="table-layout:auto"}

詳しくは、[ 監視ダッシュボードの使用 ](../../dataflows/ui/monitor.md) に関するガイドを参照してください。

## クエリサービス {#query-service}

クエリサービスを使用すると、標準 SQL を使用して Adobe Experience Platform [!DNL Data Lake] でデータに対してクエリを実行できます。任意のデータセットを [!DNL Data Lake] から結合し、クエリの結果を新しいデータセットとして取得することで、レポートや Data Science Workspace で使用したり、リアルタイム顧客プロファイルに取り込んだりできます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| クエリの隔離 | 失敗したクエリ実行を自動的に分離して中断を防ぎ、一貫したパフォーマンスを維持します。 詳しくは、[ クエリ強制隔離 ](../../query-service/ui/query-schedules.md#quarantine) のドキュメントを参照してください。 |
| クエリをキャンセル | クエリの実行を制御し、長時間実行されるクエリをキャンセルして生産性を向上させます。詳しくは、[ クエリのキャンセル ](../../query-service/ui/user-guide.md#cancel-query) のドキュメントを参照してください。 |
| スケジュールされたクエリのアラート | クエリをスケジュールしながら、プロアクティブな通知で常に情報を入手し、効率的でタイムリーなタスク管理を確保します。 [ クエリの作成時に、または既存のスケジュール済みクエリに対してインラインアクションを使用して ](../../query-service/ui/query-schedules.md#alerts-for-query-status) アラートの配信を登録できます。 詳しくは、[ インラインアクションを使用したアラートの購読 ](../../query-service/ui/monitor-queries.md#alert-subscription) ドキュメントを参照してください。 |
| スケジュール済みクエリナビゲーションの改善 | クエリテンプレートとスケジュールされた実行の間を簡単に移動して、生産性を向上させます。 詳しくは、[ スケジュールされたクエリ実行の表示 ](../../query-service/ui/query-schedules.md#scheduled-query-runs) に関するドキュメントを参照してください。 |
| 拡張クエリの出力 | データをより深く分析するために、コンソール内で最大 500 行のクエリ結果にアクセスします。詳しくは、[result count](../../query-service/ui/user-guide.md#result-count) のドキュメントを参照してください。 |
| レガシ クエリ エディターの廃止 | 2024 年 4 月 30 日（PT）現在、拡張クエリエディターは、すべてのユーザーにとってデフォルトのエディターになっています。 レガシーエディターは 2024 年 5 月 24 日（PT）に廃止され、使用できなくなります。 詳しくは、[ クエリエディターユーザーガイド ](../../query-service/ui/user-guide.md) を参照してください。 |

{style="table-layout:auto"}

クエリサービスについて詳しくは、[クエリサービスの概要](../../query-service/home.md)を参照してください。

## サンドボックス {#sandboxes}

Adobe Experience Platform は、デジタルエクスペリエンスアプリケーションをグローバルな規模で強化するように設計されています。企業ではしばしば複数のデジタルエクスペリエンスアプリケーションを並行して運用し、運用コンプライアンスを確保しながら、アプリケーションの開発、テスト、導入に注力する必要があります。このニーズに対応するために、Experience Platformには、1 つのExperience Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つサンドボックスが用意されています。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| [サンドボックスツール](../../sandboxes/ui/sandbox-tooling.md) | サンドボックスツールを使用して、サポートされているすべてのオブジェクトタイプを完全なサンドボックスパッケージに [ 書き出し ](../../sandboxes/ui/sandbox-tooling.md#export-entire-sandbox) したあと、様々なサンドボックスをまたいでパッケージを [ 読み込み ](../../sandboxes/ui/sandbox-tooling.md#import-entire-sandbox) して、オブジェクト設定をレプリケートします。 |

{style="table-layout:auto"}

サンドボックスについて詳しくは、[ サンドボックスの概要 ](../../sandboxes/home.md) を参照してください。

## セグメント化サービス {#segmentation}

[!DNL Segmentation Service] を使用すると、[!DNL Experience Platform] に保存されている、個人（顧客、見込み客、ユーザー、組織など）に関連するデータをオーディエンスにセグメント化できます。オーディエンスは、セグメント定義または [!DNL Real-Time Customer Profile] データの他のソースを通じて作成できます。これらのオーディエンスは [!DNL Experience Platform] で一元的に設定および管理されており、Adobe ソリューションから簡単にアクセスできます。

**更新された機能**

| 機能 | 説明 |
| ------- | ----------- |
| オーディエンスのライフサイクルの状態 | オーディエンスのライフサイクル状態を合理化して、ライフサイクル管理を簡素化しました。 これらのライフサイクル状態について詳しくは、[ セグメント化サービスに関する FAQ](../../segmentation/faq.md#lifecycle-states) を参照してください。 |

{style="table-layout:auto"}

[!DNL Segmentation Service] について詳しくは、[セグメント化の概要](../../segmentation/home.md)を参照してください。

## ソース {#sources}

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

Experience Platform のソースを使用して、Adobe アプリケーションまたはサードパーティのデータソースからデータを取り込みます。

**新しいソース**

| 新しいソース | 説明 |
| --- | --- |
| [!BADGE Beta]{type=Informative} [!DNL PathFactory] | [[!DNL PathFactory]  ソース ](../../sources/tutorials/ui/create/marketing-automation/pathfactory.md) を使用して、[!DNL PathFactory] からExperience Platformへの訪問者、セッション、ページビューのデータを統合します。 開始方法について詳しくは、[[!DNL PathFactory]  概要 ](../../sources/connectors/marketing-automation/pathfactory.md) を参照してください。 |
| [!DNL Teradata Vantage] | [[!DNL Teradata Vantage]  ソース ](../../sources/tutorials/ui/create/databases/teradata-vantage.md) を使用すると、ハイブリッドマルチクラウド環境からExperience Platformにデータを取り込むことができます。 開始方法について詳しくは、[[!DNL Teradata Vantage]  概要 ](../../sources/connectors/databases/teradata-vantage.md) を参照してください。 |

{style="table-layout:auto"}

**新機能および更新された機能**

| 機能 | 説明 |
| --- | --- |
| VA7 での許可リストへの登録用 IP アドレスの更新 | VA7 （北米）の許可リストに追加される IP アドレスのリストに次の IP アドレスが追加されました。 <ul><li>`20.98.198.224/29`</li><li>`20.119.28.57/32`</li><li>`20.232.89.104/29`</li><li>`20.98.195.172/32`</li><li>`172.210.218.144/28`</li></ul> 許可リストに追加する IP アドレスの包括的なリストについては、[IP アドレスの許可リストに関するドキュメント ](../../sources/ip-address-allow-list.md) を参照してください。 |
| [!DNL Azure Event Hubs] ソースを使用した新しい認証タイプのサポート | [!DNL Event Hubs] または [!DNL Azure Active Directory Authentication] を使用して、[!DNL Scoped Azure Active Directory Authentication] ソースをExperience Platformに接続できるようになりました。 詳しくは、[Experience Platformへの接続  [!DNL Event Hubs]  に関するガイド ](../../sources/tutorials/ui/create/cloud-storage/eventhub.md) を参照してください。 |
| 資格情報の取得 [!DNL Data Landing Zone] 更新 | ソースワークスペースの右側のパネルを使用して、[!DNL Data Landing Zone] 資格情報を取得できるようになりました。 また、右側のパネルを使用して資格情報を更新できるようになりました。 詳しくは、[[!DNL Data Landing Zone] UI ガイド ](../../sources/tutorials/ui/create/cloud-storage/data-landing-zone.md) を参照してください。 |

{style="table-layout:auto"}

<!--| Enhanced filtering and navigation in the sources UI workspace | Use the enhanced filtering, search, and inline action tools in the sources UI workspace to streamline your workflow. <ul><li>Use filtering and search capabilities to navigate your way through sources accounts and dataflows in your organization.</li><li>Use inline actions to modify configuration settings applied to your dataflows and improve organizational workflows. You can use inline actions to apply tags, set up alerts, or create ingestion jobs on demand.</li></ul> For more information, read the guide on [filtering sources objects in the UI](../../sources/tutorials/ui/filter.md).|-->

ソースについて詳しくは、[ ソースの概要 ](../../sources/home.md) を参照してください。
