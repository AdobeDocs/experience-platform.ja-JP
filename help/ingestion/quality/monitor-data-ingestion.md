---
keywords: Experience Platform；ホーム；人気のあるトピック；監視；監視；監視；データフロー；取得の監視；データ取得；データ取得；レコードの表示；バッチの表示；
solution: Experience Platform
title: データ取得の監視
topic-legacy: overview
description: このユーザーガイドでは、Adobe Experience Platform ユーザーインターフェイス内でデータを監視する方法の手順を説明します。このガイドでは、Adobe ID を持っていて、Adobe Experience Platform にアクセスできる必要があります。
exl-id: 85711a06-2756-46f9-83ba-1568310c9f73
source-git-commit: 3fadf7006c8ea058e469067b61950ed2d2d12e3f
workflow-type: tm+mt
source-wordcount: '618'
ht-degree: 36%

---

# データ取得の監視

データ取得を使用すると、Adobe Experience Platform でデータを取得できます。バッチ取り込みを使用すると、様々なファイルタイプ（CSV など）を使用してデータを挿入できます。また、ストリーミング取り込みを使用すると、ストリーミングエンドポイントをリアルタイムで使用して [!DNL Platform] にデータを取り込むことができます。

このユーザーガイドでは、Adobe Experience Platformユーザーインターフェイス内でデータを監視する手順を説明します。 このガイドでは、Adobe ID を持っていて、Adobe Experience Platform にアクセスできる必要があります。

## ストリーミングエンドツーエンドデータの取得の監視

[Experience PlatformUI](https://platform.adobe.com) で、左側のナビゲーションメニューで「**[!UICONTROL 監視]**」を選択し、次に「**[!UICONTROL ストリーミングエンドツーエンド]**」を選択します。

「**[!UICONTROL ストリーミングエンドツーエンド]**」の監視ページが表示されます。このワークスペースは、[!DNL Platform] が受信したストリーミングイベントの割合を示すグラフ、[[!DNL Real-time Customer Profile]](../../profile/home.md) が正常に処理したストリーミングイベントの割合を示すグラフ、および受信データの詳細なリストを提供します。

![](../images/quality/monitor-data-flows/list-streams.png)

デフォルトでは、上部のグラフには過去 7 日間の取り込み率が表示されます。 この日付範囲は、ハイライト表示されたボタンを選択して、様々な期間を表示するように調整できます。

![](../images/quality/monitor-data-flows/events-received.png)

下のグラフは、過去 7 日間で [!DNL Profile] によって正常に処理されたストリーミングイベントの割合を示しています。 この日付範囲は、ハイライト表示されたボタンを選択して、様々な期間を表示するように調整できます。

>[!NOTE]
>
>このグラフにデータを表示するには、**明示的に** を [!DNL Profile] に対して有効にする必要があります。 [!DNL Profile] のストリーミングデータを有効にする方法については、『[ データセットユーザガイド ](../../catalog/datasets/user-guide.md#enable-a-dataset-for-real-time-customer-profile)』を参照してください。

![](../images/quality/monitor-data-flows/ingested-by-profile.png)

グラフの下に、上に示した日付範囲に対応するすべてのストリーミング取得レコードのリストが表示されます。 リストの各バッチには、ID、データセット名、最終アップデート日時、バッチ内のレコード数、エラー数（エラーがある場合）が表示されます。任意のレコードを選択して、そのレコードに関する詳細情報を表示できます。

![](../images/quality/monitor-data-flows/streams.png)

### ストリーミングレコードの表示

正常にストリーミングされたレコードの詳細には、取得されたレコードの数、ファイルサイズ、取得の開始時刻および終了時刻などの情報が表示されます。

![](../images/quality/monitor-data-flows/successful-streaming.png)

失敗したストリーミングレコードの詳細には、成功したレコードと同じ情報が表示されます。

![](../images/quality/monitor-data-flows/failed-batch.png)

また、失敗したレコードは、バッチの処理中に発生したエラーの詳細を提供します。 以下の例では、データの変換または検証中に解析エラーが発生していました。

>[!NOTE]
>
>取り込まれた行にエラーがある場合、結果のメッセージで XDM が無効にならない限り、これらの行は **削除されません**。

![](../images/quality/monitor-data-flows/failed-batch-error.png)

## バッチエンドツーエンドデータの取得の監視

[[!DNL Experience Platform UI]](https://platform.adobe.com) で、左側のナビゲーションメニューの「**[!UICONTROL 監視]**」を選択します。

「**[!UICONTROL バッチエンドツーエンド]**」の監視ページが開き、以前に取得したバッチのリストが表示されます。任意のバッチを選択して、そのレコードに関する詳細情報を表示できます。

![](../images/quality/monitor-data-flows/batch-monitoring.png)

### バッチの表示

成功したバッチの詳細には、取得されたレコードの数、ファイルサイズ、取得の開始時刻および終了時刻などの情報が表示されます。

![](../images/quality/monitor-data-flows/successful-batch.png)

失敗したバッチの詳細には、成功したバッチと同じ情報に加えて、失敗したレコードの数が表示されます。

![](../images/quality/monitor-data-flows/failed-batch.png)

また、失敗したバッチは、バッチの処理中に発生したエラーの詳細を提供します。 次の例では、ユーザーの ID の最大数が原因で、取得したバッチでエラーが発生しています。

>[!NOTE]
>
>取り込まれた行にエラーがある場合、結果のメッセージで XDM が無効にならない限り、これらの行は **削除されません**。

![](../images/quality/monitor-data-flows/failed-streaming-error.png)
