---
keywords: 宛先；宛先；宛先詳細ページ；宛先詳細ページ
title: 表示先の詳細
description: '個々の宛先の詳細ページには、宛先の詳細の概要が表示されます。 宛先の詳細には、宛先名、ID、宛先にマッピングされたセグメント、アクティベーションの編集とデータフローの有効/無効を切り替えるコントロールが含まれます。 '
seo-description: 個々の宛先の詳細ページには、宛先の詳細の概要が表示されます。 宛先の詳細には、宛先名、ID、宛先にマッピングされたセグメント、アクティベーションの編集とデータフローの有効/無効を切り替えるコントロールが含まれます。
exl-id: e44e2b2d-f477-4516-8a47-3e95c2d85223
translation-type: tm+mt
source-git-commit: cc432f7c07f0f82deec653864154016638ec8138
workflow-type: tm+mt
source-wordcount: '651'
ht-degree: 6%

---

# 表示先の詳細

## 概要 {#overview}

Adobe Experience Platformユーザーインターフェイスでは、宛先の属性やアクティビティを表示および監視できます。 これらの詳細には、宛先の名前とID、宛先をアクティブ化または無効化するためのコントロールなどが含まれます。 バッチ宛先の詳細には、アクティブ化されたプロファイルレコードの指標やデータフロー実行の履歴も含まれます。

>[!NOTE]
>
>宛先の詳細ページは、[!DNL Platform] [!DNL UI]の[!UICONTROL 宛先]ワークスペースの一部です。 詳しくは、[[!UICONTROL 宛先]ワークスペースの概要](./destinations-workspace.md)を参照してください。

## 表示宛先の詳細{#view-details}

次の手順に従って、既存の宛先に関する詳細を表示します。

1. [Experience PlatformUI](https://platform.adobe.com/)にログインし、左のナビゲーションバーで「**[!UICONTROL 宛先]**」を選択します。 上部のヘッダーから「**[!UICONTROL 参照]**」を選択し、既存の宛先を表示します。

   ![参照先](../assets/ui/details-page/browse-destinations.png)

1. 左上のフィルターアイコン![Filter-icon](../assets/ui/details-page/filter.png)を選択して、並べ替えパネルを起動します。 並べ替えパネルには、すべての宛先のリストが表示されます。 リストから複数の宛先を選択して、選択した宛先に関連付けられたデータ・フローのフィルタ選択を表示できます。

   ![フィルターの宛先](../assets/ui/details-page/filter-destinations.png)

1. 表示先の名前を選択します。

   ![宛先の選択](../assets/ui/details-page/destination-select.png)

1. 宛先の詳細ページが表示され、使用可能なコントロールが表示されます。 バッチ宛先の詳細を表示している場合は、監視ダッシュボードも表示されます。

   ![宛先の詳細](../assets/ui/details-page/destination-details.png)

## 右側のレール

右側のレールに、選択した宛先に関する基本情報が表示されます。

![右パネル](../assets/ui/details-page/right-sidebar.png)

次の表に、右側のレールに表示されるコントロールと詳細を示します。

| 右側のレール項目 | 説明 |
| --- | --- |
| [!UICONTROL アクティブ化] | このコントロールを選択して、宛先にマッピングされるセグメントを編集します。 詳しくは、[宛先へのセグメントのアクティブ化](./activate-destinations.md)のガイドを参照してください。 |
| [!UICONTROL 削除] | このデータフローを削除し、既にアクティブ化されたセグメントのマッピングを解除できます（存在する場合）。 |
| [!UICONTROL 宛先名] | このフィールドは、宛先名を更新するために編集できます。 |
| [!UICONTROL 説明] | このフィールドは、任意で説明を更新または追加するために編集できます。 |
| [!UICONTROL 宛先] | オーディエンスの宛先プラットフォームを表します。詳しくは、[宛先カタログ](../catalog/overview.md)を参照してください。 |
| [!UICONTROL ステータス] | 宛先が有効か無効かを示します。 |
| [!UICONTROL マーケティングアクション] | データ管理の目的でこの宛先に適用されるマーケティングアクション（使用例）を示します。 |
| [!UICONTROL カテゴリ] | 宛先のタイプを示します。 詳しくは、[宛先カタログ](../catalog/overview.md)を参照してください。 |
| [!UICONTROL 接続タイプ] | オーディエンスが送信先に送信される際に使用されるフォームを示します。 有効な値は、[!UICONTROL Cookie]および[!UICONTROL プロファイルベース]です。 |
| [!UICONTROL 頻度] | オーディエンスが宛先に送信される頻度を示します。有効な値は、[!UICONTROL ストリーミング]と[!UICONTROL バッチ]です。 |
| [!UICONTROL ID] | 宛先が受け入れるID名前空間（`GAID`、`IDFA`、`email`など）を表します。 受け入れられるID名前空間について詳しくは、[ID名前空間の概要](../../identity-service/namespaces.md)を参照してください。 |
| [!UICONTROL 作成者] | この宛先を作成したユーザーを示します。 |
| [!UICONTROL 作成] | この宛先が作成されたUTC日時を示します。 |

{style=&quot;table-layout:auto&quot;}

## [!UICONTROL 有効]/ 無効の切り替え

「**[!UICONTROL 有効]/[!UICONTROL 無効]**」トグルを使用して開始を行い、宛先へのすべてのデータのエクスポートを一時停止できます。

![無効切り替えを有効にする](../assets/ui/details-page/enable-disable.png)

## [!UICONTROL データフローの実行]

[!UICONTROL Dataflow runs]タブは、バッチ宛先に対してデータフローの実行に関する指標データを提供します。 詳しくは、[Monitor dataflows](monitor-dataflows.md)を参照してください。

## [!UICONTROL アクティベーションデータ] {#activation-data}

「[!UICONTROL アクティベーションデータ]」タブには、開始日と終了日（該当する場合）を含む、宛先にマッピングされたセグメントのリストが表示されます。 特定のセグメントの詳細を表示するには、リストからセグメント名を選択します。

![アクティベーションデータ](../assets/ui/details-page/activation-data.png)

>[!NOTE]
>
>セグメントの詳細ページの詳細については、「[セグメントUIの概要](../../segmentation/ui/overview.md#segment-details)」を参照してください。
