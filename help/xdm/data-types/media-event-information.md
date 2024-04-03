---
title: メディアイベント情報データタイプ
description: メディアイベント情報エクスペリエンスデータモデル (XDM) データタイプについて説明します。
exl-id: 91bb7f28-b629-4044-b687-768c545ac8a2
source-git-commit: b81afb8f6c4eaedb19a58b6fe3896286f1486804
workflow-type: tm+mt
source-wordcount: '107'
ht-degree: 5%

---

# [!UICONTROL メディアイベント情報] データタイプ

[!UICONTROL メディアイベント情報] は、エクスペリエンスイベントに関連するメディアの詳細情報を記述する標準のエクスペリエンスデータモデル (XDM) データ型です。

![メディアイベント情報データタイプの図です。](../images/data-types/media-event-information.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `mediaCollection` | [!UICONTROL mediaDetails] | エクスペリエンスイベントに関連するメディア詳細情報。 このデータタイプは、 [メディアデータ収集](./media-collection-details.md) および [メディアデータレポート](./media-reporting-details.md). |
| `mediaEventTimestamp` | [!UICONTROL 文字列] | メディアイベントが発生した時刻。 |
| `mediaEventType` | [!UICONTROL 文字列] | メディアイベントタイプ。 |

{style="table-layout:auto"}

フィールドグループについて詳しくは、 [パブリック XDM リポジトリ](https://github.com/adobe/xdm/blob/master/components/datatypes/mediaevent.schema.json)
