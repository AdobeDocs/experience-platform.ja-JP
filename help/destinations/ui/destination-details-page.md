---
keywords: 宛先；宛先；宛先の詳細ページ；宛先の詳細ページ
title: 宛先の詳細を表示
description: 個々の宛先の詳細ページには、宛先の詳細の概要が表示されます。 宛先の詳細には、宛先名、ID、宛先にマッピングされたオーディエンス、アクティベーションを編集し、データフローを有効または無効にするためのコントロールが含まれます。
exl-id: e44e2b2d-f477-4516-8a47-3e95c2d85223
source-git-commit: 5e3c4f5c9a5540e0a796785c743a77c1e11821f8
workflow-type: tm+mt
source-wordcount: '1100'
ht-degree: 10%

---

# 宛先の詳細を表示

## 概要 {#overview}

Adobe Experience Platformユーザーインターフェイスで、宛先の属性とアクティビティを表示および監視できます。 これらの詳細には、宛先の名前と ID、宛先をアクティブ化または無効化するためのコントロールなどが含まれます。 詳細には、アクティブ化されたプロファイルレコードの指標、アクティブ化された ID、失敗した ID、除外された ID、データフロー実行の履歴も含まれます。

>[!NOTE]
>
>宛先の詳細ページは、 [!UICONTROL 宛先] ワークスペース ( [!DNL Platform] [!DNL UI]. 詳しくは、 [[!UICONTROL 宛先] workspace の概要](./destinations-workspace.md) を参照してください。

## 宛先の詳細を表示 {#view-details}

既存の宛先に関する詳細を表示するには、以下の手順に従います。

1. [Experience Platform UI](https://platform.adobe.com/) にログインし、左側のナビゲーションバーから「**[!UICONTROL 宛先]**」を選択します。選択 **[!UICONTROL 参照]** 上部のヘッダーから、既存の宛先を表示します。

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

| 右側のレールの項目 | 説明 |
| --- | --- |
| [!UICONTROL オーディエンスをアクティブ化] | このコントロールを選択して、宛先にマッピングされているオーディエンスを編集したり、書き出しスケジュールを更新したり、マッピングされた属性と ID を追加および削除したりします。 詳しくは、 [オーディエンスストリーミング宛先へのオーディエンスデータのアクティブ化](./activate-segment-streaming-destinations.md), [プロファイルベースの宛先へのオーディエンスデータのアクティブ化](./activate-batch-profile-destinations.md)、および [ストリーミングプロファイルベースの宛先に対するオーディエンスデータのアクティブ化](./activate-streaming-profile-destinations.md) を参照してください。 |
| [!UICONTROL 削除] | このデータフローを削除し、以前アクティブ化したオーディエンス（存在する場合）のマッピングを解除できます。 |
| [!UICONTROL 宛先名] | このフィールドは、宛先の名前を更新するために編集できます。 |
| [!UICONTROL 説明] | このフィールドは、オプションで宛先を更新または追加するために編集できます。 |
| [!UICONTROL 宛先] | オーディエンスの宛先プラットフォームを表します。詳しくは、 [宛先カタログ](../catalog/overview.md) を参照してください。 |
| [!UICONTROL ステータス] | 宛先が有効か無効かを示します。 |
| [!UICONTROL マーケティングアクション] | この宛先に適用される、データガバナンスの目的でのマーケティングアクション（使用例）を示します。 |
| [!UICONTROL カテゴリ] | 宛先のタイプを示します。 詳しくは、 [宛先カタログ](../catalog/overview.md) を参照してください。 |
| [!UICONTROL 接続タイプ] | オーディエンスを宛先に送信する際に使用するフォームを示します。 以下の値を指定できます。 [!UICONTROL Cookie] および [!UICONTROL プロファイルベース]. |
| [!UICONTROL 頻度] | オーディエンスが宛先に送信される頻度を示します。以下の値を指定できます。 [!UICONTROL ストリーミング] および [!UICONTROL バッチ]. |
| [!UICONTROL ID] | 宛先で受け入れられる ID 名前空間を表します（例： ）。 `GAID`, `IDFA`または `email`. 受け入れられる ID 名前空間について詳しくは、 [ID 名前空間の概要](../../identity-service/features/namespaces.md). |
| [!UICONTROL 作成者] | この宛先を作成したユーザーを示します。 |
| [!UICONTROL 作成日] | この宛先が作成された時点の UTC 日時を示します。 |

{style="table-layout:auto"}

## [!UICONTROL 有効]/[!UICONTROL 無効] トグル {#enabled-disabled-toggle}

以下を使用すると、 **[!UICONTROL 有効]/[!UICONTROL 無効]** 宛先へのすべてのデータ書き出しを開始および一時停止する切り替え。

![データフローの切り替えを有効または無効にする](../assets/ui/details-page/enable-disable.png)

## [!UICONTROL データフローの実行] {#dataflow-runs}

The [!UICONTROL データフローの実行] 「 」タブには、バッチ宛先とストリーミング宛先に対するデータフロー実行の指標データが表示されます。 参照： [データフローの監視](monitor-dataflows.md) を参照してください。

>[!NOTE]
>
>* 宛先監視機能は、現在、Experience Platform内のすべての宛先でサポートされています *例外* の [Adobe Target](/help/destinations/catalog/personalization/adobe-target-connection.md), [カスタムパーソナライゼーション](/help/destinations/catalog/personalization/custom-personalization.md) および [Experience Cloudオーディエンス](/help/destinations/catalog/adobe/experience-cloud-audiences.md) 宛先。
>* の [Amazon Kinesis](/help/destinations/catalog/cloud-storage/amazon-kinesis.md), [Azure イベントハブ](/help/destinations/catalog/cloud-storage/azure-event-hubs.md)、および [HTTP API](/help/destinations/catalog/streaming/http-destination.md) 宛先、除外された id、失敗した id、アクティブ化された id に関連する指標は、推定されます。 アクティベーションデータの量が多いと、指標の精度が高くなります。

![データフロー実行ビュー](../assets/ui/details-page/dataflow-runs.png)

### データフロー実行時間 {#dataflow-runs-duration}

ストリーミングとファイルベースの宛先の間で、データフローの実行の表示時間に違いがあります。

### ストリーミングの宛先 {#streaming}

また、 **[!UICONTROL 処理時間]** ほとんどのストリーミングデータフローの実行については、約 4 時間で示されます。以下の図に示すように、データフローの実際の処理時間は、データフローの実行については、はるかに短くなります。 Experience Platformが宛先への呼び出しを再試行する必要がある場合に、データフローの実行ウィンドウを長時間開いたままにし、また、到着が遅れたデータを同じ時間枠で逃さないようにします。

![ストリーミング宛先の「処理時間」列が強調表示された、データフロー実行ページの画像。](/help/destinations/assets/ui/details-page/processing-time-dataflow-run-streaming.png)

詳しくは、 [データフローの実行をストリーミング先に](/help/dataflows/ui/monitor-destinations.md#dataflow-runs-for-streaming-destinations) （監視に関するドキュメント）

### ファイルベースの宛先 {#file-based}

データフローをファイルベースの宛先に実行する場合、 **[!UICONTROL 処理時間]** 書き出すデータのサイズとシステム負荷によって異なります。 また、データフローは、ファイルベースの宛先に対して実行され、オーディエンスごとに分類されます。

![ファイルベースの宛先に対して「処理時間」列が強調表示された、データフロー実行ページの画像。](/help/destinations/assets/ui/details-page/processing-time-dataflow-run-file-based.png)

詳しくは、 [データフローは、バッチ（ファイルベース）の宛先に対して実行されます。](/help/dataflows/ui/monitor-destinations.md#dataflow-runs-for-batch-destinations) （監視に関するドキュメント）

## [!UICONTROL アクティベーションデータ] {#activation-data}

The [!UICONTROL アクティベーションデータ] 「 」タブには、宛先にマッピングされたオーディエンスのリストが表示されます。これには、開始日と終了日（該当する場合）、およびデータエクスポートに関するその他の関連情報（エクスポートのタイプ、スケジュール、頻度など）が含まれます。 特定のオーディエンスに関する詳細を表示するには、リストからオーディエンスの名前を選択します。

>[!TIP]
>
>宛先にマッピングされている属性と ID に関する詳細を表示および編集するには、「 」を選択します **[!UICONTROL オーディエンスをアクティブ化]** （内） [右レール](#right-rail).

![アクティベーションデータ表示のバッチ保存先](../assets/ui/details-page/activation-data-batch.png)

![アクティベーションデータビューのストリーミング先](../assets/ui/details-page/activation-data-streaming.png)

<!-- ### Remove multiple audiences from activation flows {#bulk-remove}

To remove multiple audiences from existing activation flows, select the audiences and then select **[!UICONTROL Remove audiences]**.

![Activation data screen highlighting the Remove audiences option.](../assets/ui/details-page/bulk-remove-audiences.png) -->

### [!BADGE ベータ版]{type=Informative} 複数のファイルをオンデマンドでバッチ保存先に書き出す {#bulk-export}

>[!NOTE]
>
この機能はベータ版で、一部のお客様のみが利用できます。 この機能へのアクセスをリクエストするには、Adobe担当者にお問い合わせください。

以下が可能です。 [複数のファイルをオンデマンドで書き出し](../ui/export-file-now.md) から **[!UICONTROL アクティベーションデータ]** ページに貼り付けます。 これをおこなうには、ファイルをオンデマンドでエクスポートするオーディエンスを選択し、 **[!UICONTROL ファイルを今すぐ書き出し]** を制御して、1 回限りのエクスポートをトリガーし、選択した各オーディエンスのファイルをバッチ保存先に配信します。

![「ファイルを今すぐ書き出し」ボタンをハイライトした画像。](../assets/ui/details-page/bulk-export-file-now.png)

### [!BADGE ベータ版]{type=Informative} バッチ保存先にエクスポートされた複数のオーディエンスのアクティベーションスケジュールを編集します {#bulk-edit-schedule}

>[!NOTE]
>
この機能はベータ版で、一部のお客様のみが利用できます。 この機能へのアクセスをリクエストするには、Adobe担当者にお問い合わせください。

複数のオーディエンスの既存のアクティベーションスケジュールを同時に編集するには、目的のオーディエンスを選択してから、「 」を選択します。 **[!UICONTROL スケジュールを編集]**. 書き出しスケジュールを定義または編集する方法について詳しくは、 [オーディエンスの書き出しをスケジュール](../ui/activate-batch-profile-destinations.md#scheduling) 」セクションに入力します。

![複数のオーディエンスのアクティベーションスケジュールを編集するオプションをハイライトするアクティベーションデータ画面。](../assets/ui/details-page/bulk-edit-schedule.png)

>[!NOTE]
>
オーディエンスの詳細ページについて詳しくは、 [セグメント化 UI の概要](../../segmentation/ui/overview.md#segment-details).
