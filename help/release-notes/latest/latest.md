---
title: Adobe Experience Platform リリースノート
description: Adobe Experience Platform の 2024年3月のリリースノート。
exl-id: f854f9e5-71be-4d56-a598-cfeb036716cb
source-git-commit: 6ad7d55ca0a544879db9738c0a4ab914fdc363bd
workflow-type: tm+mt
source-wordcount: '1726'
ht-degree: 24%

---

# Adobe Experience Platform リリースノート

**リリース日：2024年4月30日**

>[!TIP]
>
>の使用 [Adobe Experience Platform用語集](/help/landing/glossary.md) Real-time Customer Data PlatformとAdobe Experience Platformで使用される用語を理解します。 探している用語が見つからない場合は、ページのフィードバックオプションを使用して、用語集に新しい用語を追加するようにリクエストします。

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

Adobe Experience Platform では、毎日のスナップショットで得られた、組織のデータに関する重要なインサイトを表示できる複数のダッシュボードを提供しています。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| Real-time Customer Data Platform B2B インサイト | データを理解し、ビジネス上の意思決定に役立つ、アカウントとオポチュニティに関する事前設定済みのReal-Time CDP B2B データインサイトを調べます。 また、Real-Time CDP B2B データモデルを使用して独自のインサイトを作成し、データを視覚化して調査し、ダッシュボードにカスタムビジュアライゼーションを保存することもできます。 |

{style=“table-layout:auto”}

アクセス権限の付与方法やカスタムウィジェットの作成方法など、ダッシュボードの詳細については、まず[ダッシュボードの概要](../../dashboards/home.md)を参照してください。

## データ収集 {#data-collection}

Adobe Experience Platformは、クライアントサイドのカスタマーエクスペリエンスデータを収集し、Experience PlatformEdge Networkに送信します。そこでデータを強化、変換、AdobeまたはAdobe以外の宛先への配信を可能にする一連のテクノロジーを提供します。

**新機能または更新された機能**

| タイプ | 機能 | 説明 |
| --- | --- | --- |
| Insights | [!DNL Acxiom] 匿名訪問者インサイト | Web サイトの訪問者のソースを検出 [!DNL Acxiom's] 訪問者インサイト。 ジオ IP ルックアップテクノロジーを利用して、匿名ブラウザーの場所を特定します。 特定されたら、整理されたデータベース内で素早く検索すると、追加のインサイトが得られ、ブラウザーに送り返されます。 つまり、コンテンツ作成者にとっては、これらのデータポイントに合わせてコンテンツを調整する絶好の機会であり、見知らぬ人として出発した訪問者であっても、訪問者によりパーソナライズされた魅力的なエクスペリエンスを提供できます。 |
| データストリーム | [Edge Networkボットの検出](../../datastreams/bot-detection.md) | 自動プログラム、Web スクレーパー、スパイダー、スクリプト化されたスキャナーなど、人間以外のエンティティから発生するトラフィックによって、人間の訪問者から発生するイベントの特定がより困難になる場合があります。 このタイプのトラフィックは、重要なビジネス指標に悪影響を与え、誤ったトラフィックレポートにつながる可能性があります。 <br>ボット検出を使用すると、によって生成されたイベントを識別できます。 [Web SDK](../../web-sdk/home.md), [Mobile SDK](https://developer.adobe.com/client-sdks/home/) および [[!DNL Server API]](../../server-api/overview.md) 既知のクモやボットによって生成されるものです。 データストリームのボット検出を設定することで、ボットイベントとして分類したい特定の IP アドレス、IP 範囲およびリクエストヘッダーを識別できます。 <br> ボットトラフィックを識別することで、サイトまたはモバイルアプリケーションのユーザーアクティビティをより正確に測定できます。 |
| Mobile SDK | メジャーバージョンリリース | iOS Mobile Core 5.x と互換性のあるiOS拡張機能、Android Mobile Core 3.x と互換性のある Android 拡張機能、React Native Core 6.x と互換性のある React Native 拡張機能、Flutter Core 4.x と互換性のある Flutter 拡張機能など、Mobile SDK の新しいメジャーバージョンがリリースされました。 これらのリリースでは、Android SDK for Jetpack Compose のサポート、Adobe Journey Optimizer コードベースのエクスペリエンスのサポート、Flutter 向けAdobe Journey Optimizer Messaging 拡張機能の一般提供など、いくつかの新機能と機能強化が提供されています。 詳しくは、リリースノートを参照してください [Mobile SDK リリースノート](https://developer.adobe.com/client-sdks/home/release-notes/). |
| Mobile SDK | プライバシー | Appleのポリシーが更新されたので、2024 年 5 月 1 日（PT）以降、開発者はApp Storeに送信するために新しいプライバシー機能を実装する必要があります。 Mobile SDK を使用するすべてのAdobeのお客様は、5 月 1 日以降にApp Storeの承認を受けることを希望する場合、SDK のバージョン 5.x にアップグレードする必要があります。 |
| Roku SDK | Roku SDK | Roku SDK の最初のメジャーバージョンがリリースされ、Platform Edge Networkの Streaming Media がサポートされるようになりました。 |
| タグとイベント転送 | 製品内ガイダンス | Experience Platform [タグ](../../tags/home.md) および [イベントの転送](../../tags/ui/event-forwarding/overview.md) すぐに使い始めて、価値を生み出すまでの時間を短縮するのに役立つ、新しいエクスペリエンス範囲を提供します。 これらのエクスペリエンスには、新しいオンボーディング画面、製品内チュートリアル、ツールヒントが含まれます。 <br>![製品内ガイダンスがハイライト表示されたイベント転送。](../2024/assets/april/event-forwarding.png "タイプおよびマップ値タイプのフィールドがハイライト表示されたスキーマエディター。"){width="100" zoomable="yes"}<br> |
| Web SDK | Audience Manager版のお客様向けの Web SDK のシンプルな採用 | 複数の Web SDK の更新により、Audience Manager、Analytics、Target などのExperience Cloudソリューションにエクスペリエンスデータモデル（XDM）を使用しなくても、Web SDK の導入を簡単に行えるようになりました。 Audience Manager Web SDK の採用について詳しくは、次のガイドを参照してください。 <ul><li><a href="https://experienceleague.adobe.com/en/docs/audience-manager/user-guide/migrate-to-web-sdk/dil-extension-to-web-sdk">Audience Manager用にデータ収集ライブラリをAudience Managerタグ拡張機能から Web SDK タグ拡張機能に更新します</li><li><a href="https://experienceleague.adobe.com/en/docs/audience-manager/user-guide/migrate-to-web-sdk/appmeasurement-to-web-sdk">Audience Manager用にデータ収集ライブラリをAppMeasurement JavaScript ライブラリから Web SDK JavaScript ライブラリに更新します</li></ul> |

{style="table-layout:auto"}

<!--| Web SDK | [Streaming Media Collection support in Web SDK](../../web-sdk/commands/configure/streamingmedia.md) | You can now use Experience Platform Web SDK to collect data related to media sessions on your website. The collected data can include information about media playbacks, pauses, completions, and other related events. Once collected, you can send this data to Adobe Experience Platform and/or Adobe Analytics, to generate reports. This feature provides a comprehensive solution for tracking and understanding media consumption behavior on your website. <br>See the [Web SDK](../../web-sdk/commands/configure/streamingmedia.md) documentation to learn how to configure the `streamingMedia` component. <br>See the guide on [migrating your Analytics for Streaming Media implementation from Media JS to Web SDK](https://experienceleague.adobe.com/en/docs/media-analytics/using/implementation/edge-recommended/media-edge-sdk/edge-web-sdk) for more details.|-->

データ収集について詳しくは、 [データ収集の概要](../../collection/home.md).

## 宛先 {#destinations}

[!DNL Destinations] は、Adobe Experience Platform からのデータの円滑なアクティベーションを可能にする、事前定義済みの出力先プラットフォームとの統合です。宛先を使用して、クロスチャネルマーケティングキャンペーン、メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

**新機能または更新された機能** {#destinations-new-updated-functionality}

| 機能 | 説明 |
| ----------- | ----------- |
| `isRequired` Destination SDKのネストされた顧客データフィールドでパラメーターを使用できるようになりました | Destination SDKで宛先を設定する際に、以下を行えるようになりました [ネストされた顧客データフィールドを必要に応じて設定する](/help/destinations/destination-sdk/functionality/destination-configuration/customer-data-fields.md#nested-fields). これにより、宛先を設定するユーザーは、そのフィールドの値を選択するまでアクティベーションフローを続行できません。 |

{style="table-layout:auto"}

宛先の一般的な情報については、[宛先の概要](../../destinations/home.md)を参照してください。

<!--| [!BADGE Beta]{type=Informative} Remove multiple audiences and datasets from activation flows | You can now select and remove multiple audiences and datasets from destination activation flows. See the [destination details](../../destinations/ui/destination-details-page.md#bulk-remove) and [dataset export](../../destinations/ui/export-datasets.md) documentation for more details. |-->

## ID サービス {#identity-service}

Adobe Experience Platform ID サービスを使用すると、デバイスやシステム間で ID を橋渡しすることで、顧客とその行動を包括的に把握し、インパクトのある、パーソナライズされたデジタル体験をリアルタイムで提供できます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| の廃止 `/orgs/{ORG}/` api のエンドポイント | の次のエンドポイント [[!DNL Identity Service] API](https://developer.adobe.com/experience-platform-apis/references/identity-service/) 非推奨（廃止予定）になりました。<ul><li>`https://platform.adobe.io/data/core/idnamespace/orgs/{ORG}/identities`</li><li>`https://platform.adobe.io/data/core/idnamespace/orgs/{ORG}/identities/{ID}`</li></ul> 次を使用できます `/idnamespace/identities` および `/idnamespace/identities/{ID}` 同じタスクを実行し、組織内のすべての名前空間または組織内の特定の名前空間のいずれかを取得するエンドポイント。 |

{style="table-layout:auto"}

ID サービスについて詳しくは、[ID サービスの概要](../../identity-service/home.md)を参照してください。

## 監視 {#monitoring}

Experience PlatformUI のモニタリングダッシュボードを使用すると、ソース、ID サービス、リアルタイム顧客プロファイル、オーディエンスおよび宛先からのデータのジャーニーをモニタリングできます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| ダッシュボード拡張の監視 | ビジネスの使用例に基づいて、様々なデータタイプに対して監視ダッシュボードを使用できるようになりました。 監視ダッシュボードを使用して、ソース、オーディエンスおよび宛先の人物、アカウントおよび見込み客のデータタイプアクティビティを監視します。 |

{style="table-layout:auto"}

詳しくは、のガイドを参照してください。 [監視ダッシュボードの使用](../../dataflows/ui/monitor.md).

## クエリサービス {#query-service}

クエリサービスを使用すると、標準 SQL を使用して Adobe Experience Platform [!DNL Data Lake] でデータに対してクエリを実行できます。任意のデータセットを [!DNL Data Lake] から結合し、クエリの結果を新しいデータセットとして取得することで、レポートや Data Science Workspace で使用したり、リアルタイム顧客プロファイルに取り込んだりできます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| クエリの強制隔離 | 失敗したクエリ実行を自動的に分離して中断を防ぎ、一貫したパフォーマンスを維持します。 |
| クエリをキャンセル | 長時間実行されるクエリをキャンセルすることで、クエリの実行を制御し、生産性を向上させます。 |
| スケジュールされたクエリのアラート | クエリをスケジュールしながら、プロアクティブな通知で常に情報を入手し、効率的でタイムリーなタスク管理を確保します。 クエリの作成時、または既存のスケジュール済みクエリに対するインラインアクションを使用して、アラートの配信を登録できます。 |
| スケジュール済みクエリナビゲーションの改善 | クエリテンプレートとスケジュールされた実行の間を簡単に移動して、生産性を向上させます。 |
| 拡張クエリの出力 | コンソール内で最大 500 行のクエリ結果にアクセスして、データをより深く分析できます。 |
| レガシ クエリ エディターの廃止 | 2024 年 4 月 30 日（PT）現在、拡張クエリエディターは、すべてのユーザーにとってデフォルトのエディターになっています。 レガシーエディターは 2024 年 5 月 30 日（PT）に非推奨（廃止予定）となり、使用できなくなります。 |

{style=“table-layout:auto”}

クエリサービスについて詳しくは、[クエリサービスの概要](../../query-service/home.md)を参照してください。

## サンドボックス {#sandboxes}

Adobe Experience Platform は、デジタルエクスペリエンスアプリケーションをグローバルな規模で強化するように設計されています。企業ではしばしば複数のデジタルエクスペリエンスアプリケーションを並行して運用し、運用コンプライアンスを確保しながら、アプリケーションの開発、テスト、導入に注力する必要があります。このニーズに対応するために、Experience Platformでは、1 つの Platform インスタンスを別々の仮想環境に分割するサンドボックスを提供し、デジタルエクスペリエンスアプリケーションの開発と発展を支援しています。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| [サンドボックスツール](../../sandboxes/ui/sandbox-tooling.md) | サンドボックスツールを使用して次の操作を行います [export](../../sandboxes/ui/sandbox-tooling.md#export-entire-sandbox) サポートされるすべてのオブジェクトタイプを完全なサンドボックスパッケージにし、 [import](../../sandboxes/ui/sandbox-tooling.md#import-entire-sandbox) オブジェクト設定をレプリケートするための様々なサンドボックスをまたいだパッケージ。 |

{style="table-layout:auto"}

サンドボックスについて詳しくは、を参照してください。 [サンドボックスの概要](../../sandboxes/home.md).

## セグメント化サービス {#segmentation}

[!DNL Segmentation Service] を使用すると、[!DNL Experience Platform] に保存されている、個人（顧客、見込み客、ユーザー、組織など）に関連するデータをオーディエンスにセグメント化できます。オーディエンスは、セグメント定義または [!DNL Real-Time Customer Profile] データの他のソースを通じて作成できます。これらのオーディエンスは [!DNL Platform] で一元的に設定および管理されており、Adobe ソリューションから簡単にアクセスできます。

**更新された機能**

| 機能 | 説明 |
| ------- | ----------- |
| オーディエンスのライフサイクルの状態 | オーディエンスのライフサイクル状態を合理化して、ライフサイクル管理を簡素化しました。 ライフサイクル ステータスの詳細については、を参照してください。 [セグメント化サービスに関する FAQ](../../segmentation/faq.md#lifecycle-states). |

{style="table-layout:auto"}

[!DNL Segmentation Service] について詳しくは、[セグメント化の概要](../../segmentation/home.md)を参照してください。

## ソース {#sources}

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

Experience Platformのソースを使用すると、Adobeアプリケーションまたはサードパーティのデータソースからデータを取り込むことができます。

**新しいソース**

| 新しいソース | 説明 |
| --- | --- |
| [!BADGE ベータ版]{type=Informative} [!DNL PathFactory] | の使用 [[!DNL PathFactory] ソース](../../sources/tutorials/ui/create/marketing-automation/pathfactory.md) からの訪問者、セッション、ページビューのデータを統合するには [!DNL PathFactory] をExperience Platformにします。 を読み取る [[!DNL PathFactory] の概要](../../sources/connectors/marketing-automation/pathfactory.md) を開始する方法について説明します。 |
| [!DNL Teradata Vantage] | の使用 [[!DNL Teradata Vantage] ソース](../../sources/tutorials/ui/create/databases/teradata-vantage.md) ハイブリッドマルチクラウド環境からExperience Platformにデータを取り込む。 を読み取る [[!DNL Teradata Vantage] の概要](../../sources/connectors/databases/teradata-vantage.md) を開始する方法について説明します。 |

{style="table-layout:auto"}

**新機能と更新された機能**

| 機能 | 説明 |
| --- | --- |
| VA7 での許可リストへの登録用 IP アドレスの更新 | VA7 （北米）の許可リストに追加される IP アドレスのリストに次の IP アドレスが追加されました。 <ul><li>`20.98.198.224/29`</li><li>`20.119.28.57/32`</li><li>`20.232.89.104/29`</li><li>`20.98.195.172/32`</li><li>`172.210.218.144/28`</li></ul> 許可リストに追加する IP アドレスの包括的なリストについては、 [IP アドレス許可リストドキュメント](../../sources/ip-address-allow-list.md). |
| での新しい認証タイプのサポート [!DNL Azure Event Hubs] ソース | を接続できるようになりました [!DNL Event Hubs] 次のいずれかを使用してソースからExperience Platformへ [!DNL Azure Active Directory Authentication] または [!DNL Scoped Azure Active Directory Authentication]. ガイドを読む [接続 [!DNL Event Hubs] Experience Platformへ](../../sources/tutorials/ui/create/cloud-storage/eventhub.md) を参照してください。 |
| 更新先 [!DNL Data Landing Zone] 資格情報の取得 | ソースワークスペースの適切なパネルを使用して、 [!DNL Data Landing Zone] 資格情報。 また、右側のパネルを使用して資格情報を更新できるようになりました。 を読み取る [[!DNL Data Landing Zone] UI ガイド](../../sources/tutorials/ui/create/cloud-storage/data-landing-zone.md) を参照してください。 |

{style="table-layout:auto"}

<!--| Enhanced filtering and navigation in the sources UI workspace | Use the enhanced filtering, search, and inline action tools in the sources UI workspace to streamline your workflow. <ul><li>Use filtering and search capabilities to navigate your way through sources accounts and dataflows in your organization.</li><li>Use inline actions to modify configuration settings applied to your dataflows and improve organizational workflows. You can use inline actions to apply tags, set up alerts, or create ingestion jobs on demand.</li></ul> For more information, read the guide on [filtering sources objects in the UI](../../sources/tutorials/ui/filter.md).|-->

ソースについて詳しくは、 [ソースの概要](../../sources/home.md).
