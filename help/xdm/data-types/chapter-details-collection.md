---
title: チャプターの詳細コレクションのデータタイプ
description: チャプター詳細収集エクスペリエンスデータモデル（XDM）データタイプについて説明します。
exl-id: 4f841f5a-3840-4da5-a3a4-ceecde87c684
source-git-commit: 799a384556b43bc844782d8b67416c7eea77fbf0
workflow-type: tm+mt
source-wordcount: '193'
ht-degree: 11%

---

# [!UICONTROL &#x200B; チャプターの詳細 &#x200B;] 収集データタイプ

[!UICONTROL &#x200B; チャプターの詳細 &#x200B;] コレクションは、メディアコンテンツ内のチャプターまたはセグメントに関連する様々な属性を記述する、標準の Experience Data Model （XDM）データタイプです。 [!UICONTROL &#x200B; チャプターの詳細 &#x200B;] コレクション データ型を使用すると、チャプター名、オフセット、期間、チャプターインデックスなどの詳細をキャプチャできます。 メディアコレクションフィールドは、データをキャプチャし、さらに処理するために他のAdobe サービスに送信します。

![ チャプター詳細コレクションのデータタイプを示す図。](../images/data-types/chapter-details-collection.png)

>[!NOTE]
>
>各表示名には、オーディオおよびビデオパラメーターに関する詳細情報へのリンクが含まれています。 リンク先のページには、Adobeで収集された動画広告データの詳細、実装値、ネットワークパラメーター、レポート、重要な検討事項が含まれています。

| 表示名 | プロパティ | データタイプ | 必須 | 説明 |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------|-----------|----------|---------------------------------------------------|
| [[!UICONTROL &#x200B; 章の長さ又は存続期間 &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/chapter-parameters.html?lang=ja#chapter-length) | `length` | 整数 | ○ | チャプターの長さ（秒）。 |
| [[!UICONTROL &#x200B; チャプター名 &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/chapter-parameters.html?lang=ja#chapter-name) | `friendlyName` | 文字列 | × | チャプターまたはセグメント（あるいはその両方）の名前。 |
| [[!UICONTROL &#x200B; チャプターオフセット &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/chapter-parameters.html?lang=ja#chapter-offset) | `offset` | 整数 | ○ | 開始時からのコンテンツ内のチャプターのオフセット （秒）。 |
| [[!UICONTROL &#x200B; 章の位置 &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/chapter-parameters.html?lang=ja#chapter-position) | `index` | 整数 | ○ | コンテンツ内のチャプターの位置（インデックス、整数）。 |

{style="table-layout:auto"}
