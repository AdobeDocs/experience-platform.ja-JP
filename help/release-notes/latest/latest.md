---
title: Adobe Experience Platform リリースノート（2025年2月）
description: Adobe Experience Platform の 2025年2月のリリースノートです。
exl-id: f854f9e5-71be-4d56-a598-cfeb036716cb
source-git-commit: c4064771a384a90d94903ba1761fc9ee20f47747
workflow-type: tm+mt
source-wordcount: '1645'
ht-degree: 16%

---

# Adobe Experience Platform リリースノート

>[!TIP]
>
>このリリースには、Federated Audience Composition アドオンの改善が含まれています。 詳しくは、[Federated Audience Composition リリースノート ](https://experienceleague.adobe.com/en/docs/federated-audience-composition/using/release-notes) を参照してください。

**リリース日：2025年2月18日（PT）**

Adobe Experience Platformの既存の機能およびドキュメントのアップデート：

- [AI アシスタント](#ai-assistant)
- [カタログサービス](#catalog-service)
- [データ準備](#data-prep)
- [宛先](#destinations)
- [ソース](#sources)
- [セグメント化サービス](#segmentation)
- [ドキュメントの更新](#documentation-updates)
   - [Edgeのネットワークとハブの比較](#edge)
   - [ソース用の拡張フローサービス API](#flow-service)
   - [サンドボックスツールを使用したオブジェクト設定のバックアップ](#back-up-object-configurations)
   - [サンドボックスツールを使用した中核的な拠点の実現](#center-of-excellence)
   - [データレイクでのエクスペリエンスイベントデータセットの保持](#experience-event-dataset-retention)

## AI アシスタント {#ai-assistant}

Adobe Experience Platformの AI アシスタントは、Adobe アプリケーションでのワークフローの高速化に使用できる対話型エクスペリエンスです。 AI アシスタントを使用すると、製品の知識をより深く理解したり、問題をトラブルシューティングしたり、情報を検索して運用インサイトを見つけたりできます。 AI アシスタントは、Experience Platform、Real-Time Customer Data Platform、Adobe Journey Optimizer、Customer Journey Analyticsをサポートします。

**新機能**

| 機能 | 説明 |
| --- | --- |
| 運用インサイトの一般提供 | AI アシスタントの運用インサイトが一般提供（GA）されるようになりました。 運用インサイトとは、カウント、ルックアップ、系列の影響を含む、メタデータオブジェクト（属性、オーディエンス、データフロー、データセット、宛先、ジャーニー、スキーマ、ソース）に関して AI Assistant が生成する回答を指します。 オペレーショナルインサイトでは、サンドボックス内のデータは参照されません。 詳しくは、[AI アシスタント UI ガイド ](../../ai-assistant/ui-guide.md) を参照してください。 |
| 質問のオートコンプリートのサポート | AI アシスタントに質問を入力する際に、AI アシスタントが提供するおすすめの質問のリストから選択できるようになりました。 この機能を使用すると、AI アシスタントでワークフローをさらに高速化できます。 詳しくは、[AI アシスタントで質問のオートコンプリートを使用する ](../../ai-assistant/ui-guide.md#use-question-autocomplete) に関するガイドを参照してください。 |
| データセットの可観測性のサポート | AI アシスタントを使用して、ストレージサイズや行数など、特定のデータセット指標に関する質問に回答できるようになりました。 データ観察性の質問は、特定の期間でクエリをフィルタリングするために使用できる修飾子をサポートします。 詳しくは、「[AI アシスタントの質問ガイド ](../../ai-assistant/questions.md)」を参照してください。 |

{style="table-layout:auto"}

詳しくは、[AI アシスタントの概要 ](../../ai-assistant/home.md) を参照してください。

## カタログサービス {#catalog-service}

カタログサービスは、Adobe Experience Platform 内のデータの場所と系列のレコードのシステムです。Experience Platformに取り込まれるすべてのデータはファイルおよびディレクトリとして Data Lake に保存されますが、カタログには、参照や監視のために、これらのファイルおよびディレクトリのメタデータと説明が保持されます。

| 機能 | 説明 |
| --- | --- |
| 新しい API エンドポイント | 新しい [Catalog Service API /v2/dataSets/{DATASET_ID} endpoint](../../catalog/api/update-object.md#patch-v2-notation) を使用して、Adobe Experience Platform データセットのメタデータをより効率的に管理できます。 システムが欠落しているパスレベルを自動的に作成して、時間を節約し、手動の手順を減らし、エラーを最小限に抑えるため、ネストの深い複雑なデータセット属性を簡単に更新できます。 |

{style="table-layout:auto"}

カタログサービスについて詳しくは、[ カタログサービスの概要 ](../../catalog/home.md) を参照してください。

## データ準備 {#data-prep}

データ準備を使用して、エクスペリエンスデータモデル（XDM）との間でデータのマッピング、変換、検証を行う。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| マッピングのインポートおよびエクスポートのサポートの強化 | マッピングを CSV ファイルに書き出し、スプレッドシートでローカルに設定できるようになりました。 その後、UI のマッピングインターフェイスを使用して、更新したマッピングをExperience Platformに読み込むことができます。 この機能を使用すると、UI で手動で作成しなくても、多数のマッピングを設定できます。 さらに、新しいデータフローを作成する際に、マッピングのコピーをExperience Platformに直接アップロードすると、ワークフローを高速化できます。 詳しくは、[ マッピングのインポートとエクスポート ](../../data-prep/ui/mapping.md#import-mapping) に関するガイドを参照してください。 |

{style="table-layout:auto"}

詳しくは、[ データ準備の概要 ](../../data-prep/home.md) を参照してください。

## 宛先（更新日：2 月 20 日（PT）） {#destinations}

[!DNL Destinations] は、Adobe Experience Platform からのデータの円滑なアクティベーションを可能にする、事前定義済みの出力先プラットフォームとの統合です。宛先を使用して、クロスチャネルマーケティングキャンペーン、メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

**新規宛先または更新された宛先** {#new-updated-destinations}

| 宛先 | 説明 |
| --- | --- |
| [ （Beta）Marketo Engage Person Sync](/help/destinations/catalog/adobe/marketo-engage-person-sync.md) | [!DNL Marketo Engage Person Sync] コネクタを使用して、人物オーディエンスから [!DNL Marketo Engage] インスタンス内の対応するレコードに更新をストリーミングします。 この宛先コネクタはベータ版で、一部のお客様のみご利用いただけます。 アクセス権をリクエストするには、Adobe担当者にお問い合わせください。 |
| [The Trade Desk CRM 接続 ](/help/destinations/catalog/advertising/tradedesk-emails.md) 一般公開 | 接続 [!DNL The Trade Desk CRM] 一般公開されました。 CRM 宛先 [!DNL The Trade Desk] 使用して、CRM データに基づくオーディエンスのターゲティングおよび抑制のために、プロファイルを [!DNL Trade Desk] アカウントにアクティベートします。 |
| [RainFocus Attendee プロファイル接続 ](/help/destinations/catalog/marketing-automation/rainfocus.md) | [!DNL RainFocus Attendee Profiles] の宛先を使用して、顧客プロファイルをAdobe Experience Platformから [!DNL RainFocus] platform にストリーミングし、出席者プロファイルを作成および更新します。 |
| [Criteo 接続 ](/help/destinations/catalog/advertising/criteo.md) 一般公開 | [!DNL Criteo] 接続が一般公開されました。 Criteo は、信頼性の高い効果的な広告を強化し、オープンインターネットを通じてすべての消費者により豊かなエクスペリエンスを提供します。 世界最大のコマースデータセットとクラス最高の AI により、Criteo はショッピングジャーニー全体の各タッチポイントが適切な広告で適切なタイミングで顧客に届くようにパーソナライズされるようにします。 |
| [[!DNL Amazon Ads] 接続](../../destinations/catalog/advertising/amazon-ads.md) | [!DNL Amazon Ads] コネクタ（以前はベータ版）が一般公開されました。 また、コネクタは、広告に個人データを使用することに同意したすべてのプロファイルに対して同意付与のシグナルを送信するように更新されています。 新しい [Amazon広告の同意シグナル ](../../destinations/catalog/advertising/amazon-ads.md#destination-details) コントロールの詳細をご覧ください。 |

{style="table-layout:auto"}

**新機能または更新された機能** {#destinations-new-updated-functionality}

| 機能 | 説明 |
| --- | --- |
| アクセスラベルを使用した宛先データフローへのユーザーアクセスの管理 | Real-Time CDPの [[!UICONTROL  属性ベースのアクセス制御 ]](/help/access-control/abac/overview.md) 機能の一部として、[ 宛先データフロー ](/help/dataflows/ui/monitor-destinations.md) にアクセスラベルを適用できるようになりました。 この方法により、組織内のユーザーのサブセットのみが特定の宛先データフローにアクセスできるようになります。<br> **重要**:Experience Platform ユーザーインターフェイスの上部にある検索ボックスを使用して宛先データフローを検索する場合、結果には、ユーザーアクセスラベルに表示が制限されている宛先データフローが含まれる場合があります。 この動作は、今後のアップデートで修正される予定です。 |
| [2}Marketo Engage接続の ](/help/dataflows/ui/monitor-destinations.md#audience-level-dataflow-runs-for-streaming-destinations) オーディエンスレベルのレポート ](/help/destinations/catalog/adobe/marketo-engage.md)[ | この宛先のデータフローに含まれる各オーディエンスに対して、オーディエンスレベルで分類されたアクティブ化された ID、除外された ID、失敗した ID に関する [ 情報を表示 ](/help/dataflows/ui/monitor-destinations.md#audience-level-dataflow-runs-for-streaming-destinations) できるようになりました。 |
| 外部オーディエンスは、[TikTok](/help/destinations/catalog/social/tiktok.md) および [Snap Inc](/help/destinations/catalog/advertising/snap-inc.md) 接続をサポートしています | [ カスタムアップロード ](../../segmentation/ui/audience-portal.md#import-audience) および [Federated Audience Composition](https://experienceleague.adobe.com/en/docs/federated-audience-composition/using/start/audiences) から、これらの宛先に対して外部オーディエンスをアクティブ化できます。 |
| 配列、マップ、オブジェクトをクラウドストレージの宛先に書き出す | クラウドストレージの宛先に接続する際に、新しい **[!UICONTROL 配列、マップ、オブジェクトの書き出し]** トグルを使用すると、複雑なオブジェクトを新しく書き出して宛先を選択できます。 機能について ](/help/destinations/ui/export-arrays-calculated-fields.md) 詳細を参照 [。 |

{style="table-layout:auto"}

**修正および機能強化** {#destinations-fixes-and-enhancements}

- Destination SDK テストツールの問題が修正されました。 一部のお客様またはパートナーで、[ サンプルプロファイル生成ツール ](/help/destinations/destination-sdk/testing-api/streaming-destinations/sample-profile-generation-api.md) で、プロファイルの生成に使用されるスキーマにデータタイプが含まれている際にサポートされていない形式が原因で問題が発生してい `No format` した。
- Flow Service API を使用して宛先の `targetConnection` 仕様を更新する際の問題が修正されました。 場合によっては、PATCH操作が POST 操作と同様に動作し、既存のデータフローが破損することがあります。 この問題が修正され、すべてのお客様がフローサービス API を使用して `targetConnection` 仕様を更新できるようになりました。 [詳細情報](/help/destinations/api/edit-destination.md#patch-target-connection)。
- ファイルベースの宛先にプロファイルを書き出す場合、重複排除では、複数のプロファイルが同じ重複排除キーと同じ参照タイムスタンプを共有している場合に、1 つのプロファイルのみが書き出されるようにします。 このリリースには重複排除プロセスの更新が含まれており、同じ座標で連続して実行すると、常に同じ結果が得られ、一貫性が向上します。 [詳細情報](/help/destinations/ui/activate-batch-profile-destinations.md#deduplication-same-timestamp)。

詳しくは、[宛先の概要](../../destinations/home.md)を参照してください。

## セグメント化サービス {#segmentation-service}

[!DNL Segmentation Service] は、顧客ベース内のマーケティング可能なユーザーグループを区別する基準を記述することで、プロファイルの特定のサブセットを定義します。セグメントは、レコードデータ（人口統計情報など）や、顧客によるブランドとのやり取りを表す時系列イベントに基づいて作成できます。

**新機能または更新された機能**

| 機能 | 説明 |
| ------- | ----------- |
| 持続的分割 | オーディエンス構成で、永続的な分割がサポートされるようになりました。 ID 名前空間を分割ブロックに追加することで、プロファイル別に分割する際に、分割オーディエンスを一定に保つことができます。 この機能について詳しくは、[ オーディエンス構成ドキュメント ](../../segmentation/ui/audience-composition.md) を参照してください。 |

[!DNL Segmentation Service] について詳しくは、[セグメント化の概要](../../segmentation/home.md)を参照してください。

## ソース {#sources}

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

Experience Platform のソースを使用して、Adobe アプリケーションまたはサードパーティのデータソースからデータを取り込みます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| [!DNL Microsoft Dynamics] でのビューのサポート | [!DNL Microsoft Dynamics] ソースを使用する際に、`"entityType": "view"` を取り込めるようになりました。 詳しくは、[ ソースのExperience Platformへの接続 ](../../sources/tutorials/api/create/crm/ms-dynamics.md) に関するガイドを参照し  [!DNL Microsoft Dynamics]  ください。 |

{style="table-layout:auto"}

詳しくは、[ソースの概要](../../sources/home.md)を参照してください。

## ドキュメントの更新 {#documentation-updates}

### Edge Networkとハブの比較 {#edge}

[Edge Networkとハブの比較 ](../../landing/edge-and-hub-comparison.md) では、Adobe Experience Platform（ハブとEdge Network）の 2 つのサーバータイプの違いを詳しく説明します。例えば、各サーバータイプで使用できるサービス、サーバの場所、各サーバータイプを使用する際の推奨シナリオなどです。

### ソースの拡張フローサービス API リファレンス {#flow-service}

ソースの [[!DNL Flow Service] API リファレンス ](https://developer.adobe.com/experience-platform-apis/references/flow-service/#tag/Source-connections) を更新し、新しい API リクエストと応答の例を追加しました。 独自のソースをExperience Platformに統合する際の接続仕様の作成と更新には、拡張された API リファレンスを使用します。 また、拡張された API 参照を使用して、ソースエンティティの状態遷移の実行、既存のソース接続とターゲット接続の更新、特定のフィルタリング条件が指定されたフローとフロー仕様の取得を行うこともできます。

### サンドボックスツールを使用したオブジェクト設定のバックアップ {#back-up-object-configurations}

サンドボックスツールを使用してバックアップパッケージを作成し、オブジェクト設定が確実に保存および保護される手順について詳しくは、[ オブジェクト設定のバックアップガイド ](../../sandboxes/use-cases/backup-object-configuration.md) を参照してください。

### サンドボックスツールを使用した中核的な拠点の実現 {#center-of-excellence}

主要な設定を効率的に共有するための中核拠点として機能する「ゴールデンサンドボックス」パッケージの作成手順については、[ 中核拠点ガイド ](../../sandboxes/use-cases/center-of-excellence.md) を参照してください。

### データレイクでのエクスペリエンスイベントデータセットの保持 {#experience-event-dataset-retention}

Time-To-Live （TTL）を使用して、Adobe Experience Platformでのエクスペリエンスイベントデータセットの保持を制御します。 [ このガイドでは ](../../catalog/datasets/experience-event-dataset-retention-ttl-guide.md)TTL 設定を評価、設定および管理して、古いレコードを自動的に削除し、ストレージを最適化し、データの関連性を維持する方法について説明します。 データのライフサイクル管理を強化するためのベストプラクティス、実際のユースケース、主な考慮事項を確認します。
