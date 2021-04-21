---
keywords: Experience Platform；ホーム；人気のあるトピック；監視；モニタ；データフロー；モニタ取り込み；データ取り込み；データ取り込み；表示レコード；表示バッチ；
solution: Experience Platform
title: データ収集の監視
topic-legacy: overview
description: このユーザーガイドでは、Adobe Experience Platform ユーザーインターフェイス内でデータを監視する方法の手順を説明します。このガイドでは、Adobe ID を持っていて、Adobe Experience Platform にアクセスできる必要があります。
exl-id: 85711a06-2756-46f9-83ba-1568310c9f73
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '572'
ht-degree: 67%

---

# データ取得の監視

データ取得を使用すると、Adobe Experience Platform でデータを取得できます。バッチインジェストを使用すると、様々なファイルタイプ（CSVなど）を使用してデータを挿入できます。また、ストリーミングエンドポイントをリアルタイムで使用して[!DNL Platform]にデータを取り込むことができます。

このユーザーガイドでは、Adobe Experience Platform ユーザーインターフェイス内でデータを監視する方法の手順を説明します。このガイドでは、Adobe ID を持っていて、Adobe Experience Platform にアクセスできる必要があります。

## ストリーミングエンドツーエンドデータの取得の監視

[Experience Platform UI](https://platform.adobe.com) で、左側のナビゲーションメニューの「**[!UICONTROL 監視]**」をクリックしてから、「**[!UICONTROL ストリーミングエンドツーエンド]**」をクリックします。

![](../images/quality/monitor-data-flows/click-streaming-end-to-end.png)

「**[!UICONTROL ストリーミングエンドツーエンド]**」の監視ページが表示されます。このワークスペースは、[!DNL Platform]が受信したストリームイベントの割合を示すグラフ、[[!DNL Real-time Customer Profile]](../../profile/home.md)が正常に処理したストリームイベントの割合と、入力データの詳細リストを示すグラフを提供します。

![](../images/quality/monitor-data-flows/list-streams.png)

デフォルトで、上のグラフには、過去7日間の摂取率が表示されます。 ハイライト表示されたボタンをクリックして、この日付範囲を調整し、様々な期間を表示できます。

![](../images/quality/monitor-data-flows/list-streams-focus-on-top-graph.png)

下のグラフは、過去7日間で[!DNL Profile]によってストリームイベントが正常に処理された割合を示しています。 ハイライト表示されたボタンをクリックして、この日付範囲を調整し、様々な期間を表示できます。

>[!NOTE]
>
>このグラフにデータを表示するには、[!DNL Profile]に対して&#x200B;**明示的に**&#x200B;有効にする必要があります。 [!DNL Profile]のストリーミングデータを有効にする方法については、[datasetsユーザーガイド](../../catalog/datasets/user-guide.md#enable-a-dataset-for-real-time-customer-profile)を参照してください。

![](../images/quality/monitor-data-flows/list-streams-focus-on-bottom-graph.png)

グラフの下には、上に示した日付範囲に対応するすべてのストリーミング取り込みレコードがリストされています。 リストの各バッチには、ID、データセット名、最終アップデート日時、バッチ内のレコード数、エラー数（エラーがある場合）が表示されます。任意のレコードをクリックして、そのレコードに関する詳細情報を表示できます。

![](../images/quality/monitor-data-flows/list-streams-focus-on-streams.png)

### ストリーミングレコードの表示

正常にストリーミングされたレコードの詳細には、取得されたレコードの数、ファイルサイズ、取得の開始時刻および終了時刻などの情報が表示されます。

![](../images/quality/monitor-data-flows/successful-streaming-record.png)

失敗したストリーミングレコードの詳細には、成功したレコードと同じ情報が表示されます。

![](../images/quality/monitor-data-flows/failed-batch.png)

また、失敗したレコードは、バッチの処理中に発生したエラーの詳細を提供します。次の例では、カタログの datasetId の検証中にシステムエラーが発生しています。

![](../images/quality/monitor-data-flows/failed-batch-details.png)

## バッチエンドツーエンドデータの取得の監視

[[!DNL Experience Platform UI]](https://platform.adobe.com)で、左側のナビゲーションメニューの&#x200B;**[!UICONTROL Monitoring]**&#x200B;をクリックします。

![](../images/quality/monitor-data-flows/click-monitoring.png)

「**[!UICONTROL バッチエンドツーエンド]**」の監視ページが開き、以前に取得したバッチのリストが表示されます。任意のバッチをクリックして、そのレコードに関する詳細情報を表示できます。

![](../images/quality/monitor-data-flows/list-batches.png)

### バッチの表示

成功したバッチの詳細には、取得されたレコードの数、ファイルサイズ、取得の開始時刻および終了時刻などの情報が表示されます。

![](../images/quality/monitor-data-flows/successful-batch.png)

失敗したバッチの詳細には、成功したバッチと同じ情報に加えて、失敗したレコードの数が表示されます。

![](../images/quality/monitor-data-flows/failed-streaming-record.png)

また、失敗したバッチは、バッチの処理中に発生したエラーの詳細を提供します。次の例では、`_experience` の不明なフィールドを使用したため、取得したバッチでエラーが発生しています。

![](../images/quality/monitor-data-flows/failed-streaming-record-details.png)
