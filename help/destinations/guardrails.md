---
keywords: Experience Platform;アクティベーション;トラブルシューティング;ガードレール;ガイドライン;制限
title: データアクティベーションのデフォルトガードレール
solution: Experience Platform
product: experience platform
type: Documentation
description: データアクティベーションのデフォルトの使用方法とレート制限について詳しく説明します。
exl-id: a755f224-3329-42d6-b8a9-fadcf2b3ca7b
source-git-commit: c550186c9cb3bc580a227633e0d7c0b574ecc2e8
workflow-type: tm+mt
source-wordcount: '1744'
ht-degree: 42%

---

# データ活用のガードレール

>[!IMPORTANT]
>
>このガードレール ページに加えて、実際の使用制限について、セールスオーダーと対応する[製品説明](https://helpx.adobe.com/jp/legal/product-descriptions.html)のライセンス使用権限を確認してください。

このページでは、アクティベーション動作に関するデフォルトの使用方法とレートの制限について説明します。次のガードレールを確認する際は、正しく[宛先に接続されている](/help/destinations/ui/connect-destination.md)とみなされます。

>[!NOTE]
>
>* ほとんどのお客様は、これらのデフォルトの上限を超えることはありません。カスタム制限について詳しくは、カスタマーケア担当者にお問い合わせください。
>* このドキュメントで概要を説明する上限は、常に改善されています。 定期的にチェックして、更新を確認してください。
>* 個々のダウンストリームの制限によっては、一部の宛先は、このページで説明するものよりも厳しいガードレールを持つ場合があります。また、接続してデータをアクティブ化する宛先のページの[カタログ](/help/destinations/catalog/overview.md)を確認します。

## ガードレール タイプ {#limit-types}

このドキュメントでは、次の 2 種類のデフォルトの上限について説明します。

| ガードレール型 | 説明 |
|----------|---------|
| **パフォーマンスガードレール （ソフト制限）** | パフォーマンスガードレールは、ユースケースの範囲に関連する使用制限です。 パフォーマンスのガードレールを超えると、パフォーマンスの低下や遅延が発生する場合があります。 Adobeはこのようなパフォーマンス低下の責任を負いません。 パフォーマンスのガードレールを常に超えているお客様は、パフォーマンスの低下を回避するために、追加の容量のライセンスを取得できます。 |
| **システムが適用するガードレール （ハード制限）** | システムで適用されるガードレールは、[!DNL Real-Time CDP] UIまたはAPIによって適用されます。 UIとAPIによってブロックされたり、エラーが返されたりするため、これらの制限を超えることはできません。 |

{style="table-layout:auto"}


## アクティベーションの制限 {#activation-limits}

以下のガードレールは、宛先に対してリアルタイム顧客プロファイルデータをアクティブ化する際の推奨される制限を提供します。

### 一般的なアクティベーションガードレール {#general-activation-guardrails}

以下のガードレールは、通常、[すべての宛先タイプ](/help/destinations/destination-types.md#destination-types)を通してアクティベーションに適用されます。

| ガードレール | 上限 | 上限のタイプ | 説明 |
| --- | --- | --- | --- |
| 1つの宛先に対する最大オーディエンス数 | 250 | パフォーマンスガードレール | 1つの宛先インスタンスに最大250個のオーディエンスをマッピングすることをお勧めします。 <br><br>宛先に対して250を超えるオーディエンスをアクティブ化する必要がある場合は、次のいずれかを実行できます。 <ul><li> アクティブ化したくないオーディエンスをマッピング解除したり</li><li>[新しい宛先インスタンスを作成し](ui/connect-destination.md)、それにオーディエンスをマッピングします。</li></ul> <br>一部の宛先の場合、宛先にマッピングされるオーディエンスは250未満に制限される場合があります。 これらの宛先は、ページの下の各セクションで呼び出されます。 |
| 宛先にマッピングされる属性の最大数 | 50 | パフォーマンスガードレール | 複数の宛先および宛先タイプの場合、書き出し用にマッピングするプロファイル属性および ID を選択できます。最適なパフォーマンスを得るには、最大50個の属性を宛先インスタンスにマッピングする必要があります。 |
| 宛先の最大数 | 100 | システム強制ガードレール | データを接続してアクティブ化できる宛先は、サンドボックスごとに&#x200B;*最大100個まで作成できます*。 [エッジパーソナライゼーションの宛先（カスタムパーソナライゼーション）](#edge-destinations-activation)は、100 件の推奨される宛先のうち、最大 10 件を構成できます。 |
| 宛先に対してアクティブ化されるデータのタイプ | プロファイルデータ（ID および ID マップを含む） | システム強制ガードレール | 現在、宛先へ&#x200B;*プロファイルレコード属性*&#x200B;の書き出しのみ可能です。イベントデータを記述する XDM 属性は、現時点では書き出しでサポートされていません。 |
| 宛先に対してアクティブ化されるデータのタイプ - 配列およびマップ属性のサポート | 一部を利用可能 | システム強制ガードレール | 配列属性を[&#x200B; ファイルベースの宛先](/help/destinations/destination-types.md#file-based)に書き出すことができます。 [機能について詳しくは](/help/destinations/ui/export-arrays-maps-objects.md)を参照してください。 |

{style="table-layout:auto"}

### ストリーミングのアクティベーション {#streaming-activation}

以下のガードレールは、[ストリーミングの宛先](/help/destinations/ui/activate-segment-streaming-destinations.md)を通じたアクティベーションに適用されます。

| ガードレール | 上限 | 上限のタイプ | 説明 |
| --- | --- | --- | --- |
| 1 秒あたりのアクティベーション数（プロファイル書き出しを含む HTTP メッセージ） | なし | - | 現在、パートナー宛先の API エンドポイントに Experience Platform から送信される 1 秒あたりのメッセージ数に制限はありません。<br> 制限や待ち時間は、Experience Platform がデータを送信するエンドポイントによって決まります。また、データの接続とアクティベーションを行う宛先の[カタログ](/help/destinations/catalog/overview.md)ページも確認するようにします。 |

{style="table-layout:auto"}

### バッチ（ファイルベース）のアクティベーション {#batch-file-based-activation}

以下のガードレールは、[バッチ（ファイルベース）の宛先](/help/destinations/ui/activate-batch-profile-destinations.md)を通じたアクティベーションに適用されます。

| ガードレール | 上限 | 上限のタイプ | 説明 |
| --- | --- | --- | --- |
| アクティベーションの頻度 | 1 日に 1 回、またはより頻繁な 3 時間、6 時間、8 時間、12 時間ごとに 1 回の増分書き出しを行います。 | システム強制ガードレール | バッチ書き出しの増分頻度について詳しくは、[完全ファイルの書き出し](/help/destinations/ui/activate-batch-profile-destinations.md#export-full-files)および[増分ファイルの書き出し](/help/destinations/ui/activate-batch-profile-destinations.md#export-incremental-files)ドキュメントの節を参照してください。 |
| 特定の時間に書き出すことができるオーディエンスの最大数 | 100 | パフォーマンスガードレール | バッチ宛先インスタンスに最大100個のオーディエンスを追加することをお勧めします。 |
| アクティベートするファイルあたりの最大行数（レコード数） | 500 万 | システム強制ガードレール | Adobe Experience Platform は、書き出したファイルを、ファイルあたり 500 万件のレコード（行）で自動的に分割します。各行は 1 つのプロファイルを表します。`filename.csv`、`filename_2.csv`、`filename_3.csv` のように、分割ファイル名には、ファイルが大きな書き出しの一部であることを示す数字が付加されます。詳しくは、「バッチの宛先をアクティベート」チュートリアルの[スケジュールの節](/help/destinations/ui/activate-batch-profile-destinations.md#scheduling)を参照してください。 |
| 宛先インスタンスでアクティブ化できる外部オーディエンスの最大数（例：FAC、カスタムアップロード、オーディエンス構成） | 20 | システム強制ガードレール | バッチファイルベースの宛先に対して外部オーディエンス（例：[Federated Audience Composition](/help/segmentation/ui/audience-portal.md#fac)、[&#x200B; カスタムアップロード &#x200B;](/help/segmentation/ui/audience-portal.md#import-audience)、[Audience Composition](/help/segmentation/ui/audience-portal.md#audience-composition)）をアクティブ化する場合、宛先インスタンスでアクティブ化できるオーディエンスは20個までという制限があります。 これらのオーディエンスタイプについて詳しくは、[&#x200B; オーディエンスタイプとカスタマイズ &#x200B;](/help/segmentation/ui/audience-portal.md#customize)を参照してください。 バッチファイルベースの宛先に対する外部オーディエンスのアクティブ化[&#x200B; ワークフローについて詳しくは、こちらを参照してください](/help/destinations/ui/activate-batch-profile-destinations.md#select-audiences)。 |

{style="table-layout:auto"}

### アドホックアクティベーション {#ad-hoc-activation}

以下のガードレールは、[アドホックアクティベーション](/help/destinations/api/ad-hoc-activation-api.md)メソッドに適用されます。

| ガードレール | 上限 | 上限のタイプ | 説明 |
| --- | --- | --- | --- |
| アドホックアクティベーションジョブごとにアクティブ化されたオーディエンス | 80 | システム強制ガードレール | 現在、各アドホックアクティベーションジョブでは、最大80のオーディエンスをアクティベートできます。 1つのジョブにつき80を超えるオーディエンスをアクティベートしようとすると、ジョブが失敗します。 この動作は、今後のリリースで変更される可能性があります。 |
| オーディエンスごとの同時アドホックアクティベーションジョブ | 1 | システム強制ガードレール | オーディエンスごとに複数の同時アドホックアクティベーションジョブを実行しないでください。 |

{style="table-layout:auto"}

### エッジパーソナライゼーションの宛先のアクティベーション {#edge-destinations-activation}

以下のガードレールは、[エッジパーソナライゼーションの宛先](/help/destinations/destination-types.md#advanced-enterprise-destinations)を通じたアクティベーションに適用されます。

| ガードレール | 上限 | 上限のタイプ | 説明 |
| --- | --- | --- | --- |
| [カスタムパーソナライゼーション](/help/destinations/catalog/personalization/custom-personalization.md)の宛先の最大数 | 10 | パフォーマンスガードレール | サンドボックスごとに最大10個のカスタムパーソナライゼーション宛先インスタンスを設定できます。 |
| サンドボックスごとにパーソナライゼーションの宛先にマッピングされる属性の最大数 | 30 | パフォーマンスガードレール | サンドボックスごとに、最大30個の属性をパーソナライゼーション宛先インスタンスにマッピングできます。 |

{style="table-layout:auto"}

### データセットの書き出し {#dataset-exports}

データセットの書き出しは現在、**[!UICONTROL First Full and then Incremental]** [&#x200B; パターン &#x200B;](/help/destinations/ui/export-datasets.md#scheduling)でサポートされています。 このセクション *で説明するガードレールは、データセット書き出しワークフローの設定後に最初に発生する完全な書き出し*&#x200B;に適用されます。

<!--

| Guardrail | Limit | Limit Type | Description |
| --- | --- | --- | --- |
| Size of exported datasets | 5 billion records | Soft | The limit described here for dataset exports is a *soft guardrail*. For example, while the user interface will not block you from exporting datasets larger than 5 billion records, the behavior is unpredictable and exports might either fail or have very long export latency. |

{style="table-layout:auto"}

-->

#### データセットの種類 {#dataset-types}

データセット書き出しガードレールは、次に示すように、Experience Platformから書き出された2種類のデータセットに適用されます。

**XDM Experience Events スキーマに基づくデータセットと、他のスキーマに基づくデータセット**

XDM Experience Events スキーマに基づくデータセットの場合、データセットスキーマには最上位のタイムスタンプ列が含まれます。 データは追加のみによって取り込まれます。 他のスキーマに基づくデータセットの場合、データセットスキーマにはタイムスタンプ列が含まれ、データはアップサート方式で取り込まれます。

以下のソフトガードレールは、Experience Platformから書き出されたすべてのデータセットに適用されます。 異なるデータセットと圧縮タイプに固有のハードガードレールも以下で確認してください。

| ガードレール | 上限 | 上限のタイプ | 説明 |
| --- | --- | --- | --- |
| 書き出されたデータセットのサイズ | 50億のレコード | パフォーマンスガードレール | データセットの書き出しについてここで説明する制限は、*ソフトガードレール*&#x200B;です。 例えば、ユーザーインターフェイスは、50億レコードを超えるデータセットの書き出しをブロックしませんが、動作は予測不可能であり、書き出しは失敗するか、書き出し遅延が非常に長い場合があります。 |

{style="table-layout:auto"}

#### スケジュールされたデータセット書き出しのガードレール {#scheduled-dataset-exports}

スケジュールされたデータセットの書き出しまたは定期的なデータセットの書き出しの場合、以下のガードレールは、書き出されたファイルの2つの形式（JSONまたはparquet）と同じで、データセットタイプごとにグループ化されます。

>[!WARNING]
>
>JSON ファイルへの書き出しは、圧縮モードでのみサポートされます。

| データセットタイプ | ガードレール | ガードレール型 | 説明 |
|---------|----------|---------|-------|
| **XDM Experience Events スキーマ**&#x200B;に基づくデータセット | 過去365日間のデータ | システム強制ガードレール | 前年のデータが書き出されます。 |
| XDM Experience Events スキーマ以外の&#x200B;**任意のスキーマに基づくデータセット** | 宛先インスタンス内のすべての書き出されたファイルの100億レコード | システム強制ガードレール | データセットのレコード数は、圧縮されたJSONまたはparquet ファイルの場合は100億未満、非圧縮のparquet ファイルの場合は100万未満である必要があります。そうでない場合、書き出しは失敗します。 書き出そうとするデータセットが許可されたしきい値を超えている場合は、データセットのサイズを小さくします。 |

{style="table-layout:auto"}

<!--

#### Ad-hoc dataset exports

Exporting datasets in an-hoc manner is currently supported via API only. For ad-hoc dataset exports, you must use the backfill parameter in the API to limit the timeframe of exported data. 

The guardrails below are the same whether you are exporting parquet of JSON files ad-hoc. 

**Parquet and JSON output**

|Dataset type | Backfill parameter provided | Guardrail | Guardrail type | Description |
|---------|---------|-----------|-----------|------------|
| Datasets based on the **XDM Experience Events schema** | <p><ul><li>Both start and end date provided in `backfill` parameter in API call</li><li>Incomplete `backfill` parameter provided in API call</li></ul></p> | <p><ul><li>Last 30 days</li><li>Last 365 days</li></ul></p> | Hard | <p><ul><li>The export fails if the `startDate - endDate` interval is over 30 days</li><li>Either the `startDate` or `endDate` are missing or incorrectly formatted in the API call. Expected format: `yyyy-MM-dd'T'HH:mm:ss.SSS'Z'`</li></ul></p> |
| Datasets based on the **XDM Individual Profile schema** | - | Ten billion records across all files exported in a destination instance | Hard | The record count of the dataset must be less than ten billion for compressed JSON or parquet files and one million for uncompressed parquet files, otherwise the export fails. Reduce the size of the dataset that you are trying to export if it is larger than the allowed threshold. |

{style="table-layout:auto"}

-->

詳しくは、[&#x200B; データセットの書き出しについて](/help/destinations/ui/export-datasets.md)を参照してください。


### Destination SDK ガードレール {#destination-sdk-guardrails}

[Destination SDK](/help/destinations/destination-sdk/overview.md) は、Experience Platform の宛先統合パターンを設定し、選択したデータと認証形式に基づいて、オーディエンスとプロファイルのデータをエンドポイントに配信できるようにする設定 API のスイートです。以下のガードレールは、Destination SDK を使用して設定する宛先に適用されます。

| ガードレール | 上限 | 上限のタイプ | 説明 |
| --- | --- | --- | --- |
| [プライベートカスタム宛先](/help/destinations/destination-sdk/overview.md#productized-custom-integrations)の最大数 | 5 | パフォーマンスガードレール | Destination SDK を使用して、最大 5 つのプライベートカスタムストリーミングまたはバッチの宛先を作成できます。宛先を 5 つ以上作成する必要がある場合は、カスタムケア担当者にお問い合わせください。 |
| Destination SDK のプロファイル書き出しポリシー | <ul><li>`maxBatchAgeInSecs`（最小 301、最大 3,600）</li><li>`maxNumEventsInBatch`（最小 1,000、最大 10,000）</li></ul> | システム強制ガードレール | 「[設定可能な集計](destination-sdk/functionality/destination-configuration/aggregation-policy.md#configurable-aggregation)」オプションを使用する場合は、HTTP メッセージが API ベースの宛先に送信される頻度と、メッセージに含めるプロファイル数を決定する最小値と最大値に注意してください。 |
| Destination SDKのOAuth 2 トークンの有効期間 | 推奨される最小24時間 | パフォーマンスガードレール | [OAuth 2認証](/help/destinations/destination-sdk/functionality/destination-configuration/oauth2-authorization.md)を使用する宛先の場合、Adobeでは、アクセストークンの有効期間の値を最低24時間に設定することをお勧めします。 有効期間が1時間未満のトークンとの接続は、アクティベーション中にプロファイルがドロップされます。 |

{style="table-layout:auto"}

### 宛先のスロットルと再試行ポリシー {#destination-throttling-and-retry-policy}

特定の宛先に対するしきい値や制限のスロットルの詳細。 また、この節では、宛先の再試行ポリシーに関する情報も提供します。

| 宛先のタイプ | 説明 |
| --- | --- |
| エンタープライズの宛先（HTTP API、Amazon Kinesis、Azure EventHubs） | Experience Platformでは、95%の確率で、各企業向け宛先インスタンスに対して1秒間に10千件を下回るリクエストを送信し、正常に送信されたメッセージに対してスループット遅延を10分未満に抑えようとします。 <br> エンタープライズ環境の宛先へのリクエストが失敗した場合、Experience Platform は失敗したリクエストを保存し、リクエストをエンドポイントに送信するために 2 回再試行します。 |

{style="table-layout:auto"}

## 次の手順 {#next-steps}

その他のExperience Platform サービスのガードレール、エンドツーエンドの待ち時間に関する情報、および[!DNL Real-Time CDP]製品説明ドキュメントからのライセンス情報について詳しくは、次のドキュメントを参照してください。

* [Real-Time CDPのガードレール](/help/rtcdp/guardrails/overview.md)
* 様々なExperience Platform サービスの[&#x200B; エンドツーエンドの待ち時間ダイアグラム &#x200B;](https://experienceleague.adobe.com/docs/blueprints-learn/architecture/architecture-overview/deployment/guardrails.html?lang=en#end-to-end-latency-diagrams)。
* [Real-Time Customer Data Platform（B2C Edition - PrimeおよびUltimate パッケージ） &#x200B;](https://helpx.adobe.com/jp/legal/product-descriptions/real-time-customer-data-platform-b2c-edition-prime-and-ultimate-packages.html)
* [Real-Time Customer Data Platform（B2P - PrimeおよびUltimate パッケージ） &#x200B;](https://helpx.adobe.com/legal/product-descriptions/real-time-customer-data-platform-b2p-edition-prime-and-ultimate-packages.html)
* [Real-Time Customer Data Platform（B2B - PrimeおよびUltimate パッケージ） &#x200B;](https://helpx.adobe.com/legal/product-descriptions/real-time-customer-data-platform-b2b-edition-prime-and-ultimate-packages.html)
