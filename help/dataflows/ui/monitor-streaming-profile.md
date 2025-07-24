---
title: ストリーミングプロファイル取り込みの監視
description: 監視ダッシュボードを使用してストリーミングプロファイルの取り込みを監視する方法について説明します
hide: true
hidefromtoc: true
source-git-commit: 2f78b70761ef676fe4ab61b89b6cbb261c99e9ca
workflow-type: tm+mt
source-wordcount: '786'
ht-degree: 3%

---

# ストリーミングプロファイル取り込みの監視

Adobe Experience Platform UI のモニタリングダッシュボードを使用すると、組織内のストリーミングプロファイル取り込み率をリアルタイムにモニタリングできます。 この機能を使用すると、ストリーミングデータに関するスループット、待ち時間、およびデータ品質指標の透明性を高めることができます。 さらに、この機能を使用して、プロアクティブなアラートと実用的なインサイトの取得を行い、潜在的な容量違反とデータ取り込みの問題を特定できます。

監視ダッシュボードを使用して、組織内のストリーミングプロファイル取り込みジョブの割合と指標を監視する方法については、次のガイドを参照してください。

## ストリーミングプロファイル取り込みダッシュボードについて

ストリーミングプロファイル取り込みの監視ダッシュボードでは、[!UICONTROL &#x200B; スループット &#x200B;]、[!UICONTROL &#x200B; 取り込み &#x200B;]、[!UICONTROL &#x200B; 待ち時間 &#x200B;] の 3 つの異なる指標カテゴリを使用できます。

* **スループット**: **[!UICONTROL スループット]** を選択すると、設定された期間にExperience Platformで処理されているデータ量に関する情報が表示されます。 システムの効率と処理能力を評価するには、この指標を参照してください。
   * **処理能力**：定義された条件下でサンドボックスが処理できるデータの最大量です。
   * **リクエストスループット**：取り込みシステムがイベントを受信する速度（1 秒あたりのイベント数）。
   * **処理スループット**：受信イベントペイロードをシステムが正常に取り込んで処理した割合（1 秒あたりのイベント数で測定）。
* **取り込み**: [!UICONTROL &#x200B; 取り込み &#x200B;] を選択して、サンドボックス内の取り込みジョブに関する情報を表示します。 これらの取り込みジョブは、次の 3 つの異なる指標で測定されます。
   * **作成されたレコード**：指定された期間内に作成されたレコードの総数。 この指標は、サンドボックスでのデータ取り込みプロセスの成功を表します。
   * **失敗したレコード**: エラーが原因で取り込まれなかったレコードの合計数です。
   * **削除されたレコード**：容量制限に違反したために削除されたレコードの合計数です。
* **待ち時間**: [!UICONTROL &#x200B; 待ち時間 &#x200B;] を選択すると、Experience Platformがリクエストに応答するまで、または指定された期間内に操作を完了するまでにかかる時間に関する情報が表示されます。

## ストリーミングプロファイル取り込み用の監視指標 {#streaming-profile-metrics}

>[!CONTEXTUALHELP]
>id="platform_monitoring_streaming_profile"
>title="ストリーミングプロファイル取り込みの監視"
>abstract="ストリーミングプロファイルの監視ダッシュボードには、スループット、取り込み率、待ち時間に関する情報が表示されます。 このダッシュボードを使用して、データ処理指標の表示、理解および分析を行います。 のプロファイルをExperience Platformにストリーミングします。"
>text="Learn more in documentation"

>[!CONTEXTUALHELP]
>id="platform_monitoring_streaming_profile_request_throughput"
>title="リクエストのスループット"
>abstract="この指標は、取り込みシステムに入るイベントの 1 秒あたりの数を表します。"
>text="Learn more in documentation"

>[!CONTEXTUALHELP]
>id="platform_monitoring_streaming_profile_processing_throughput"
>title="処理スループット"
>abstract="この指標は、システムによって 1 秒ごとに正常に取り込まれたイベントの数を表します。"
>text="Learn more in documentation"

>[!CONTEXTUALHELP]
>id="platform_monitoring_streaming_profile_p95_ingestion_latency"
>title="P95 取得待ち時間"
>abstract="この指標は、Experience Platformにイベントが到達してからプロファイルストアに正常に取り込まれるまでの 95 パーセンタイルの待ち時間を測定します。"
>text="Learn more in documentation"

>[!CONTEXTUALHELP]
>id="platform_monitoring_streaming_profile_max_throughput"
>title="最大スループット"
>abstract="この指標は、ストリーミングプロファイル取り込みに入る 1 秒あたりのインバウンドリクエストの最大数を表します。"
>text="Learn more in documentation"

>[!CONTEXTUALHELP]
>id="platform_monitoring_streaming_profile_records_ingested"
>title="取り込まれたレコード"
>abstract="この指標は、設定された時間枠内にプロファイルストアに取り込まれたレコードの合計数を表します。"
>text="Learn more in documentation"

>[!CONTEXTUALHELP]
>id="platform_monitoring_streaming_profile_records_failed"
>title="失敗したレコード"
>abstract="この指標は、設定された時間内に、エラーが原因でプロファイルストアへの取り込みに失敗したレコードの合計数を表します。"
>text="Learn more in documentation"

>[!CONTEXTUALHELP]
>id="platform_monitoring_streaming_profile_records_skipped"
>title="スキップされたレコード"
>abstract="この指標は、設定または容量超過が原因で、設定された期間内にドロップされたレコードの合計数を表します。"
>text="Learn more in documentation"

>[!CONTEXTUALHELP]
>id="platform_monitoring_streaming_profile_error_details"
>title="エラーの詳細"
>abstract="この指標は、エラーが原因で失敗したイベントの数を表します。"
>text="Learn more in documentation"

| 指標 | 説明 | ディメンション | 測定頻度 |
| --- | --- | --- | --- |
| リクエストのスループット | この指標は、取り込みシステムに入るイベントの 1 秒あたりの数を表します。 | サンドボックス/データフロー | 60 秒ごとにデータを更新するリアルタイム監視。 |
| 処理スループット | この指標は、システムによって 1 秒ごとに正常に取り込まれたイベントの数を表します。 | サンドボックス/データフロー | 60 秒ごとにデータを更新するリアルタイム監視。 |
| P95 取得待ち時間 | この指標は、Experience Platformにイベントが到達してからプロファイルストアに正常に取り込まれるまでの 95 パーセンタイルの待ち時間を測定します。 | サンドボックス/データフロー | 60 秒ごとにデータを更新するリアルタイム監視。 |
| 最大スループット |
| 取り込まれたレコード | この指標は、設定された時間枠内にプロファイルストアに取り込まれたレコードの合計数を表します。 | <ul><li>サンドボックス/データフロー</li><li>データフローの実行</li></ul> | <ul><li>サンドボックス/データフロー：60 秒ごとにデータを更新するリアルタイム監視。</li><li>データフロー実行：15 分でグループ化されました。</li></ul> |
| 失敗したレコード | この指標は、設定された時間内に、エラーが原因でプロファイルストアへの取り込みに失敗したレコードの合計数を表します。 | <ul><li>サンドボックス/データフロー</li><li>データフローの実行</li></ul> | <ul><li>サンドボックス/データフロー：60 秒ごとにデータを更新するリアルタイム監視。</li><li>データフロー実行：15 分でグループ化されました。</li></ul> |
| スキップされたレコード | この指標は、設定または容量超過が原因で、設定された期間内にドロップされたレコードの合計数を表します。 | <ul><li>サンドボックス/データフロー</li><li>データフローの実行</li></ul> | <ul><li>サンドボックス/データフロー：60 秒ごとにデータを更新するリアルタイム監視。</li><li>データフロー実行：15 分でグループ化されました。</li></ul> |
| エラーの詳細 | この指標は、エラーが原因で失敗したイベントの数を表します。 | データフローの実行 | 1 時間ごとのウィンドウにグループ化。 |

{style="table-layout:auto"}