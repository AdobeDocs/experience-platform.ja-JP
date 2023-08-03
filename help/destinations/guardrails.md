---
keywords: Experience Platform;アクティベーション;トラブルシューティング;ガードレール;ガイドライン;制限
title: アクティベーションデータのデフォルトのガードレール
solution: Experience Platform
product: experience platform
type: Documentation
description: データアクティベーションのデフォルトの使用方法とレート制限について詳しく説明します。
exl-id: a755f224-3329-42d6-b8a9-fadcf2b3ca7b
source-git-commit: f360df6273986be35340432c72d8f8620f339b67
workflow-type: tm+mt
source-wordcount: '1272'
ht-degree: 77%

---

# アクティベーションデータのガードレール

このページでは、アクティベーション動作に関するデフォルトの使用方法とレートの制限について説明します。次のガードレールを確認する際は、正しく[宛先に接続されている](/help/destinations/ui/connect-destination.md)とみなされます。

>[!NOTE]
>
>* ほとんどのお客様は、これらのデフォルトの上限を超えることはありません。カスタムの上限について詳しくは、カスタマーケア担当者にお問い合わせください。
>* このドキュメントで概要を説明する上限は、常に改善されています。 定期的にアップデートを確認してください。
>* 個々のダウンストリームの制限によっては、一部の宛先は、このページで説明するものよりも厳しいガードレールを持つ場合があります。また、接続してデータをアクティブ化する宛先のページの[カタログ](/help/destinations/catalog/overview.md)を確認します。

## 上限のタイプ {#limit-types}

このドキュメントでは、次の 2 種類のデフォルトの上限について説明します。

* **ソフトリミット：**&#x200B;ソフトリミットを超えることは可能ですが、ソフトリミットはシステムのパフォーマンスに推奨されるガイドラインです。
* **ハードリミット：**&#x200B;ハードリミットは絶対最大値を示します。Experience Platformの UI または API では、この制限を超えることはできません。超えるとエラーが返されます。


## アクティベーションの制限 {#activation-limits}

次のガードレールは、宛先に対してリアルタイム顧客プロファイルデータをアクティブ化する際の推奨制限を提供します。

### 一般的なアクティベーションガードレール {#general-activation-guardrails}

以下のガードレールは、通常、[すべての宛先タイプ](/help/destinations/destination-types.md#destination-types)を通してアクティベーションに適用されます。

| ガードレール | 上限 | 上限のタイプ | 説明 |
| --- | --- | --- | --- |
| 1 つの宛先に対するオーディエンスの最大数 | 250 | ソフト | 最大 250 個のオーディエンスをデータフローの 1 つの宛先にマッピングすることをお勧めします。 <br><br> 宛先に対して 250 を超えるオーディエンスをアクティブ化する必要がある場合は、次のいずれかを実行できます。 <ul><li> アクティブ化しないオーディエンスのマッピングを解除する。または</li><li>新しいデータフローを目的の宛先に作成し、オーディエンスをこの新しいデータフローにマッピングします。</li></ul> <br> 一部の宛先では、その宛先にマッピングされたオーディエンスの数が 250 人未満に制限される場合があることに注意してください。 これらの宛先は、ページの下の各セクションで呼び出されます。 |
| 宛先の最大数 | 100 | ソフト | *サンドボックスごとに*、接続してデータをアクティブ化できる宛先を最大 100 個作成することをお勧めします。[エッジパーソナライゼーションの宛先（カスタムパーソナライゼーション）](#edge-destinations-activation)は、100 件の推奨される宛先のうち、最大 10 件を構成できます。 |
| 宛先にマッピングされる属性の最大数 | 50 | ソフト | 複数の宛先および宛先タイプの場合、書き出し用にマッピングするプロファイル属性および ID を選択できます。最適なパフォーマンスを得るには、最大 50 個の属性をデータフローで宛先にマッピングする必要があります。 |
| 宛先に対してアクティブ化されるデータのタイプ | プロファイルデータ（ID および ID マップを含む） | ハード | 現在、宛先へ&#x200B;*プロファイルレコード属性*&#x200B;の書き出しのみ可能です。イベントデータを記述する XDM 属性は、現時点では書き出しでサポートされていません。 |
| 宛先に対してアクティブ化されるデータのタイプ - 配列およびマップ属性のサポート | 使用不可 | ハード | 現時点では、*配列またはマップ属性*&#x200B;を宛先に書き出すことは&#x200B;**不可能**&#x200B;です。このルールの例外は、ストリーミングとファイルベースの両方のアクティベーションで書き出される [ID マップ](/help/xdm/field-groups/profile/identitymap.md)です。 |

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
| アクティベーションの頻度 | 1 日に 1 回、またはより頻繁な 3 時間、6 時間、8 時間、12 時間ごとに 1 回の増分書き出しを行います。 | ハード | バッチ書き出しの増分頻度について詳しくは、[完全ファイルの書き出し](/help/destinations/ui/activate-batch-profile-destinations.md#export-full-files)および[増分ファイルの書き出し](/help/destinations/ui/activate-batch-profile-destinations.md#export-incremental-files)ドキュメントの節を参照してください。 |
| 特定の時間にエクスポート可能なオーディエンスの最大数 | 100 | ソフト | バッチ宛先データフローには、最大 100 個のオーディエンスを追加することをお勧めします。 |
| アクティベートするファイルあたりの最大行数（レコード数） | 500 万 | ハード | Adobe Experience Platform は、書き出したファイルを、ファイルあたり 500 万件のレコード（行）で自動的に分割します。各行は 1 つのプロファイルを表します。`filename.csv`、`filename_2.csv`、`filename_3.csv` のように、分割ファイル名には、ファイルが大きな書き出しの一部であることを示す数字が付加されます。詳しくは、「バッチの宛先をアクティベート」チュートリアルの[スケジュールの節](/help/destinations/ui/activate-batch-profile-destinations.md#scheduling)を参照してください。 |

{style="table-layout:auto"}

### アドホックアクティベーション {#ad-hoc-activation}

以下のガードレールは、[アドホックアクティベーション](/help/destinations/api/ad-hoc-activation-api.md)メソッドに適用されます。

| ガードレール | 上限 | 上限のタイプ | 説明 |
| --- | --- | --- | --- |
| アドホックアクティベーションジョブごとにアクティブ化されたオーディエンス | 80 | ハード | 現在、各アドホックアクティベーションジョブは、最大 80 人のオーディエンスをアクティブ化できます。 1 回のジョブで 80 を超えるオーディエンスをアクティブ化しようとすると、ジョブが失敗します。 この動作は、今後のリリースで変更される可能性があります。 |
| オーディエンスごとの同時アドホックアクティベーションジョブ | 1 | ハード | オーディエンスごとに複数の同時アドホックアクティベーションジョブを実行しないでください。 |

{style="table-layout:auto"}

### エッジパーソナライゼーションの宛先のアクティベーション {#edge-destinations-activation}

以下のガードレールは、[エッジパーソナライゼーションの宛先](/help/destinations/destination-types.md#streaming-profile-export)を通じたアクティベーションに適用されます。

| ガードレール | 上限 | 上限のタイプ | 説明 |
| --- | --- | --- | --- |
| [カスタムパーソナライゼーション](/help/destinations/catalog/personalization/custom-personalization.md)の宛先の最大数 | 10 | ソフト | データフローは、サンドボックスあたり 10 個のカスタムパーソナライゼーションの宛先に設定できます。 |
| サンドボックスごとにパーソナライゼーションの宛先にマッピングされる属性の最大数 | 30 | ハード | サンドボックスごとに、データフローでパーソナライゼーションの宛先にマッピングできる属性は最大 30 個です。 |
| 1 つのにマッピングされるオーディエンスの最大数 [Adobe Target](/help/destinations/catalog/personalization/adobe-target-connection.md) 宛先 | 50 | ソフト | 1 つのAdobe Targetの宛先に対するアクティベーションフローで、最大 50 個のオーディエンスをアクティブ化できます。 |

{style="table-layout:auto"}

## [!BADGE ベータ版]{type=Informative} 個のデータセットの書き出し {#dataset-exports}

データセットのエクスポートは、現在、 **[!UICONTROL 最初にフルにしてから増分にしてください]** [pattern](/help/destinations/ui/export-datasets.md#scheduling). この節で説明するガードレールは、データセットエクスポートワークフローの設定後に発生する最初の完全エクスポートに適用されます。

| ガードレール | 上限 | 上限のタイプ | 説明 |
| --- | --- | --- | --- |
| 書き出されたデータセットのサイズ | 50 億レコード | ソフト | ここで説明するデータセットエクスポートの制限は、次のとおりです。 *ソフトガードレール*. 例えば、ユーザーインターフェイスで 50 億レコードを超えるデータセットの書き出しがブロックされることはありませんが、動作は予測不可能で、書き出しに失敗したり、書き出しに非常に長い時間がかかる場合があります。 |

{style="table-layout:auto"}

<!--

### Dataset Types {#dataset-types}

Datasets exported from Experience Platform can be of two types, as described below:

**Timeseries**
Timeseries datasets are also known as *XDM Experience Events* datasets in Experience Platform terminology.
The dataset schema includes a top level *timestamp* column. Data is ingested in an append-only fashion.

**Record** 
Record datasets are also known as *XDM Individual Profile* datasets in Experience Platform terminology.
The dataset schema does not include a top level *timestamp* column. Data is ingested in upsert fashion.

The guardrails below are grouped by the format of the exported file, and then further by dataset type.

**Parquet output**

|Dataset type | Compression | Guardrail | Description |
|---------|----------|---------|-----------|
| Timeseries | N/A | Last seven days per file | The data from the last seven days only is exported. |
| Record | N/A | Five billion records per file | Only the data from the last seven days is exported. |

{style="table-layout:auto"}

**JSON output**

|Dataset type | Compression | Guardrail | Description |
|---------|----------|---------|-----------|
| Timeseries | N/A | Last seven days per file | The data from the last seven days only is exported. |
| <p>Record</p> | <p><ul><li>Yes</li><li>No</li></ul></p> | <p><ul><li>Five billion records per compressed file</li><li>One million records per uncompressed file</li></ul></p> | <p>The record count of the dataset must be less than five billion for compressed files and one million for uncompressed files, otherwise the export fails. Reduce the size of the dataset that you are trying to export if it is larger than the allowed threshold.</p> |

{style="table-layout:auto"}

-->

<!--

<table>
<thead>
  <tr>
    <th>Output format</th>
    <th>Dataset type</th>
    <th>Compression</th>
    <th>Guardrail</th>
    <th>Description</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td rowspan="2">Parquet</td>
    <td>Timeseries</td>
    <td>-</td>
    <td>Last seven days per file</td>
    <td>Only the data from the last seven days is exported.</td>
  </tr>
  <tr>
    <td>Record</td>
    <td>-</td>
    <td>Five billion records per file</td>
    <td>The record count of the dataset must be less than five billion, otherwise the export fails. Reduce the size of the dataset that you are trying to export if it is larger than the allowed threshold.</td>
  </tr>
  <tr>
    <td rowspan="3">JSON</td>
    <td>Timeseries</td>
    <td>-</td>
    <td>Last seven days per file</td>
    <td>Only the data from the last seven days is exported.</td>
  </tr>
  <tr>
    <td rowspan="2">Record</td>
    <td>Yes</td>
    <td>Five billion records per file</td>
    <td>The record count of the dataset must be less than five billion, otherwise the export fails. Reduce the size of the dataset that you are trying to export if it is larger than the allowed threshold.</td>
  </tr>
  <tr>
    <td>No</td>
    <td>One million records per file</td>
    <td>The record count of the dataset must be less than one million, otherwise the export fails. Reduce the size of the dataset that you are trying to export if it is larger than the allowed threshold.</td>
  </tr>
</tbody>
</table>

-->

### Destination SDK ガードレール {#destination-sdk-guardrails}

[Destination SDK](/help/destinations/destination-sdk/overview.md) は、Experience Platform の宛先統合パターンを設定し、選択したデータと認証形式に基づいて、オーディエンスとプロファイルのデータをエンドポイントに配信できるようにする設定 API のスイートです。以下のガードレールは、Destination SDK を使用して設定する宛先に適用されます。

| ガードレール | 上限 | 上限のタイプ | 説明 |
| --- | --- | --- | --- |
| [プライベートカスタム宛先](/help/destinations/destination-sdk/overview.md#productized-custom-integrations)の最大数 | 5 | ソフト | Destination SDK を使用して、最大 5 つのプライベートカスタムストリーミングまたはバッチの宛先を作成できます。宛先を 5 つ以上作成する必要がある場合は、カスタムケア担当者にお問い合わせください。 |
| Destination SDK のプロファイル書き出しポリシー | <ul><li>`maxBatchAgeInSecs`（最小 1.800、最大 3.600）</li><li>`maxNumEventsInBatch`（最小 1.000、最大 10.000）</li></ul> | ハード | 「[設定可能な集計](destination-sdk/functionality/destination-configuration/aggregation-policy.md#configurable-aggregation)」オプションを使用する場合は、HTTP メッセージが API ベースの宛先に送信される頻度と、メッセージに含めるプロファイル数を決定する最小値と最大値に注意してください。 |

{style="table-layout:auto"}

### 宛先のスロットルと再試行ポリシー {#destination-throttling-and-retry-policy}

特定の宛先に対するしきい値や制限のスロットルの詳細。 また、この節では、宛先の再試行ポリシーに関する情報も提供します。

| 宛先のタイプ | 説明 |
| --- | --- |
| エンタープライズの宛先（HTTP API、Amazon Kinesis、Azure EventHubs） | Experience Platformは 95％の確率で、エンタープライズ宛先の各データフローにおいて、送信に成功したメッセージのスループット待ち時間を 10 分未満、リクエスト数を 1 秒あたり 10,000 件未満で提供しようと試みます。<br> エンタープライズ環境の宛先へのリクエストが失敗した場合、Experience Platform は失敗したリクエストを保存し、リクエストをエンドポイントに送信するために 2 回再試行します。 |

{style="table-layout:auto"}

## その他の Experience Platform サービス向けガードレール {#guardrails-other-services}

その他の Experience Platform サービスのガードレール情報を表示します。

* [データ取り込み](/help/ingestion/guardrails.md)のガードレール
* [[!DNL Identity Service] データ](/help/identity-service/guardrails.md)のガードレール
* [[!DNL Real-Time Customer Profile] データ](/help/profile/guardrails.md)のガードレール
* [[!DNL Query Service] データ](/help/query-service/guardrails.md)のガードレール
