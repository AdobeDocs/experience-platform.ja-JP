---
title: Chapter Details コレクションのデータタイプ
description: Chapter Details Collection Experience Data Model(XDM) のデータ型について説明します。
source-git-commit: c6108bb692c80c79e115ac7b1488d95a7039ffcf
workflow-type: tm+mt
source-wordcount: '193'
ht-degree: 10%

---

# [!UICONTROL チャプターの詳細] コレクションデータタイプ

[!UICONTROL チャプターの詳細] コレクションは、メディアコンテンツ内のチャプターまたはセグメントに関連する様々な属性を記述する標準の Experience Data Model(XDM) データ型です。 以下を使用します。 [!UICONTROL チャプターの詳細] チャプター名、オフセット、期間、チャプターインデックスなどの詳細をキャプチャするためのコレクションデータ型です。 メディアコレクションフィールドは、データをキャプチャして他のAdobe サービスに送信し、さらに処理します。

![Chapter Details Collection データ型の図です。](../images/data-types/chapter-details-collection.png)

>[!NOTE]
>
>各ディスプレイ名には、オーディオおよびビデオパラメーターに関する詳細情報へのリンクが含まれます。 リンクされたページには、Adobe、実装値、ネットワークパラメーター、レポートおよび重要な考慮事項によって収集されるビデオ広告データの詳細が含まれます。

| 表示名 | プロパティ | データタイプ | 必須 | 説明 |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------|-----------|----------|---------------------------------------------------|
| [[!UICONTROL チャプターの長さまたは期間]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/chapter-parameters.html#chapter-length) | `length` | 整数 | ○ | チャプターの長さ（秒）。 |
| [[!UICONTROL チャプター名]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/chapter-parameters.html#chapter-name) | `friendlyName` | 文字列 | × | チャプターまたはセグメントの名前。 |
| [[!UICONTROL チャプターオフセット]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/chapter-parameters.html#chapter-offset) | `offset` | 整数 | ○ | 開始時からのコンテンツ内のチャプターのオフセット（秒）。 |
| [[!UICONTROL チャプター位置]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/chapter-parameters.html#chapter-position) | `index` | 整数 | ○ | コンテンツ内のチャプターの位置（インデックス、整数）。 |

{style="table-layout:auto"}
