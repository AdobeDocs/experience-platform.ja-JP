---
title: チャプター詳細のレポートデータタイプ
description: チャプターの詳細レポートのエクスペリエンスデータモデル (XDM) データタイプについて説明します。
source-git-commit: fe239bee3c853d43c04200092f59537dfeb00c87
workflow-type: tm+mt
source-wordcount: '255'
ht-degree: 7%

---

# [!UICONTROL チャプターの詳細] レポートのデータタイプ

[!UICONTROL チャプターの詳細] レポートは、メディアコンテンツ内のチャプターまたはセグメントに関連する様々な属性を記述する標準の Experience Data Model(XDM) データ型です。 以下を使用します。 [!UICONTROL チャプターの詳細] チャプター名、期間、位置、ID、再生ステータス（開始/完了）、各チャプターの閲覧時間などの詳細を取り込むためのレポートデータ型です。 メディアレポートフィールドは、Adobe サービスが送信したメディアコレクションフィールドを分析するために使用します。 このデータは、他の特定のユーザー指標と共に、計算され、レポートされます。

![チャプター詳細レポートのデータタイプを示す図です。](../images/data-types/chapter-details-reporting.png)

>[!NOTE]
>
>各ディスプレイ名には、オーディオおよびビデオパラメーターに関する詳細情報へのリンクが含まれます。 リンクされたページには、Adobe、実装値、ネットワークパラメーター、レポートおよび重要な考慮事項によって収集されるビデオ広告データの詳細が含まれます。

| 表示名 | プロパティ | データタイプ | 説明 |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------|-----------|--------------------------------------------------------------|
| [[!UICONTROL チャプター完了]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/chapter-parameters.html#chapter-complete) | `isCompleted` | ブール値 | チャプターが完了したかどうか。 |
| [[!UICONTROL チャプター ID]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/chapter-parameters.html#chapter) | `ID` | 文字列 | 自動生成されたチャプターの ID。 |
| [[!UICONTROL チャプターの長さまたは期間]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/chapter-parameters.html#chapter-length) | `length` | 整数 | チャプターの長さ（秒）。 |
| [[!UICONTROL チャプター名]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/chapter-parameters.html#chapter-name) | `friendlyName` | 文字列 | チャプターまたはセグメントの名前。 |
| [[!UICONTROL チャプターオフセット]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/chapter-parameters.html#chapter-offset) | `offset` | 整数 | 開始時からのコンテンツ内のチャプターのオフセット（秒）。 |
| [[!UICONTROL チャプター位置]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/chapter-parameters.html#chapter-position) | `index` | 整数 | コンテンツ内のチャプターの位置（インデックス、整数）。 |
| [[!UICONTROL チャプター開始済み]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/chapter-parameters.html#chapter-start) | `isStarted` | ブール値 | チャプターが開始したかどうか。 |
| [[!UICONTROL チャプター再生時間]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/chapter-parameters.html#chapter-time-spent) | `timePlayed` | 整数 | チャプターの閲覧時間（秒）。 |
