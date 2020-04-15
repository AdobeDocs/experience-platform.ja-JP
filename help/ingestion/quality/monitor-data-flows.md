---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: データの取り込みの監視
topic: overview
translation-type: tm+mt
source-git-commit: 9cbc22a34613aeb58a2c5090b10978ae4428dbdb

---


# データの取り込みの監視

データの取り込みを使用すると、データをAdobe Experience Platformに取り込むことができます。 バッチインジェストを使用すると、様々なファイルタイプ（CSVなど）を使用してデータを挿入できます。また、ストリーミングインジェストを使用すると、リアルタイムでストリーミングエンドポイントを使用してプラットフォームにデータを取り込めます。

このユーザーガイドでは、Adobe Experience Platformユーザーインターフェイス内でデータを監視する手順を説明します。 このガイドでは、Adobe IDを持っていて、Adobe Experience Platformにアクセスできる必要があります。

## ストリーミングのエンドツーエンドのデータ取り込みの監視

エクスペリエ [ンスプラットフォームUIで](https://platform.adobe.com)、左側のナビゲ **ーションメニューの「監視」をクリックし、「エンドツーエン** ドのストリーミング」をクリックします ****。

![](../images/quality/monitor-data-flows/click-streaming-end-to-end.png)

[ストリ *ーミングエンドツーエンドの監視* ]ページが表示されます。 このワークスペースには、プラットフォームが受信するストリームイベントの割合を示すグラフ、リアルタイムイベントプロファイルが正常に処理したストリーム顧客の割合、および受信データの詳細リストを示すグラフが表示されます [](../../profile/home.md)。

![](../images/quality/monitor-data-flows/list-streams.png)

デフォルトでは、上のグラフには過去7日間の摂取率が表示されます。 この日付範囲は、強調表示されたボタンをクリックして、様々な期間を表示するように調整できます。

![](../images/quality/monitor-data-flows/list-streams-focus-on-top-graph.png)

下のグラフには、過去7日間のストリームイベントが正常に処理された割合がプロファイル別に表示されます。 この日付範囲は、強調表示されたボタンをクリックして、様々な期間を表示するように調整できます。

> [!NOTE] このグラフにデータを表示するには、データを明示的に有効にする **** プロファイル。 ストリーミングデータをプロファイル可能にする方法については、『データセットユーザガイド』 [を参照してくださ](../../catalog/datasets/user-guide.md#enable-a-dataset-for-real-time-customer-profile)い。

![](../images/quality/monitor-data-flows/list-streams-focus-on-bottom-graph.png)

グラフの下に、上に示したリスト範囲に対応するすべてのストリーミング取り込みレコードが表示されます。 一覧に表示される各バッチには、ID、データセット名、最終更新時点、バッチ内のレコード数、エラー数（存在する場合）が表示されます。 任意のレコードをクリックすると、そのレコードに関する詳細情報を表示できます。

![](../images/quality/monitor-data-flows/list-streams-focus-on-streams.png)

### ストリーミングレコードの表示

正常にストリーミングされたレコードの詳細を表示すると、取り込まれたレコードの数、ファイルサイズ、取り込み開始、終了時間などの情報が表示されます。

![](../images/quality/monitor-data-flows/successful-streaming-record.png)

失敗したストリーミングレコードの詳細には、成功したレコードと同じ情報が表示されます。

![](../images/quality/monitor-data-flows/failed-batch.png)

また、失敗したレコードは、バッチの処理中に発生したエラーの詳細を提供します。 次の例では、カタログからdatasetIdを検証中にシステムエラーが発生しました。

![](../images/quality/monitor-data-flows/failed-batch-details.png)

## バッチのエンドツーエンドのデータ取り込みの監視

Experience Platform UIで、左側のナビゲ [ーションメニュー](https://platform.adobe.com)**の「監視** 」をクリックします。

![](../images/quality/monitor-data-flows/click-monitoring.png)

バッ **チエンドツーエンドの監視ページが** 、以前に取り込んだバッチのリストが表示されます。 任意のバッチをクリックすると、そのレコードに関する詳細情報を表示できます。

![](../images/quality/monitor-data-flows/list-batches.png)

### バッチの表示

正常に完了したバッチの詳細を表示すると、取り込まれたレコード数、ファイルサイズ、取り込み開始および終了時間などの情報が表示されます。

![](../images/quality/monitor-data-flows/successful-batch.png)

失敗したバッチの詳細には、成功したバッチと同じ情報と、失敗したレコード数の追加が表示されます。

![](../images/quality/monitor-data-flows/failed-streaming-record.png)

また、失敗したバッチは、バッチの処理中に発生したエラーの詳細を提供します。 次の例では、不明なフィールドのを使用したので、取り込むバッチでエラーが発生しまし `_experience`た。

![](../images/quality/monitor-data-flows/failed-streaming-record-details.png)