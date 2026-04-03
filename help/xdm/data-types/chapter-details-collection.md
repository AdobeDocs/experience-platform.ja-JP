---
title: 章詳細収集データタイプ
description: 詳細については、「収集エクスペリエンスデータモデル（XDM）」データタイプを参照してください。
exl-id: 4f841f5a-3840-4da5-a3a4-ceecde87c684
source-git-commit: 58f69a78fb3c622c8741d7a1618f15509c160a5b
workflow-type: tm+mt
source-wordcount: '177'
ht-degree: 12%

---

# [!UICONTROL Chapter Details] コレクション データ型

[!UICONTROL Chapter Details] コレクションは、メディアコンテンツ内のチャプターまたはセグメントに関連する様々な属性を記述する標準のExperience Data Model （XDM） データ型です。 [!UICONTROL Chapter Details] コレクションのデータ型を使用して、章名、オフセット、期間、章インデックスなどの詳細を取得します。 メディアコレクションフィールドは、データを取り込み、さらなる処理のために他のAdobe サービスに送信します。

![章の詳細収集データ型の図。](../images/data-types/chapter-details-collection.png)

>[!NOTE]
>
>各表示名には、オーディオおよびビデオのパラメーターに関する詳細情報へのリンクが含まれています。 リンクされたページには、Adobeによって収集されたビデオとデータ、実装値、ネットワークパラメーター、レポート、および重要な考慮事項の詳細が含まれています。

| 表示名 | プロパティ | データタイプ | 必須 | 説明 |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------|-----------|----------|---------------------------------------------------|
| [[!UICONTROL Chapter Length Or Duration]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/chapter-parameters.html#chapter-length) | `length` | 整数 | ○ | チャプターの長さ（秒）。 |
| [[!UICONTROL Chapter Name]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/chapter-parameters.html#chapter-name) | `friendlyName` | 文字列 | × | 章やセグメントの名前。 |
| [[!UICONTROL Chapter Offset]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/chapter-parameters.html#chapter-offset) | `offset` | 整数 | ○ | コンテンツ内の章の先頭からのオフセット（秒単位）。 |
| [[!UICONTROL Chapter Position]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/chapter-parameters.html#chapter-position) | `index` | 整数 | ○ | コンテンツ内の章の位置（インデックス、整数）。 |

{style="table-layout:auto"}
