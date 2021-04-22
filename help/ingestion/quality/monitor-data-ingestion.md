---
keywords: Experience Platform；ホーム；人気のあるトピック；監視；モニタ；データフロー；モニタ取り込み；データ取り込み；データ取り込み；表示レコード；表示バッチ；
solution: Experience Platform
title: データ収集の監視
topic-legacy: overview
description: このユーザーガイドでは、Adobe Experience Platform ユーザーインターフェイス内でデータを監視する方法の手順を説明します。このガイドでは、Adobe ID を持っていて、Adobe Experience Platform にアクセスできる必要があります。
exl-id: 85711a06-2756-46f9-83ba-1568310c9f73
translation-type: tm+mt
source-git-commit: 6bedd5ec0865e858a337155deb80309a54e30892
workflow-type: tm+mt
source-wordcount: '576'
ht-degree: 38%

---

# データ取得の監視

データ取得を使用すると、Adobe Experience Platform でデータを取得できます。バッチインジェストを使用すると、様々なファイルタイプ（CSVなど）を使用してデータを挿入できます。また、ストリーミングエンドポイントをリアルタイムで使用して[!DNL Platform]にデータを取り込むことができます。

このユーザガイドでは、Adobe Experience Platformユーザーインターフェイス内でデータを監視する手順を説明します。 このガイドでは、Adobe ID を持っていて、Adobe Experience Platform にアクセスできる必要があります。

## ストリーミングエンドツーエンドデータの取得の監視

[Experience PlatformUI](https://platform.adobe.com)で、左側のナビゲーションメニューの「**[!UICONTROL 監視]**」を選択し、続いて「**[!UICONTROL ストリーミングエンドツーエンド]**」を選択します。

「**[!UICONTROL ストリーミングエンドツーエンド]**」の監視ページが表示されます。このワークスペースは、[!DNL Platform]が受信したストリームイベントの割合を示すグラフ、[[!DNL Real-time Customer Profile]](../../profile/home.md)が正常に処理したストリームイベントの割合と、入力データの詳細リストを示すグラフを提供します。

![](../images/quality/monitor-data-flows/list-streams.png)

デフォルトで、上のグラフには、過去7日間の摂取率が表示されます。 この日付範囲は、強調表示されたボタンを選択して、様々な期間を表示するように調整できます。

![](../images/quality/monitor-data-flows/events-received.png)

下のグラフは、過去7日間で[!DNL Profile]によってストリームイベントが正常に処理された割合を示しています。 この日付範囲は、強調表示されたボタンを選択して、様々な期間を表示するように調整できます。

>[!NOTE]
>
>このグラフにデータを表示するには、[!DNL Profile]に対して&#x200B;**明示的に**&#x200B;有効にする必要があります。 [!DNL Profile]のストリーミングデータを有効にする方法については、[datasetsユーザーガイド](../../catalog/datasets/user-guide.md#enable-a-dataset-for-real-time-customer-profile)を参照してください。

![](../images/quality/monitor-data-flows/ingested-by-profile.png)

グラフの下には、上に示した日付範囲に対応するすべてのストリーミング取り込みレコードがリストされています。 リストの各バッチには、ID、データセット名、最終アップデート日時、バッチ内のレコード数、エラー数（エラーがある場合）が表示されます。任意のレコードを選択すると、そのレコードに関する詳細情報を確認できます。

![](../images/quality/monitor-data-flows/streams.png)

### ストリーミングレコードの表示

正常にストリーミングされたレコードの詳細には、取得されたレコードの数、ファイルサイズ、取得の開始時刻および終了時刻などの情報が表示されます。

![](../images/quality/monitor-data-flows/successful-streaming.png)

失敗したストリーミングレコードの詳細には、成功したレコードと同じ情報が表示されます。

![](../images/quality/monitor-data-flows/failed-batch.png)

また、失敗したレコードは、バッチの処理中に発生したエラーの詳細を提供します。 以下の例では、データの変換または検証時に解析エラーが発生していました。

![](../images/quality/monitor-data-flows/failed-batch-error.png)

## バッチエンドツーエンドデータの取得の監視

[[!DNL Experience Platform UI]](https://platform.adobe.com)で、左側のナビゲーションメニューの「**[!UICONTROL 監視]**」を選択します。

「**[!UICONTROL バッチエンドツーエンド]**」の監視ページが開き、以前に取得したバッチのリストが表示されます。任意のバッチを選択して、そのレコードに関する詳細情報を表示できます。

![](../images/quality/monitor-data-flows/batch-monitoring.png)

### バッチの表示

成功したバッチの詳細には、取得されたレコードの数、ファイルサイズ、取得の開始時刻および終了時刻などの情報が表示されます。

![](../images/quality/monitor-data-flows/successful-batch.png)

失敗したバッチの詳細には、成功したバッチと同じ情報に加えて、失敗したレコードの数が表示されます。

![](../images/quality/monitor-data-flows/failed-batch.png)

また、失敗したバッチは、バッチの処理中に発生したエラーの詳細を提供します。 以下の例では、ユーザーのIDの最大数が含まれているので、取り込んだバッチにエラーが発生しました。

![](../images/quality/monitor-data-flows/failed-streaming-error.png)
