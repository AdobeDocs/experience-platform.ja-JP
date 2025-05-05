---
title: チャプターの詳細レポートデータタイプ
description: チャプター詳細レポートの Experience Data Model （XDM）データタイプについて説明します。
exl-id: 73ebfbe3-66c3-4ef9-9944-d9cb5772127b
source-git-commit: 799a384556b43bc844782d8b67416c7eea77fbf0
workflow-type: tm+mt
source-wordcount: '255'
ht-degree: 8%

---

# [!UICONTROL &#x200B; チャプターの詳細 &#x200B;] レポートデータタイプ

[!UICONTROL &#x200B; チャプターの詳細 &#x200B;] レポートは、メディアコンテンツ内のチャプターまたはセグメントに関連する様々な属性を記述する、標準の Experience Data Model （XDM）データタイプです。 [!UICONTROL &#x200B; チャプターの詳細 &#x200B;] レポートデータタイプを使用すると、チャプター名、期間、位置、ID、再生ステータス（開始/完了）、各チャプターでの滞在時間などの詳細をキャプチャできます。 メディアレポートフィールドは、Adobe サービスが送信したメディアコレクションフィールドを分析するためにユーザーが使用します。 このデータは、他の特定のユーザー指標と共に計算され、レポートされます。

![ チャプター詳細レポートデータタイプの図。](../images/data-types/chapter-details-reporting.png)

>[!NOTE]
>
>各表示名には、オーディオおよびビデオパラメーターに関する詳細情報へのリンクが含まれています。 リンク先のページには、Adobeで収集された動画広告データの詳細、実装値、ネットワークパラメーター、レポート、重要な検討事項が含まれています。

| 表示名 | プロパティ | データタイプ | 説明 |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------|-----------|--------------------------------------------------------------|
| [[!UICONTROL &#x200B; チャプター完了 &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/chapter-parameters.html#chapter-complete) | `isCompleted` | ブール値 | チャプターが完了したかどうか。 |
| [[!UICONTROL &#x200B; チャプター ID]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/chapter-parameters.html#chapter) | `ID` | 文字列 | チャプターの自動生成された ID。 |
| [[!UICONTROL &#x200B; 章の長さ又は存続期間 &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/chapter-parameters.html#chapter-length) | `length` | 整数 | チャプターの長さ（秒）。 |
| [[!UICONTROL &#x200B; チャプター名 &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/chapter-parameters.html#chapter-name) | `friendlyName` | 文字列 | チャプターまたはセグメント（あるいはその両方）の名前。 |
| [[!UICONTROL &#x200B; チャプターオフセット &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/chapter-parameters.html#chapter-offset) | `offset` | 整数 | 開始時からのコンテンツ内のチャプターのオフセット （秒）。 |
| [[!UICONTROL &#x200B; 章の位置 &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/chapter-parameters.html#chapter-position) | `index` | 整数 | コンテンツ内のチャプターの位置（インデックス、整数）。 |
| [[!UICONTROL &#x200B; チャプターの概要 &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/chapter-parameters.html#chapter-start) | `isStarted` | ブール値 | チャプターが開始されているかどうか。 |
| [[!UICONTROL &#x200B; チャプター再生時間 &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/chapter-parameters.html#chapter-time-spent) | `timePlayed` | 整数 | チャプターに費やした時間（秒）。 |
