---
keywords: Experience Platform；ホーム；人気の高いトピック；監視；監視；データフロー；取得の監視；データ取得；データ取得；レコードの表示；バッチの表示；
solution: Experience Platform
title: データ取得の監視
topic-legacy: overview
description: このユーザーガイドでは、Adobe Experience Platform ユーザーインターフェイス内でデータを監視する方法の手順を説明します。このガイドでは、Adobe ID を持っていて、Adobe Experience Platform にアクセスできる必要があります。
exl-id: 85711a06-2756-46f9-83ba-1568310c9f73
source-git-commit: 34e0381d40f884cd92157d08385d889b1739845f
workflow-type: tm+mt
source-wordcount: '654'
ht-degree: 34%

---

# データ取得の監視

データ取得を使用すると、Adobe Experience Platform でデータを取得できます。バッチ取り込みを使用すると、様々なファイルタイプ（CSV など）を使用してデータを挿入できます。また、ストリーミング取り込みを使用すると、データをに取り込むことができます。 [!DNL Platform] リアルタイムでのストリーミングエンドポイントの使用

このユーザーガイドでは、Adobe Experience Platformユーザーインターフェイス内でデータを監視する手順を説明します。 このガイドでは、Adobe ID を持っていて、Adobe Experience Platform にアクセスできる必要があります。

## ストリーミングエンドツーエンドデータの取得の監視 {#monitor-streaming-end-to-end-data-ingestion}

>[!CONTEXTUALHELP]
>id="platform_ingestion_streaming_ingestionrate"
>title="取り込み率"
>abstract="1 秒あたりに正常に処理されたイベント数。"
>text="Learn more in the documentation"
>additional-url="http://www.adobe.com/go/monitor-dataflows-en" text="UI でのソースのデータフローの監視"

>[!TIP]
>
>特定の日付の合計イベント数を計算するには、次の式を使用します。 `total events / day = ingestion rate * 60 * 60 * 24`.

内 [Experience PlatformUI](https://platform.adobe.com)を選択します。 **[!UICONTROL 監視]** 左側のナビゲーションメニューで、 **[!UICONTROL エンドツーエンドのストリーミング]**.

「**[!UICONTROL ストリーミングエンドツーエンド]**」の監視ページが表示されます。このワークスペースには、が受信したストリーミングイベントの割合を示すグラフが表示されます。 [!DNL Platform]：で正常に処理されたストリーミングイベントの割合を示すグラフ。 [[!DNL Real-Time Customer Profile]](../../profile/home.md)、および受信データの詳細なリスト。

![](../images/quality/monitor-data-flows/list-streams.png)

デフォルトでは、上部のグラフには、過去 7 日間の取り込み率が表示されます。 ハイライト表示されたボタンを選択して、この日付範囲を調整し、様々な期間を表示できます。

![](../images/quality/monitor-data-flows/events-received.png)

下部のグラフには、が正常に処理したストリーミングイベントの割合が次の値で表示されます： [!DNL Profile] 過去 7 日間 ハイライト表示されたボタンを選択して、この日付範囲を調整し、様々な期間を表示できます。

>[!NOTE]
>
>データをこのグラフに表示するには、次の条件を満たす必要があります。 **明示的に** 有効 [!DNL Profile]. のストリーミングデータを有効にする方法を学ぶには [!DNL Profile]、 [データセットユーザーガイド](../../catalog/datasets/user-guide.md#enable-a-dataset-for-real-time-customer-profile).

![](../images/quality/monitor-data-flows/ingested-by-profile.png)

グラフの下には、上に示した日付範囲に対応するすべてのストリーミング取り込みレコードのリストが表示されます。 リストの各バッチには、ID、データセット名、最終アップデート日時、バッチ内のレコード数、エラー数（エラーがある場合）が表示されます。任意のレコードを選択して、そのレコードに関する詳細情報を表示できます。

![](../images/quality/monitor-data-flows/streams.png)

### ストリーミングレコードの表示

正常にストリーミングされたレコードの詳細には、取得されたレコードの数、ファイルサイズ、取得の開始時刻および終了時刻などの情報が表示されます。

![](../images/quality/monitor-data-flows/successful-streaming.png)

失敗したストリーミングレコードの詳細には、成功したレコードと同じ情報が表示されます。

![](../images/quality/monitor-data-flows/failed-batch.png)

また、失敗したレコードは、バッチの処理中に発生したエラーの詳細を提供します。 次の例では、データの変換または検証時に解析エラーが発生していました。

>[!NOTE]
>
>取り込まれた行にエラーがある場合、それらの行は **not** 結果として生成されるメッセージが無効な XDM にならない限り、は削除されます。

![](../images/quality/monitor-data-flows/failed-batch-error.png)

## バッチエンドツーエンドデータの取得の監視

内 [[!DNL Experience Platform UI]](https://platform.adobe.com)を選択します。 **[!UICONTROL 監視]** をクリックします。

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
>取り込まれた行にエラーがある場合、それらの行は **not** 結果として生成されるメッセージが無効な XDM にならない限り、は削除されます。

![](../images/quality/monitor-data-flows/failed-streaming-error.png)
