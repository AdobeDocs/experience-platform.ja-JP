---
title: 章詳細レポートデータタイプ
description: 詳細レポート エクスペリエンスデータモデル（XDM）データタイプについて説明します。
exl-id: 73ebfbe3-66c3-4ef9-9944-d9cb5772127b
source-git-commit: 58f69a78fb3c622c8741d7a1618f15509c160a5b
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 9%

---

# [!UICONTROL Chapter Details] レポート データ型

[!UICONTROL Chapter Details] レポートは、メディアコンテンツ内のチャプターまたはセグメントに関連する様々な属性を記述する標準エクスペリエンスデータモデル （XDM） データタイプです。 [!UICONTROL Chapter Details] レポート データ型を使用して、章名、期間、位置、ID、再生ステータス （開始/完了）、各章に費やした時間などの詳細を取得します。 Media Reporting フィールドは、Adobe サービスが送信したMedia Collection フィールドを分析するために使用されます。 このデータは、他の特定のユーザー指標とともに計算され、レポートされます。

![章の詳細レポート データ タイプの図。](../images/data-types/chapter-details-reporting.png)

>[!NOTE]
>
>各表示名には、オーディオおよびビデオのパラメーターに関する詳細情報へのリンクが含まれています。 リンクされたページには、Adobeによって収集されたビデオとデータ、実装値、ネットワークパラメーター、レポート、および重要な考慮事項の詳細が含まれています。

| 表示名 | プロパティ | データタイプ | 説明 |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------|-----------|--------------------------------------------------------------|
| [[!UICONTROL Chapter Completed]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/chapter-parameters.html?lang=ja#chapter-complete) | `isCompleted` | ブール値 | 章が完了したかどうかを確認します。 |
| [[!UICONTROL Chapter ID]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/chapter-parameters.html?lang=ja#chapter) | `ID` | 文字列 | 章の自動生成されたID。 |
| [[!UICONTROL Chapter Length Or Duration]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/chapter-parameters.html?lang=ja#chapter-length) | `length` | 整数 | チャプターの長さ（秒）。 |
| [[!UICONTROL Chapter Name]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/chapter-parameters.html?lang=ja#chapter-name) | `friendlyName` | 文字列 | 章やセグメントの名前。 |
| [[!UICONTROL Chapter Offset]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/chapter-parameters.html?lang=ja#chapter-offset) | `offset` | 整数 | コンテンツ内の章の先頭からのオフセット（秒単位）。 |
| [[!UICONTROL Chapter Position]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/chapter-parameters.html?lang=ja#chapter-position) | `index` | 整数 | コンテンツ内の章の位置（インデックス、整数）。 |
| [[!UICONTROL Chapter Started]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/chapter-parameters.html?lang=ja#chapter-start) | `isStarted` | ブール値 | 章が始まったか否か。 |
| [[!UICONTROL Chapter Time Played]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/chapter-parameters.html?lang=ja#chapter-time-spent) | `timePlayed` | 整数 | 章に費やした時間（秒単位）。 |
