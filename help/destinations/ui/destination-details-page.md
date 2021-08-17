---
keywords: 宛先；宛先；宛先の詳細ページ；宛先の詳細ページ
title: 宛先の詳細の表示
description: '個々の宛先の詳細ページには、宛先の詳細の概要が表示されます。 宛先の詳細には、宛先名、ID、宛先にマッピングされたセグメント、アクティベーションを編集し、データフローを有効/無効にするためのコントロールが含まれます。 '
seo-description: 個々の宛先の詳細ページには、宛先の詳細の概要が表示されます。 宛先の詳細には、宛先名、ID、宛先にマッピングされたセグメント、アクティベーションを編集し、データフローを有効/無効にするためのコントロールが含まれます。
exl-id: e44e2b2d-f477-4516-8a47-3e95c2d85223
source-git-commit: a619227de30513bb06a22ce7b4f2fc13847c1ab6
workflow-type: tm+mt
source-wordcount: '668'
ht-degree: 6%

---

# 宛先の詳細の表示

## 概要 {#overview}

Adobe Experience Platformユーザーインターフェイスで、宛先の属性とアクティビティを表示および監視できます。 これらの詳細には、宛先の名前とID、宛先をアクティブ化または無効化するためのコントロールなどが含まれます。 バッチ宛先の詳細には、アクティブ化されたプロファイルレコードの指標とデータフロー実行の履歴も含まれます。

>[!NOTE]
>
>宛先の詳細ページは、[!DNL Platform] [!DNL UI]の[!UICONTROL 宛先]ワークスペースに含まれています。 詳しくは、「[[!UICONTROL 宛先]ワークスペースの概要](./destinations-workspace.md)」を参照してください。

## 宛先の詳細の表示 {#view-details}

既存の宛先に関する詳細を表示するには、以下の手順に従います。

1. [Experience PlatformUI](https://platform.adobe.com/)にログインし、左側のナビゲーションバーから「**[!UICONTROL 宛先]**」を選択します。 上部のヘッダーから「**[!UICONTROL 参照]**」を選択して、既存の宛先を表示します。

   ![宛先の参照](../assets/ui/details-page/browse-destinations.png)

1. 左上のフィルターアイコン![フィルターアイコン](../assets/ui/details-page/filter.png)を選択して、並べ替えパネルを起動します。 並べ替えパネルには、すべての宛先のリストが表示されます。 リストから複数の宛先を選択して、選択した宛先に関連付けられたデータフローのフィルター選択を表示できます。

   ![宛先のフィルター](../assets/ui/details-page/filter-destinations.png)

1. 表示する宛先の名前を選択します。

   ![宛先の選択](../assets/ui/details-page/destination-select.png)

1. 宛先の詳細ページが表示され、使用可能なコントロールが示されます。 バッチ保存先の詳細を表示している場合は、監視ダッシュボードも表示されます。

   ![宛先の詳細](../assets/ui/details-page/destination-details.png)

## 右パネル

右側のレールには、選択した宛先に関する基本情報が表示されます。

![右パネル](../assets/ui/details-page/right-sidebar.png)

次の表に、右側のパネルに表示されるコントロールと詳細を示します。

| 右パネルの項目 | 説明 |
| --- | --- |
| [!UICONTROL アクティブ化] | このコントロールを選択して、宛先にマッピングされるセグメントを編集します。 詳しくは、[ストリーミング先](./activate-segment-streaming-destinations.md)をセグメント化するオーディエンスデータのアクティブ化[プロファイルベースのバッチ宛先](./activate-batch-profile-destinations.md)に対するオーディエンスデータのアクティブ化[のガイドを参照してください。](./activate-streaming-profile-destinations.md) |
| [!UICONTROL 削除] | このデータフローを削除し、以前アクティブ化したセグメントが存在する場合は、そのセグメントのマッピングを解除できます。 |
| [!UICONTROL 宛先名] | このフィールドは、宛先の名前を更新するために編集できます。 |
| [!UICONTROL 説明] | このフィールドは、オプションで宛先の説明を更新または追加するために編集できます。 |
| [!UICONTROL 宛先] | オーディエンスの宛先プラットフォームを表します。詳しくは、[宛先カタログ](../catalog/overview.md)を参照してください。 |
| [!UICONTROL ステータス] | 宛先が有効か無効かを示します。 |
| [!UICONTROL マーケティングアクション] | データガバナンスの目的でこの宛先に適用されるマーケティングアクション（使用例）を示します。 |
| [!UICONTROL カテゴリ] | 宛先のタイプを示します。 詳しくは、[宛先カタログ](../catalog/overview.md)を参照してください。 |
| [!UICONTROL 接続タイプ] | オーディエンスが宛先に送信されるフォームを示します。 [!UICONTROL Cookie]および[!UICONTROL プロファイルベース]の値を指定できます。 |
| [!UICONTROL 頻度] | オーディエンスが宛先に送信される頻度を示します。[!UICONTROL ストリーミング]と[!UICONTROL バッチ]などの値を指定できます。 |
| [!UICONTROL ID] | `GAID`、`IDFA`、`email`など、宛先で受け入れられるID名前空間を表します。 受け入れられるID名前空間について詳しくは、[ID名前空間の概要](../../identity-service/namespaces.md)を参照してください。 |
| [!UICONTROL 作成者] | この宛先を作成したユーザーを示します。 |
| [!UICONTROL 作成] | この宛先が作成されたUTCの日時を示します。 |

{style=&quot;table-layout:auto&quot;}

## [!UICONTROL 有効]/ 無効切り替え

「**[!UICONTROL 有効]/[!UICONTROL 無効]**」切り替えを使用して、宛先へのすべてのデータ書き出しを開始および一時停止できます。

![無効切り替えを有効にする](../assets/ui/details-page/enable-disable.png)

## [!UICONTROL データフローの実行]

[!UICONTROL 「Dataflow runs]」タブは、バッチ宛先へのデータフロー実行の指標データを提供します。 詳しくは、[データフローの監視](monitor-dataflows.md)を参照してください。

## [!UICONTROL アクティベーションデータ] {#activation-data}

「[!UICONTROL アクティベーションデータ]」タブには、宛先にマッピングされたセグメントのリストが表示されます。このリストには、開始日と終了日（該当する場合）が含まれます。 特定のセグメントの詳細を表示するには、リストからセグメント名を選択します。

![アクティベーションデータ](../assets/ui/details-page/activation-data.png)

>[!NOTE]
>
>セグメントの詳細ページについて詳しくは、[セグメント化UIの概要](../../segmentation/ui/overview.md#segment-details)を参照してください。
