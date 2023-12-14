---
title: メディアイベント情報データタイプ
description: メディアイベント情報エクスペリエンスデータモデル (XDM) データタイプについて説明します。
source-git-commit: 65f3dcf1cacfbc4e8a598244810d238bd88f64bd
workflow-type: tm+mt
source-wordcount: '93'
ht-degree: 6%

---

# [!UICONTROL メディアイベント情報] データタイプ

[!UICONTROL メディアイベント情報] は、エクスペリエンスイベントに関連するメディアの詳細情報を記述する標準のエクスペリエンスデータモデル (XDM) データ型です。

![メディアイベント情報データタイプの図です。](../images/data-types/media-event-information.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `mediaCollection` | [[!UICONTROL mediaDetails]](./media-details-information.md) | エクスペリエンスイベントに関連するメディア詳細情報。 |
| `mediaEventTimestamp` | [!UICONTROL 文字列] | メディアイベントが発生した時刻。 |
| `mediaEventType` | [!UICONTROL 文字列] | メディアイベントタイプ。 |

{style="table-layout:auto"}

フィールドグループについて詳しくは、 [パブリック XDM リポジトリ](https://github.com/adobe/xdm/blob/master/components/datatypes/mediaevent.schema.json)
