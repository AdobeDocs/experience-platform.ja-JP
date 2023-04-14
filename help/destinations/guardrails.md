---
keywords: Experience Platform;アクティベーション;トラブルシューティング;ガードレール;ガイドライン;制限
title: アクティベーションデータのデフォルトのガードレール
solution: Experience Platform
product: experience platform
type: Documentation
description: データアクティベーションのデフォルトの使用方法とレート制限について詳しく説明します。
exl-id: a755f224-3329-42d6-b8a9-fadcf2b3ca7b
source-git-commit: 1132c5166f1271f1b8eb0c618b83d028b413b991
workflow-type: tm+mt
source-wordcount: '1177'
ht-degree: 98%

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
| 単一の宛先に対するセグメントの最大数 | 250 | ソフト | データフロー内の単一の宛先に最大 250 個のセグメントをマッピングすることをお勧めします。<br><br> 宛先に対して 250 を超えるセグメントをアクティブ化する必要がある場合は、次のいずれかを実行できます。 <ul><li> アクティブ化をやめるセグメントのマッピングを解除する。または</li><li>新しいデータフローを目的の宛先に作成し、セグメントをこの新しいデータフローにマッピングします。</li></ul> <br> 一部の宛先では、その宛先にマッピングされたセグメントの数が 250 個未満に制限される場合があることに注意してください。これらの宛先は、ページの下の各セクションで呼び出されます。 |
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
| 任意の時間に書き出せるセグメントの最大数 | 100 | ソフト | バッチ宛先データフローに最大 100 個のセグメントを追加することをお勧めします。 |
| アクティベートするファイルあたりの最大行数（レコード数） | 500 万 | ハード | Adobe Experience Platform は、書き出したファイルを、ファイルあたり 500 万件のレコード（行）で自動的に分割します。各行は 1 つのプロファイルを表します。`filename.csv`、`filename_2.csv`、`filename_3.csv` のように、分割ファイル名には、ファイルが大きな書き出しの一部であることを示す数字が付加されます。詳しくは、「バッチの宛先をアクティベート」チュートリアルの[スケジュールの節](/help/destinations/ui/activate-batch-profile-destinations.md#scheduling)を参照してください。 |

{style="table-layout:auto"}

### アドホックアクティベーション {#ad-hoc-activation}

以下のガードレールは、[アドホックアクティベーション](/help/destinations/api/ad-hoc-activation-api.md)メソッドに適用されます。

| ガードレール | 上限 | 上限のタイプ | 説明 |
| --- | --- | --- | --- |
| アドホックアクティベーションジョブごとにアクティベートされたセグメント | 80 | ハード | 現在、各アドホックアクティベーションジョブは、最大 80 個のセグメントをアクティベートできます。1 つのジョブにつき 80 個を超えるセグメントをアクティベートしようとすると、ジョブが失敗します。この動作は、今後のリリースで変更される可能性があります。 |
| セグメントごとの同時アドホックアクティベーションジョブ | 1 | ハード | セグメントごとに複数の同時アドホックアクティベーションジョブを実行しないでください。 |

{style="table-layout:auto"}

### エッジパーソナライゼーションの宛先のアクティベーション {#edge-destinations-activation}

以下のガードレールは、[エッジパーソナライゼーションの宛先](/help/destinations/destination-types.md#streaming-profile-export)を通じたアクティベーションに適用されます。

| ガードレール | 上限 | 上限のタイプ | 説明 |
| --- | --- | --- | --- |
| [カスタムパーソナライゼーション](/help/destinations/catalog/personalization/custom-personalization.md)の宛先の最大数 | 10 | ソフト | データフローは、サンドボックスあたり 10 個のカスタムパーソナライゼーションの宛先に設定できます。 |
| サンドボックスごとにパーソナライゼーションの宛先にマッピングされる属性の最大数 | 30 | ハード | サンドボックスごとに、データフローでパーソナライゼーションの宛先にマッピングできる属性は最大 30 個です。 |
| 1 つの [Adobe Target](/help/destinations/catalog/personalization/adobe-target-connection.md) の宛先にマッピングされるセグメントの最大数 | 50 | ソフト | 1 つの Adobe Target の宛先に対して、アクティベーションフローで最大 50 個のセグメントをアクティブ化できます。 |

{style="table-layout:auto"}

### Destination SDK ガードレール {#destination-sdk-guardrails}

[Destination SDK](/help/destinations/destination-sdk/overview.md) は、Experience Platform の宛先統合パターンを設定し、選択したデータと認証形式に基づいて、オーディエンスとプロファイルのデータをエンドポイントに配信できるようにする設定 API のスイートです。以下のガードレールは、Destination SDK を使用して設定する宛先に適用されます。

| ガードレール | 上限 | 上限のタイプ | 説明 |
| --- | --- | --- | --- |
| [プライベートカスタム宛先](/help/destinations/destination-sdk/overview.md#productized-custom-integrations)の最大数 | 5 | ソフト | Destination SDK を使用して、最大 5 つのプライベートカスタムストリーミングまたはバッチの宛先を作成できます。宛先を 5 つ以上作成する必要がある場合は、カスタムケア担当者にお問い合わせください。 |
| Destination SDK のプロファイル書き出しポリシー | <ul><li>`maxBatchAgeInSecs`（最小 1.800、最大 3.600）</li><li>`maxNumEventsInBatch`（最小 1.000、最大 10.000）</li></ul> | ハード | 「[設定可能な集計](/help/destinations/destination-sdk/destination-configuration.md#configurable-aggregation)」オプションを使用する場合は、HTTP メッセージが API ベースの宛先に送信される頻度と、メッセージに含めるプロファイル数を決定する最小値と最大値に注意してください。 |

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
