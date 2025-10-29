---
keywords: Experience Platform;アクティベーション;トラブルシューティング;ガードレール;ガイドライン;制限
title: データアクティベーション用のデフォルトガードレール
solution: Experience Platform
product: experience platform
type: Documentation
description: データアクティベーションのデフォルトの使用方法とレート制限について詳しく説明します。
exl-id: a755f224-3329-42d6-b8a9-fadcf2b3ca7b
source-git-commit: 1b507e9846a74b7ac2d046c89fd7c27a818035ba
workflow-type: tm+mt
source-wordcount: '1712'
ht-degree: 48%

---

# データアクティベーション用のガードレール

>[!IMPORTANT]
>
>このガードレール ページに加えて、販売注文と対応する [&#x200B; 製品説明 &#x200B;](https://helpx.adobe.com/jp/legal/product-descriptions.html) でライセンスの使用権限を確認してください。

このページでは、アクティベーション動作に関するデフォルトの使用方法とレートの制限について説明します。次のガードレールを確認する際は、正しく[宛先に接続されている](/help/destinations/ui/connect-destination.md)とみなされます。

>[!NOTE]
>
>* ほとんどのお客様は、これらのデフォルトの上限を超えることはありません。カスタムの上限について詳しくは、カスタマーケア担当者にお問い合わせください。
>* このドキュメントで概要を説明する上限は、常に改善されています。 定期的にアップデートを確認してください。
>* 個々のダウンストリームの制限によっては、一部の宛先は、このページで説明するものよりも厳しいガードレールを持つ場合があります。また、接続してデータをアクティブ化する宛先のページの[カタログ](/help/destinations/catalog/overview.md)を確認します。

## ガードレール タイプ {#limit-types}

このドキュメントでは、次の 2 種類のデフォルトの上限について説明します。

| ガードレール タイプ | 説明 |
|----------|---------|
| **パフォーマンスガードレール（ソフトリミット）** | パフォーマンスガードレールは、ユースケースのスコーピングに関連する使用制限です。 パフォーマンスガードレールを超えると、パフォーマンスの低下や待ち時間が発生する場合があります。 Adobeは、このようなパフォーマンス低下に対する責任を負いません。 パフォーマンスのガードレールを常に超えるお客様は、パフォーマンスの低下を避けるために、追加容量のライセンスを選択できます。 |
| **システムで適用されるガードレール（ハードリミット）** | システムに適用されたガードレールは、Real-Time CDP UI または API によって適用されます。 これらは、UI と API によってこれをブロックされるか、エラーを返すので、超えることはできない制限です。 |

{style="table-layout:auto"}


## アクティベーションの制限 {#activation-limits}

次のガードレールは、宛先にリアルタイム顧客プロファイルデータをアクティブ化する際の推奨上限を示します。

### 一般的なアクティベーションガードレール {#general-activation-guardrails}

以下のガードレールは、通常、[すべての宛先タイプ](/help/destinations/destination-types.md#destination-types)を通してアクティベーションに適用されます。

| ガードレール | 上限 | 上限のタイプ | 説明 |
| --- | --- | --- | --- |
| 単一の宛先に対するオーディエンスの最大数 | 250 | パフォーマンスガードレール | データフロー内の単一の宛先に最大 250 個のオーディエンスをマッピングすることをお勧めします。 <br><br> 宛先に対して 250 を超えるオーディエンスをアクティブ化する必要がある場合は、次のいずれかを実行できます。 <ul><li> アクティブ化をやめるオーディエンスのマッピングを解除する。または</li><li>新しいデータフローを目的の宛先に作成し、オーディエンスをこの新しいデータフローにマッピングします。</li></ul> <br> 一部の宛先では、その宛先にマッピングされたオーディエンスの数が 250 個未満に制限される場合があることに注意してください。 これらの宛先は、ページの下の各セクションで呼び出されます。 |
| 宛先にマッピングされる属性の最大数 | 50 | パフォーマンスガードレール | 複数の宛先および宛先タイプの場合、書き出し用にマッピングするプロファイル属性および ID を選択できます。最適なパフォーマンスを得るには、最大 50 個の属性をデータフローで宛先にマッピングする必要があります。 |
| 宛先の最大数 | 100 | システムに適用されたガードレール | *サンドボックスごとに*、接続してデータをアクティブ化できる宛先を最大 100 個作成できます。 [エッジパーソナライゼーションの宛先（カスタムパーソナライゼーション）](#edge-destinations-activation)は、100 件の推奨される宛先のうち、最大 10 件を構成できます。 |
| 宛先に対してアクティブ化されるデータのタイプ | プロファイルデータ（ID および ID マップを含む） | システムに適用されたガードレール | 現在、宛先へ&#x200B;*プロファイルレコード属性*&#x200B;の書き出しのみ可能です。イベントデータを記述する XDM 属性は、現時点では書き出しでサポートされていません。 |
| 宛先に対してアクティブ化されるデータのタイプ - 配列およびマップ属性のサポート | 部分的に使用可能 | システムに適用されたガードレール | 配列属性を [&#x200B; ファイルベースの宛先 &#x200B;](/help/destinations/destination-types.md#file-based) に書き出すことができます。 機能について [&#x200B; 詳細を参照 &#x200B;](/help/destinations/ui/export-arrays-maps-objects.md)。 |

{style="table-layout:auto"}

### ストリーミングのアクティベーション {#streaming-activation}

以下のガードレールは、[ストリーミングの宛先](/help/destinations/ui/activate-segment-streaming-destinations.md)を通じたアクティベーションに適用されます。

| ガードレール | 上限 | 上限のタイプ | 説明 |
| --- | --- | --- | --- |
| 1 秒あたりのアクティベーション数（プロファイル書き出しを含む HTTP メッセージ） | N/A | - | 現在、パートナー宛先の API エンドポイントに Experience Platform から送信される 1 秒あたりのメッセージ数に制限はありません。<br> 制限や待ち時間は、Experience Platform がデータを送信するエンドポイントによって決まります。また、データの接続とアクティベーションを行う宛先の[カタログ](/help/destinations/catalog/overview.md)ページも確認するようにします。 |

{style="table-layout:auto"}

### バッチ（ファイルベース）のアクティベーション {#batch-file-based-activation}

以下のガードレールは、[バッチ（ファイルベース）の宛先](/help/destinations/ui/activate-batch-profile-destinations.md)を通じたアクティベーションに適用されます。

| ガードレール | 上限 | 上限のタイプ | 説明 |
| --- | --- | --- | --- |
| アクティベーションの頻度 | 1 日に 1 回、またはより頻繁な 3 時間、6 時間、8 時間、12 時間ごとに 1 回の増分書き出しを行います。 | システムに適用されたガードレール | バッチ書き出しの増分頻度について詳しくは、[完全ファイルの書き出し](/help/destinations/ui/activate-batch-profile-destinations.md#export-full-files)および[増分ファイルの書き出し](/help/destinations/ui/activate-batch-profile-destinations.md#export-incremental-files)ドキュメントの節を参照してください。 |
| 特定の時間に書き出せるオーディエンスの最大数 | 100 | パフォーマンスガードレール | バッチ宛先データフローに最大 100 個のオーディエンスを追加することをお勧めします。 |
| アクティベートするファイルあたりの最大行数（レコード数） | 500 万 | システムに適用されたガードレール | Adobe Experience Platform は、書き出したファイルを、ファイルあたり 500 万件のレコード（行）で自動的に分割します。各行は 1 つのプロファイルを表します。`filename.csv`、`filename_2.csv`、`filename_3.csv` のように、分割ファイル名には、ファイルが大きな書き出しの一部であることを示す数字が付加されます。詳しくは、「バッチの宛先をアクティベート」チュートリアルの[スケジュールの節](/help/destinations/ui/activate-batch-profile-destinations.md#scheduling)を参照してください。 |
| データフローでアクティブ化するカスタムアップロードオーディエンスの最大数 | 10 | システムに適用されたガードレール | バッチファイルベースの宛先に対して [&#x200B; カスタムアップロードオーディエンス &#x200B;](/help/segmentation/ui/audience-portal.md#import-audience) をアクティブ化する場合、データフローでアクティブ化できるオーディエンスは 10 個に制限されています。 詳しくは、ワークフロー [&#x200B; バッチファイルベースの宛先に対するカスタムアップロードオーディエンスの有効化 &#x200B;](/help/destinations/ui/activate-batch-profile-destinations.md#select-audiences) を参照してください。 |

{style="table-layout:auto"}

### アドホックアクティベーション {#ad-hoc-activation}

以下のガードレールは、[アドホックアクティベーション](/help/destinations/api/ad-hoc-activation-api.md)メソッドに適用されます。

| ガードレール | 上限 | 上限のタイプ | 説明 |
| --- | --- | --- | --- |
| アドホックアクティベーションジョブごとにアクティベートされたオーディエンス | 80 | システムに適用されたガードレール | 現在、各アドホックアクティベーションジョブは、最大 80 個のオーディエンスをアクティベートできます。 1 つのジョブにつき 80 個を超えるオーディエンスをアクティベートしようとすると、ジョブが失敗します。 この動作は、今後のリリースで変更される可能性があります。 |
| オーディエンスごとの同時アドホックアクティベーションジョブ | 1 | システムに適用されたガードレール | オーディエンスごとに複数の同時アドホックアクティベーションジョブを実行しないでください。 |

{style="table-layout:auto"}

### エッジパーソナライゼーションの宛先のアクティベーション {#edge-destinations-activation}

以下のガードレールは、[エッジパーソナライゼーションの宛先](/help/destinations/destination-types.md#advanced-enterprise-destinations)を通じたアクティベーションに適用されます。

| ガードレール | 上限 | 上限のタイプ | 説明 |
| --- | --- | --- | --- |
| [カスタムパーソナライゼーション](/help/destinations/catalog/personalization/custom-personalization.md)の宛先の最大数 | 10 | パフォーマンスガードレール | データフローは、サンドボックスあたり 10 個のカスタムパーソナライゼーションの宛先に設定できます。 |
| サンドボックスごとにパーソナライゼーションの宛先にマッピングされる属性の最大数 | 30 | システムに適用されたガードレール | サンドボックスごとに、データフローでパーソナライゼーションの宛先にマッピングできる属性は最大 30 個です。 |
| 1 つの [Adobe Target](/help/destinations/catalog/personalization/adobe-target-connection.md) の宛先にマッピングされるオーディエンスの最大数 | 50 | パフォーマンスガードレール | 1 つのAdobe Targetの宛先に対して、アクティベーションフローで最大 50 個のオーディエンスをアクティブ化できます。 |

{style="table-layout:auto"}

### データセットの書き出し {#dataset-exports}

データセットの書き出しは、現在、**[!UICONTROL First Full and then Incremental]** [pattern](/help/destinations/ui/export-datasets.md#scheduling) でサポートされています。 この節で説明しているガードレールは *データセット書き出しワークフローの設定後に発生する* 最初の完全書き出しに適用される）。

<!--

| Guardrail | Limit | Limit Type | Description |
| --- | --- | --- | --- |
| Size of exported datasets | 5 billion records | Soft | The limit described here for dataset exports is a *soft guardrail*. For example, while the user interface will not block you from exporting datasets larger than 5 billion records, the behavior is unpredictable and exports might either fail or have very long export latency. |

{style="table-layout:auto"}

-->

#### データセットタイプ {#dataset-types}

データセット書き出しガードレールは、以下に説明するように、Experience Platformから書き出された 2 種類のデータセットに適用されます。

**XDM エクスペリエンスイベントスキーマに基づくデータセットと、他のスキーマに基づくデータセット**

XDM エクスペリエンスイベントスキーマに基づくデータセットの場合、データセットスキーマには、トップレベルのタイムスタンプ列が含まれます。 データは、追加専用で取り込まれます。 他のスキーマに基づくデータセットの場合、データセットスキーマにはタイムスタンプ列が含まれ、データがアップサート方式で取り込まれる場合があります。

以下のソフトガードレールは、Experience Platformから書き出されたすべてのデータセットに適用されます。 様々なデータセットと圧縮タイプに固有の、後述するハードガードレールも確認してください。

| ガードレール | 上限 | 上限のタイプ | 説明 |
| --- | --- | --- | --- |
| 書き出されたデータセットのサイズ | 50 億件のレコード | パフォーマンスガードレール | ここで説明するデータセット書き出しの制限は *ソフトガードレール* です。 例えば、ユーザーインターフェイスでは 50 億レコードを超えるデータセットの書き出しはブロックされませんが、動作は予測できず、書き出しが失敗したり、非常に長い書き出し待ち時間が発生したりする場合があります。 |

{style="table-layout:auto"}

#### スケジュールされたデータセット書き出しのガードレール

スケジュール設定された、または繰り返しデータセットの書き出しの場合、以下のガードレールは、書き出されたファイル（JSON または parquet）の 2 つの形式と同じで、データセットタイプごとにグループ化されています。

>[!WARNING]
>
>JSON ファイルへの書き出しは、圧縮モードでのみサポートされます。

| データセットタイプ | ガードレール | ガードレール タイプ | 説明 |
|---------|----------|---------|-------|
| **XDM エクスペリエンスイベントスキーマ** に基づくデータセット | 過去 365 日間のデータ | システムに適用されたガードレール | 昨年のデータが書き出されます。 |
| **XDM エクスペリエンスイベントスキーマを除くすべてのスキーマ** に基づくデータセット | データフローで書き出されたすべてのファイルで 100 億件のレコード | システムに適用されたガードレール | 圧縮された JSON ファイルまたは parquet ファイルの場合はデータセットのレコード数が 100 億未満、非圧縮の parquet ファイルの場合は 100 万未満である必要があり、そうでない場合は書き出しが失敗します。 書き出そうとしているデータセットが許可されるしきい値を超えている場合は、そのデータセットのサイズを小さくします。 |

{style="table-layout:auto"}

<!--

#### Ad-hoc dataset exports

Exporting datasets in an-hoc manner is currently supported via API only. For ad-hoc dataset exports, you must use the backfill parameter in the API to limit the timeframe of exported data. 

The guardrails below are the same whether you are exporting parquet of JSON files ad-hoc. 

**Parquet and JSON output**

|Dataset type | Backfill parameter provided | Guardrail | Guardrail type | Description |
|---------|---------|-----------|-----------|------------|
| Datasets based on the **XDM Experience Events schema** |  <p><ul><li>Both start and end date provided in `backfill` parameter in API call</li><li>Incomplete `backfill` parameter provided in API call</li></ul></p> | <p><ul><li>Last 30 days</li><li>Last 365 days</li></ul></p> | Hard | <p><ul><li>The export fails if the `startDate - endDate` interval is over 30 days</li><li>Either the `startDate` or `endDate` are missing or  incorrectly formatted in the API call. Expected format: `yyyy-MM-dd'T'HH:mm:ss.SSS'Z'`</li></ul></p> |
| Datasets based on the **XDM Individual Profile schema** |  - | Ten billion records across all files exported in a dataflow | Hard | The record count of the dataset must be less than ten billion for compressed JSON or parquet files and one million for uncompressed parquet files, otherwise the export fails. Reduce the size of the dataset that you are trying to export if it is larger than the allowed threshold. |

{style="table-layout:auto"}

-->

詳しくは、[&#x200B; データセットの書き出し &#x200B;](/help/destinations/ui/export-datasets.md) を参照してください。


### Destination SDK ガードレール {#destination-sdk-guardrails}

[Destination SDK](/help/destinations/destination-sdk/overview.md) は、Experience Platform の宛先統合パターンを設定し、選択したデータと認証形式に基づいて、オーディエンスとプロファイルのデータをエンドポイントに配信できるようにする設定 API のスイートです。以下のガードレールは、Destination SDK を使用して設定する宛先に適用されます。

| ガードレール | 上限 | 上限のタイプ | 説明 |
| --- | --- | --- | --- |
| [プライベートカスタム宛先](/help/destinations/destination-sdk/overview.md#productized-custom-integrations)の最大数 | 5 | パフォーマンスガードレール | Destination SDK を使用して、最大 5 つのプライベートカスタムストリーミングまたはバッチの宛先を作成できます。宛先を 5 つ以上作成する必要がある場合は、カスタムケア担当者にお問い合わせください。 |
| Destination SDK のプロファイル書き出しポリシー | <ul><li>`maxBatchAgeInSecs`（最小 1,800、最大 3,600）</li><li>`maxNumEventsInBatch`（最小 1,000、最大 10,000）</li></ul> | システムに適用されたガードレール | 「[設定可能な集計](destination-sdk/functionality/destination-configuration/aggregation-policy.md#configurable-aggregation)」オプションを使用する場合は、HTTP メッセージが API ベースの宛先に送信される頻度と、メッセージに含めるプロファイル数を決定する最小値と最大値に注意してください。 |

{style="table-layout:auto"}

### 宛先のスロットルと再試行ポリシー {#destination-throttling-and-retry-policy}

特定の宛先に対するしきい値や制限のスロットルの詳細。 また、この節では、宛先の再試行ポリシーに関する情報も提供します。

| 宛先のタイプ | 説明 |
| --- | --- |
| エンタープライズの宛先（HTTP API、Amazon Kinesis、Azure EventHubs） | Experience Platformは 95％の確率で、エンタープライズ宛先の各データフローにおいて、送信に成功したメッセージのスループット待ち時間を 10 分未満、リクエスト数を 1 秒あたり 10,000 件未満で提供しようと試みます。<br> エンタープライズ環境の宛先へのリクエストが失敗した場合、Experience Platform は失敗したリクエストを保存し、リクエストをエンドポイントに送信するために 2 回再試行します。 |

{style="table-layout:auto"}

## 次の手順

他のExperience Platform サービスのガードレール、エンドツーエンドの待ち時間の情報およびReal-Time CDP Product Description のドキュメントからのライセンス情報について詳しくは、次のドキュメントを参照してください。

* [Real-Time CDP ガードレール](/help/rtcdp/guardrails/overview.md)
* 様々なExperience Platform サービス用の [&#x200B; エンドツーエンドの待ち時間の図 &#x200B;](https://experienceleague.adobe.com/docs/blueprints-learn/architecture/architecture-overview/deployment/guardrails.html?lang=ja#end-to-end-latency-diagrams)。
* [Real-Time Customer Data Platform（B2C Edition - PrimeおよびUltimate パッケージ） &#x200B;](https://helpx.adobe.com/jp/legal/product-descriptions/real-time-customer-data-platform-b2c-edition-prime-and-ultimate-packages.html)
* [Real-Time Customer Data Platform（B2P - PrimeおよびUltimate パッケージ） &#x200B;](https://helpx.adobe.com/jp/legal/product-descriptions/real-time-customer-data-platform-b2p-edition-prime-and-ultimate-packages.html)
* [Real-Time Customer Data Platform（B2B - PrimeおよびUltimate パッケージ） &#x200B;](https://helpx.adobe.com/jp/legal/product-descriptions/real-time-customer-data-platform-b2b-edition-prime-and-ultimate-packages.html)
