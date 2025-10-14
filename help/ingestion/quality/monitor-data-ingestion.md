---
keywords: Experience Platform；ホーム；人気のトピック；監視；監視；データフロー；取り込みの監視；データ取り込み；レコードの表示；バッチの表示；
solution: Experience Platform
title: データ取り込みの監視
description: このユーザーガイドでは、Adobe Experience Platform ユーザーインターフェイス内でデータを監視する方法の手順を説明します。このガイドでは、Adobe ID を持っていて、Adobe Experience Platform にアクセスできる必要があります。
exl-id: 85711a06-2756-46f9-83ba-1568310c9f73
source-git-commit: b48c24ac032cbf785a26a86b50a669d7fcae5d97
workflow-type: tm+mt
source-wordcount: '649'
ht-degree: 37%

---

# データ取得の監視

データ取得を使用すると、Adobe Experience Platform でデータを取得できます。バッチ取り込みを使用すると、様々なファイルタイプ（CSV など）でデータを挿入できます。または、ストリーミング取り込みを使用すると、ストリーミングエンドポイントを使用してリアルタイムにデータを [!DNL Experience Platform] に取り込むことができます。

このユーザガイドでは、Adobe Experience Platform ユーザーインターフェイス内でデータをモニタリングする手順を説明します。 このガイドでは、Adobe ID を持っていて、Adobe Experience Platform にアクセスできる必要があります。

## ストリーミングエンドツーエンドデータの取得の監視 {#monitor-streaming-end-to-end-data-ingestion}

>[!CONTEXTUALHELP]
>id="platform_ingestion_streaming_ingestionrate"
>title="取り込みレート"
>abstract="正常に処理された 1 秒間あたりのイベントの数。"
>text="Learn more in the documentation"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/dataflows/ui/monitor-sources.html?lang=ja" text="UI でソースのデータフローを監視"

>[!TIP]
>
>特定の日付の合計イベント数を計算するには、式 `total events / day = ingestion rate * 60 * 60 * 24` を使用します。

[Experience Platform UI](https://platform.adobe.com) で、左側のナビゲーションメニューの **[!UICONTROL モニタリング]** を選択し、続いて **[!UICONTROL エンドツーエンドのストリーミング]** を選択します。

「**[!UICONTROL ストリーミングエンドツーエンド]**」の監視ページが表示されます。このワークスペースには、[!DNL Experience Platform] が受信したストリーミングイベントの割合を表示するグラフ、[[!DNL Real-Time Customer Profile]](../../profile/home.md) が正常に処理したストリーミングイベントの割合を表示するグラフおよび受信データの詳細なリストが表示されます。

![](../images/quality/monitor-data-flows/list-streams.png)

デフォルトでは、上部のグラフには、過去 7 日間の取り込み率が表示されます。 この日付範囲は、ハイライト表示されたボタンを選択することで、様々な期間を表示するように調整できます。

![](../images/quality/monitor-data-flows/events-received.png)

下のグラフは、過去 7 日間に [!DNL Profile] で処理され、正常にストリーミングされたイベントの割合を示します。 この日付範囲は、ハイライト表示されたボタンを選択することで、様々な期間を表示するように調整できます。

>[!NOTE]
>
>このグラフにデータを表示するには、データを [!DNL Profile] で **明示的に** 有効にする必要があります。 [!DNL Profile] のストリーミングデータを有効にする方法については、[&#x200B; データセットユーザーガイド &#x200B;](../../catalog/datasets/user-guide.md#enable-a-dataset-for-real-time-customer-profile) を参照してください。

![](../images/quality/monitor-data-flows/ingested-by-profile.png)

グラフの下には、上記に表示された日付範囲に対応するすべてのストリーミング取り込みレコードのリストがあります。 リストの各バッチには、ID、データセット名、最終アップデート日時、バッチ内のレコード数、エラー数（エラーがある場合）が表示されます。レコードの詳細を表示するには、任意のレコードを選択します。

![](../images/quality/monitor-data-flows/streams.png)

### ストリーミングレコードの表示

正常にストリーミングされたレコードの詳細には、取得されたレコードの数、ファイルサイズ、取得の開始時刻および終了時刻などの情報が表示されます。

![](../images/quality/monitor-data-flows/successful-streaming.png)

失敗したストリーミングレコードの詳細には、成功したレコードと同じ情報が表示されます。

![](../images/quality/monitor-data-flows/failed-batch.png)

また、失敗したレコードには、バッチの処理中に発生したエラーの詳細も表示されます。 次の例では、データの変換または検証時に解析エラーが発生しました。

>[!NOTE]
>
>取り込んだ行にエラーがある場合、結果のメッセージで無効な XDM が生成されない限り、これらの行は削除 **されません**。

![](../images/quality/monitor-data-flows/failed-batch-error.png)

## バッチエンドツーエンドデータの取得の監視

[[!DNL Experience Platform UI]](https://platform.adobe.com) で、左側のナビゲーションメニューの **[!UICONTROL モニタリング]** を選択します。

「**[!UICONTROL バッチエンドツーエンド]**」の監視ページが開き、以前に取得したバッチのリストが表示されます。そのレコードに関する詳細情報については、任意のバッチを選択できます。

![](../images/quality/monitor-data-flows/batch-monitoring.png)

### バッチの表示

成功したバッチの詳細には、取得されたレコードの数、ファイルサイズ、取得の開始時刻および終了時刻などの情報が表示されます。

![](../images/quality/monitor-data-flows/successful-batch.png)

失敗したバッチの詳細には、成功したバッチと同じ情報に加えて、失敗したレコードの数が表示されます。

![](../images/quality/monitor-data-flows/failed-batch.png)

また、失敗したバッチには、バッチの処理中に発生したエラーの詳細が表示されます。 次の例では、取り込んだバッチにエラーが発生しました。これは、その人物の ID の最大数があるためです。

>[!NOTE]
>
>取り込んだ行にエラーがある場合、結果のメッセージで無効な XDM が生成されない限り、これらの行は削除 **されません**。

![](../images/quality/monitor-data-flows/failed-streaming-error.png)
