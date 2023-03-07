---
keywords: 宛先；宛先；宛先の詳細ページ；宛先の詳細ページ
title: 宛先の詳細を表示
description: 個々の宛先の詳細ページには、宛先の詳細の概要が表示されます。 宛先の詳細には、宛先名、ID、宛先にマッピングされたセグメント、アクティベーションを編集し、データフローを有効/無効にするためのコントロールが含まれます。
exl-id: e44e2b2d-f477-4516-8a47-3e95c2d85223
source-git-commit: 0a300660ce0fc53c403d2ceeb3d4d7d2c32ac117
workflow-type: tm+mt
source-wordcount: '912'
ht-degree: 21%

---

# 宛先の詳細を表示

## 概要 {#overview}

Adobe Experience Platformユーザーインターフェイスで、宛先の属性とアクティビティを表示および監視できます。 これらの詳細には、宛先の名前と ID、宛先をアクティブ化または無効化するためのコントロールなどが含まれます。 詳細には、アクティブ化されたプロファイルレコードの指標、アクティブ化された ID、失敗した ID、除外された ID、データフロー実行の履歴も含まれます。

>[!NOTE]
>
>宛先の詳細ページは、 [!UICONTROL 宛先] ワークスペース [!DNL Platform] [!DNL UI]. 詳しくは、 [[!UICONTROL 宛先] workspace の概要](./destinations-workspace.md) を参照してください。

## 宛先の詳細を表示 {#view-details}

既存の宛先に関する詳細を表示するには、以下の手順に従います。

1. [Experience Platform UI](https://platform.adobe.com/) にログインし、左側のナビゲーションバーから「**[!UICONTROL 宛先]**」を選択します。選択 **[!UICONTROL 参照]** をクリックして、既存の宛先を表示します。

   ![宛先の参照](../assets/ui/details-page/browse-destinations.png)

1. 左上のフィルターアイコン ![フィルターアイコン](../assets/ui/details-page/filter.png) を選択して、並べ替えパネルを開きます。並べ替えパネルには、すべての宛先のリストが表示されます。 リストから複数の宛先を選択して、選択した宛先に関連付けられた特定のデータフローを表示できます。

   ![宛先のフィルタリング](../assets/ui/details-page/filter-destinations.png)

1. 表示する宛先の名前を選択します。

   ![宛先を選択](../assets/ui/details-page/destination-select.png)

1. 宛先の詳細ページが表示され、使用可能なコントロールが示されます。

   ![宛先の詳細](../assets/ui/details-page/destination-details.png)

## 右側のレール {#right-rail}

右側のレールには、選択した宛先に関する基本情報が表示されます。

![右パネル](../assets/ui/details-page/right-sidebar.png)

次の表に、右側のパネルで提供されるコントロールと詳細を示します。

| 右側のレール項目 | 説明 |
| --- | --- |
| [!UICONTROL セグメントのアクティブ化] | このコントロールを選択して、宛先にマッピングされているセグメントを編集したり、書き出しスケジュールを更新したり、マッピングされた属性と ID を追加および削除したりします。 詳しくは、 [ストリーミング宛先をセグメント化するためのオーディエンスデータのアクティブ化](./activate-segment-streaming-destinations.md), [プロファイルベースの宛先へのオーディエンスデータのアクティブ化](./activate-batch-profile-destinations.md)、および [ストリーミングプロファイルベースの宛先に対するオーディエンスデータのアクティブ化](./activate-streaming-profile-destinations.md) を参照してください。 |
| [!UICONTROL 削除] | このデータフローを削除し、以前アクティブ化したセグメントが存在する場合はそのセグメントのマッピングを解除できます。 |
| [!UICONTROL 宛先名] | このフィールドは、宛先の名前を更新するために編集できます。 |
| [!UICONTROL 説明] | このフィールドは、オプションで宛先を更新または追加するために編集できます。 |
| [!UICONTROL 宛先] | オーディエンスの宛先プラットフォームを表します。詳しくは、 [宛先カタログ](../catalog/overview.md) を参照してください。 |
| [!UICONTROL ステータス] | 宛先が有効か無効かを示します。 |
| [!UICONTROL マーケティングアクション] | この宛先に適用される、データガバナンス目的のマーケティングアクション（使用例）を示します。 |
| [!UICONTROL カテゴリ] | 宛先のタイプを示します。 詳しくは、 [宛先カタログ](../catalog/overview.md) を参照してください。 |
| [!UICONTROL 接続タイプ] | オーディエンスを宛先に送信する際に使用するフォームを示します。 以下の値を指定できます。 [!UICONTROL Cookie] および [!UICONTROL プロファイルベース]. |
| [!UICONTROL 頻度] | オーディエンスが宛先に送信される頻度を示します。以下の値を指定できます。 [!UICONTROL ストリーミング] および [!UICONTROL バッチ]. |
| [!UICONTROL ID] | 宛先で受け入れられる ID 名前空間を表します（例： ） `GAID`, `IDFA`または `email`. 受け入れられる ID 名前空間について詳しくは、 [id 名前空間の概要](../../identity-service/namespaces.md). |
| [!UICONTROL 作成者] | この宛先を作成したユーザーを示します。 |
| [!UICONTROL 作成日] | この宛先が作成された時点の UTC 日時を示します。 |

{style="table-layout:auto"}

## [!UICONTROL 有効]/[!UICONTROL 無効] 切り替え {#enabled-disabled-toggle}

以下を使用して、 **[!UICONTROL 有効]/[!UICONTROL 無効]** 宛先へのすべてのデータ書き出しを開始および一時停止する切り替え。

![データフローの切り替えを有効または無効にする](../assets/ui/details-page/enable-disable.png)

## [!UICONTROL データフローの実行] {#dataflow-runs}

この [!UICONTROL データフローの実行] 「 」タブには、バッチ宛先とストリーミング宛先に対するデータフロー実行の指標データが表示されます。 参照： [データフローの監視](monitor-dataflows.md) を参照してください。

>[!NOTE]
>
>* 宛先監視機能は、現在、Experience Platform内のすべての宛先でサポートされています *例外* の [Adobe Target](/help/destinations/catalog/personalization/adobe-target-connection.md), [カスタムパーソナライゼーション](/help/destinations/catalog/personalization/custom-personalization.md) および [Experience Cloudオーディエンス](/help/destinations/catalog/adobe/experience-cloud-audiences.md) 宛先。
>* [Amazon Kinesis](/help/destinations/catalog/cloud-storage/amazon-kinesis.md) 宛先、[Azure Event Hubs](/help/destinations/catalog/cloud-storage/azure-event-hubs.md) 宛先および [HTTP API](/help/destinations/catalog/streaming/http-destination.md) 宛先については、除外された ID、失敗した ID およびアクティブ化された ID は現在表示されません。


![データフロー実行ビュー](../assets/ui/details-page/dataflow-runs.png)

### データフロー実行時間 {#dataflow-runs-duration}

ストリーミングとファイルベースの宛先の間で、データフローの実行の表示時間に違いがあります。

### ストリーミングの宛先 {#streaming}

また、 **[!UICONTROL 処理時間]** ほとんどのストリーミングデータフローの実行については、約 4 時間で示されます。以下の図に示すように、データフローの実際の処理時間は、データフローの実行に関してははるかに短くなります。 Experience Platformが宛先への呼び出しを再試行する必要がある場合に、データフローの実行ウィンドウを長時間開いたままにし、また、到着が遅れたデータを同じ時間枠で逃さないようにします。

![ストリーミング宛先の「処理時間」列が強調表示された、データフロー実行ページの画像。](/help/destinations/assets/ui/details-page/processing-time-dataflow-run-streaming.png)

詳しくは、 [データフローの実行をストリーミング先に](/help/dataflows/ui/monitor-destinations.md#dataflow-runs-for-streaming-destinations) 監視に関するドキュメント

### ファイルベースの宛先 {#file-based}

データフローをファイルベースの宛先に実行する場合、 **[!UICONTROL 処理時間]** 書き出すデータのサイズとシステム負荷によって異なります。 また、データフローは、ファイルベースの宛先に対して実行され、セグメントごとに分類されます。

![ファイルベースの宛先に対して「処理時間」列が強調表示された、データフロー実行ページの画像。](/help/destinations/assets/ui/details-page/processing-time-dataflow-run-file-based.png)

詳しくは、 [データフローは、バッチ（ファイルベース）の宛先に対して実行されます](/help/dataflows/ui/monitor-destinations.md#dataflow-runs-for-batch-destinations) 監視に関するドキュメント

## [!UICONTROL アクティベーションデータ] {#activation-data}

「[!UICONTROL アクティベーションデータ]」タブには、宛先にマッピングされたセグメントのリストが表示されます。これには、セグメントの開始日と終了日（該当する場合）やデータ書き出しに関連するその他の情報（書き出しタイプ、スケジュール、頻度など）が含まれます。特定のセグメントに関する詳細を表示するには、リストからそのセグメントの名前を選択します。

>[!TIP]
>
>宛先にマッピングされている属性と ID に関する詳細を表示および編集するには、「 」を選択します **[!UICONTROL セグメントのアクティブ化]** 内 [右レール](#right-rail).

![アクティベーションデータ表示のバッチ保存先](../assets/ui/details-page/activation-data-batch.png)

![アクティベーションデータビューのストリーミング先](../assets/ui/details-page/activation-data-streaming.png)

>[!NOTE]
>
>セグメントの詳細ページについて詳しくは、 [セグメント化 UI の概要](../../segmentation/ui/overview.md#segment-details).
