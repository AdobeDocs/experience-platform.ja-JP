---
keywords: Experience Platform；アクティベーション；トラブルシューティング；ガードレール；ガイドライン；制限
title: アクティベーションデータのデフォルトのガードレール
solution: Experience Platform
product: experience platform
type: Documentation
description: データアクティベーションのデフォルトの使用状況とレート制限について詳しく説明します。
source-git-commit: 69496d2e00ce866413786160d4524cabd03ae350
workflow-type: tm+mt
source-wordcount: '1198'
ht-degree: 18%

---

# アクティベーションデータのガードレール

このページでは、アクティベーション動作に関するデフォルトの使用方法とレートの制限について説明します。 次のガードレールを確認する際は、正しいと見なされます [宛先に接続済み](/help/destinations/ui/connect-destination.md).

>[!NOTE]
>
>* ほとんどのお客様は、これらのデフォルトの上限を超えることはありません。カスタムの上限について詳しくは、カスタマーケア担当者にお問い合わせください。
>* このドキュメントで概要を説明する上限は、常に改善されています。 定期的にアップデートを確認してください。
>* 個々のダウンストリームの制限によっては、一部の宛先は、このページで説明するものよりも厳しいガードレールを持つ場合があります。 また、 [カタログ](/help/destinations/catalog/overview.md) 接続してデータをアクティブ化する宛先のページ。


## 上限のタイプ {#limit-types}

このドキュメントでは、次の 2 種類のデフォルトの上限について説明します。

* **ソフトリミット：**&#x200B;ソフトリミットを超えることは可能ですが、ソフトリミットはシステムのパフォーマンスに推奨されるガイドラインです。
* **ハードリミット：** ハードリミットは絶対最大値を示します。Experience Platformの UI または API では、この制限を超えることはできません。超えると、エラーが返されます。


## アクティベーションの制限 {#activation-limits}

次のガードレールは、宛先に対してリアルタイム顧客プロファイルデータをアクティブ化する際の推奨制限を提供します。

### 一般的なアクティベーションガードレール {#general-activation-guardrails}

以下のガードレールは、通常、 [すべての宛先タイプ](/help/destinations/destination-types.md#destination-types).

| ガードレール | 上限 | 上限のタイプ | 説明 |
| --- | --- | --- | --- |
| 1 つの宛先に対するセグメントの最大数 | 250 | ソフト | データフロー内の単一の宛先に最大 250 個のセグメントをマッピングすることをお勧めします。 <br><br> 宛先に対して 250 を超えるセグメントをアクティブ化する必要がある場合は、次のいずれかを実行できます。 <ul><li> アクティブ化しなくなったセグメントのマッピングを解除する。または</li><li>新しいデータフローを目的の宛先に作成し、セグメントをこの新しいデータフローにマッピングします。</li></ul> <br> 一部の宛先では、その宛先にマッピングされたセグメントの数が 250 個未満に制限される場合があることに注意してください。 これらの宛先は、ページの下の各セクションで呼び出されます。 |
| 宛先の最大数 | 100 | ソフト | に接続してデータをアクティブ化できる宛先を最大 100 個作成することをお勧めします。 *サンドボックスごと*. [エッジパーソナライゼーションの宛先（カスタムパーソナライゼーション）](#edge-destinations-activation) は、100 件の推奨される宛先のうち、最大 10 件を構成できます。 |
| 宛先にマッピングされる属性の最大数 | 50 | ソフト | 複数の宛先および宛先タイプの場合、書き出し用にマッピングするプロファイル属性および ID を選択できます。 最適なパフォーマンスを得るには、最大 50 個の属性をデータフローで宛先にマッピングする必要があります。 |
| 宛先に対してアクティブ化されるデータのタイプ | プロファイルデータ（ID および ID マップを含む） | ハード | 現在、書き出しのみ可能です *プロファイルレコード属性* 宛先に送信します。 イベントデータを記述する XDM 属性は、現時点では書き出しでサポートされていません。 |
| 宛先に対してアクティブ化されるデータのタイプ — 配列およびマップ属性のサポート | 使用不可 | ハード | 現時点では、 **not** 輸出可能 *配列またはマップ属性* 宛先に送信します。 このルールの例外は、 [id マップ](/help/xdm/field-groups/profile/identitymap.md)：ストリーミングとファイルベースの両方のアクティベーションで書き出されます。 |

{style=&quot;table-layout:auto&quot;}

### ストリーミングの有効化 {#streaming-activation}

以下のガードレールは、を通じたアクティベーションに適用されます [ストリーミング先](/help/destinations/ui/activate-segment-streaming-destinations.md).

| ガードレール | 上限 | 上限のタイプ | 説明 |
| --- | --- | --- | --- |
| 1 秒あたりのアクティベーション数（プロファイル書き出しを含む HTTP メッセージ） | N/A | - | 現在、パートナー宛先の API エンドポイントにExperience Platformから送信される 1 秒あたりのメッセージ数に制限はありません。 <br> 制限や待ち時間は、Experience Platformがデータを送信するエンドポイントによって決まります。 また、 [カタログ](/help/destinations/catalog/overview.md) 接続してデータをアクティブ化する宛先のページ。 |

{style=&quot;table-layout:auto&quot;}

### バッチ（ファイルベース）のアクティベーション {#batch-file-based-activation}

以下のガードレールは、を通じたアクティベーションに適用されます [バッチ（ファイルベース）の宛先](/help/destinations/ui/activate-batch-profile-destinations.md).

| ガードレール | 上限 | 上限のタイプ | 説明 |
| --- | --- | --- | --- |
| 有効化の頻度 | 1 日に 1 回、または 3 時間、6 時間、8 時間、12 時間ごとに 1 回以上の増分エクスポートを頻繁におこないます。 | ハード | 詳しくは、 [完全なファイルをエクスポート](/help/destinations/ui/activate-batch-profile-destinations.md#export-full-files) および [増分ファイルの書き出し](/help/destinations/ui/activate-batch-profile-destinations.md#export-incremental-files) ドキュメントの節で、バッチエクスポートの頻度の増分に関する詳細を参照してください。 |
| 特定の時間にエクスポートできるセグメントの最大数 | 100 | ソフト | バッチ宛先データフローには、最大 100 個のセグメントを追加することをお勧めします。 |
| アクティブ化するファイルあたりの最大行数（レコード数） | 500 万 | ハード | Adobe Experience Platformは、エクスポートしたファイルを、ファイルあたり 500 万件のレコード（行）で自動的に分割します。 各行は 1 つのプロファイルを表します。`filename.csv`、`filename_2.csv`、`filename_3.csv` のように、分割ファイル名には、ファイルが大きな書き出しの一部であることを示す数字が付加されます。詳しくは、 [スケジュールセクション](/help/destinations/ui/activate-batch-profile-destinations.md#scheduling) 「バッチ保存先のアクティブ化」チュートリアル |

{style=&quot;table-layout:auto&quot;}

### アドホックアクティベーション {#ad-hoc-activation}

以下のガードレールは、 [アドホックアクティベーション](/help/destinations/api/ad-hoc-activation-api.md) メソッド。

| ガードレール | 上限 | 上限のタイプ | 説明 |
| --- | --- | --- | --- |
| アドホックアクティベーションジョブごとにアクティブ化されたセグメント | 80 | ハード | 現在、各アドホックアクティベーションジョブは、最大 80 個のセグメントをアクティブ化できます。 1 つのジョブにつき 80 個を超えるセグメントをアクティブ化しようとすると、ジョブが失敗します。 この動作は、今後のリリースで変更される可能性があります。 |
| セグメントごとの同時アドホックアクティベーションジョブ | 1 | ハード | セグメントごとに複数の同時アドホックアクティベーションジョブを実行しないでください。 |

{style=&quot;table-layout:auto&quot;}

### エッジパーソナライゼーションの宛先のアクティベーション {#edge-destinations-activation}

以下のガードレールは、を通じたアクティベーションに適用されます [エッジパーソナライゼーションの宛先](/help/destinations/destination-types.md#streaming-profile-export).

| ガードレール | 上限 | 上限のタイプ | 説明 |
| --- | --- | --- | --- |
| 最大数 [カスタムパーソナライゼーション](/help/destinations/catalog/personalization/custom-personalization.md) 宛先 | 10 | ソフト | データフローは、サンドボックスあたり 10 個のカスタムパーソナライゼーションの宛先に設定できます。 |
| サンドボックスごとにパーソナライゼーションの宛先にマッピングされる属性の最大数 | 20 | ハード | サンドボックスごとに、データフローでパーソナライゼーション宛先にマッピングできる属性は最大 20 個です。 |
| 1 つにマッピングされたセグメントの最大数 [Adobe Target](/help/destinations/catalog/personalization/adobe-target-connection.md) 宛先 | 50 | ソフト | 1 つのAdobe Targetの宛先に対して、アクティベーションフローで最大 50 個のセグメントをアクティブ化できます。 |

{style=&quot;table-layout:auto&quot;}

### Destination SDKガードレール {#destination-sdk-guardrails}

[ Destination SDK は、Experience Platform の宛先統合パターンを設定し、選択したデータと認証形式に基づいて、オーディエンスとプロファイルのデータをエンドポイントに配信できるようにする設定 API のスイートです。](/help/destinations/destination-sdk/overview.md)以下のガードレールは、Destination SDKを使用して設定する宛先に適用されます。

| ガードレール | 上限 | 上限のタイプ | 説明 |
| --- | --- | --- | --- |
| 最大数 [プライベートカスタム宛先](/help/destinations/destination-sdk/overview.md#productized-custom-integrations) | 5 | ソフト | Destination SDKを使用して、最大 5 つのプライベートカスタムストリーミングまたはバッチの宛先を作成できます。 そのような宛先を 5 つ以上作成する必要がある場合は、カスタムケア担当者にお問い合わせください。 |
| Destination SDKのプロファイル書き出しポリシー | <ul><li>`maxBatchAgeInSecs` （1.800 以上 3.600 以下）</li><li>`maxNumEventsInBatch` （最小 1.000、最大 10.000）</li></ul> | ハード | を使用する場合、 [設定可能な集計](/help/destinations/destination-sdk/destination-configuration.md#configurable-aggregation) オプションを使用する場合は、HTTP メッセージが API ベースの宛先に送信される頻度と、メッセージに含めるプロファイル数を決定する最小値と最大値に注意してください。 |

{style=&quot;table-layout:auto&quot;}

### 宛先のスロットルと再試行ポリシー {#destination-throttling-and-retry-policy}

特定の宛先に対するしきい値や制限の詳細。 また、この節では、宛先の再試行ポリシーに関する情報も提供します。

| 宛先のタイプ | 説明 |
| --- | --- |
| エンタープライズの宛先 (HTTP API、Amazon Kinesis、Azure EventHubs) | 95%の確率で、Experience Platformは、各データフローの 1 秒あたり 10,000 リクエスト未満の割合で正常に送信されたメッセージに対して、10 分未満のスループット待ち時間を提供しようとします。 <br> エンタープライズ環境の宛先へのリクエストが失敗した場合、Experience Platformは失敗したリクエストを保存し、リクエストをエンドポイントに送信するために 2 回再試行します。 |

{style=&quot;table-layout:auto&quot;}

## 他のガードレールサービス用Experience Platform {#guardrails-other-services}

他のガードレールサービスのガードレール情報をExperience Platformします。

* のガードレール [データ取得](/help/ingestion/guardrails.md)
* のガードレール [[!DNL Identity Service] データ](/help/identity-service/guardrails.md)
* のガードレール [[!DNL Real-time Customer Profile] データ](/help/profile/guardrails.md)
* のガードレール [[!DNL Query Service] データ](/help/query-service/guardrails.md)