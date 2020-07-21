---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: データ取り込みの監視
topic: overview
translation-type: tm+mt
source-git-commit: 73a492ba887ddfe651e0a29aac376d82a7a1dcc4
workflow-type: tm+mt
source-wordcount: '519'
ht-degree: 0%

---


# データ取り込みの監視

データ取り込みを使用すると、Adobe Experience Platformにデータを取り込むことができます。 バッチインジェストを使用すると、様々なファイルタイプ（CSVなど）を使用してデータを挿入できます。また、ストリーミングインジェストを使用すると、データをリアルタイムでストリーミングエンドポイントを使用して取り込むことができます。 [!DNL Platform]

このユーザーガイドでは、Adobe Experience Platformユーザーインターフェイス内のデータを監視する手順を説明します。 このガイドを使用するには、Adobe IDとAdobe Experience Platformへのアクセス権が必要です。

## ストリーミングのエンド・ツー・エンドのデータ取り込みの監視

[Experience PlatformUIで、左側のナビゲーションメニューの「](https://platform.adobe.com)監視 **[!UICONTROL 」をクリックし、「]** Streaming end-to-end ****」をクリックします。

![](../images/quality/monitor-data-flows/click-streaming-end-to-end.png)

[ *[!UICONTROL ストリーミングエンドツーエンドの監視]* ]ページが表示されます。 このワークスペースには、受信したストリームイベントの割合を示すグラフ [!DNL Platform]、によって正常に処理されたストリームイベントの割合を示すグラフ、 [!DNL Real-time Customer Profile](../../profile/home.md)および入力データの詳細リストが表示されます。

![](../images/quality/monitor-data-flows/list-streams.png)

デフォルトで、上のグラフには、過去7日間の摂取率が表示されます。 この日付範囲は、強調表示されたボタンをクリックすると、様々な期間を表示するように調整できます。

![](../images/quality/monitor-data-flows/list-streams-focus-on-top-graph.png)

下のグラフには、過去7日間で正常に処理されたストリームイベントの割合 [!DNL Profile] が表示されます。 この日付範囲は、強調表示されたボタンをクリックすると、様々な期間を表示するように調整できます。

>[!NOTE]
>
>このグラフにデータを表示するには、データを **明示的に有効にする必要があり**[!DNL Profile]ます。 のストリーミングデータを有効にする方法につ [!DNL Profile]いては、『 [datasetsユーザガイド](../../catalog/datasets/user-guide.md#enable-a-dataset-for-real-time-customer-profile)』を参照してください。

![](../images/quality/monitor-data-flows/list-streams-focus-on-bottom-graph.png)

グラフの下には、上に示した日付範囲に対応するすべてのストリーミング取り込みレコードがリストされています。 一覧に表示される各バッチには、ID、データセット名、最終更新時点、バッチ内のレコード数、エラー数（存在する場合）が表示されます。 任意のレコードをクリックすると、そのレコードに関する詳細情報を表示できます。

![](../images/quality/monitor-data-flows/list-streams-focus-on-streams.png)

### ストリーミングレコードの表示

ストリーミングされたレコードの詳細を表示すると、取り込まれたレコード数、ファイルサイズ、取り込み開始、終了時間などの情報が表示されます。

![](../images/quality/monitor-data-flows/successful-streaming-record.png)

失敗したストリーミングレコードの詳細には、成功したレコードと同じ情報が表示されます。

![](../images/quality/monitor-data-flows/failed-batch.png)

また、失敗したレコードは、バッチの処理中に発生したエラーの詳細を提供します。 次の例では、カタログからのdatasetIdの検証中にシステムエラーが発生しました。

![](../images/quality/monitor-data-flows/failed-batch-details.png)

## バッチのエンドツーエンドのデータ取り込みの監視

で、左側のナビゲ [!DNL Experience Platform UI](https://platform.adobe.com)ーションメニューの[ **[!UICONTROL 監視]** ]をクリックします。

![](../images/quality/monitor-data-flows/click-monitoring.png)

「 **[!UICONTROL バッチエンドツーエンドの監視]** 」ページが表示され、以前に取り込んだバッチのリストが表示されます。 任意のバッチをクリックすると、そのレコードに関する詳細情報を表示できます。

![](../images/quality/monitor-data-flows/list-batches.png)

### バッチの表示

正常に完了したバッチの詳細を表示すると、取り込まれたレコード数、ファイルサイズ、取り込み開始、終了時間などの情報が表示されます。

![](../images/quality/monitor-data-flows/successful-batch.png)

失敗したバッチの詳細には、成功したバッチと同じ情報が表示され、レコード数の追加に失敗しました。

![](../images/quality/monitor-data-flows/failed-streaming-record.png)

また、失敗したバッチは、バッチの処理中に発生したエラーの詳細を提供します。 以下の例では、不明なフィールドのを使用しているので、取り込んだバッチにエラーが発生しました `_experience`。

![](../images/quality/monitor-data-flows/failed-streaming-record-details.png)